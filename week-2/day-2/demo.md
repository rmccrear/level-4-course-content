# Day 2, Hour 1: Review and Advanced CRUD Operations with Mongoose

Welcome to Day 2 of Week 2! Today, we will review the CRUD operations we learned yesterday and introduce some more advanced scenarios. This session will help solidify your understanding of how to interact with MongoDB using Mongoose.

## Objectives

- Review basic CRUD operations using Mongoose.
- Perform more complex queries and updates.
- Learn to handle validation and errors more effectively.

## Review of Basic CRUD Operations

### Step 1: Set Up the Project

1. **Initialize the Project and Install Dependencies**:

   ```bash
   mkdir express-mongodb-advanced
   cd express-mongodb-advanced
   npm init -y
   npm install express mongoose
   ```

2. **Create the Server**:

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

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 2: Define the Schema and Model

Refer to the [Mongoose Schema documentation](https://mongoosejs.com/docs/guide.html#definition).

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
     age: {
       type: Number,
       required: true,
     },
     isActive: {
       type: Boolean,
       default: true,
     },
   });

   const User = mongoose.model('User', userSchema);

   module.exports = User;
   ```

### Step 3: Create and Read Users

Refer to the [Mongoose Models documentation](https://mongoosejs.com/docs/models.html).

1. **Update `index.js` to include routes for creating and reading users**:

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

   // Route to add a new user
   app.post('/users', async (req, res) => {
     try {
       const user = new User(req.body);
       await user.save();
       res.status(201).json(user);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Route to get all users
   app.get('/users', async (req, res) => {
     try {
       const users = await User.find();
       res.json(users);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   // Route to get active users
   app.get('/users/active', async (req, res) => {
     try {
       const activeUsers = await User.find({ isActive: true });
       res.json(activeUsers);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 4: Test Create and Read Operations

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Create a User**:

     - Method: POST
     - URL: `http://localhost:3000/users`
     - Body: JSON
       ```json
       {
         "name": "John Doe",
         "email": "john@example.com",
         "age": 25
       }
       ```

   - **Get All Users**:

     - Method: GET
     - URL: `http://localhost:3000/users`

   - **Get Active Users**:
     - Method: GET
     - URL: `http://localhost:3000/users/active`

### Step 5: Update and Delete Users

Refer to the [Mongoose Models documentation](https://mongoosejs.com/docs/models.html#updating) and [Queries documentation](https://mongoosejs.com/docs/queries.html).

1. **Update `index.js` to include update and delete routes**:

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

   // Route to add a new user
   app.post('/users', async (req, res) => {
     try {
       const user = new User(req.body);
       await user.save();
       res.status(201).json(user);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Route to get all users
   app.get('/users', async (req, res) => {
     try {
       const users = await User.find();
       res.json(users);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   // Route to get active users
   app.get('/users/active', async (req, res) => {
     try {
       const activeUsers = await User.find({ isActive: true });
       res.json(activeUsers);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   // Route to update a user
   app.put('/users/:id', async (req, res) => {
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

   // Route to deactivate a user
   app.put('/users/:id/deactivate', async (req, res) => {
     try {
       const user = await User.findByIdAndUpdate(
         req.params.id,
         { isActive: false },
         { new: true },
       );
       if (!user) {
         return res.status(404).json({ error: 'User not found' });
       }
       res.json(user);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Route to delete a user
   app.delete('/users/:id', async (req, res) => {
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

### Step 6: Test Update and Delete Operations

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Update a User**:

     - Method: PUT
     - URL: `http://localhost:3000/users/<user-id>`
     - Body: JSON
       ```json
       {
         "name": "Jane Doe",
         "email": "jane@example.com",
         "age": 28
       }
       ```

   - **Deactivate a User**:

     - Method: PUT
     - URL: `http://localhost:3000/users/<user-id>/deactivate`

   - **Delete a User**:
     - Method: DELETE
     - URL: `http://localhost:3000/users/<user-id>`

## Finding Information in the Mongoose Documentation

Mongoose has comprehensive documentation that is very useful when you're building applications. Here's how you can find information in the Mongoose documentation:

1. **Visit the Mongoose Documentation**:

   - Go to [Mongoose Docs](https://mongoosejs.com/docs/).

2. **Explore the Guides**:

   - The "Guides" section covers various topics, including schemas, models, queries, and validation.

3. **API Documentation**:

   - The "API" section provides detailed information about Mongoose methods and properties.

4. **Search the Documentation**:
   - Use the search bar at the top of the Mongoose documentation page to quickly find what you're looking for.

## Conclusion

In this session, we've reviewed the basic CRUD operations using Mongoose and performed more complex scenarios such as finding active users and deactivating users. We've also reinforced how to find relevant information in the Mongoose documentation. These skills are essential for building and managing MongoDB databases in your Express applications. In the next session, we will explore more advanced features of Mongoose.

<!--! Hour 2 -->

# Day 2, Hour 2: Advanced Queries and Error Handling with Mongoose

In Hour 2, we will build on the basics we reviewed earlier and explore more advanced queries and error handling techniques in Mongoose.

## Objectives

- Perform advanced queries with Mongoose.
- Implement effective error handling.
- Use validation to ensure data integrity.

## Advanced Queries

### Step 1: Set Up Advanced Query Routes

Let's add routes for performing advanced queries, such as finding users by age range and sorting results.

1. **Update `index.js` to include advanced query routes**:

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

   // Route to add a new user
   app.post('/users', async (req, res) => {
     try {
       const user = new User(req.body);
       await user.save();
       res.status(201).json(user);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Route to get all users
   app.get('/users', async (req, res) => {
     try {
       const users = await User.find();
       res.json(users);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

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

   // Route to update a user
   app.put('/users/:id', async (req, res) => {
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

   // Route to deactivate a user
   app.put('/users/:id/deactivate', async (req, res) => {
     try {
       const user = await User.findByIdAndUpdate(
         req.params.id,
         { isActive: false },
         { new: true },
       );
       if (!user) {
         return res.status(404).json({ error: 'User not found' });
       }
       res.json(user);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Route to delete a user
   app.delete('/users/:id', async (req, res) => {
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

### Step 2: Test Advanced Queries

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Get Users Within Age Range**:
     - Method: GET
     - URL: `http://localhost:3000/users/age-range/20/30`

## Error Handling

Proper error handling is crucial for building robust applications. Refer to the [Mongoose Error Handling documentation](https://mongoosejs.com/docs/middleware.html#error-handling-middleware).

### Step 3: Implement Enhanced Error Handling

1. **Update `index.js` to include enhanced error handling**:

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

   // Route to add a new user
   app.post('/users', async (req, res) => {
     try {
       const user = new User(req.body);
       await user.save();
       res.status(201).json(user);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Route to get all users
   app.get('/users', async (req, res) => {
     try {
       const users = await User.find();
       res.json(users);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

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

   // Route to update a user
   app.put('/users/:id', async (req, res) => {
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

   // Route to deactivate a user
   app.put('/users/:id/deactivate', async (req, res) => {
     try {
       const user = await User.findByIdAndUpdate(
         req.params.id,
         { isActive: false },
         { new: true },
       );
       if (!user) {
         return res.status(404).json({ error: 'User not found' });
       }
       res.json(user);
     } catch (error) {
       res.status(400).json({ error: error.message });
     }
   });

   // Route to delete a user
   app.delete('/users/:id', async (req, res) => {
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

   // Enhanced error-handling middleware
   app.use((err, req, res, next) => {
     console.error(err.stack);
     res.status(err.status || 500).json({
       error: err.message || 'Internal Server Error',
     });
   });

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 4: Test Enhanced Error Handling

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Trigger an Error**:
     - Method: GET
     - URL: `http://localhost:3000/users/invalid-id`
     - This should return a 500 status with the error message.

## Validation

Mongoose provides built-in validators and the ability to create custom validators. Refer to the [Mongoose Validation documentation](https://mongoosejs.com/docs/validation.html).

### Step 5: Implement Validation

1. **Update `models/User.js` to include validation**:

   ```js
   // models/User.js
   const mongoose = require('mongoose');

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

   const User = mongoose.model('User', userSchema);

   module.exports = User;
   ```

### Step 6: Test Validation

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Create a User with Invalid Data**:
     - Method: POST
     - URL: `http://localhost:3000/users`
     - Body: JSON
       ```json
       {
         "name": "",
         "email": "invalid-email",
         "age": 17
       }
       ```
     - This should return a 400 status with validation error messages.

## Conclusion

In this session, we've explored advanced queries, enhanced error handling, and validation with Mongoose. These techniques are essential for building robust and reliable applications. In the next session, we will continue to deepen our understanding of Mongoose with more advanced features.
