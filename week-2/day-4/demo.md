# Day 4, Hour 1: Setting Up a New Project for Session Management and RBAC

Welcome to Day 4 of Week 2! Today, we will start by setting up a new project to explore session management and role-based access control (RBAC). This session will cover how to protect routes and manage user roles to restrict access to certain parts of your application.

## Objectives

- Set up a new Express project.
- Implement middleware to protect routes.
- Understand role-based access control and its importance.

### Step 1: Set Up a New Project

1. **Initialize the Project and Install Dependencies**:

   ```bash
   mkdir express-session-rbac
   cd express-session-rbac
   npm init -y
   npm install express mongoose bcryptjs jsonwebtoken
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

main().catch((err) => console.log(err));

async function main() {
  await mongoose.connect('your connection string here, place in .env');
  console.log('connected to mongoose');
}
```

### Step 2: Define the User Schema with Role and Password Hashing

1. **Create `models/User.js`**:

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
   // This next part can also be done inside the controller function instead of as a method here
   userSchema.methods.comparePassword = async function (candidatePassword) {
     return await bcrypt.compare(candidatePassword, this.password);
   };

   const User = mongoose.model('User', userSchema);

   module.exports = User;
   ```

### Step 3: Implement Registration and Login Endpoints

1. **Update `index.js` to include registration and login routes**:

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

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 4: Test Registration and Login Endpoints

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

### Step 5: Implement Middleware for Route Protection

We'll use middleware to check if a user is authenticated before allowing access to certain routes.

1. **Create Middleware for Authentication**:

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

### Step 6: Protect Routes with Authentication Middleware

1. **Update `index.js` to include protected routes**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const User = require('./models/User');
   const auth = require('./middleware/auth');
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

   // Protected route example
   app.get('/dashboard', auth, (req, res) => {
     res.send('This is the dashboard.');
   });

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 7: Test Protected Routes

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Access Protected Route**:
     - Method: GET
     - URL: `http://localhost:3000/dashboard`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - If the token is valid, you should see "This is the dashboard." If not, you should receive an error message.

## Conclusion

In this session, we've set up a new Express project, implemented user registration and login with password hashing, and created middleware for route protection. In the next session, we will implement role-based access control (RBAC) to restrict access based on user roles.

<!--! Hour 2 -->

# Day 4, Hour 2: Implementing Role-Based Access Control (RBAC)

Welcome to Hour 2 of Day 4! In this session, we will implement role-based access control (RBAC) to restrict access based on user roles. We'll build on the project we started in Hour 1.

## Objectives

- Understand role-based access control (RBAC).
- Update the user schema to include roles.
- Implement middleware to check user roles.
- Protect routes based on user roles.

### Step 1: Update User Schema to Include Roles

1. **Update `models/User.js` to include a role field**:

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

### Step 2: Create Middleware for Role-Based Access Control

1. **Create Middleware for Role-Based Access Control**:

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

### Step 3: Protect Routes with Role-Based Access Control

1. **Update `index.js` to include RBAC middleware and protected routes**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const User = require('./models/User');
   const auth = require('./middleware/auth');
   const role = require('./middleware/role');
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
       const { name, email, password, role } = req.body;
       const user = new User({ name, email, password, role });
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

   // Protected route example
   app.get('/dashboard', auth, (req, res) => {
     res.send('This is the dashboard.');
   });

   // Admin-only route example
   app.get('/admin', auth, role('admin'), (req, res) => {
     res.send('This is the admin panel.');
   });

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 4: Test Role-Based Access Control

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Register an Admin User**:

     - Method: POST
     - URL: `http://localhost:3000/register`
     - Body: JSON
       ```json
       {
         "name": "Admin User",
         "email": "admin@example.com",
         "password": "adminpassword123",
         "role": "admin"
       }
       ```

   - **Register a Regular User**:

     - Method: POST
     - URL: `http://localhost:3000/register`
     - Body: JSON
       ```json
       {
         "name": "Regular User",
         "email": "user@example.com",
         "password": "userpassword123",
         "role": "user"
       }
       ```

   - **Login as Admin User**:

     - Method: POST
     - URL: `http://localhost:3000/login`
     - Body: JSON
       ```json
       {
         "email": "admin@example.com",
         "password": "adminpassword123"
       }
       ```
     - Copy the token from the response.

   - **Access Admin Route as Admin**:

     - Method: GET
     - URL: `http://localhost:3000/admin`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - If the token belongs to an admin, you should see "This is the admin panel." If not, you should receive an access denied error.

   - **Access Admin Route as Regular User**:
     - Method: GET
     - URL: `http://localhost:3000/admin`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - If the token belongs to a regular user, you should receive an access denied error.

## Conclusion

In this session, we've implemented role-based access control (RBAC) to restrict access based on user roles. We updated the user schema to include roles, created middleware to check user roles, and protected routes based on user roles. In the next session, we will continue to build on these concepts and explore more advanced topics in authentication and authorization.
