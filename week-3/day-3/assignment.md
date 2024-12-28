# Assignment: Backend - Assignment 3

## Objective

- Add more endpoints to the API
- Implement CRUD operations for users
- Refine product endpoints
- Handle errors and edge cases

## Instructions

#### Step 1: Add More Endpoints to the API

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

#### Step 2: Refine Product Endpoints

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

### Step 3: Handle Errors and Edge Cases

**Note:** Double check that you are on express version 5.0.0 or higher. If not, you may need to update your express version in order for errors to be handled in async functions routes.

```bash
  npm install express@latest
```

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

### Step 4: Test the API

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

**Note:** All requests to `products` and `users` should now include the `Authorization` header with the JWT token. Requests to `/auth/login` and `/auth/register` should not include the `Authorization` header. This is because you don't need to be signed in to sign in, and you don't need to be registered to register!

You can set the header `Authorization` in each request (as shown bellow), or you can set it for the entire collection in Postman using the `Bearer Auth` setting of the collection.

   - **Get All Users (Admin Only)**:

     - Method: GET
     - URL: `http://localhost:3000/users`
     - Header:
       - `Authorization: Bearer <admin_jwt_token>`
     - Screenshot the GET request and response.

   - **Get a Single User by ID**:

     - Method: GET
     - URL: `http://localhost:3000/users/<user_id>`
     - Header:
       - `Authorization: Bearer <user_jwt_token>`
     - Screenshot the GET request and response.

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

     - Screenshot the PUT request and response.

   - **Delete a User by ID (Admin Only)**:

     - Method: DELETE
     - URL: `http://localhost:3000/users/<user_id>`
     - Header:
       - `Authorization: Bearer <admin_jwt_token>`
     - Screenshot the DELETE request and response.

   - **Get All Products or Filter by Category**:

     - Method: GET
     - URL: `http://localhost:3000/products?category=mugs`
     - Screenshot the GET request and response.

   - **Get a Single Product by ID**:

     - Method: GET
     - URL: `http://localhost:3000/products/<product_id>`
     - Screenshot the GET request and response.

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

     - Screenshot the PUT request and response.

   - **Delete a Product by ID**:
     - Method: DELETE
     - URL: `http://localhost:3000/products/<product_id>`
     - Screenshot the DELETE request and response.

### Submission

- **GitHub Repository**: Create a new repository named `coffee-shop-backend`. Push your project to the repository and submit the URL. Ensure it includes all necessary files to run the server, including the `README.md`.
- **Screenshots**: Include the screenshots of your POST, GET, PUT, and DELETE requests in the `README.md`.

### Rubric

| Criteria           | Limited (0 pts)                       | Partial (3 pts)                          | Complete (5 pts)                                                |
| ------------------ | ------------------------------------- | ---------------------------------------- | --------------------------------------------------------------- |
| **Project Setup**  | Significant setup issues              | Minor issues in setup                    | Project set up correctly with required packages                 |
| **User Routes**    | Routes not implemented                | Routes implemented with minor issues     | User routes correctly implemented and working                   |
| **Product Routes** | Routes not refined                    | Routes refined with minor issues         | Product routes refined with category filter                     |
| **Middleware**     | Middleware not implemented            | Middleware implemented with minor issues | Authentication and error handling middleware implemented        |
| **API Testing**    | Requests and responses not documented | Some requests and responses documented   | All required requests and responses documented with screenshots |
