# Assignment: MongoDB - Assignment 2

## Objective

- Review basic CRUD operations using Mongoose.
- Perform more complex queries and updates.
- Learn to handle validation and errors more effectively.

## Instructions

### Part 1: Set Up the Project

1. **Initialize the Project and Install Dependencies**:

   ```bash
   mkdir express-mongodb-advanced
   cd express-mongodb-advanced
   npm init -y
   npm install express mongoose
   ```

2. **Create the Server**:

   - Create a file named `index.js` and set up a basic Express server with Mongoose connection.

### Part 2: Define the Schema and Model

1. **Create `models/User.js`**:
   - Define a user schema with the following fields: `name`, `email`, `age`, and `isActive`.

### Part 3: Create and Read Users

1. **Update `index.js` to include routes for creating and reading users**:
   - Route to add a new user (`POST /users`).
   - Route to get all users (`GET /users`).
   - Route to get active users (`GET /users/active`).

### Part 4: Test Create and Read Operations

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

   - **Get Active Users**:

     - Method: GET
     - URL: `http://localhost:3000/users/active`

   - **Take Screenshots**:
     - Screenshot of POST request to create a user.
     - Screenshot of GET request to retrieve all users.
     - Screenshot of GET request to retrieve active users.

### Part 5: Update and Delete Users

1. **Update `index.js` to include update and delete routes**:
   - Route to update a user (`PUT /users/:id`).
   - Route to deactivate a user (`PUT /users/:id/deactivate`).
   - Route to delete a user (`DELETE /users/:id`).

### Part 6: Test Update and Delete Operations

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

   - **Deactivate a User**:

     - Method: PUT
     - URL: `http://localhost:3000/users/<user-id>/deactivate`

   - **Delete a User**:

     - Method: DELETE
     - URL: `http://localhost:3000/users/<user-id>`

   - **Take Screenshots**:
     - Screenshot of PUT request to update a user.
     - Screenshot of PUT request to deactivate a user.
     - Screenshot of DELETE request to delete a user.

## Submission

- **GitHub Repository**: Create a new repository named `express-mongodb-advanced`. Push your project to the repository and submit the URL. Ensure it includes all necessary files to run the server, including the `README.md`.
- **Screenshots**: Include the screenshots of your POST, GET, PUT, and DELETE requests in the `README.md`.

## Rubric

| Criteria                 | Limited (0 pts)                          | Partial (3 pts)                            | Complete (5 pts)                                              |
| ------------------------ | ---------------------------------------- | ------------------------------------------ | ------------------------------------------------------------- |
| **Project Setup**        | Significant setup issues                 | Minor issues in setup                      | Project set up correctly with required packages               |
| **Schema and Model**     | Schema and model not defined             | Schema and model defined with minor issues | Schema and model defined correctly                            |
| **Route Implementation** | Significant issues or routes not working | Minor issues in route implementation       | All routes are correctly implemented and working              |
| **CRUD Operations**      | CRUD operations not working              | CRUD operations working with minor issues  | CRUD operations working correctly                             |
| **Screenshots**          | Screenshots not provided or incomplete   | Some screenshots provided                  | All required screenshots provided and included in `README.md` |
