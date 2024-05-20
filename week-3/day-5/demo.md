# Day 5: Finishing the API and Deploying to Render.com

## Overview

In today's session, we will finalize our API by adding any missing features, ensuring it is production-ready, and deploying it to Render.com.

## Day 5 Objectives

- Finalize the API with any remaining features
- Prepare the API for deployment
- Deploy the API to Render.com

## Step 1: Finalize the API

### 1. Implement Role-Based Access Control (RBAC)

1. **Create Middleware for RBAC**:

   ```js
   // middleware/role.js
   const role = (requiredRole) => {
     return (req, res, next) => {
       if (req.user.role !== requiredRole) {
         return res.status(403).json({ error: 'Access denied.' });
       }
       next();
     };
   };

   module.exports = role;
   ```

2. **Protect Routes with Roles**:

   ```js
   // routes/products.js
   const express = require('express');
   const router = express.Router();
   const Product = require('../models/Product');
   const upload = require('../middleware/upload');
   const auth = require('../middleware/auth');
   const role = require('../middleware/role');

   // Create a new product with image upload (admin only)
   router.post('/', auth, role('admin'), upload, async (req, res) => {
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

   // Other routes remain the same, add `auth` and `role` middleware as needed
   ```

### 2. Implement Input Validation

1. **Install Joi for Validation**:

   ```bash
   npm install joi
   ```

2. **Create Validation Middleware**:

   ```js
   // middleware/validate.js
   const Joi = require('joi');

   const validateProduct = (req, res, next) => {
     const schema = Joi.object({
       name: Joi.string().required(),
       description: Joi.string().required(),
       price: Joi.number().required(),
       category: Joi.string().required(),
       stock: Joi.number().required(),
       imageUrl: Joi.string().uri(),
     });

     const { error } = schema.validate(req.body);
     if (error) {
       return res.status(400).json({ error: error.details[0].message });
     }
     next();
   };

   module.exports = { validateProduct };
   ```

3. **Use Validation Middleware in Routes**:

   ```js
   // routes/products.js
   const express = require('express');
   const router = express.Router();
   const Product = require('../models/Product');
   const upload = require('../middleware/upload');
   const auth = require('../middleware/auth');
   const role = require('../middleware/role');
   const { validateProduct } = require('../middleware/validate');

   // Create a new product with image upload (admin only)
   router.post(
     '/',
     auth,
     role('admin'),
     upload,
     validateProduct,
     async (req, res) => {
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
     },
   );

   // Other routes remain the same, add `auth`, `role`, and `validate` middleware as needed
   ```

### 3. Prepare for Deployment

1. **Set Up Environment Variables**:

   Create a `.env` file for environment variables.

   ```plaintext
   PORT=3000
   MONGODB_URI=your-mongodb-connection-string
   JWT_SECRET=your_jwt_secret
   ```

2. **Use Environment Variables in the Application**:

   Update your `index.js` to use environment variables.

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
   require('dotenv').config();
   const app = express();
   const port = process.env.PORT || 3000;

   // Middleware to parse JSON bodies
   app.use(express.json());

   // Serve static files
   app.use('/uploads', express.static(path.join(__dirname, 'uploads')));

   // Connect to MongoDB
   mongoose
     .connect(process.env.MONGODB_URI, {
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

## Step 2: Deploy to Render.com

### 1. Create a Render.com Account

- Go to [Render.com](https://render.com) and create an account if you don't have one.

### 2. Create a New Web Service

1. **Create New Web Service**:

   - Click on "New" and select "Web Service".

2. **Connect Your Repository**:

   - Connect your GitHub repository containing the project.

3. **Configure the Service**:

   - Name: `coffee-shop-backend`
   - Environment: `Node`
   - Build Command: `npm install`
   - Start Command: `node index.js`
   - Environment Variables: Add the variables from your `.env` file.

4. **Deploy**:
   - Click "Create Web Service" to start the deployment.

## Step 3: Test the Deployed API

1. **Get the URL of Your Deployed API**:

   - Once the deployment is complete, get the URL from Render.com.

2. **Test the Endpoints**:

   - Use Thunder Client, Postman, or any API testing tool to test the deployed endpoints using the provided URL.

   - **Example**:
     - **URL**: `https://your-service-url.onrender.com/products`
     - **Method**: GET

## Conclusion

In this session, we finalized our API by adding role-based access control, input validation, and preparing the project for deployment. We then deployed the API to Render.com and tested the endpoints. This concludes our backend development for the coffee shop e-commerce site.
