# Day 3, Hour 1: Advanced Routing in Express

Welcome to Day 3 of Level 4! Today, we will dive deeper into Express by exploring advanced routing techniques. This will include organizing routes using Express Router and handling more complex URL patterns.

## Objectives

- Practice setting up a new Express server.
- Learn how to use Express Router to organize routes.
- Understand how to handle route parameters.
- Create a basic nested route structure.

## Setting Up a New Express Server

Let's start by setting up a new Express server from scratch to reinforce the steps.

### Step 1: Initialize Your Project

Create a new directory for your project and navigate into it:

```bash
mkdir express-advanced-routing
cd express-advanced-routing
```

Initialize a new Node.js project:

```bash
npm init -y
```

Install Express:

```bash
npm install express
```

Create a sample data file and share with the students:

```json
// data.json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "role": "admin",
    "profile": {
      "age": 30,
      "location": "New York",
      "interests": ["coding", "hiking", "reading"]
    }
  },
  {
    "id": 2,
    "name": "Jane Smith",
    "email": "jane.smith@example.com",
    "role": "user",
    "profile": {
      "age": 25,
      "location": "San Francisco",
      "interests": ["traveling", "music", "sports"]
    }
  },
  {
    "id": 3,
    "name": "Sam Johnson",
    "email": "sam.johnson@example.com",
    "role": "moderator",
    "profile": {
      "age": 28,
      "location": "Chicago",
      "interests": ["gaming", "movies", "technology"]
    }
  },
  {
    "id": 4,
    "name": "Alice Brown",
    "email": "alice.brown@example.com",
    "role": "user",
    "profile": {
      "age": 22,
      "location": "Austin",
      "interests": ["art", "photography", "yoga"]
    }
  },
  {
    "id": 5,
    "name": "Michael Green",
    "email": "michael.green@example.com",
    "role": "user",
    "profile": {
      "age": 35,
      "location": "Seattle",
      "interests": ["cooking", "gardening", "cycling"]
    }
  }
]
```

### Step 2: Create the Server

Create a file named `index.js` and set up a basic Express server:

```js
// index.js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

Start the server using Nodemon:

```bash
npm install --save-dev nodemon
npm run dev
```

### Step 3: Using Express Router

Express Router helps you organize routes in a modular way.

1. Create a new directory named `routes`:

   ```bash
   mkdir routes
   ```

2. Inside the `routes` directory, create a file named `index.js`:

   ```js
   // routes/index.js
   const express = require('express');
   const router = express.Router();

   router.get('/', (req, res) => {
     res.send('Hello from the main router!');
   });

   router.get('/about', (req, res) => {
     res.send('About Us');
   });

   module.exports = router;
   ```

3. Update `index.js` in the root directory to use the new router:

   ```js
   // index.js
   const express = require('express');
   const app = express();
   const port = 3000;
   const routes = require('./routes/index');

   // Use the routes defined in the router
   app.use('/', routes);

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 4: Test the Refactored Routes

Start your server using Nodemon:

```bash
npm run dev
```

Test the routes in your browser or using Thunder Client/Postman:

- `http://localhost:3000/` - Should display "Hello from the main router!".
- `http://localhost:3000/about` - Should display "About Us".

## Handling Route Parameters

Route parameters are used to capture values specified in the URL.

### Step 5: Add Routes with Parameters

1. Modify `routes/index.js` to include a route with URL parameters:

   ```js
   // routes/index.js
   const express = require('express');
   const router = express.Router();

   router.get('/', (req, res) => {
     res.send('Hello from the main router!');
   });

   router.get('/about', (req, res) => {
     res.send('About Us');
   });

   router.get('/user/:id', (req, res) => {
     const userId = req.params.id;
     res.send(`User ID: ${userId}`);
   });

   module.exports = router;
   ```

### Step 6: Test the Route with URL Parameters

Navigate to `http://localhost:3000/user/123` in your browser or use Thunder Client/Postman to see the response "User ID: 123".

## Nested Routes

Express Router allows you to create nested routes for better organization.

### Step 7: Create Nested Routes

1. Inside the `routes` directory, create a file named `users.js`:

   ```js
   // routes/users.js
   const express = require('express');
   const router = express.Router();

   router.get('/', (req, res) => {
     res.send('List of all users');
   });

   router.get('/:id', (req, res) => {
     const userId = req.params.id;
     res.send(`User ID: ${userId}`);
   });

   module.exports = router;
   ```

2. Update `routes/index.js` to use the nested routes:

   ```js
   // routes/index.js
   const express = require('express');
   const router = express.Router();
   const usersRouter = require('./users');

   router.get('/', (req, res) => {
     res.send('Hello from the main router!');
   });

   router.get('/about', (req, res) => {
     res.send('About Us');
   });

   // Use the users router for routes starting with /users
   router.use('/users', usersRouter);

   module.exports = router;
   ```

### Step 8: Test the Nested Routes

Start your server using Nodemon:

```bash
npm run dev
```

Test the routes in your browser or using Thunder Client/Postman:

- `http://localhost:3000/users` - Should return "List of all users".
- `http://localhost:3000/users/123` - Should return "User ID: 123".

## Conclusion

In this session, we've practiced setting up a new Express server, used Express Router to organize routes, handled route parameters, and created nested routes. These techniques are essential for building scalable and maintainable Express applications.

In the next session, we will explore error handling in Express and learn how to create custom error handlers.

<!--! Hour 2 -->

In Hour 2, we will focus on error handling in Express. Error handling is crucial for building robust applications, as it helps you manage and respond to unexpected situations gracefully.

## Objectives

- Understand the basics of error handling in Express.
- Learn how to create custom error handlers.
- Practice handling errors in routes.

## Understanding Error Handling in Express

Express provides a default error handler, but you can define your own custom error handlers to handle specific types of errors and provide more meaningful responses to clients.

### Step 1: Basic Error Handling

Let's start by modifying our server to include a basic error handler.

1. Modify `index.js` to include an error handler:

   ```js
   // index.js
   const express = require('express');
   const app = express();
   const port = 3000;
   const routes = require('./routes/index');

   // Use the routes defined in the router
   app.use('/', routes);

   // Custom error handling middleware
   app.use((err, req, res, next) => {
     console.error(err.stack);
     res.status(500).json({ error: 'Something went wrong!' });
   });

   // Catch-all route for undefined routes
   app.use((req, res) => {
     res.status(404).json({ error: '404 Not Found' });
   });

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 2: Creating Custom Error Handlers

Custom error handlers allow you to manage specific types of errors more effectively. Let's create a custom error handler for 404 errors.

1. Modify `index.js` to include a custom 404 error handler:

   ```js
   // index.js
   const express = require('express');
   const app = express();
   const port = 3000;
   const routes = require('./routes/index');

   // Use the routes defined in the router
   app.use('/', routes);

   // Custom error handling middleware
   app.use((err, req, res, next) => {
     console.error(err.stack);
     res.status(500).json({ error: 'Something went wrong!' });
   });

   // Custom 404 error handler
   app.use((req, res, next) => {
     res.status(404).json({ error: '404 Not Found' });
   });

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 3: Triggering Errors in Routes

To see our error handlers in action, let's create a route that deliberately throws an error.

1. Modify `routes/index.js` to include a route that throws an error:

   ```js
   // routes/index.js
   const express = require('express');
   const router = express.Router();

   router.get('/', (req, res) => {
     res.send('Hello from the main router!');
   });

   router.get('/about', (req, res) => {
     res.send('About Us');
   });

   router.get('/user/:id', (req, res) => {
     const userId = req.params.id;
     res.send(`User ID: ${userId}`);
   });

   // Route that throws an error
   router.get('/error', (req, res, next) => {
     const err = new Error('This is a deliberate error.');
     err.status = 500;
     next(err);
   });

   module.exports = router;
   ```

### Step 4: Test Error Handling

Start your server using Nodemon:

```bash
npm run dev
```

Navigate to `http://localhost:3000/error` in your browser or use Thunder Client/Postman to see the error response `{ "error": "Something went wrong!" }`.

### Step 5: Custom Error Messages

You can customize error messages and statuses by modifying the error handler.

1. Modify `index.js` to customize error messages and statuses:

   ```js
   // index.js
   const express = require('express');
   const app = express();
   const port = 3000;
   const routes = require('./routes/index');

   // Use the routes defined in the router
   app.use('/', routes);

   // Custom error handling middleware
   app.use((err, req, res, next) => {
     console.error(err.stack);
     res.status(err.status || 500).json({
       error: err.message || 'Something went wrong!',
     });
   });

   // Custom 404 error handler
   app.use((req, res, next) => {
     res.status(404).json({ error: '404 Not Found' });
   });

   app.listen(port, () => {
     console.log(`Server is running at http://localhost:${port}`);
   });
   ```

### Step 6: Test Custom Error Messages

Navigate to `http://localhost:3000/error` again to see the customized error message `{ "error": "This is a deliberate error." }`.

## Conclusion

In this session, we've learned the basics of error handling in Express, created custom error handlers, and practiced handling errors in routes. Proper error handling is crucial for building robust and user-friendly applications.

In the next session, we will explore more advanced middleware and practice building a more complex API.
