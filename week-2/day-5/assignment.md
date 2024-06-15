# Assignment: MongoDB - Assignment 5

## Objective

- Review and practice MongoDB and Mongoose basics.
- Implement CRUD operations, authentication, and role-based access control (RBAC).
- Test endpoints and verify functionalities with screenshots.

## Instructions

### Part 1: Set Up a Simple Express Project

1. **Initialize the Project and Install Dependencies**:

   ```bash
   mkdir mongodb-practice
   cd mongodb-practice
   npm init -y
   npm install express mongoose bcryptjs jsonwebtoken
   ```

2. **Set Up the Server**:

   - Create a file named `index.js` and set up a basic Express server with Mongoose connection.

### Part 2: Create the User Model

1. **Create `models/User.js`**:
   - Define a user schema with the following fields: `name`, `email`, `password`, and `role`.
   - Implement password hashing using bcrypt before saving the user.

### Part 3: Implement CRUD Operations

1. **Update `index.js` to include CRUD routes**:
   - Route to register a new user with hashed passwords (`POST /register`).
   - Route to login a user and return a JWT token (`POST /login`).
   - Route to get all users (`GET /users`).
   - Route to update a user (`PUT /users/:id`).
   - Route to delete a user (`DELETE /users/:id`).

### Part 4: Test CRUD Operations

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Register a User**:

     - Method: POST
     - URL: `http://localhost:3000/register`
     - Body: JSON

       ```json
       {
         "name": "John Doe",
         "email": "john@example.com",
         "password": "password123",
         "role": "user"
       }
       ```

     - Screenshot the POST request to register a user.

   - **Login a User**:

     - Method: POST
     - URL: `http://localhost:3000/login`
     - Body: JSON

       ```json
       {
         "email": "john@example.com",
         "password": "password123"
       }
       ```

     - If successful, this should return a JWT token.
     - Screenshot the POST request to login a user with the returned JWT token.

   - **Get All Users**:

     - Method: GET
     - URL: `http://localhost:3000/users`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - Screenshot the GET request to get all users.

   - **Update a User**:

     - Method: PUT
     - URL: `http://localhost:3000/users/<user_id>`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - Body: JSON

       ```json
       {
         "name": "Jane Doe"
       }
       ```

     - Screenshot the PUT request to update a user.

   - **Delete a User**:
     - Method: DELETE
     - URL: `http://localhost:3000/users/<user_id>`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - Screenshot the DELETE request to delete a user.

### Part 5: Implement Role-Based Access Control (RBAC)

1. **Create Middleware for Authentication and Role-Based Access Control**:

   - Create a file named `middleware/auth.js` to verify JWT tokens.
   - Create a file named `middleware/role.js` to check user roles.

2. **Protect Routes with Authentication and RBAC Middleware**:
   - Route to access a protected resource (`GET /dashboard`).
   - Admin-only route example (`GET /admin`).

### Part 6: Test Role-Based Access Control

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

   - **Access Protected Route**:

     - Method: GET
     - URL: `http://localhost:3000/dashboard`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - Screenshot the GET request to access the protected route.

   - **Register an Admin User**:

     - Method: POST
     - URL: `http://localhost:3000/register`
     - Body: JSON

       ```json
       {
         "name": "Admin User",
         "email": "admin@example.com",
         "password": "adminpassword123",
         "role": "admin"
       }
       ```

     - Screenshot the POST request to register an admin user.

   - **Register a Regular User**:

     - Method: POST
     - URL: `http://localhost:3000/register`
     - Body: JSON

       ```json
       {
         "name": "Regular User",
         "email": "user@example.com",
         "password": "userpassword123",
         "role": "user"
       }
       ```

     - Screenshot the POST request to register a regular user.

   - **Access Admin Route as Admin**:

     - Method: GET
     - URL: `http://localhost:3000/admin`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - If the token belongs to an admin, you should see "This is the admin panel."
     - Screenshot the GET request to access the admin route as an admin.

   - **Access Admin Route as Regular User**:
     - Method: GET
     - URL: `http://localhost:3000/admin`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - If the token belongs to a regular user, you should receive an access denied error.
     - Screenshot the GET request to access the admin route as a regular user.

### Submission

- **GitHub Repository**: Create a new repository named `mongodb-practice`. Push your project to the repository and submit the URL. Ensure it includes all necessary files to run the server, including the `README.md`.
- **Screenshots**: Include the screenshots of your POST and GET requests in the `README.md`.

### Rubric

| Criteria                      | Limited (0 pts)                          | Partial (3 pts)                            | Complete (5 pts)                                              |
| ----------------------------- | ---------------------------------------- | ------------------------------------------ | ------------------------------------------------------------- |
| **Project Setup**             | Significant setup issues                 | Minor issues in setup                      | Project set up correctly with required packages               |
| **Schema and Model**          | Schema and model not defined             | Schema and model defined with minor issues | Schema and model defined correctly                            |
| **Route Implementation**      | Significant issues or routes not working | Minor issues in route implementation       | All routes are correctly implemented and working              |
| **Authentication Middleware** | Middleware not implemented               | Middleware implemented with minor issues   | Middleware implemented correctly                              |
| **Role-Based Access Control** | RBAC not implemented                     | RBAC implemented with minor issues         | RBAC implemented correctly                                    |
| **Screenshots**               | Screenshots not provided or incomplete   | Some screenshots provided                  | All required screenshots provided and included in `README.md` |
