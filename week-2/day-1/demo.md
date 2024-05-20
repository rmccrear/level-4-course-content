# Day 1, Hour 1: Introduction to MongoDB and Setting Up MongoDB Atlas

Welcome to Week 2 of Level 4! This week, we will focus on integrating MongoDB into our Express applications. Today, we will start by learning about MongoDB, setting up a MongoDB Atlas account, and connecting to our database from an Express application.

## Objectives

- Understand what MongoDB is and why it's used.
- Set up a MongoDB Atlas account.
- Connect to MongoDB from an Express application using Mongoose.

## What is MongoDB?

MongoDB is a NoSQL database that stores data in flexible, JSON-like documents. It is designed to handle large volumes of data and can scale easily with your application. MongoDB is known for its flexibility, scalability, and ease of use.

### Key Features of MongoDB

- **Document-oriented**: Stores data in JSON-like documents.
- **Schema-less**: Allows for dynamic schemas, making it easy to store varied data.
- **Scalable**: Can handle large amounts of data and high traffic.

## Setting Up MongoDB Atlas

MongoDB Atlas is a cloud-based service that makes it easy to set up, operate, and scale MongoDB deployments.

### Step 1: Create a MongoDB Atlas Account

1. **Sign Up**: Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) and sign up for a free account.
2. **Create a Cluster**:
   - After signing up, click on "Build a Cluster".
   - Select the free tier and choose your preferred cloud provider and region.
   - Click "Create Cluster".

### Step 2: Create a Database User

1. **Add a Database User**:
   - Go to the "Database Access" section.
   - Click on "Add New Database User".
   - Create a username and password. Make sure to keep these credentials secure as you'll need them to connect to your database.

### Step 3: Whitelist Your IP Address

1. **Network Access**:
   - Go to the "Network Access" section.
   - Click on "Add IP Address".
   - Whitelist your current IP address or allow access from anywhere (not recommended for production).

### Step 4: Connect to Your Cluster

1. **Connect**:
   - Go to the "Clusters" section.
   - Click on "Connect" for your cluster.
   - Select "Connect Your Application".
   - Choose the "Driver" as "Node.js" and version "3.6 or later".
   - Copy the connection string. It should look something like this:
     ```
     mongodb+srv://<username>:<password>@cluster0.mongodb.net/test?retryWrites=true&w=majority
     ```

## Connecting to MongoDB from an Express Application

We will use Mongoose, an ODM (Object Data Modeling) library for MongoDB and Node.js, to connect to our database and interact with it.

### Step 5: Set Up Your Express Application

1. **Initialize Your Project**:

   ```bash
   mkdir express-mongodb
   cd express-mongodb
   npm init -y
   npm install express mongoose
   ```

2. **Create the Server**:

   Create a file named `index.js` and set up a basic Express server:

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

   app.get('/', (req, res) => {
     res.send('Hello, World!');
   });

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 6: Test the Connection

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Check the Output**:
   - If the connection is successful, you should see "Connected to MongoDB" in the console.
   - Navigate to `http://localhost:3000/` in your browser to see "Hello, World!".

## Conclusion

In this session, we've learned about MongoDB, set up a MongoDB Atlas account, and connected to MongoDB from an Express application using Mongoose. This is the first step in integrating MongoDB into your applications. In the next session, we will dive deeper into defining schemas and models in Mongoose and performing CRUD operations.

<!--! Hour 2 -->

# Day 1, Hour 2: Basic CRUD Operations with Mongoose

Welcome to Day 1, Hour 2 of Week 2! This session will focus on performing basic CRUD (Create, Read, Update, Delete) operations using Mongoose. We'll keep it simple and learn how to interact with our MongoDB database by creating, reading, updating, and deleting documents.

## Objectives

- Perform basic CRUD operations using Mongoose.
- Learn how to find relevant information in the Mongoose documentation.

## Basic CRUD Operations

### Step 1: Create a Simple Schema and Model

To define a schema in Mongoose, we refer to the [Mongoose Schema documentation](https://mongoosejs.com/docs/guide.html#definition).

1. **Create `models/User.js` to define a simple schema**:

   ```js
   // models/User.js
   const mongoose = require('mongoose');

   // Define the schema as per the Mongoose documentation
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
   });

   const User = mongoose.model('User', userSchema);

   module.exports = User;
   ```

### Step 2: Create and Read Users

To perform basic CRUD operations, we refer to the [Mongoose Models documentation](https://mongoosejs.com/docs/models.html).

1. **Update `index.js` to include routes for creating and reading users**:

   ```js
   // index.js
   const express = require('express');
   const mongoose = require('mongoose');
   const User = require('./models/User');
   const app = express();
   const port = 3000;

   // Middleware to parse JSON bodies, refer to Express documentation for this
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
   // Refer to Mongoose create operations in Models documentation
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
   // Refer to Mongoose find operations in Models documentation
   app.get('/users', async (req, res) => {
     try {
       const users = await User.find();
       res.json(users);
     } catch (error) {
       res.status(500).json({ error: error.message });
     }
   });

   app.get('/', (req, res) => {
     res.send('Hello, World!');
   });

   // Error-handling middleware
   // Refer to Express error-handling documentation
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

### Step 3: Test Create and Read Operations

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

### Step 4: Update and Delete Users

To update and delete documents, refer to the [Mongoose Models documentation](https://mongoosejs.com/docs/models.html#updating) and [Queries documentation](https://mongoosejs.com/docs/queries.html).

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

   // Route to update a user
   // Refer to Mongoose update operations in Models documentation
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

   // Route to delete a user
   // Refer to Mongoose delete operations in Models documentation
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

   app.get('/', (req, res) => {
     res.send('Hello, World!');
   });

   // Error-handling middleware
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

### Step 5: Test Update and Delete Operations

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

In this session, we've performed basic CRUD operations using Mongoose and learned how to find information in the Mongoose documentation. These skills are essential for building and managing MongoDB databases in your Express applications. In the next session, we will explore more advanced features of Mongoose.
