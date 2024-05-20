### Week 3: Building a Backend for a Coffee Shop E-Commerce Site

# Day 4: Advanced Features

## Overview

In today's session, we will add advanced features to our API, such as pagination, sorting, and filtering. These features will improve the usability and performance of our e-commerce backend.

## Day 4 Objectives

- Implement pagination for product listings
- Add sorting and filtering capabilities
- Handle file uploads for product images

## Step 1: Implement Pagination for Product Listings

1. **Update Product Routes to Include Pagination**:

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

   // Get all products with pagination
   router.get('/', async (req, res) => {
     try {
       const {
         page = 1,
         limit = 10,
         category,
         sortBy,
         sortOrder = 'asc',
       } = req.query;
       const filter = category ? { category } : {};
       const sort = sortBy ? { [sortBy]: sortOrder === 'asc' ? 1 : -1 } : {};
       const products = await Product.find(filter)
         .sort(sort)
         .limit(parseInt(limit))
         .skip((page - 1) * limit);
       const total = await Product.countDocuments(filter);
       res.json({ total, products });
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

## Step 2: Handle File Uploads for Product Images

1. **Install Multer for File Uploads**:

   ```bash
   npm install multer
   ```

2. **Set Up Multer Middleware**:

   ```js
   // middleware/upload.js
   const multer = require('multer');
   const path = require('path');

   // Set storage engine
   const storage = multer.diskStorage({
     destination: './uploads/',
     filename: function (req, file, cb) {
       cb(
         null,
         file.fieldname + '-' + Date.now() + path.extname(file.originalname),
       );
     },
   });

   // Initialize upload
   const upload = multer({
     storage: storage,
     limits: { fileSize: 1000000 },
     fileFilter: function (req, file, cb) {
       checkFileType(file, cb);
     },
   }).single('image');

   // Check file type
   function checkFileType(file, cb) {
     const filetypes = /jpeg|jpg|png|gif/;
     const extname = filetypes.test(
       path.extname(file.originalname).toLowerCase(),
     );
     const mimetype = filetypes.test(file.mimetype);

     if (mimetype && extname) {
       return cb(null, true);
     } else {
       cb('Error: Images Only!');
     }
   }

   module.exports = upload;
   ```

3. **Update Product Routes to Handle File Uploads**:

   ```js
   // routes/products.js
   const express = require('express');
   const router = express.Router();
   const Product = require('../models/Product');
   const upload = require('../middleware/upload');

   // Create a new product with image upload
   router.post('/', upload, async (req, res) => {
     try {
       const { name, description, price, category, stock } = req.body;
       const imageUrl = req.file ? `/uploads/${req.file.filename}` : '';
       const product = new Product({
         name,
         description,
         price,
         category,
         stock,
         imageUrl,
       });
       await product.save();
       res.status(201).json(product);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Get all products with pagination
   router.get('/', async (req, res) => {
     try {
       const {
         page = 1,
         limit = 10,
         category,
         sortBy,
         sortOrder = 'asc',
       } = req.query;
       const filter = category ? { category } : {};
       const sort = sortBy ? { [sortBy]: sortOrder === 'asc' ? 1 : -1 } : {};
       const products = await Product.find(filter)
         .sort(sort)
         .limit(parseInt(limit))
         .skip((page - 1) * limit);
       const total = await Product.countDocuments(filter);
       res.json({ total, products });
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
   router.put('/:id', upload, async (req, res) => {
     try {
       const { name, description, price, category, stock } = req.body;
       const imageUrl = req.file
         ? `/uploads/${req.file.filename}`
         : req.body.imageUrl;
       const product = await Product.findByIdAndUpdate(
         req.params.id,
         { name, description, price, category, stock, imageUrl },
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

## Step 3: Integrate File Upload Middleware with the Server

1. **Update Server Setup**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const path = require('path');
   const productRoutes = require('./routes/products');
   const authRoutes = require('./routes/auth');
   const userRoutes = require('./routes/users');
   const auth = require('./middleware/auth');
   const errorHandler = require('./middleware/errorHandler');
   const app = express();
   const port = 3000;

   // Middleware to parse JSON bodies
   app.use(express.json());

   // Serve static files
   app.use('/uploads', express.static(path.join(__dirname, 'uploads')));

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

   - **Create a Product with Image Upload**:
     - Method: POST
     - URL: `http://localhost:3000/products`
     - Headers:
       - `Authorization: Bearer <your_jwt_token>`
     - Body: Form Data
       - `name`: `Coffee Mug`
       - `description`: `A large coffee mug.`
       - `price`: `12.99`
       - `category`: `mugs`
       - `stock`: `100`
       - `image`: Upload a file
   - **Get All Products with Pagination and Filtering**:
     - Method: GET
     - URL: `http://localhost:3000/products?page=1&limit=10&category=mugs&sortBy=price&sortOrder=desc`

## Conclusion

In this session, we've added advanced features to our API, such as pagination, sorting, filtering, and file uploads. These features improve the usability and performance of our e-commerce backend. In the next session, we will review our API and practice integrating it with a frontend application.
