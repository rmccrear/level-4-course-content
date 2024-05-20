# Day 5, Hour 1: Comprehensive Review of MongoDB and Mongoose

Welcome to Day 5 of Week 2! Today, we will review everything we've learned about MongoDB and Mongoose. This session will reinforce key concepts and give you the opportunity to practice and ask questions.

## Objectives

- Review MongoDB and Mongoose basics.
- Reinforce CRUD operations.
- Revisit advanced queries and validations.
- Practice user authentication and RBAC.

### Step 1: Review MongoDB and Mongoose Basics

**MongoDB Basics:**

- MongoDB is a NoSQL database.
- It stores data in flexible, JSON-like documents.
- Key concepts include collections, documents, and fields.

**Mongoose Basics:**

- Mongoose is an ODM (Object Data Modeling) library for MongoDB.
- It provides a schema-based solution to model your data.
- Key concepts include schemas, models, and queries.

### Step 2: Practice Basic CRUD Operations

1. **Create a new project folder and initialize it**:

   ```bash
   mkdir mongodb-review
   cd mongodb-review
   npm init -y
   npm install express mongoose bcryptjs jsonwebtoken
   ```

2. **Set Up the Project**:

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

   // Route to register a new user
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

3. **Create the User Model**:

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

### Step 3: Revisit Advanced Queries and Validations

**Advanced Queries**:

- Finding users by criteria (e.g., age range, active status).
- Sorting and limiting results.

**Example**:

```js
// Route to get users within an age range
app.get('/users/age-range/:min/:max', async (req, res) => {
  const { min, max } = req.params;
  try {
    const users = await User.find({
      age: { $gte: min, $lte: max },
    }).sort({ age: 1 });
    res.json(users);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});
```

**Validations**:

- Ensuring data integrity with Mongoose validators.

**Example**:

```js
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Name is required'],
  },
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    match: [/.+\@.+\..+/, 'Please enter a valid email address'],
  },
  age: {
    type: Number,
    required: [true, 'Age is required'],
    min: [18, 'Must be at least 18'],
    max: [65, 'Must be at most 65'],
  },
  isActive: {
    type: Boolean,
    default: true,
  },
});
```

### Step 4: Review User Authentication and RBAC

**Authentication**:

- User registration with hashed passwords.
- User login with JWT token generation.

**Role-Based Access Control (RBAC)**:

- Protecting routes with middleware.
- Restricting access based on user roles.

### Step 5: Practice and Questions

- **Practice**: Create a few routes and implement the above concepts.
- **Questions**: Open floor for questions and clarifications.

### Conclusion

In this session, we've reviewed MongoDB and Mongoose basics, CRUD operations, advanced queries, validations, user authentication, and RBAC. This comprehensive review should reinforce your understanding and help you solidify these concepts.

<!--! Hour 2 -->

# Day 5, Hour 2: Hands-On Practice and Q&A Session

Welcome to Hour 2 of Day 5! In this session, we will continue our review by practicing what we've learned throughout the week. We will also have a Q&A session to address any questions or uncertainties.

## Objectives

- Practice implementing CRUD operations with Mongoose.
- Reinforce user authentication and RBAC concepts.
- Address questions and clarify any doubts.

### Step 1: Hands-On Practice

**Task 1: Set Up a Simple Express Project**

1. **Initialize the Project**:

   ```bash
   mkdir mongodb-practice
   cd mongodb-practice
   npm init -y
   npm install express mongoose bcryptjs jsonwebtoken
   ```

2. **Set Up the Server**:

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

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

3. **Create the User Model**:

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

4. **Implement CRUD Operations**:

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

   // Route to register a new user
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

   // Route to get all users
   app.get('/users', auth, async (req, res) => {
     try {
       const users = await User.find();
       res.json(users);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   // Route to update a user
   app.put('/users/:id', auth, async (req, res) => {
     try {
       const user = await User.findByIdAndUpdate(req.params.id, req.body, {
         new: true,
         runValidators: true,
       });
       if (!user) {
         return res.status(404).json({ error: 'User not found' });
       }
       res.json(user);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Route to delete a user
   app.delete('/users/:id', auth, async (req, res) => {
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

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

5. **Test CRUD Operations**:

   - **Register a User**:
     - Method: POST
     - URL: `http://localhost:3000/register`
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
     - URL: `http://localhost:3000/login`
     - Body: JSON
       ```json
       {
         "email": "john@example.com",
         "password": "password123"
       }
       ```
     - Copy the token from the response.
   - **Get All Users**:
     - Method: GET
     - URL: `http://localhost:3000/users`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
   - **Update a User**:
     - Method: PUT
     - URL: `http://localhost:3000/users/<user_id>`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - Body: JSON
       ```json
       {
         "name": "Jane Doe"
       }
       ```
   - **Delete a User**:
     - Method: DELETE
     - URL: `http://localhost:3000/users/<user_id>`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`

### Step 2: Address Questions and Clarify Doubts

**Open Q&A Session:**

- **Questions**: Ask any questions related to MongoDB, Mongoose, CRUD operations, authentication, or RBAC.
- **Clarifications**: Provide clarifications on any concepts that are still unclear.
- **Examples**: Go through additional examples or revisit previous examples as needed.

### Conclusion

In this session, we've reviewed and practiced everything we've learned about MongoDB and Mongoose throughout the week. We've reinforced CRUD operations, user authentication, and RBAC concepts. Use this time to solidify your understanding and address any remaining questions.
