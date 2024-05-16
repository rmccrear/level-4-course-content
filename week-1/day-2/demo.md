# Day 2, Hour 1: Handling HTTP Methods and Request Parameters in Express

Welcome to Day 2 of Level 4! Today, we will dive deeper into Express by exploring different HTTP methods and handling request parameters. This will help us create more interactive and functional web applications.

## Objectives

- Understand different HTTP methods and their uses.
- Learn how to handle URL parameters in Express.
- Learn how to handle query parameters in Express.
- Practice sending and receiving JSON data.

## HTTP Methods

In web development, HTTP methods define the type of action to be performed. The most common HTTP methods are:

- **GET**: Retrieve data from the server.
- **POST**: Send data to the server.
- **PUT**: Update existing data on the server.
- **DELETE**: Remove data from the server.

Let's update our Express server to handle these different methods.

### Step 1: Set Up Routes for Different HTTP Methods

- Follow the steps in yesterdays setup to build a new Express app.

```js
// index.js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.json({ message: 'About Us' });
});

app.get('/contact', (req, res) => {
  res.json({ message: 'Contact Us' });
});

// Catch-all route for undefined routes
app.use((req, res) => {
  res.status(404).json({ error: '404 Not Found' });
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

Modify `index.js` to include routes for handling GET, POST, PUT, and DELETE requests:

```js
// index.js
const express = require('express');
const app = express();
const port = 3000;

// Middleware to parse JSON bodies
app.use(express.json());

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.json({ message: 'About Us' });
});

app.get('/contact', (req, res) => {
  res.json({ message: 'Contact Us' });
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
});

app.put('/contact', (req, res) => {
  res.json({ message: 'Contact information updated', data: req.body });
});

app.delete('/contact', (req, res) => {
  res.json({ message: 'Contact information deleted' });
});

// Catch-all route for undefined routes
app.use((req, res) => {
  res.status(404).json({ error: '404 Not Found' });
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

### Step 2: Test HTTP Methods with Thunder Client or Postman

#### Using Thunder Client:

1. Open Thunder Client in VS Code.
2. Create a new request and set the method to POST, PUT, or DELETE.
3. Test the following URLs:
   - `POST http://localhost:3000/contact` with a JSON body like `{ "name": "John Doe", "email": "john@example.com" }`.
   - `PUT http://localhost:3000/contact` with a JSON body like `{ "name": "Jane Doe", "email": "jane@example.com" }`.
   - `DELETE http://localhost:3000/contact`.

#### Using Postman:

1. Open Postman.
2. Create a new request and set the method to POST, PUT, or DELETE.
3. Test the following URLs:
   - `POST http://localhost:3000/contact` with a JSON body like `{ "name": "John Doe", "email": "john@example.com" }`.
   - `PUT http://localhost:3000/contact` with a JSON body like `{ "name": "Jane Doe", "email": "jane@example.com" }`.
   - `DELETE http://localhost:3000/contact`.

## Handling URL Parameters

URL parameters are used to capture values specified in the URL. Express makes it easy to capture these values and use them in your route handlers.

### Step 3: Add a Route with URL Parameters

Modify `index.js` to include a route with URL parameters:

```js
// index.js
const express = require('express');
const app = express();
const port = 3000;

// Middleware to parse JSON bodies
app.use(express.json());

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.json({ message: 'About Us' });
});

app.get('/contact', (req, res) => {
  res.json({ message: 'Contact Us' });
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
});

app.put('/contact', (req, res) => {
  res.json({ message: 'Contact information updated', data: req.body });
});

app.delete('/contact', (req, res) => {
  res.json({ message: 'Contact information deleted' });
});

app.get('/user/:id', (req, res) => {
  const userId = req.params.id;
  res.json({ message: `User ID: ${userId}` });
});

// Catch-all route for undefined routes
app.use((req, res) => {
  res.status(404).json({ error: '404 Not Found' });
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

### Step 4: Test the Route with URL Parameters

Navigate to `http://localhost:3000/user/123` in your browser or use Thunder Client/Postman to see the response `{ "message": "User ID: 123" }`.

## Handling Query Parameters

Query parameters are used to pass data to the server as part of the URL. They are typically used in GET requests.

### Step 5: Add a Route with Query Parameters

Modify `index.js` to include a route that handles query parameters:

```js
// index.js
const express = require('express');
const app = express();
const port = 3000;

// Middleware to parse JSON bodies
app.use(express.json());

app.get('/', (req, res) => {
  res.send('Hello, World!');
});

app.get('/about', (req, res) => {
  res.json({ message: 'About Us' });
});

app.get('/contact', (req, res) => {
  res.json({ message: 'Contact Us' });
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
});

app.put('/contact', (req, res) => {
  res.json({ message: 'Contact information updated', data: req.body });
});

app.delete('/contact', (req, res) => {
  res.json({ message: 'Contact information deleted' });
});

app.get('/user/:id', (req, res) => {
  const userId = req.params.id;
  res.json({ message: `User ID: ${userId}` });
});

app.get('/search', (req, res) => {
  const { term, sort } = req.query;
  res.json({ message: `Search Term: ${term}, Sort By: ${sort}` });
});

// Catch-all route for undefined routes
app.use((req, res) => {
  res.status(404).json({ error: '404 Not Found' });
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

### Step 6: Test the Route with Query Parameters

Navigate to `http://localhost:3000/search?term=javascript&sort=asc` in your browser or use Thunder Client/Postman to see the response `{ "message": "Search Term: javascript, Sort By: asc" }`.

## Conclusion

In this session, we've learned how to handle different HTTP methods and request parameters in Express. We also practiced sending and receiving JSON data and tested our API using Thunder Client or Postman. These skills are crucial for building interactive web applications.

In the next session, we will dive deeper into working with middleware and creating more complex routes.

<!--! Hour 2 -->

In Hour 2, we will explore middleware in Express. Middleware functions are functions that have access to the request object (`req`), the response object (`res`), and the next middleware function in the application’s request-response cycle. These functions can execute any code, make changes to the request and response objects, end the request-response cycle, and call the next middleware function.

## Objectives

- Understand what middleware is and how it works in Express.
- Learn how to create and use custom middleware.
- Use third-party middleware to enhance your application.

## What is Middleware?

Middleware functions are a core part of Express applications. They are functions that execute during the lifecycle of a request to the server. Each middleware function can perform specific tasks, such as logging requests, handling authentication, parsing request bodies, and more.

### Step 1: Creating Custom Middleware

Let's start by creating a simple custom middleware function that logs the details of each incoming request.

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
  res.json({ message: 'About Us' });
});

app.get('/contact', (req, res) => {
  res.json({ message: 'Contact Us' });
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
});

app.put('/contact', (req, res) => {
  res.json({ message: 'Contact information updated', data: req.body });
});

app.delete('/contact', (req, res) => {
  res.json({ message: 'Contact information deleted' });
});

app.get('/user/:id', (req, res) => {
  const userId = req.params.id;
  res.json({ message: `User ID: ${userId}` });
});

app.get('/search', (req, res) => {
  const { term, sort } = req.query;
  res.json({ message: `Search Term: ${term}, Sort By: ${sort}` });
});

// Catch-all route for undefined routes
app.use((req, res) => {
  res.status(404).json({ error: '404 Not Found' });
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

### Step 2: Test the Custom Middleware

Start your server using Nodemon:

```bash
npm run dev
```

Make a request to any of the routes (e.g., `http://localhost:3000/`). You should see the request details logged in the console.

## Using Third-Party Middleware

Express provides a variety of third-party middleware to handle common tasks. One common middleware is `body-parser`, which helps parse incoming request bodies.

### Step 3: Install and Use `body-parser`

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
  res.json({ message: 'About Us' });
});

app.get('/contact', (req, res) => {
  res.json({ message: 'Contact Us' });
});

app.post('/contact', (req, res) => {
  res.json({ message: 'Contact form submitted', data: req.body });
});

app.put('/contact', (req, res) => {
  res.json({ message: 'Contact information updated', data: req.body });
});

app.delete('/contact', (req, res) => {
  res.json({ message: 'Contact information deleted' });
});

app.get('/user/:id', (req, res) => {
  const userId = req.params.id;
  res.json({ message: `User ID: ${userId}` });
});

app.get('/search', (req, res) => {
  const { term, sort } = req.query;
  res.json({ message: `Search Term: ${term}, Sort By: ${sort}` });
});

// Catch-all route for undefined routes
app.use((req, res) => {
  res.status(404).json({ error: '404 Not Found' });
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});
```

### Step 4: Test `body-parser`

Use Thunder Client or Postman to send a POST request to `http://localhost:3000/contact` with a JSON body like `{ "name": "John Doe", "email": "john@example.com" }`. You should see the parsed JSON data in the response.

## Conclusion

In this session, we've learned what middleware is and how it works in Express. We created custom middleware to log request details and used third-party middleware (`body-parser`) to parse incoming JSON request bodies. Understanding middleware is crucial for building scalable and maintainable Express applications.
