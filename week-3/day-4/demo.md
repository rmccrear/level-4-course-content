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
         .skip((page - 1) * limit).exec();
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

### Set Up Cloudinary and Multer for Image Uploads

1. **Install Required Packages**:

   Install `multer` and `multer-storage-cloudinary` for handling file uploads and integrating with Cloudinary.

   ```sh
   npm install multer multer-storage-cloudinary
   ```

2. **Configure Cloudinary**:

   Create a `cloudinary.js` file to configure and export your Cloudinary settings.

   ```javascript
   const cloudinary = require('cloudinary').v2;
   const { CloudinaryStorage } = require('multer-storage-cloudinary');

   cloudinary.config({
     cloud_name: process.env.CLOUDINARY_CLOUD_NAME,
     api_key: process.env.CLOUDINARY_KEY,
     api_secret: process.env.CLOUDINARY_SECRET,
   });

   const storage = new CloudinaryStorage({
     cloudinary: cloudinary,
     params: {
       folder: 'coffee',
       allowedFormats: ['jpg', 'png', 'jpeg'],
     },
   });

   module.exports = { cloudinary, storage };
   ```

3. **Set Up Multer Middleware**:

   Configure Multer to use Cloudinary storage and include it in your route to handle file uploads.

   ```javascript
   const multer = require('multer');
   const { storage } = require('../config/cloudinary');
   const upload = multer({ storage });

   module.exports = upload;
   ```

4. **Update Product Routes to Handle File Uploads**:

   Include the upload middleware in your route to handle image uploads when creating and updating products.

   ```javascript
   const upload = require('../middleware/upload');

   // Create a new product with image upload
   router.post('/', upload.single('image'), async (req, res) => {
     try {
       const { name, description, price, category, stock } = req.body;
       const imageUrl = req.file.path; // Cloudinary returns the path of the uploaded image
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
   ```
