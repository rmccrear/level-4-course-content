### Week 3: Building a Backend for a Coffee Shop E-Commerce Site

# Day 3: Building RESTful APIs

## Overview

In today's session, we will build out our RESTful API by adding more endpoints and refining our existing routes. We'll focus on designing RESTful endpoints, implementing CRUD operations for different resources, and handling errors and edge cases.

## Day 3 Objectives

- Add more endpoints to the API
- Implement CRUD operations for users
- Refine product endpoints
- Handle errors and edge cases

## Step 1: Add More Endpoints to the API

1. **Update the User Routes**:

   ```js
   // routes/users.js
   const express = require('express');
   const router = express.Router();
   const User = require('../models/User');
   const auth = require('../middleware/auth');
   const bcrypt = require('bcryptjs');

   // Get all users (admin only)
   router.get('/', auth, async (req, res) => {
     if (req.user.role !== 'admin') {
       return res.status(403).json({ error: 'Access denied.' });
     }
     try {
       const users = await User.find();
       res.json(users);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   // Get a single user by ID
   router.get('/:id', auth, async (req, res) => {
     try {
       const user = await User.findById(req.params.id);
       if (!user) {
         return res.status(404).json({ error: 'User not found' });
       }
       res.json(user);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   // Update a user by ID
   router.put('/:id', auth, async (req, res) => {
     try {
       const { name, email, password, role } = req.body;
       const user = await User.findById(req.params.id);
       if (!user) {
         return res.status(404).json({ error: 'User not found' });
       }
       if (name) user.name = name;
       if (email) user.email = email;
       if (password) {
         const salt = await bcrypt.genSalt(10);
         user.password = await bcrypt.hash(password, salt);
       }
       if (role) user.role = role;
       await user.save();
       res.json(user);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Delete a user by ID (admin only)
   router.delete('/:id', auth, async (req, res) => {
     if (req.user.role !== 'admin') {
       return res.status(403).json({ error: 'Access denied.' });
     }
     try {
       const user = await User.findByIdAndDelete(req.params.id);
       if (!user) {
         return res.status(404).json({ error: 'User not found' });
       }
       res.json({ message: 'User deleted successfully' });
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   module.exports = router;
   ```

2. **Integrate User Routes with the Server**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const productRoutes = require('./routes/products');
   const authRoutes = require('./routes/auth');
   const userRoutes = require('./routes/users');
   const auth = require('./middleware/auth');
   const app = express();
   const port = 3000;

   // Middleware to parse JSON bodies
   app.use(express.json());

   // Connect to MongoDB
   mongoose
     .connect('your-mongodb-connection-string-here', {
       useNewUrlParser: true,
       useUnifiedTopology: true,
     })
     .then(() => {
       console.log('Connected to MongoDB');
     })
     .catch((error) => {
       console.error('Error connecting to MongoDB:', error);
     });

   // Use the product, auth, and user routes
   app.use('/products', auth, productRoutes);
   app.use('/auth', authRoutes);
   app.use('/users', auth, userRoutes);

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

## Step 2: Refine Product Endpoints

1. **Add Category Filter to Product Routes**:

   ```js
   // routes/products.js
   const express = require('express');
   const router = express.Router();
   const Product = require('../models/Product');

   // Create a new product
   router.post('/', async (req, res) => {
     try {
       const product = new Product(req.body);
       await product.save();
       res.status(201).json(product);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Get all products or filter by category
   router.get('/', async (req, res) => {
     try {
       const { category } = req.query;
       const filter = category ? { category } : {};
       const products = await Product.find(filter);
       res.json(products);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   // Get a single product by ID
   router.get('/:id', async (req, res) => {
     try {
       const product = await Product.findById(req.params.id);
       if (!product) {
         return res.status(404).json({ error: 'Product not found' });
       }
       res.json(product);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   // Update a product by ID
   router.put('/:id', async (req, res) => {
     try {
       const product = await Product.findByIdAndUpdate(
         req.params.id,
         req.body,
         { new: true, runValidators: true },
       );
       if (!product) {
         return res.status(404).json({ error: 'Product not found' });
       }
       res.json(product);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Delete a product by ID
   router.delete('/:id', async (req, res) => {
     try {
       const product = await Product.findByIdAndDelete(req.params.id);
       if (!product) {
         return res.status(404).json({ error: 'Product not found' });
       }
       res.json({ message: 'Product deleted successfully' });
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   module.exports = router;
   ```

## Step 3: Handle Errors and Edge Cases

1. **Create Error Handling Middleware**:

   ```js
   // middleware/errorHandler.js
   const errorHandler = (err, req, res, next) => {
     console.error(err.stack);
     res.status(500).json({ error: 'Something went wrong!' });
   };

   module.exports = errorHandler;
   ```

2. **Integrate Error Handling Middleware with the Server**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const productRoutes = require('./routes/products');
   const authRoutes = require('./routes/auth');
   const userRoutes = require('./routes/users');
   const auth = require('./middleware/auth');
   const errorHandler = require('./middleware/errorHandler');
   const app = express();
   const port = 3000;

   // Middleware to parse JSON bodies
   app.use(express.json());

   // Connect to MongoDB
   mongoose
     .connect('your-mongodb-connection-string-here', {
       useNewUrlParser: true,
       useUnifiedTopology: true,
     })
     .then(() => {
       console.log('Connected to MongoDB');
     })
     .catch((error) => {
       console.error('Error connecting to MongoDB:', error);
     });

   // Use the product, auth, and user routes
   app.use('/products', auth, productRoutes);
   app.use('/auth', authRoutes);
   app.use('/users', auth, userRoutes);

   // Use the error handling middleware
   app.use(errorHandler);

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

## Step 4: Test the API

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Get All Users (Admin Only)**:
     - Method: GET
     - URL: `http://localhost:3000/users`
     - Header:
       - `Authorization: Bearer <admin_jwt_token>`
   - **Get a Single User by ID**:
     - Method: GET
     - URL: `http://localhost:3000/users/<user_id>`
     - Header:
       - `Authorization: Bearer <user_jwt_token>`
   - **Update a User by ID**:
     - Method: PUT
     - URL: `http://localhost:3000/users/<user_id>`
     - Header:
       - `Authorization: Bearer <user_jwt_token>`
     - Body: JSON
       ```json
       {
         "name": "Jane Doe",
         "email": "jane@example.com"
       }
       ```
   - **Delete a User by ID (Admin Only)**:
     - Method: DELETE
     - URL: `http://localhost:3000/users/<user_id>`
     - Header:
       - `Authorization: Bearer <admin_jwt_token>`
   - **Get All Products or Filter by Category**:
     - Method: GET
     - URL: `http://localhost:3000/products?category=mugs`
   - **Get a Single Product by ID**:
     - Method: GET
     - URL: `http://localhost:3000/products/<product_id>`
   - **Update a Product by ID**:
     - Method: PUT
     - URL: `http://localhost:3000/products/<product_id>`
     - Body: JSON
       ```json
       {
         "name": "Large Coffee Mug",
         "description": "A large coffee mug, perfect for your morning coffee.",
         "price": 14.99,
         "category": "mugs",
         "stock": 50,
         "imageUrl": "http://example.com/mug.jpg"
       }
       ```
   - **Delete a Product by ID**:
     - Method: DELETE
     - URL: `http://localhost:3000/products/<product_id>`

## Conclusion

In this session, we've added more endpoints to our API, implemented CRUD operations for users, refined product endpoints, and handled errors and edge cases. In the next session, we will add advanced features like pagination, sorting, and filtering to our API.
