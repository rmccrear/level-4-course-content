# Day One Setup for Coffee Shop Frontend

## Objective

Set up a solid foundation for the project by creating a Next.js app, integrating Storybook for component development, and establishing a connection with the backend API. Follow an incremental approach to ensure all components and connections work as expected before moving forward.

---

## **1. Setup Next.js and Repository**

1. **Create a New Next.js App**:

This should be next 15.1 or later.

   ```bash
   npx create-next-app@latest coffee-shop-frontend
   cd coffee-shop-frontend
   ```

2. **Initialize a Git Repository**:

Note: git may have been initialized during the Next.js setup. If so, skip `git init`. You will need to create a new repository on GitHub and push your code to it. The following commands should be equivalent to the code provided by GitHub when creating a new repository.

   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin <your-repo-url>
   git push -u origin main
   ```

3. **Install Dependencies**:

   ```bash
   npm install axios @tanstack/react-query
   ```

    - `axios`: For making API requests.
    - `@tanstack/react-query`: For managing API data and caching.

  Be sure to git commit after installing dependencies.

4. **Set Up Tailwind CSS (Only if not done by Next.js installer)**:

Note: This may have been completed by the Next.js installer. If so, skip this step.

   ```bash
   npx tailwindcss init -p
   ```

   - Update `tailwind.config.js`:
     ```javascript
     module.exports = {
       content: ["./pages/**/*.{js,ts,jsx,tsx}", "./components/**/*.{js,ts,jsx,tsx}"],
       theme: {
         extend: {},
       },
       plugins: [],
     };
     ```

   - Note: Delete the default content in `styles/globals.css`.
    Add or keep Tailwind CSS to `styles/globals.css`:
     ```css
     @tailwind base;
     @tailwind components;
     @tailwind utilities;
     ```

5. **Start the Development Server**:

   ```bash
   npm run dev
   ```

   Verify the app runs at [http://localhost:3000](http://localhost:3000).

---

## **2. Setup Storybook**

1. **Install Storybook**:

   ```bash
   npx storybook@latest init
   ```

2. **Run Storybook**:

   ```bash
   npm run storybook
   ```

   Verify Storybook runs at [http://localhost:6006](http://localhost:6006).

3. **Update Tailwind CSS in Storybook**:

   - In the file `.storybook/previews`, import the Tailwind CSS file:
     ```javascript
     /** @type { import('@storybook/react').Preview } */
     import '../src/styles/globals.css'; // add this line
     ...
     ```

4. **Test Storybook**:

   - Add a sample component with a prop-type:
     ```javascript
        // components/Button.js
        import PropTypes from "prop-types";
        
        export default function Button({ label }) {
          return <button className="btn btn-primary">{label}</button>;
        }
        
        Button.propTypes = {
          label: PropTypes.string.isRequired,
        };
     ```

   - Add a Story for it, and add a control value for "label".
   - Verify the button appears in Storybook.
   - Note: you should see a little button in Storybook until you set the label.

---

## **3. Setup All Components with a "Hello World"**

1. **Create Initial Components**:

   - Start with foundational components: `Navbar`, `Footer`, `Button`, `Loader`, and `Toast`.
   - Example:
     ```javascript
     // components/Navbar.js
     import PropTypes from 'prop-types';
     export default function Navbar({title}) {
       return <div className="navbar bg-base-100">Hello World Navbar {title} </div>;
     }
     Navbar.propTypes = {
       // Add prop-types here
        title: PropTypes.string.isRequired,
     };
    ```

2. **Add Stories for Each Component**:

  - Click the + sign in the Storybook interface to add a new story.
  - Set the controls if needed.


3. **Test Each Component in Storybook**:
   Run Storybook and visually verify all components display correctly.

---

## \*\*4. Set Up \*\***`.env`**

1. **Create an `.env.local` File**:
   ```plaintext
   NEXT_PUBLIC_API_BASE_URL=http://localhost:3000
   NEXT_PUBLIC_API_BASE_URL_PROD=https://api.onrender.com
   ```

2. **Access Environment Variables**:

   - Example usage in `api.js`:
     ```javascript
     import axios from 'axios';

     const api = axios.create({
       baseURL: process.env.NEXT_PUBLIC_API_BASE_URL,
     });

     export default api;
     ```

3. **Test API Configuration**:
   Use the environment variable in the app and log the base URL to confirm it works.

---

## **5. Connect to Backend with "Hello World"**

## **6. Setup DaisyUI**

1. **Install DaisyUI**:

   ```bash
   npm install daisyui
   ```

2. **Configure DaisyUI in Tailwind CSS**:

   - Update `tailwind.config.js`:
     ```javascript
     module.exports = {
       content: ["./pages/**/*.{js,ts,jsx,tsx}", "./components/**/*.{js,ts,jsx,tsx}"],
       theme: {
         extend: {},
       },
       plugins: [require("daisyui")],
     };
     ```

3. **Test DaisyUI**:

   - Add a sample button in a component:
     ```javascript
     export default function TestButton() {
       return <button className="btn btn-primary">Test DaisyUI</button>;
     }
     ```

   - View the button in your app or Storybook to verify DaisyUI is working correctly.

4. **Explore DaisyUI Components**:
   Use DaisyUI's [documentation](https://daisyui.com/components/) for pre-designed components to quickly enhance your app's UI.

1. **Setup API Integration**:
   - Example:
     ```javascript
     ```

```javascript
// components/HelloBackend.js
import api from '../utils/api';
```

```javascript
export default function HelloBackend() {
  const fetchGreeting = async () => {
    const response = await api.get('/hello-world');
    console.log(response.data);
  };
```

```javascript
  fetchGreeting();
  return (<h1>Hello Backend.. check console</h1>)
};
```



2. **Test Backend Connection**:
   - Start the backend server.
   - Verify the frontend successfully fetches and logs the backend response.

---

## **Incremental Development Tips**

- **Write-Test-Write Feedback Loop**: Write small code increments, test visually in Storybook, then continue.
- **Use Storybook Often**: Ensure every component renders correctly before integrating it into pages.
- **Debug Early**: Test backend connections and component integration in isolation to catch issues early.

---

By following these steps, you'll have a functional, testable foundation to build your Coffee Shop frontend with confidence.

