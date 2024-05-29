# Assignment: Express - Assignment 5

## Objective

- Review Express basics, routing, and middleware.
- Set up and use EJS in an Express application.

## Instructions

1. **Project Initialization**

   - Create a new directory named `express-review`.
   - Navigate into the directory.
   - Initialize a new Node.js project.
   - Install Express and EJS.

2. **Create the Server**

   - Create a file named `index.js`.
   - Set up a basic Express server that listens on port 3000.
   - Set EJS as the templating engine.
   - Create a route that renders an EJS template with a title and message.

3. **Set Up Middleware**

   - Install and use `body-parser`, `morgan`, and `helmet` middleware.
   - Create and use a custom middleware function that logs request details.

4. **Create and Use EJS Templates**

   - Create a `views` directory.
   - Inside `views`, create an `index.ejs` file.
   - Render a dynamic HTML page using EJS.

5. **Create EJS Partials**

   - Create a `partials` directory inside `views`.
   - Create `header.ejs` and `footer.ejs` partials.
   - Include the partials in the main EJS template.

6. **Add a Route to Render User Data**
   - Create a new route `/users` that renders a list of users using an EJS template.
   - Pass a list of users to the EJS template and render it dynamically.

## Submission Guidelines

- Create a new GitHub repository named `express-review`.
- Push your project to the repository.
- Submit the link to your GitHub repository.

## Rubric

| Criteria               | Limited (0 pts)            | Partial (3 pts)                      | Complete (5 pts)                               |
| ---------------------- | -------------------------- | ------------------------------------ | ---------------------------------------------- |
| Project Initialization | Significant setup issues   | Minor issues in setup                | Project setup correctly with required packages |
| Server Creation        | Server does not run        | Server runs but issues with response | Server runs and responds correctly             |
| Middleware Setup       | Middleware not implemented | Middleware implemented with issues   | Middleware implemented correctly               |
| EJS Template Rendering | EJS templates not rendered | EJS templates rendered with issues   | EJS templates rendered correctly               |
| EJS Partials           | Partials not used          | Partials used with issues            | Partials used correctly                        |
| Dynamic Data Rendering | Dynamic data not rendered  | Dynamic data rendered with issues    | Dynamic data rendered correctly                |
