### Week 3: Building a Backend for a Coffee Shop E-Commerce Site

# Day 1: Setting Up the Project and Defining the Product Model

## Overview

This week, we will build a backend API for a coffee shop e-commerce site. The backend will manage products that the coffee shop sells. Over the next five days, we will progressively build this API, which can be integrated into a Next.js client-side e-commerce frontend.

## Day 1 Objectives

- Set up a new Express project
- Connect to MongoDB using Mongoose
- Define the Product model

## Step 1: Set Up the Project

1. **Initialize the Project and Install Dependencies**:

   ```bash
   mkdir coffee-shop-backend
   cd coffee-shop-backend
   npm init -y
   npm install express mongoose
   ```

2. **Set Up the Server**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
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

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

## Step 2: Define the Product Model

1. **Create the Product Schema and Model**:

   ```js
   // models/Product.js
   const mongoose = require('mongoose');

   const productSchema = new mongoose.Schema({
     name: {
       type: String,
       required: true,
     },
     description: {
       type: String,
       required: true,
     },
     price: {
       type: Number,
       required: true,
     },
     category: {
       type: String,
       required: true,
     },
     stock: {
       type: Number,
       required: true,
     },
     imageUrl: {
       type: String,
       required: true,
     },
   });

   const Product = mongoose.model('Product', productSchema);

   module.exports = Product;
   ```

## Step 3: Implement Basic CRUD Operations

1. **Set Up the Routes**:

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

   // Get all products
   router.get('/', async (req, res) => {
     try {
       const products = await Product.find();
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

2. **Integrate the Routes with the Server**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const productRoutes = require('./routes/products');
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

   // Use the product routes
   app.use('/products', productRoutes);

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

   - **Create a Product**:
     - Method: POST
     - URL: `http://localhost:3000/products`
     - Body: JSON
       ```json
       {
         "name": "Coffee Mug",
         "description": "A large coffee mug.",
         "price": 12.99,
         "category": "mugs",
         "stock": 100,
         "imageUrl": "http://example.com/mug.jpg"
       }
       ```
   - **Get All Products**:
     - Method: GET
     - URL: `http://localhost:3000/products`
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

In this session, we've set up a new Express project, connected to MongoDB using Mongoose, and defined the Product model. We've also implemented basic CRUD operations for products and tested them using Thunder Client or Postman. In the next session, we will add more advanced features to our API.


