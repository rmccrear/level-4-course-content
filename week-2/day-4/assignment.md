# Assignment: MongoDB - Assignment 4

## Objective

- Set up a new Express project.
- Implement middleware to protect routes.
- Understand role-based access control and its importance.
- Implement registration and login functionality with role-based access control (RBAC).

## Instructions

### Part 1: Set Up a New Project

1. **Initialize the Project and Install Dependencies**:

   ```bash
   mkdir express-session-rbac
   cd express-session-rbac
   npm init -y
   npm install express mongoose bcryptjs jsonwebtoken
   ```

2. **Create the Server**:

   - Create a file named `index.js` and set up a basic Express server with Mongoose connection.

### Part 2: Define the User Schema with Role and Password Hashing

1. **Create `models/User.js`**:
   - Define a user schema with the following fields: `name`, `email`, `password`, and `role`.
   - Implement password hashing using bcrypt before saving the user.

### Part 3: Implement Registration and Login Endpoints

1. **Update `index.js` to include registration and login routes**:
   - Route to register a new user with hashed passwords (`POST /register`).
   - Route to login a user and return a JWT token (`POST /login`).

### Part 4: Test Registration and Login Endpoints

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
         "password": "password123"
       }
       ```

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

   - **Take Screenshots**:
     - Screenshot of the POST request to register a user.
     - Screenshot of the POST request to login a user with the returned JWT token.

### Part 5: Implement Middleware for Route Protection

1. **Create Middleware for Authentication**:

   - Create a file named `middleware/auth.js` and implement middleware to check if a user is authenticated by verifying the JWT token.

### Part 6: Protect Routes with Authentication Middleware

1. **Update `index.js` to include protected routes**:
   - Route to access a protected resource (`GET /dashboard`).

### Part 7: Test Protected Routes

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
     - If the token is valid, you should see "This is the dashboard." If not, you should receive an error message.

   - **Take Screenshot**:
     - Screenshot of the GET request to access the protected route.

### Part 8: Implement Role-Based Access Control (RBAC)

1. **Create Middleware for Role-Based Access Control**:

   - Create a file named `middleware/role.js` and implement middleware to check user roles.

2. **Update `index.js` to include RBAC middleware and protected routes**:
   - Route to access an admin-only resource (`GET /admin`).

### Part 9: Test Role-Based Access Control

1. **Start Your Server**:

   ```bash
   npm run dev
   ```

2. **Test with Thunder Client or Postman**:

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

   - **Login as Admin User**:

     - Method: POST
     - URL: `http://localhost:3000/login`
     - Body: JSON
       ```json
       {
         "email": "admin@example.com",
         "password": "adminpassword123"
       }
       ```
     - Copy the token from the response.

   - **Access Admin Route as Admin**:

     - Method: GET
     - URL: `http://localhost:3000/admin`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - If the token belongs to an admin, you should see "This is the admin panel." If not, you should receive an access denied error.

   - **Access Admin Route as Regular User**:

     - Method: GET
     - URL: `http://localhost:3000/admin`
     - Header:
       - `Authorization: Bearer <your_jwt_token>`
     - If the token belongs to a regular user, you should receive an access denied error.

   - **Take Screenshots**:
     - Screenshot of the POST request to register an admin user.
     - Screenshot of the POST request to register a regular user.
     - Screenshot of the GET request to access the admin route as an admin.
     - Screenshot of the GET request to access the admin route as a regular user.

## Submission

- **GitHub Repository**: Create a new repository named `express-session-rbac`. Push your project to the repository and submit the URL. Ensure it includes all necessary files to run the server, including the `README.md`.
- **Screenshots**: Include the screenshots of your POST and GET requests in the `README.md`.

## Rubric

| Criteria                      | Limited (0 pts)                          | Partial (3 pts)                            | Complete (5 pts)                                              |
| ----------------------------- | ---------------------------------------- | ------------------------------------------ | ------------------------------------------------------------- |
| **Project Setup**             | Significant setup issues                 | Minor issues in setup                      | Project set up correctly with required packages               |
| **Schema and Model**          | Schema and model not defined             | Schema and model defined with minor issues | Schema and model defined correctly                            |
| **Authentication Middleware** | Middleware not implemented               | Middleware implemented with minor issues   | Middleware implemented correctly                              |
| **Role-Based Access Control** | RBAC not implemented                     | RBAC implemented with minor issues         | RBAC implemented correctly                                    |
| **Screenshots**               | Screenshots not provided or incomplete   | Some screenshots provided                  | All required screenshots provided and included in `README.md` |
