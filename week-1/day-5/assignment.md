# Day 5: Review and Introduction to EJS

Welcome to Day 5 of Level 4! Today, we will review everything we've covered this week, and then we'll introduce EJS (Embedded JavaScript) as a templating engine for creating dynamic HTML pages.

## Objectives

- Review Express basics, routing, and middleware.
- Learn how to set up and use EJS in an Express application.

## Review of the Week

Let's start by reviewing the key concepts we've covered this week:

### Setting Up an Express Server

1. **Initialize a new project**:

   ```bash
   mkdir express-review
   cd express-review
   npm init -y
   npm install express
   ```

2. **Create a basic server**:

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

### Using Middleware

Middleware functions have access to the request object, the response object, and the next middleware function in the application’s request-response cycle.

1. **Custom middleware**:

   ```js
   // Custom middleware to log request details
   const requestLogger = (req, res, next) => {
     console.log(
       `${req.method} request for '${req.url}' - ${new Date().toISOString()}`,
     );
     next();
   };

   app.use(requestLogger);
   ```

2. **Third-party middleware**:

   ```bash
   npm install body-parser
   npm install morgan
   npm install helmet
   ```

   ```js
   const bodyParser = require('body-parser');
   const morgan = require('morgan');
   const helmet = require('helmet');

   app.use(morgan('dev'));
   app.use(helmet());
   app.use(bodyParser.json());
   ```

### Using Express Router

1. **Create a router**:

   ```js
   // routes/index.js
   const express = require('express');
   const router = express.Router();

   router.get('/', (req, res) => {
     res.send('Hello from the main router!');
   });

   module.exports = router;
   ```

2. **Use the router**:
   ```js
   const routes = require('./routes/index');
   app.use('/', routes);
   ```

### Handling Errors

1. **Error-handling middleware**:
   ```js
   app.use((err, req, res, next) => {
     console.error(err.stack);
     res.status(err.status || 500).json({
       error: err.message || 'Internal Server Error',
     });
   });
   ```

## Introduction to EJS

EJS (Embedded JavaScript) is a simple templating language that lets you generate HTML markup with plain JavaScript.

### Step 1: Set Up EJS

1. **Install EJS**:

   ```bash
   npm install ejs
   ```

2. **Set EJS as the templating engine**:

   ```js
   // index.js
   const express = require('express');
   const bodyParser = require('body-parser');
   const morgan = require('morgan');
   const helmet = require('helmet');
   const app = express();
   const port = 3000;

   // Set EJS as the templating engine
   app.set('view engine', 'ejs');

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

   // Simple route to render an EJS template
   app.get('/', (req, res) => {
     res.render('index', {
       title: 'Hello, World!',
       message: 'Welcome to EJS with Express!',
     });
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

### Step 2: Create EJS Templates

1. **Create a `views` directory**:

   ```bash
   mkdir views
   ```

2. **Create an `index.ejs` file inside the `views` directory**:
   ```html
   <!-- views/index.ejs -->
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <meta
         name="viewport"
         content="width=device-width, initial-scale=1.0"
       />
       <title><%= title %></title>
     </head>
     <body>
       <h1><%= message %></h1>
     </body>
   </html>
   ```

### Step 3: Test EJS Templates

Start your server using Nodemon:

```bash
npm run dev
```

Navigate to `http://localhost:3000/` in your browser to see the rendered HTML page with the title "Hello, World!" and the message "Welcome to EJS with Express!".

## Conclusion

In this session, we've reviewed the key concepts covered this week, including setting up an Express server, using middleware, organizing routes with Express Router, and handling errors. We also introduced EJS as a templating engine for creating dynamic HTML pages.

Next week, we will start integrating MongoDB into our Express application. Great job this week, and have a fantastic weekend!

<!--! Hour 2 -->

In Hour 2, we will dive deeper into using EJS with Express. We'll learn how to pass data to our templates and create partials for reusable components. This session will also include a comprehensive review of the week's content.

## Objectives

- Learn how to pass data to EJS templates.
- Understand how to create and use EJS partials.
- Review the key concepts covered this week.

## Passing Data to EJS Templates

EJS allows you to pass data from your Express application to your templates, making it easy to render dynamic content.

### Step 1: Modify the Route to Pass Data

Modify the route in `index.js` to pass more data to the EJS template:

```js
// index.js
const express = require('express');
const bodyParser = require('body-parser');
const morgan = require('morgan');
const helmet = require('helmet');
const app = express();
const port = 3000;

// Set EJS as the templating engine
app.set('view engine', 'ejs');

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

// Route to render an EJS template
app.get('/', (req, res) => {
  const data = {
    title: 'Hello, World!',
    message: 'Welcome to EJS with Express!',
    users: [
      { name: 'Alice', age: 25 },
      { name: 'Bob', age: 30 },
      { name: 'Charlie', age: 35 },
    ],
  };
  res.render('index', data);
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

### Step 2: Modify the EJS Template to Display Data

Update `views/index.ejs` to display the passed data:

```html
<!-- views/index.ejs -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1.0"
    />
    <title><%= title %></title>
  </head>
  <body>
    <h1><%= message %></h1>
    <ul>
      <% users.forEach(user => { %>
      <li><%= user.name %> - <%= user.age %> years old</li>
      <% }) %>
    </ul>
  </body>
</html>
```

### Step 3: Test the Data Display

Start your server using Nodemon:

```bash
npm run dev
```

Navigate to `http://localhost:3000/` in your browser to see the list of users rendered dynamically.

## Creating and Using EJS Partials

Partials in EJS are used to include reusable components across multiple pages, such as headers, footers, and navigation bars.

### Step 4: Create EJS Partials

1. **Create a `partials` directory inside `views`**:

   ```bash
   mkdir views/partials
   ```

2. **Create a `header.ejs` partial**:

   ```html
   <!-- views/partials/header.ejs -->
   <header>
     <h1><%= title %></h1>
     <nav>
       <ul>
         <li><a href="/">Home</a></li>
         <li><a href="/about">About</a></li>
         <li><a href="/contact">Contact</a></li>
       </ul>
     </nav>
   </header>
   ```

3. **Create a `footer.ejs` partial**:

   ```html
   <!-- views/partials/footer.ejs -->
   <footer>
     <p>&copy; 2024 Pet Adoption</p>
   </footer>
   ```

### Step 5: Use Partials in the EJS Template

Update `views/index.ejs` to include the partials:

```html
<!-- views/index.ejs -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta
      name="viewport"
      content="width=device-width, initial-scale=1.0"
    />
    <title><%= title %></title>
  </head>
  <body>
    <%- include('partials/header') %>
    <main>
      <h1><%= message %></h1>
      <ul>
        <% users.forEach(user => { %>
        <li><%= user.name %> - <%= user.age %> years old</li>
        <% }) %>
      </ul>
    </main>
    <%- include('partials/footer') %>
  </body>
</html>
```

### Step 6: Test the Partials

Start your server using Nodemon:

```bash
npm run dev
```

Navigate to `http://localhost:3000/` in your browser to see the header and footer included in the page.

## Comprehensive Review

### Key Concepts Covered This Week

1. **Setting Up an Express Server**:

   - Initialize a project, install Express, create a basic server.

2. **Using Middleware**:

   - Custom middleware, third-party middleware (`body-parser`, `morgan`, `helmet`).

3. **Using Express Router**:

   - Create and use routers to organize routes.

4. **Handling Errors**:

   - Error-handling middleware to catch and manage errors.

5. **Introduction to EJS**:
   - Set up EJS, pass data to templates, create and use partials.

### Practice Exercise

Create a new route `/users` that renders a list of users using an EJS template.

1. **Add the route to `index.js`**:

   ```js
   app.get('/users', (req, res) => {
     const users = [
       { name: 'Alice', age: 25 },
       { name: 'Bob', age: 30 },
       { name: 'Charlie', age: 35 },
     ];
     res.render('users', { title: 'User List', users });
   });
   ```

2. **Create `views/users.ejs`**:

   ```html
   <!-- views/users.ejs -->
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <meta
         name="viewport"
         content="width=device-width, initial-scale=1.0"
       />
       <title><%= title %></title>
     </head>
     <body>
       <%- include('partials/header') %>
       <main>
         <h1><%= title %></h1>
         <ul>
           <% users.forEach(user => { %>
           <li><%= user.name %> - <%= user.age %> years old</li>
           <% }) %>
         </ul>
       </main>
       <%- include('partials/footer') %>
     </body>
   </html>
   ```

### Test the New Route

Navigate to `http://localhost:3000/users` in your browser to see the list of users rendered dynamically using EJS.

## Conclusion

In this session, we've reviewed the key concepts covered this week and delved deeper into using EJS with Express. You've learned how to pass data to templates, create and use partials, and apply these skills to build dynamic web pages. Great job this week, and keep practicing!

Next week, we will start integrating MongoDB into our Express application. Have a fantastic weekend!
