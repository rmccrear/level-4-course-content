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

- Follow the steps in yesterday's setup to build a new Express app.

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
// Normally when we use an API we get back JSON
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

## Practical Example with Custom Parameters

Let's include a more practical example where custom parameters are useful. We will set up a route to search for users by their ID and return the user's details in JSON format.

First, let's create a sample dataset:

```json
const users = [
  { id: 1, name: 'John Doe', email: 'john@example.com' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com' },
  { id: 3, name: 'Bob Johnson', email: 'bob@example.com' }
];
```

Now, modify the `/user/:id` route to search for the user by ID:

```js
// index.js
const express = require('express');
const app = express();
const port = 3000;

const users = [
  { id: 1, name: 'John Doe', email: 'john@example.com' },
  { id: 2, name: 'Jane Smith', email: 'jane@example.com' },
  { id: 3, name: 'Bob Johnson', email: 'bob@example.com' }
];

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
  const userId = parseInt(req.params.id, 10);
  const user = users.find(u => u.id === userId);

  if (user) {
    res.json(user);
  } else {
    res.status(404).json({ error: 'User not found' });
  }
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

### Step 7: Test the Custom Parameter Route

Navigate to `http://localhost:3000/user/1` in your browser or use Thunder Client/Postman to see the user details for the user with ID 1. For example, the response should be:

```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com"
}
```

## Conclusion

In this session, we've learned how to handle different HTTP methods and request parameters in Express. We also practiced sending and receiving JSON data and tested our API using Thunder Client or Postman. These skills are crucial for building interactive web applications.

In the next session, we will dive deeper into working with middleware and creating more complex routes.
