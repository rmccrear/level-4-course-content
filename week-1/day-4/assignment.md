# Assignment: Express - Assignment 4

## Objective

- Understand middleware in Express.
- Create and use custom middleware.
- Explore and use third-party middleware.

## Instructions

1. **Project Initialization**

   - Create a new directory named `express-middleware`.
   - Navigate into the directory.
   - Initialize a new Node.js project.
   - Install Express.

2. **Create the Server**

   - Create a file named `index.js`.
   - Set up a basic Express server that listens on port 3000.
   - Create a route that responds with "Hello, World!" at the root URL.

3. **Create Custom Middleware**

   - Add a custom middleware function that logs the details of each incoming request.
   - Ensure the middleware is used before defining the routes.

4. **Test Custom Middleware**

   - Start the server using Nodemon.
   - Make a request to the root URL and verify that the request details are logged in the console.

5. **Use Third-Party Middleware**

   - Install `body-parser`.
   - Modify the server to use `body-parser` to parse JSON bodies.
   - Create a POST route at `/contact` that responds with the parsed JSON data.

6. **Test Third-Party Middleware**

   - Start the server using Nodemon.
   - Use Thunder Client or Postman to send a POST request to `/contact` with a JSON body.
   - Verify that the parsed JSON data is included in the response.

7. **Explore Middleware Stack**
   - Create another custom middleware function that logs request headers.
   - Ensure the middleware functions are executed in the correct order.

## Submission Guidelines

- Create a new GitHub repository named `express-middleware`.
- Push your project to the repository.
- Submit the link to your GitHub repository.

## Rubric

| Criteria               | Limited (0 pts)                        | Partial (3 pts)                                | Complete (5 pts)                             |
| ---------------------- | -------------------------------------- | ---------------------------------------------- | -------------------------------------------- |
| Project Initialization | Significant setup issues               | Minor issues in setup                          | Project setup correctly                      |
| Server Creation        | Server does not run                    | Server runs but issues with response           | Server runs and responds to root URL         |
| Custom Middleware      | Custom middleware not implemented      | Custom middleware implemented with issues      | Custom middleware implemented correctly      |
| Third-Party Middleware | Third-party middleware not implemented | Third-party middleware implemented with issues | Third-party middleware implemented correctly |
| Middleware Stack       | Middleware stack not demonstrated      | Middleware stack demonstrated with issues      | Middleware stack demonstrated correctly      |
