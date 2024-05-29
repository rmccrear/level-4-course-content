# Assignment: Express Assignment 1

## Objective

Gain hands-on experience with Express by setting up a simple server with specified routes.

## Instructions

### Part 1: Create and Clone Your GitHub Repository

1. **Create a New GitHub Repository**:

   - Name your repository `express-assignment-1`.
   - Initialize it with a `.gitignore` for Node.js and a `README.md`.

2. **Clone Your Repository**:
   - Clone your repository to your local machine.
   - Navigate to the cloned repository's directory to begin your project setup.

### Part 2: Setup and Configuration

1. **Initialize a Node.js Project**:

   - Within your project directory, run `npm init -y` to generate a `package.json` file.

2. **Install Express**:
   - Add Express as a dependency with `npm install express`.

### Part 3: Implement the Server

1. **Create an Express Server**:
   - Implement an Express server with the following routes:
     - `GET /` that returns "Welcome to the Express server!"
     - `GET /about` that returns "This is the about page."
     - `POST /data` that accepts JSON data and returns it back.

### Part 4: Use Nodemon for Development

1. **Install Nodemon**:

   - Add Nodemon as a development dependency with `npm install --save-dev nodemon`.

2. **Update `package.json`**:

   - Update the `scripts` section to include a `dev` script:

     ```json
     "scripts": {
       "start": "node index.js",
       "dev": "nodemon index.js"
     }
     ```

### Part 5: Test the Routes

1. **Use Thunder Client or Postman**:
   - Test the following routes:
     - `GET /` should return "Welcome to the Express server!"
     - `GET /about` should return "This is the about page."
     - `POST /data` should accept and return JSON data.

## Submission

- **GitHub Repository**: Submit the URL of your GitHub repository. Make sure it includes all necessary files to run the server, the updated `README.md`, and is set to public.

## Rubric

| Criteria                 | Limited (0 pts)                          | Partial (3 pts)                                | Complete (5 pts)                                         |
| ------------------------ | ---------------------------------------- | ---------------------------------------------- | -------------------------------------------------------- |
| **GitHub Setup**         | Significant issues in repository setup   | Repository has minor issues                    | Repository is correctly created, cloned, and initialized |
| **Express Installation** | Significant issues in installation       | Minor issues in installation                   | Express is correctly installed                           |
| **Route Implementation** | Significant issues or routes not working | Minor issues in route implementation           | All routes are correctly implemented and working         |
| **Nodemon Setup**        | Significant issues in Nodemon setup      | Minor issues in Nodemon setup                  | Nodemon is correctly set up and working                  |
| **Code Quality**         | Significant issues in code quality       | Minor issues in code organization or practices | Code is well-organized and follows best practices        |
