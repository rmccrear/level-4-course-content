# Day 1: Introduction to Express

Welcome to Week 1 of Level 4! Today, we will start with an introduction to Express, a minimal and flexible Node.js web application framework. Express provides a robust set of features to develop web and mobile applications.

## Objectives

- Understand what a server is.
- Learn what Express is and why it's useful.
- Set up a basic Express server.

## What is a Server?

A server is a computer program or device that provides functionality for other programs or devices, called "clients." Servers can provide various functionalities, often called "services," such as hosting websites, storing data, and handling requests from clients.

In web development, a server listens for incoming requests from clients (like web browsers), processes those requests, and sends back responses (like HTML pages, data, etc.).

## What is Express?

Express is a lightweight framework for building web applications in Node.js. It simplifies the process of creating a server and provides a set of tools to handle various tasks like routing, middleware, and more.

- **Minimal**: As a framework, it doesn't impose many decisions and allows for flexibility.
- **Flexible**: Supports various middleware to handle different parts of the request-response cycle.
- **Robust**: Provides powerful URL routing and a robust middleware system.

## Setting Up Your First Express Server

Let's get started by setting up a basic Express server.

### Step 1: Initialize Your Project

First, create a new directory for your project and navigate into it:

```bash
mkdir express-demo
cd express-demo
```

Initialize a new Node.js project:

```bash
npm init -y
```

This will create a `package.json` file with default settings.

### Step 2: Install Express

Install Express using npm:

```bash
npm install express
```

### Step 3: Create the Server

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

### Step 4: Run Your Server

Run the server using the following command:

```bash
node index.js
```

Open your browser and navigate to `http://localhost:3000`. You should see "Hello, World!" displayed on the page.

## Understanding the Code

Let's break down the code in `index.js`:

- **Importing Express**:

  ```js
  const express = require('express');
  ```

  This line imports the Express module.

- **Creating an Express Application**:

  ```js
  const app = express();
  ```

  This line creates an instance of an Express application.

- **Defining a Route**:

  ```js
  app.get('/', (req, res) => {
    res.send('Hello, World!');
  });
  ```

  This line defines a route that listens for GET requests at the root URL ('/') and sends back a response "Hello, World!".

- **Starting the Server**:

  ```js
  app.listen(port, () => {
    console.log(`Server is running at http://localhost:${port}`);
  });
  ```

  This line starts the server on port 3000 and logs a message to the console when the server is running.

## Conclusion

Today, we've set up a basic Express server and learned about what a server is. This foundational knowledge will be essential as we build more complex applications.

In the next session, we'll dive deeper into handling different routes and using more features of Express.

<!--! Hour 2 -->

In Hour 2, we will learn about Nodemon, a utility that helps with automatic server restarts during development. We will also extend our Express application by handling different routes and serving JSON responses. Additionally, we'll use Thunder Client or Postman to test our API.

## Objectives

- Set up Nodemon to automatically restart the server on changes.
- Understand how to handle multiple routes in Express.
- Serve JSON responses.
- Test the API using Thunder Client or Postman.

## Setting Up Nodemon

Nodemon is a utility that monitors for any changes in your source and automatically restarts your server. It's a helpful tool during development to avoid manually restarting the server every time you make a change.

### Step 1: Install Nodemon

Install Nodemon as a development dependency:

```bash
npm install --save-dev nodemon
```

### Step 2: Update package.json

Update the `scripts` section of your `package.json` to use Nodemon for running the server:

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js"
}
```

With this setup, you can run the server in development mode using Nodemon by running:

```bash
npm run dev
```

### Step 3: Run the Server with Nodemon

Start the server with Nodemon:

```bash
npm run dev
```

Make a change to your `index.js` file and save it. Nodemon should automatically restart the server, and you should see the updated output in the terminal.

## Handling Routes in Express

Express makes it easy to handle different routes in your application. Let's add a few more routes to our server.

### Step 4: Add Routes and Serve JSON

Modify `index.js` to include additional routes that serve JSON responses:

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

### Step 5: Test the Routes with Thunder Client or Postman

#### Using Thunder Client:

1. Open your VS Code and install the Thunder Client extension if you haven't already.
2. Open Thunder Client from the sidebar.
3. Create a new request and set the method to GET.
4. Test the following URLs:
   - `http://localhost:3000/` - Should display "Hello, World!".
   - `http://localhost:3000/about` - Should return `{ "message": "About Us" }`.
   - `http://localhost:3000/contact` - Should return `{ "message": "Contact Us" }`.

#### Using Postman:

1. Open Postman.
2. Create a new request and set the method to GET.
3. Test the following URLs:
   - `http://localhost:3000/` - Should display "Hello, World!".
   - `http://localhost:3000/about` - Should return `{ "message": "About Us" }`.
   - `http://localhost:3000/contact` - Should return `{ "message": "Contact Us" }`.

### Step 6: Adding a 404 JSON Response

Modify the catch-all route to return a JSON response:

```js
// index.js
const express = require('express');
const app = express();
const port = 3000;

app.use(express.json()); // this may be needed for the assignment
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

### Step 7: Test the 404 JSON Response

Test an undefined route, such as `http://localhost:3000/undefined-route`, and you should see `{ "error": "404 Not Found" }`.

## Conclusion

In this session, we've learned how to use Nodemon to automatically restart our server on changes, handle multiple routes in Express, and serve JSON responses. We also tested our API using Thunder Client or Postman. These skills are essential for building robust web applications.

In the next session, we will dive deeper into handling different HTTP methods and working with request parameters.
