# Assignment: Express - Assignment 2

## Objective

Understand and implement basic HTTP methods (GET and POST) in an Express application. Handle URL and query parameters in Express routes, and practice sending and receiving JSON data.

## Instructions

### Part 1: Create and Clone Your GitHub Repository

1. **Create a New GitHub Repository**:

   - Name your repository `express-http-methods`.
   - Initialize it with a `.gitignore` for Node.js and a README.md.

2. **Clone Your Repository**:
   - Navigate to the cloned repository's directory to begin your project setup.

### Part 2: Setup and Configuration

1. **Initialize a Project**:

   - Within your project directory, run `npm init -y` to generate necessary files.

2. **Install Dependencies**:
   - Add the necessary dependencies with `npm install express`.

### Part 3: Implement Routes for GET and POST Requests

1. **Setup Basic Routing**:

   - Create a new file named `index.js`.
   - Set up routes to handle GET and POST requests.

2. **Middleware**:

   - Include middleware to parse JSON bodies.

3. **Create Routes**:
   - Implement routes to handle the following:
     - A GET request to the root (`/`) that responds with a welcome message.
     - A GET request to `/about` that responds with a JSON message.
     - A POST request to `/contact` that responds with the submitted data.
   - Ensure there is a catch-all route for undefined routes that returns a 404 status code.

### Part 4: Handle URL and Query Parameters

1. **URL Parameters**:

   - Add a route to handle URL parameters for `/user/:id`.

2. **Query Parameters**:
   - Add a route to handle query parameters for `/search`.

### Part 5: Testing

1. **Test the Routes**:
   - Use Thunder Client or Postman to test the routes.
   - Example tests:
     - **POST /contact**: Send a JSON body with name and email.
     - **GET /user/:id**: Replace `:id` with a sample user ID.
     - **GET /search**: Include query parameters `term` and `sort`.

## Submission

- **GitHub Repository**: Submit the URL of your GitHub repository. Ensure it includes all necessary files to run the project and an updated README.md, and is set to public.
- **Additional Requirements (if any)**: Please specify if you need the assignment deployed or any screenshots for verification.

## Rubric

### Setup and Basic Routing

- **Complete (5 pts)**: Express app setup with basic routes.
- **Partial (3 pts)**: Express app setup with incomplete routes.
- **Limited (0 pts)**: Express app setup incorrectly.

### Handling Different HTTP Methods

- **Complete (5 pts)**: GET and POST methods handled correctly.
- **Partial (3 pts)**: One of the HTTP methods handled correctly.
- **Limited (0 pts)**: HTTP methods not handled or incorrect.

### Handling URL Parameters

- **Complete (5 pts)**: URL parameter route works correctly.
- **Partial (3 pts)**: URL parameter route partially works.
- **Limited (0 pts)**: URL parameter route does not work.

### Handling Query Parameters

- **Complete (5 pts)**: Query parameter route works correctly.
- **Partial (3 pts)**: Query parameter route partially works.
- **Limited (0 pts)**: Query parameter route does not work.

### Submission and GitHub Repo

- **Complete (5 pts)**: Correct repository structure and code.
- **Partial (3 pts)**: Partially correct repository structure.
- **Limited (0 pts)**: Incorrect repository structure.
