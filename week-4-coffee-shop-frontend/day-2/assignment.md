# Day 2 Guide: Coffee Shop Frontend

## Objective:  

Continue building the project you started in Day 1. Today, you will create pages for the Coffee Shop frontend, then break them into reusable components while the layout and structure are fresh in your mind. Utilize Storybook to verify components, and enhance the design for consistency across the application.

Note: If you are unable to complete the full assignment, focus on the login pages and the product pages. If you can't get to the cart, you can still make a decent project. Then, you can fill in the other pages later if you have time, or leave them as a stretch goal. However, some of the pages in the Cart functionality are repetitive, so you may be able to reuse components from the product page for the cart page.

Here are some helpful resources you may look into:

- [Your UI as a Tree](https://react.dev/learn/understanding-your-ui-as-a-tree) for understanding how to break down your UI into components.
- [Rendering Lists](https://react.dev/learn/describing-the-ui#rendering-lists) for arrays of data
- [Adding interactivity with React](https://react.dev/learn/adding-interactivity) for button clicks and form submissions.
- [React Forms](https://daveceddia.com/react-forms/). You may find this helpful in day three when finish design and begin work on functionality.

---

**Note:** Before you begin, create a new branch for your work. Make sure your main branch is up to date. Then create a new branch called design-components.

```bash
git switch main
git pull
git switch -c design-components
```

## **1. Create Basic Pages**
Set up the following blank pages in the `pages` directory. These pages will serve as the foundation of your application:

### **Required Pages**:
- `index.jsx`
- `products.jsx`
- `product/[id].jsx`
- `cart.jsx`
- `checkout.jsx`
- `signup.jsx`
- `signin.jsx`

### **Optional Admin Pages**:
- `admin/list-products.jsx`
- `admin/create-product.jsx`
- `admin/view-product.jsx`
- `admin/update-product.jsx`  
  *(Note: `update-product.jsx` will include a delete product confirmation popup.)*

Add a mock data json file for products and for the cart. You can use this data to test your components and pages.

You can put them in a `mocks` folder in the root of your project. You can name them `products.json` and `cart.json`. Grab some images from the internet or generate them from AI and put them in the `public` folder. You can use these images in your mock data.

You may copy some data from https://dummyjson.com/docs/products

(Note: take care to note differences in the structure of the data from our actual backend (id vs _id, etc.))

Or you may use this example data:

### Example: Products Data (`/src/mocks/products.json`)
```json
[
  {
    "name": "Espresso",
    "description": "A strong and concentrated coffee beverage.",
    "price": 2.5,
    "category": "Beverage",
    "stock": 10,
    "imageUrl": "/sample-images/espresso.jpg",
    "_id": 0
  },
  {
    "name": "Cappuccino",
    "description": "An Italian coffee drink that is traditionally prepared with equal parts espresso, steamed milk, and milk foam.",
    "price": 3.5,
    "category": "Beverage",
    "stock": 5,
    "imageUrl": "/sample-images/cappuccino.jpg",
    "_id": 1
  },
  {
    "name": "Croissant",
    "description": "A buttery, flaky, viennoiserie pastry named for its crescent shape.",
    "price": 2,
    "category": "Food",
    "stock": 8,
    "imageUrl": "/sample-images/croissant.jpg",
    "_id": 2
  },
  {
    "name": "Muffin",
    "description": "A small, sweet baked good that is typically made with ingredients such as flour, sugar, eggs, and butter.",
    "price": 2.5,
    "category": "Food",
    "stock": 6,
    "imageUrl": "/sample-images/muffin.jpg",
    "_id": 3
  }
]
```
### Example Cart data

`/src/mocks/cart.json`

```json
{
    "_id": "1",
    "products": [
        {
          "name": "Espresso",
          "description": "A strong and concentrated coffee beverage.",
          "price": 2.5,
          "category": "Beverage",
          "stock": 10,
          "imageUrl": "/sample-images/espresso.jpg",
          "_id": 0
        },
        {
          "name": "Croissant",
          "description": "A buttery, flaky, viennoiserie pastry named for its crescent shape.",
          "price": 2,
          "category": "Food",
          "stock": 8,
          "imageUrl": "/sample-images/croissant.jpg",
          "_id": 2
        },
        {
          "name": "Muffin",
          "description": "A small, sweet baked good that is typically made with ingredients such as flour, sugar, eggs, and butter.",
          "price": 2.5,
          "category": "Food",
          "stock": 6,
          "imageUrl": "/sample-images/muffin.jpg",
          "_id": 3
        }
    ],
    "user": "1"
}
```

Remember to git commit with a message like: "Add basic pages for the Coffee Shop frontend."

### Example: Home (Splash) Page (`src/pages/index.jsx`)
```jsx
export default function Home() {
  return (
    <div>
      <h1>Welcome to the Coffee Shop!</h1>
    </div>
  );
}
```

### Example 2: Product Page (`src/pages/products/[id].jsx`)
```jsx
export default function ProductPage({ id }) {
  return (
    <div>
      <h1>Product Page for product &num; { id }</h1>
    </div>
  );
}
```

### Example 3: Products Page (`src/pages/products/index.jsx`)
```jsx
export default function ProductsPage() {
  return (
    <div>
      <h1>Products Page</h1>
    </div>
  );
}
```

---

## **2. Splash Page (Home Page)**
### **Step 1**: Create the Layout
In `index.jsx`, build a splash page that includes the following:
- **Header**
- **Hero Image with Call-to-Action (CTA) Button**
- **Brief Description**
- **Footer**

### **Step 2**: Break  Spash Page Into Components
- Split the page into components, such as `Header`, `Footer`, `HeroSection`, and `Button`.
- Verify each component works in Storybook.
- Create a new story for each new component.
- Remember, your Button component should accept a `label` prop.
- If you like, you may have a prop that accepts a handleClick function.

### **Step 3**: Example: Update the Button to handle click events

- in `src/components/Button.jsx`:
  ```jsx
  export default function Button({ label, handleClick }) {
    return (
      <button onClick={handleClick} className="btn btn-primary">
        {label}
      </button>
    );
  }
  ```
- in `index.jsx`:
  ```jsx
  import Button from '../components/Button';
  import { useRouter } from 'next/router';
  ...
  export default function Home() {
    const router = useRouter();
    function handleCtaClick() {
      console.log('CTA button clicked!');
      router.push('/signup');
    }
    return (
      <div>
      ...
        <Button label="Sign Up Now" handleClick={handleCtaClick} />
      ...
      </div>
    );
  }
  ```

  This will allow the button to navigate to the signup page when clicked, but also keep the Button component flexible for other uses.

### **Step 4**: Verify in Storybook

Verify that your Button still works in Storybook.

**git commit** with a message like "Add Button component with click handling."
---

## **3. Signup Page**

Follow a similar process to the Splash page for the signup page and the rest of the pages. The goal is to build out your design, then break it into components. This will help you keep things organized. Focus on visual design and components. You will add functionality, like button clicks, later.

### **Step 1**: Layout
- Include the header and footer components from the splash page.
- Add a form with the following fields:
  - Name
  - Email
  - Password
  - Submit Button

### **Step 2**: Break the Form Into a Component
- Create a `SignupForm` component and use the following prop:
  - **`buttonLabel`**: A string that determines the button's text.
- Use **PropTypes** to validate `buttonLabel` as a string.

Example (Note 1: insert your own JSX and design choices into this component.) (Note 2: Don't forget to include PropTypes so you can validate the prop types in storybook.):
```jsx
import PropTypes from 'prop-types';
import Button from '@components/Button';

export default function SignupForm({ buttonLabel }) {
  return (
    <form className="form">
      <input type="text" placeholder="Name" />
      <input type="email" placeholder="Email" />
      <input type="password" placeholder="Password" />
      <Button label="Sign Up" handleClick={()=>{console.log("clicked sign up")}}/>
    </form>
  );
}

SignupForm.propTypes = {
  buttonLabel: PropTypes.string.isRequired,
};
```

### **Step 3**: Verify in Storybook
- Verify your story for `SignupForm` with a mock button label.

git commit with a message like "Add SignupForm component."

---

## **4. Signin Page**
- Reuse the `SignupForm` component for the `signin` page.
- Pass `"Signin"` as the `buttonLabel` prop.
- Pass a different `handleClick` function to the `Button` component that logs "clicked sign in".

git commit with a message like "Add Signin page form"

---

## **5. Product Page**
### **Step 1**: Layout
- In `src/pages/product/[id].jsx`, create a layout for a single product page.
- Include a header and footer.
- import the data from the mock products file at `src/mocks/products.json`.
- Create a product card displaying:
  - Product image
  - Name
  - Description
  - Price
  - Add to Cart Button

**Starter code for Product Page**:
```jsx
import products from '../../mocks/products.json';
import Button from '@components/Button';

// In Next.js, the id is passed as the second argument to the page component
// this comes from the file name `products/[id].jsx`
export default function ProductPage(props, { id }) {
  const product = products[id];
  return (
    <div>
      {/* TODO: Put this in CardProduct --> */}
      <h1>{product.name}</h1>
      <img src={product.imageUrl} alt={product.name} />
      <p>{product.description}</p>
      <p>${product.price}</p>
      <Button title="Add to Cart"></Button>
    </div>
  );
}
```

### **Step 2**: Break Into a `ProductCard` Component
- Create or update your `ProductCard` component.
- Pass the product data as a prop to the `ProductCard` component.
- Use **PropTypes** to validate the product as an object.

**Example**: Note: insert your own JSX and design choices into this component:
```jsx
import PropTypes from 'prop-types';
import Button from '@components/Button';

export default function ProductCard({ product }) {
  return (
    <div className="card">
      <img src={product.image} alt={product.name} />
      <h3>{product.name}</h3>
      <p>{product.description}</p>
      <p>${product.price}</p>
      <Button label="Add to Cart"/>
    </div>
  );
}

ProductCard.propTypes = {
  product: PropTypes.object.isRequired,
};
```

### **Step 3**: Verify in Storybook
- Verify your story for `ProductCard` with mock product data.

git commit with a message like "Add Product page."

---

## **6. Products Page**
- in `src/pages/products/index.jsx`, create a layout for the products page.
- Use the `ProductCard` component to display a list of products.
- Create mock data for products and iterate over them using `.map()`.
- See [Rendering Lists](https://react.dev/learn/describing-the-ui#rendering-lists) in the React documentation for more information.

Example:
Note: you may put the mock data in a separate file and import it. You may also add test images to the `public` folder in your project.
```jsx
import ProductCard from '../components/ProductCard';
import products from '../mocks/products.json';
import '@/styles/products.css';

// const products = [
//   { _id: 1, name: 'Coffee A', description: 'Rich and smooth.', price: 10, image: '/coffee-a.jpg' },
//   { _id: 2, name: 'Coffee B', description: 'Dark roast.', price: 12, image: '/coffee-b.jpg' },
// ];

export default function ProductsPage() {
  const productsJSX = products.map((product) => {
    // Use key prop every time you use map.
    // This is a unique identifier for each product.
    // React is not smart enough to keep track of the order of items in a list.
    // so we need to give it help by providing a unique key prop.
    return (<ProductCard key={product._id} product={product} />)
  });
  return (
    <div className="products-grid">
      {productsJSX}
    </div>
  );
}
```

- Use Flexbox or Grid for layout.

git commit with a message like "Add Products page to display multiple products."

---

## **7. Cart Page**
- in `src/pages/cart.jsx`, create a layout for the cart page.
- Create mock cart data (copy from product mock data).
- Use `ProductCard` to display cart items, with the button labeled `"Remove from Cart"`.
- Add a `"Checkout"` button at the bottom.
- Use Flexbox for layout.
- Optional: Break the cart items into a `CartItems` component if you don't like the way it looks with a standard Product component.

git commit with a message like "Add Cart page."

---

## **8. Checkout Page**
- in `src/pages/checkout.jsx`, create a layout for the checkout page.
- Include the header and footer.
- Create a form to collect:
  - Address
  - Phone Number
- Add a `"Buy Now"` button.
- For now, leave the payment functionality as a placeholder. You could have an alert that says "Thank you for your patronage" when the button is clicked.
- Break the form into a `CheckoutForm` component.
- Create a new story in Storybook for the `CheckoutForm` component.

git commit with a message like "Add Checkout page."

---

## **9. Design and CSS**
- Spend some time improving the overall look and feel of your pages.
- Use Tailwind CSS and DaisyUI for styling.
- Consider screen sharing with a peer to get feedback and inspiration for your design.
- "Timebox" your work to stay on schedule. Try and complete the design of your project today. We will work on adding functionality in the next few days and we may not have time to redesign the pages.

git commit with a message like "Improve design and styling."

---

## **10. Update README**
### **Include the Following**:
1. Screenshots of the pages.
2. Brief explanation of your design process.
3. Address future goals related to design. 
  - Selecting photos or splash page design.
  - Color schemes.
  - Font choices.
  - Responsive design considerations.
  - Accessibility features.

git commit with a message like "Update README with design details."

Then create a pull request to merge your changes into the main branch.

```bash
git add .
git commit -m "Update README with design details."
git push origin design-components
```

On GitHub, create a pull request to merge your changes into the main branch. Then click the "Merge pull request" button to merge your changes.

Finally, on your local machine, switch back to the main branch and pull the changes:

```bash
git switch main
git pull
```

You may safely delete the `design-components` branch now that the changes have been merged into the main branch. Or you may keep it for future reference.

```bash
git branch -d design-components
```

Tomorrow, you will branch again from the main branch to continue working on the project.

### Submit Your Work

Submit the GitHub repository link for the project. Be sure to include the link to the deployed app on Vercel.

If your deploy stops working, take a screenshot of the error message on Vercel and submit it with your repository link. Give a brief description of the work that you completed after the deploy stopped working.


---

By the end of Day 2, you should have functional, styled pages and reusable components that are tested in Storybook and ready for further development!