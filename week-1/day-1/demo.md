# Day 1: Introduction to Express

Welcome to Level 2! Today, we will start with an introduction to Express, a minimal and flexible Node.js web application framework.

## Objectives

- Understand what a server is.
- Learn what Express is and why it's useful.
- Set up a basic Express server.

## Instructor Notes

### What is a Server?

- A server is a computer program or device that provides functionality for other programs or devices (clients).
- Servers handle requests from clients (like web browsers) and send back responses (like HTML pages, data, etc.).

### What is Express?

- **Minimal**: Allows flexibility with fewer built-in decisions.
- **Flexible**: Supports middleware to handle different request-response cycle parts.
- **Robust**: Offers powerful URL routing and middleware systems.

### Setting Up Your First Express Server

1. **Initialize Your Project**:

   - Create a new directory and initialize a Node.js project:

     ```bash
     mkdir express-demo
     cd express-demo
     npm init -y
     ```

2. **Install Express**:

   - Install Express using npm:

     ```bash
     npm install express

     ```

3. **Create the Server**:

   - Create `index.js` and set up a basic server:

     ```js
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

4. **Run Your Server**:

   - Run the server and navigate to `http://localhost:3000` to see "Hello, World!".

     ```bash

     node index.js
     ```

## Code Snippets

### Basic Express Server Setup

```js
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

# Hour 2: Using Nodemon and Handling Routes

## Objectives

- Set up Nodemon to automatically restart the server on changes.
- Handle multiple routes in Express.
- Serve JSON responses.
- Test the API using Thunder Client or Postman.

## Instructor Notes

### Setting Up Nodemon

1. **Install Nodemon**:

   - Install as a development dependency:

     ```bash
     npm install --save-dev nodemon
     ```

2. **Update `package.json`**:

   - Modify scripts to use Nodemon:

     ```json
     "scripts": {
       "start": "node index.js",
       "dev": "nodemon index.js"
     }
     ```

3. **Run Server with Nodemon**:

   - Use Nodemon to start the server:

     ```bash
     npm run dev
     ```

### Handling Routes in Express

1. **Add Additional Routes**:

   - Modify `index.js` to include more routes:

     ```js
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

     app.use((req, res) => {
       res.status(404).json({ error: '404 Not Found' });
     });

     app.listen(port, () => {
       console.log(`Server is running at http://localhost:${port}`);
     });
     ```

### Testing the Routes

- **Using Thunder Client or Postman**:
  - Test routes: `http://localhost:3000/`, `http://localhost:3000/about`, `http://localhost:3000/contact`.

### Nodemon Setup

```json
"scripts": {
  "start": "node index.js",
  "dev": "nodemon index.js"
}
```
