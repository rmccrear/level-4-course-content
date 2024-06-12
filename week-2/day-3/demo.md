# Day 3, Hour 1: Introduction to Authentication and Storing Passwords

Welcome to Day 3 of Week 2! Today, we will start learning about authentication. We will focus on user registration and storing passwords securely. In this hour, we will begin by storing passwords in plain text and then discuss why this is a bad practice. We will then introduce password hashing as a secure alternative.

## Objectives

- Understand the basics of authentication.
- Learn about the risks of storing passwords in plain text.
- Understand what hashing is and why it's important.
- Store user passwords securely using hashing.

## Introduction to Authentication

Authentication is the process of verifying the identity of a user. When users log into a system, they provide credentials (usually a username and password) to prove their identity.

### Step 1: Set Up the Project

1. **Initialize the Project and Install Dependencies**:

   ```bash
   mkdir express-mongodb-auth
   cd express-mongodb-auth
   npm init -y
   npm install express mongoose
   ```

2. **Create the Server**:

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

### Step 2: Define the User Schema (Plain Text Passwords)

1. **Create `models/User.js`**:

   ```js
   // models/User.js
   const mongoose = require('mongoose');

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
   });

   const User = mongoose.model('User', userSchema);

   module.exports = User;
   ```

### Step 3: Implement Registration Endpoint (Plain Text Passwords)

1. **Update `index.js` to include a registration route**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const User = require('./models/User');
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

   // Route to register a new user with plain text passwords
   app.post('/register', async (req, res) => {
     try {
       const { name, email, password } = req.body;
       const user = new User({ name, email, password });
       await user.save();
       res.status(201).json({ message: 'User registered successfully' });
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 4: Test Registration Endpoint (Plain Text Passwords)

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Register a User**:
     - Method: POST
     - URL: `http://localhost:3000/register`
     - Body: JSON
       ```json
       {
         "name": "John Doe",
         "email": "john@example.com",
         "password": "password123"
       }
       ```

3. **Check the Database**:

   - Connect to your MongoDB database and check the `users` collection. You should see the user's password stored in plain text.

### Step 5: Explain the Risks of Storing Plain Text Passwords

**Why Storing Plain Text Passwords is Bad:**

- **Security Risks**: If an attacker gains access to your database, they can see all user passwords.
- **Privacy Violations**: Users often reuse passwords across multiple sites. If their password is exposed, it can lead to breaches on other sites as well.
- **Reputation Damage**: Storing passwords in plain text is considered a serious security flaw and can damage your reputation if discovered.

### Step 6: Introduce Hashing

**What is Hashing?**

Hashing is a one-way cryptographic function that converts a password into a fixed-size string of characters, which is typically a hash value. Hashing algorithms are designed to be irreversible, meaning you can't convert the hash back into the original password.

**Why Use Hashing?**

- **Security**: Even if an attacker gains access to your database, they won't see the actual passwords, only the hashed values.
- **Data Integrity**: Hashing helps maintain the integrity of passwords by ensuring they are stored securely.

### Step 7: Update the User Schema to Hash Passwords

1. **Update `models/User.js` to hash passwords**:

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

   const User = mongoose.model('User', userSchema);

   module.exports = User;
   ```

### Step 8: Implement Registration Endpoint (Hashed Passwords)

1. **Update `index.js` to include the new registration route**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const User = require('./models/User');
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

   // Route to register a new user with hashed passwords
   async function register(req, res, next) {
     try {
       const { name, email, password } = req.body;
       const user = new User({ name, email, password });
       user.save();
       const createdUser = await User.findOne({ email }).exec();
       res.status(201).json(createdUser); // Normally an API will return an object with a message "User created"
       // A second API call from the client would be made to get the new User, but we can return it like this
     } catch (error) {
       res.status(500).send({ error: error.message });
     }
   }

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 9: Test Registration Endpoint (Hashed Passwords)

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Register a User**:
     - Method: POST
     - URL: `http://localhost:3000/register`
     - Body: JSON
       ```json
       {
         "name": "Jane Doe",
         "email": "jane@example.com",
         "password": "password123"
       }
       ```

3. **Check the Database**:

   - Connect to your MongoDB database and check the `users` collection. You should see the user's password stored as a hashed value.

## Conclusion

In this session, we've introduced the basics of authentication and discussed the importance of storing passwords securely. We've demonstrated the risks of storing passwords in plain text and implemented password hashing using bcrypt. In the next session, we will build on this foundation to implement a login endpoint and verify user credentials.

<!--! Hour 2 -->

# Day 3, Hour 2: Implementing User Login with Password Verification

Welcome to Hour 2 of Day 3! In this session, we will implement the login functionality. We'll focus on verifying user credentials and returning a token upon successful login.

## Objectives

- Understand how to verify hashed passwords.
- Implement a login endpoint to authenticate users.
- Generate a JWT token upon successful login.

### Step 1: Understand Password Verification

In the previous session, we learned how to hash passwords before storing them in the database. Now, we need to verify the hashed password when a user tries to log in. We use bcrypt to compare the hashed password with the plain text password provided by the user.

### Step 2: Implement Password Verification

1. **Update `models/User.js` to include a password verification method**:

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

### Step 3: Implement the Login Endpoint

1. **Update `index.js` to include the login route**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const User = require('./models/User');
   const jwt = require('jsonwebtoken');
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

   // Route to register a new user with hashed passwords
   app.post('/register', async (req, res) => {
     try {
       const { name, email, password } = req.body;
       const user = new User({ name, email, password });
       await user.save();
       res.status(201).json({ message: 'User registered successfully' });
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Route to login a user
   app.post('/login', async (req, res) => {
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
       const token = jwt.sign({ userId: user._id }, 'your_jwt_secret', {
         expiresIn: '1h',
       });
       res.json({ token });
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 4: Test the Login Endpoint

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Login a User**:

     - Method: POST
     - URL: `http://localhost:3000/login`
     - Body: JSON

       ```json
       {
         "email": "john@example.com",
         "password": "password123"
       }
       ```

     - If successful, this should return a JWT token.

### Step 5: Explain JWT Tokens

**What is a JWT Token?**

JWT (JSON Web Token) is a compact, URL-safe token that represents a set of claims. It's commonly used for authentication and information exchange. A JWT token is composed of three parts: Header, Payload, and Signature.

**Why Use JWT Tokens?**

- **Security**: JWT tokens can be signed to ensure the data has not been tampered with.
- **Scalability**: They can be used across different domains and servers.
- **Efficiency**: JWT tokens are stateless, meaning they don't require server-side sessions.

**Structure of a JWT Token**:

- **Header**: Contains the token type (JWT) and the hashing algorithm (e.g., HMAC SHA256).
- **Payload**: Contains the claims (user data, metadata).
- **Signature**: Ensures the token is not altered.

### Step 6: Discuss Best Practices for Storing Tokens

1. **Storing JWT Tokens**:
   - Store tokens securely on the client-side (e.g., localStorage or cookies).
   - Ensure tokens are transmitted over HTTPS.
   - Set appropriate expiration times for tokens.

## Conclusion

In this session, we've implemented the login functionality with password verification using bcrypt. We've also introduced JWT tokens for authentication. In the next session, we will cover maintaining sessions after authentication and implementing role-based access control (RBAC).
