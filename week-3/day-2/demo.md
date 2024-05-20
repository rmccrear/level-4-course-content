### Week 3: Building a Backend for a Coffee Shop E-Commerce Site

# Day 2: Implementing Authentication

## Overview

In today's session, we will add user authentication to our backend. We'll implement user registration and login using JWT for secure authentication.

## Day 2 Objectives

- Set up user authentication with JWT
- Create user registration and login endpoints
- Protect routes with authentication middleware

## Step 1: Set Up User Authentication

1. **Install Additional Dependencies**:

   ```bash
   npm install bcryptjs jsonwebtoken
   ```

2. **Create the User Model**:

   ```js
   // models/User.js
   const mongoose = require('mongoose');
   const bcrypt = require('bcryptjs');

   const userSchema = new mongoose.Schema({
     name: {
       type: String,
       required: true,
     },
     email: {
       type: String,
       required: true,
       unique: true,
     },
     password: {
       type: String,
       required: true,
     },
     role: {
       type: String,
       default: 'user',
       enum: ['user', 'admin'],
     },
   });

   // Hash the password before saving the user
   userSchema.pre('save', async function (next) {
     if (!this.isModified('password')) {
       return next();
     }
     try {
       const salt = await bcrypt.genSalt(10);
       this.password = await bcrypt.hash(this.password, salt);
       next();
     } catch (error) {
       next(error);
     }
   });

   // Method to compare passwords
   userSchema.methods.comparePassword = async function (candidatePassword) {
     return await bcrypt.compare(candidatePassword, this.password);
   };

   const User = mongoose.model('User', userSchema);

   module.exports = User;
   ```

## Step 2: Implement Registration and Login Endpoints

1. **Set Up the Routes**:

   ```js
   // routes/auth.js
   const express = require('express');
   const router = express.Router();
   const User = require('../models/User');
   const jwt = require('jsonwebtoken');

   // Register a new user
   router.post('/register', async (req, res) => {
     try {
       const { name, email, password, role } = req.body;
       const user = new User({ name, email, password, role });
       await user.save();
       res.status(201).json({ message: 'User registered successfully' });
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Login a user
   router.post('/login', async (req, res) => {
     try {
       const { email, password } = req.body;
       const user = await User.findOne({ email });
       if (!user) {
         return res.status(400).json({ error: 'Invalid email or password' });
       }
       const isMatch = await user.comparePassword(password);
       if (!isMatch) {
         return res.status(400).json({ error: 'Invalid email or password' });
       }
       const token = jwt.sign(
         { userId: user._id, role: user.role },
         'your_jwt_secret',
         { expiresIn: '1h' },
       );
       res.json({ token });
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   module.exports = router;
   ```

2. **Create Authentication Middleware**:

   ```js
   // middleware/auth.js
   const jwt = require('jsonwebtoken');

   const auth = (req, res, next) => {
     const token = req.header('Authorization').replace('Bearer ', '');
     if (!token) {
       return res
         .status(401)
         .json({ error: 'Access denied. No token provided.' });
     }
     try {
       const decoded = jwt.verify(token, 'your_jwt_secret');
       req.user = decoded;
       next();
     } catch (error) {
       res.status(400).json({ error: 'Invalid token.' });
     }
   };

   module.exports = auth;
   ```

3. **Integrate the Routes with the Server**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const productRoutes = require('./routes/products');
   const authRoutes = require('./routes/auth');
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

   // Use the product and auth routes
   app.use('/products', productRoutes);
   app.use('/auth', authRoutes);

   // Protect product routes
   app.use('/products', auth, productRoutes);

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

## Step 3: Test the API

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Register a User**:
     - Method: POST
     - URL: `http://localhost:3000/auth/register`
     - Body: JSON
       ```json
       {
         "name": "John Doe",
         "email": "john@example.com",
         "password": "password123",
         "role": "user"
       }
       ```
   - **Login a User**:
     - Method: POST
     - URL: `http://localhost:3000/auth/login`
     - Body: JSON
       ```json
       {
         "email": "john@example.com",
         "password": "password123"
       }
       ```
     - Copy the token from the response.
   - **Access Protected Product Routes**:
     - Method: GET
     - URL: `http://localhost:3000/products`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - If the token is valid, you should see the products.

## Conclusion

In this session, we've set up user authentication with JWT, created user registration and login endpoints, and protected product routes with authentication middleware. In the next session, we will implement more advanced features and refine our API.
