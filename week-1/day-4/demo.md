# Day 4, Hour 1: Understanding and Using Middleware in Express

Welcome to Day 4 of Level 4! Today, we will focus on middleware in Express. Middleware functions are a fundamental part of Express applications, enabling you to handle requests, modify responses, and manage the flow of your application.

## Objectives

- Understand what middleware is and how it works in Express.
- Learn how to create and use custom middleware.
- Explore common third-party middleware.

## What is Middleware?

Middleware functions are functions that have access to the request object (`req`), the response object (`res`), and the next middleware function in the application’s request-response cycle. These functions can execute any code, make changes to the request and response objects, end the request-response cycle, and call the next middleware function.

### Step 1: Setting Up the Server

As always, let's start by setting up a new Express server to practice.

#### Initialize Your Project

Create a new directory for your project and navigate into it:

```bash
mkdir express-middleware
cd express-middleware
```

Initialize a new Node.js project:

```bash
npm init -y
```

Install Express:

```bash
npm install express
```

#### Create the Server

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

### Step 2: Creating Custom Middleware

Let's create a simple custom middleware function that logs the details of each incoming request.

#### Add Custom Middleware

Modify `index.js` to include a custom middleware function:

```js
// index.js
const express = require('express');
const app = express();
const port = 3000;

// Custom middleware function to log request details
const requestLogger = (req, res, next) => {
  console.log(
    `${req.method} request for '${req.url}' - ${new Date().toISOString()}`,
  );
  next(); // Call next() to pass control to the next middleware function
};

// Use the custom middleware
app.use(requestLogger);

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.send('About Us');
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

### Step 3: Testing Custom Middleware

Start your server using Nodemon:

```bash
npm run dev
```

Make a request to any of the routes (e.g., `http://localhost:3000/`). You should see the request details logged in the console.

### Step 4: Using Third-Party Middleware

Express provides a variety of third-party middleware to handle common tasks. One common middleware is `body-parser`, which helps parse incoming request bodies.

#### Install and Use `body-parser`

Install `body-parser`:

```bash
npm install body-parser
```

Modify `index.js` to use `body-parser`:

```js
// index.js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const port = 3000;

// Custom middleware function to log request details
const requestLogger = (req, res, next) => {
  console.log(
    `${req.method} request for '${req.url}' - ${new Date().toISOString()}`,
  );
  next(); // Call next() to pass control to the next middleware function
};

// Use the custom middleware
app.use(requestLogger);

// Use body-parser middleware to parse JSON bodies
app.use(bodyParser.json());

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.send('About Us');
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

### Step 5: Testing `body-parser`

Use Thunder Client or Postman to send a POST request to `http://localhost:3000/contact` with a JSON body like `{ "name": "John Doe", "email": "john@example.com" }`. You should see the parsed JSON data in the response.

### Step 6: Understanding the Middleware Stack

Express executes middleware functions in the order they are defined. This sequence of middleware function executions is called the middleware stack. Understanding this stack is crucial for managing the flow of your application.

#### Example of Middleware Stack

Modify `index.js` to include multiple middleware functions:

```js
// index.js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const port = 3000;

// Middleware function to log request details
const requestLogger = (req, res, next) => {
  console.log(
    `${req.method} request for '${req.url}' - ${new Date().toISOString()}`,
  );
  next(); // Call next() to pass control to the next middleware function
};

// Middleware function to log request headers
const logHeaders = (req, res, next) => {
  console.log('Request Headers:', req.headers);
  next();
};

// Use the custom middleware
app.use(requestLogger);
app.use(logHeaders);

// Use body-parser middleware to parse JSON bodies
app.use(bodyParser.json());

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.send('About Us');
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

### Step 7: Testing the Middleware Stack

Start your server using Nodemon:

```bash
npm run dev
```

Make a request to any of the routes (e.g., `http://localhost:3000/`). You should see the request details and headers logged in the console.

## Conclusion

In this session, we've learned what middleware is and how it works in Express. We created custom middleware functions, used third-party middleware (`body-parser`), and explored the middleware stack. Understanding middleware is crucial for building scalable and maintainable Express applications.

In the next session, we will continue building our application with more advanced middleware and complex routes.

<!--! Hour 2 -->

In Hour 2, we will continue our exploration of middleware in Express. We'll look into more advanced uses of middleware, including error-handling middleware, conditional middleware, and third-party middleware for tasks like authentication and logging.

## Objectives-

- Understand how to create error-handling middleware.
- Learn how to use conditional middleware.
- Explore third-party middleware for authentication and logging.

## Error-Handling Middleware

Error-handling middleware is defined similarly to other middleware functions, but it has four arguments instead of three: `err`, `req`, `res`, and `next`. This type of middleware is used to catch and handle errors throughout your application.

### Step 1: Creating an Error-Handling Middleware

Modify `index.js` to include an error-handling middleware:

```js
// index.js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const port = 3000;

// Middleware function to log request details
const requestLogger = (req, res, next) => {
  console.log(
    `${req.method} request for '${req.url}' - ${new Date().toISOString()}`,
  );
  next(); // Call next() to pass control to the next middleware function
};

// Middleware function to log request headers
const logHeaders = (req, res, next) => {
  console.log('Request Headers:', req.headers);
  next();
};

// Middleware to simulate an error
const simulateError = (req, res, next) => {
  const error = new Error('Something went wrong!');
  error.status = 500;
  next(error);
};

// Use the custom middleware
app.use(requestLogger);
app.use(logHeaders);

// Use body-parser middleware to parse JSON bodies
app.use(bodyParser.json());

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.send('About Us');
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
});

// Route to trigger error
app.get('/error', simulateError);

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

### Step 2: Testing Error-Handling Middleware

Start your server using Nodemon:

```bash
npm run dev
```

Navigate to `http://localhost:3000/error` in your browser or use Thunder Client/Postman to see the error response `{ "error": "Something went wrong!" }`.

## Conditional Middleware

Conditional middleware runs only if certain conditions are met. This can be useful for applying specific middleware to certain routes or under certain conditions.

### Step 3: Creating Conditional Middleware

Modify `index.js` to include conditional middleware:

```js
// index.js
const express = require('express');
const bodyParser = require('body-parser');
const app = express();
const port = 3000;

// Middleware function to log request details
const requestLogger = (req, res, next) => {
  console.log(
    `${req.method} request for '${req.url}' - ${new Date().toISOString()}`,
  );
  next(); // Call next() to pass control to the next middleware function
};

// Middleware function to log request headers
const logHeaders = (req, res, next) => {
  console.log('Request Headers:', req.headers);
  next();
};

// Conditional middleware to simulate an error only on /error route
const conditionalError = (req, res, next) => {
  if (req.path === '/error') {
    const error = new Error('This is a conditional error!');
    error.status = 500;
    next(error);
  } else {
    next();
  }
};

// Use the custom middleware
app.use(requestLogger);
app.use(logHeaders);

// Use body-parser middleware to parse JSON bodies
app.use(bodyParser.json());

// Use conditional middleware
app.use(conditionalError);

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.send('About Us');
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
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

### Step 4: Testing Conditional Middleware

Start your server using Nodemon:

```bash
npm run dev
```

Navigate to `http://localhost:3000/error` in your browser or use Thunder Client/Postman to see the conditional error response `{ "error": "This is a conditional error!" }`.

## Using Third-Party Middleware

Third-party middleware can handle various tasks like authentication, logging, security, and more. Let's explore two commonly used middleware: `morgan` for logging and `helmet` for security.

### Step 5: Install and Use `morgan` for Logging

Install `morgan`:

```bash
npm install morgan
```

Modify `index.js` to use `morgan`:

```js
// index.js
const express = require('express');
const bodyParser = require('body-parser');
const morgan = require('morgan');
const app = express();
const port = 3000;

// Middleware to log requests using morgan
app.use(morgan('dev'));

// Middleware to log request headers
const logHeaders = (req, res, next) => {
  console.log('Request Headers:', req.headers);
  next();
};

// Use body-parser middleware to parse JSON bodies
app.use(bodyParser.json());

// Use logHeaders middleware
app.use(logHeaders);

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.send('About Us');
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
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

### Step 6: Install and Use `helmet` for Security

Install `helmet`:

```bash
npm install helmet
```

Modify `index.js` to use `helmet`:

```js
// index.js
const express = require('express');
const bodyParser = require('body-parser');
const morgan = require('morgan');
const helmet = require('helmet');
const app = express();
const port = 3000;

// Middleware to log requests using morgan
app.use(morgan('dev'));

// Middleware to secure Express apps by setting various HTTP headers using helmet
app.use(helmet());

// Middleware to log request headers
const logHeaders = (req, res, next) => {
  console.log('Request Headers:', req.headers);
  next();
};

// Use body-parser middleware to parse JSON bodies
app.use(bodyParser.json());

// Use logHeaders middleware
app.use(logHeaders);

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.send('About Us');
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
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

### Step 7: Testing Third-Party Middleware

Start your server using Nodemon:

```bash
npm run dev
```

Make requests to any of the routes (e.g., `http://localhost:3000/`) and observe the logs generated by `morgan` and the security headers added by `helmet`.

## Conclusion-

In this session, we've explored advanced middleware concepts in Express. We created error-handling middleware, used conditional middleware, and explored third-party middleware for logging and security. Understanding and effectively using middleware is crucial for building robust and maintainable Express applications.

In the next session, we will continue building our application with more complex routes and integrating a database.
