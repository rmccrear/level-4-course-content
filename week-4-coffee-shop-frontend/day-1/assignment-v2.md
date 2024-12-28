# Day One Setup for Coffee Shop Frontend

## Objective

Set up a solid foundation for the project by creating a Next.js app, integrating Storybook for component development, and establishing a connection with the backend API. Follow an incremental approach to ensure all components and connections work as expected before moving forward.

---

## **1. Setup Next.js and Repository**

1. **Create a New Next.js App**:

   ```bash
   npx create-next-app@latest coffee-shop-frontend
   cd coffee-shop-frontend
   ```

   - During setup, select **Use Tailwind CSS** and **Use Pages Router** options.

2. **Initialize a Git Repository**:

   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin <your-repo-url>
   git push -u origin main
   ```

3. **Test Application**:

   ```bash
   npm run dev
   ```

   Verify the app runs at [http://localhost:3000](http://localhost:3000).

4. Update your README.md file.

- delete the default content.
- add an outline of what you plan to do or the template provided in the assignment.

5. Update index.js file.

- delete the default content.
- add a simple heading to test the app.

```jsx
export default function Home() {
  return <h1>Coffee Shop Frontend Splash Page</h1>;
}
```

6. Deploy to Vercel.

It's a good idea to deploy your app to Vercel right away to ensure it works as expected in a production environment.

- Push your code to GitHub.
- Create a new project on Vercel.
- Import your project from GitHub.
- Deploy your project.
- Note: you may want to have a special branch for deployment, such as `main` or `production`.
- Note: Although you have no Environment Variables yet, remember to update your Environment Variables in Vercel to match your `.env.local` file as you add them.
- Check your deployed app to ensure it works as expected after each commit to see if you can catch any bugs as they are made.

---

## **2. Setup Storybook**

1. **Install Storybook**:

   ```bash
   npx storybook@latest init
   ```

2. **Run Storybook**:

Storybook may will already be running after the installation. If not, you can start it with:

   ```bash
   npm run storybook
   ```

   Verify Storybook runs at [http://localhost:6006](http://localhost:6006).

    - Note: Delete the default content in `styles/globals.css`.
    Add or keep Tailwind CSS to `styles/globals.css`:
     ```css
     @tailwind base;
     @tailwind components;
     @tailwind utilities;
     ```

- Add your global CSS to storybook. In the file `.storybook/previews`, import the Tailwind CSS file:

     ```javascript
     /** @type { import('@storybook/react').Preview } */
     import '../src/styles/globals.css'; // add this line
     ...
     ```

3. **Add a Button Component to Test**:

   - First, delete the `stories` directory in the `src` folder.

   - Next, create a simple button component to test in Storybook:

     ```javascript
     // components/Button.js
      import PropTypes from 'prop-types';
      export default function Button({ label }) {
        return <button className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">{label}</button>;
      }
      Button.propTypes = {
        label: PropTypes.string.isRequired,
      };
     ```

   - Open Storybook's UI by running:

     ```bash
     npm run storybook
     ```

   - In Storybook, create a new story:
     - Navigate to the Storybook interface.
     - Use the UI options to create a story for the `Button` component.

   - Test the button in the Storybook interface to ensure it renders correctly.
   - Open Storybook's UI by running:

     ```bash
     npm run storybook
     ```

   - In Storybook, create a new story:
     - Navigate to the Storybook interface.
     - Use the UI options to create a story for the `Button` component.

   - Run Storybook and confirm the button renders correctly.

---

## **3. Create Skeleton for All Components**

1. **Create Components**:
   - Create files for the following components:
     - Navbar.jsx
     - Footer.jsx
     - Button.jsx
     - Loader.jsx
     - Toast.jsx
     - ProductCard.jsx
     - CartItem.jsx
     - CartSummary.jsx

2. **Add Skeleton Code and Stories**:
   - Do not complete the components yet; add a simple "Hello World" message to each component. You will have the opportunity to add more functionality later.
   - Example for `Navbar`:

     ```javascript
     // components/Navbar.js
     import PropTypes from 'prop-types';
     export default function Navbar({title}) {
       return <div className="navbar">Navbar Component {title}</div>;
     }
     Navbar.propTypes = {
       // Add prop-types here
        title: PropTypes.string.isRequired,
     };
     ```

   - Make use of snippets or copy and paste boilerplate code to speed up the process.
   - Test the components in Storybook to ensure they render correctly.
   - Click the + sign in the Storybook interface to add a new story.
   - Don't forget to click update story in storybook to save the changes, if you update controls (props).
   - Commit often to track your progress.

3. **Test Each Component in Storybook**:
   Verify all components render correctly in Storybook.

4. **Update README.md**:
   - Document the components created and the progress made.
   - Example:

     ```markdown
     # Coffee Shop Frontend

     ## Components
     - Navbar
     - Footer
     - Button
     - Loader
     - Toast
     - ProductCard
     - CartItem
     - CartSummary
     ```

     Add one screenshot of the Storybook interface to your README.md. 
---

## **4. Setup .env File and Connect to Backend**

1. **Create `.env.local` File**:

   ```plaintext
   NEXT_PUBLIC_API_BASE_URL=http://localhost:3000
   NEXT_PUBLIC_API_BASE_URL_PROD=https://api.onrender.com
   ```

2. **Connect to Backend**:
   - Create a simple component:

     ```javascript
     // components/HelloBackend.js
     import api from '../utils/api';

     export default function HelloBackend() {
       const fetchGreeting = async () => {
         const response = await api.get('/hello-world');
         console.log(response.data);
       };

       fetchGreeting();

       return <div>Hello Backend</div>;
     }
     ```

3. **Test Backend Connection**:
   - Verify the console logs the response from the backend.

---

## **5. Setup DaisyUI**

1. **Install DaisyUI**:

   ```bash
   npm install daisyui
   ```

2. **Configure DaisyUI**:
   - Add DaisyUI to `tailwind.config.js`:

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
   - Add a sample component:

     ```javascript
     export default function TestButton() {
       return <button className="btn btn-primary">Test DaisyUI</button>;
     }
     ```

   - Verify the button renders with DaisyUI styling.

---

## **6. Setup Custom DaisyUI Theme**

1. **Use DaisyUI Theme Generator**:
   - Visit the [DaisyUI Theme Generator](https://daisyui.com/theme-generator/).
   - Customize the theme colors to align with your coffee shop branding.
   - Copy the generated theme configuration.

2. **Configure the Custom Theme**:
   - Update `tailwind.config.js` with the copied theme configuration:

     ```javascript
     module.exports = {
       plugins: [require("daisyui")],
       daisyui: {
         themes: [
           {
             coffeeShop: {
               primary: "#6B4F4F", // Replace with your custom color
               secondary: "#FFD700", // Replace with your custom color
               accent: "#C0C0C0", // Replace with your custom color
               neutral: "#3D4451", // Replace with your custom color
               "base-100": "#FFFFFF", // Replace with your custom color
             },
           },
         ],
       },
     };
     ```

3. **Test the Theme**:
   - Apply the theme in your app:

     ```javascript
     // pages/_app.js
     import '../styles/globals.css';

     export default function MyApp({ Component, pageProps }) {
       return (
         <div data-theme="coffeeShop">
           <Component {...pageProps} />
         </div>
       );
     }
     ```

   - Verify the custom theme is applied and renders as expected.

1. **Configure Custom Theme**:
   - Update `tailwind.config.js`:

     ```javascript
     module.exports = {
       plugins: [require("daisyui")],
       daisyui: {
         themes: [
           {
             coffeeShop: {
               primary: "#6B4F4F",
               secondary: "#FFD700",
               accent: "#C0C0C0",
               neutral: "#3D4451",
               "base-100": "#FFFFFF",
             },
           },
         ],
       },
     };
     ```

2. **Test Theme**:
   - Apply the theme in your app:

     ```javascript
     // pages/_app.js
     import '../styles/globals.css';

     export default function MyApp({ Component, pageProps }) {
       return (
         <div data-theme="coffeeShop">
           <Component {...pageProps} />
         </div>
       );
     }
     ```

   - Verify the theme is applied.

---

## **7. Create Custom CSS Rule for a Component**

1. **Add Custom CSS**:
   - Example for `Navbar`:

     ```css
     /* styles/navbar.css */
     .navbar {
       background-color: #6b4f4f;
       color: white;
     }
     ```

2. **Import CSS**:

   ```javascript
   // components/Navbar.js
   import '../styles/navbar.css';

   export default function Navbar() {
       return <div className="navbar">Navbar Component</div>;
   }
   ```

3. **Test Custom CSS**:
   - Verify the custom style is applied to the `Navbar` component.

---

## **8. Test and Create README.md**

1. **Test Application**:
   - Ensure all components work in Storybook and the app.
   - Verify the backend connection and DaisyUI integration.

2. **Create `README.md`**:
   - Document the setup steps, dependencies, and testing process.
   - Example:

     ```markdown
     # Coffee Shop Frontend

     ## Setup
     1. Clone the repository.
     2. Run `npm install` to install dependencies.
     3. Start the app with `npm run dev`.

     ## Features
     - Tailwind CSS with DaisyUI integration.
     - Storybook for component development.
     - Backend connection with a sample HelloBackend component.
     ```

---

By following these steps, you'll have a robust, tested foundation to build your Coffee Shop frontend efficiently.