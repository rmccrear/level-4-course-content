# Day 3 Guide: Coffee Shop Frontend Project

## Objective:  
Today, you will add functionality to your Coffee Shop frontend project. You will implement stub functions to handle user interactions, manipulate mock data, and save it to localStorage. By the end of the day, you will have a functional frontend with mock data that will serve as a foundation for connecting to a backend in the future.

Note: here are a few videos to help you understand the concepts for today.

- [Prop Drilling](https://www.youtube.com/watch?v=RVFAyFWO4go&t=5750s)
- [useState](https://www.youtube.com/watch?v=yvTGXH7uybA)

---

## Step 0: **Setup .env File and Connect to Backend**

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

## **Step 1: Create Stub Functions**

### **Instructions**:
1. **Stub Functions**: Create empty functions in the respective Page components. For now, these will log messages to the console or display alerts.
2. **Placement**:
   - Add the functions inside the **Page function** of each page.
   - Some will be triggered on page load using `useEffect`.
   - Others will be triggered by button clicks.
3. **Prop Drilling**: Pass functions as props to child components where needed.

### **List of Stub Functions and Locations**:
| **Page/Component**             | **Functions to Add**                          |
|---------------------------------|-----------------------------------------------|
| **Header Component**            | `goToLogin`, `goToSignup`                     |
| **Register Page** (`signup.jsx`) | `registerUser(user)`                          |
| **Login Page** (`login.jsx`)    | `signInUser(username, password)`              |
| **Product List Page**           | `loadProducts()`, `filterProducts(category, start, limit)`, `viewProduct(product)` |
| **Product Display Page**        | `fetchProduct(id)`, `addToCart(product)`      |
| **Cart Page**                   | `loadCart()`, `addToCart(product)`, `removeFromCart(product)`, `saveCartToLocalStorage(cart)` |
| **Utility Functions** (`util/index.js`) | `loadProductsFromLocalStorage`, `loadCartFromLocalStorage`, `saveCartToLocalStorage`, `saveProductsToLocalStorage`, `saveUserToLocalStorage`, `loginUserToLocalStorage` |

### **Example**:
```javascript
// src/pages/cart.jsx
import { useEffect, useState } from 'react';

import ProductCard from '@/components/ProductCard';

// Stub data for Cart Page
import data from '@/mocks/cart.json';

export default function CartPage() {
  const [cart, setCart] = useState([]);

// Stub functions for Cart Page
  const loadCart = () => console.log("Cart loaded");
  const addToCart = (product) => console.log(`Added ${product.name} to cart`);
  const removeFromCart = (product) => console.log(`Removed ${product.name} from cart`);
  const saveCartToLocalStorage = (cart) => console.log("Cart saved to localStorage");

  // use useEffect to put initialization code.
  useEffect(() => {
    loadCart();
    setCart([...data]);
  }, []);

  const handleClick = (product) => {
    addToCart(product);
  };

  // Note how we pass the handleClick function to the ProductCard component.
  // This will passed again to the button in the ProductCard component.
  // Passing a prop multiple times is called prop drilling.
  return (<div>
  <h1>Shopping Cart: {cart.length} items in cart</h1>
  {cart.map((product) => (
    <ProductCard 
      key={product.id}
      product={product}
      handleClick={() => handleClick(product)} 
    />
  ))}

  </div>);
}
```

### **Git Commit**:
```bash
git add src/pages/ src/components/ src/util/
git commit -m "Add stub functions for each page and utility functions for localStorage"
```

---

## **Step 2: Implement Product List Page Functionality**

### **Instructions**:
1. Replace stub functions with real functionality to:
   - Load and filter products.
   - Pass `viewProduct` to the button in `ProductCard` using **prop drilling**.
2. Use mock data to populate the product list.
   - check the Appendix at the end of this document for examples of mock data and utility functions.

Observe how the `viewProduct` function is passed down to the `ProductCard` component and then to the `Button` component. This is an example of **prop drilling**. You will often use this technique to pass functions down to child components. It can be tedious, but it's a common pattern in React.

Example of `viewProduct` function:
```javascript
// in src/pages/products/index.jsx
const viewProduct = (product) => {
  router.push(`/products/${product.id}`);
};
...
<ProductCard product={product} handleClick={() => viewProduct(product)} />
...
```

```javascript
// in src/components/ProductCard.jsx
<Button handleClick={() => handleClick(product)}>View Product</button>
```

```javascript
// in src/components/Button.jsx
export default function Button({ handleClick, label }) {
  return <button onClick={handleClick}>{label}</button>;
}
```


### **Git Commit**:
```bash
git add src/pages/products/
git commit -m "Implement functionality for Product List Page with mock data and prop drilling"
```

---

## **Step 3: Implement Product Display Page Functionality**

### **Instructions**:
1. Use `fetchProduct(id)` to load mock product data by ID.
2. Pass `addToCart` into the button in the `ProductCard` component using **prop drilling**.

### **Git Commit**:
```bash
git add src/pages/products/[id].jsx
git commit -m "Implement functionality for Product Display Page with addToCart prop drilling"
```

---

## **Step 4: Implement Shopping Cart Page Functionality**

### **Instructions**:
1. Replace the stub functions for `loadCart`, `addToCart`, `removeFromCart`, and `saveCartToLocalStorage` with real functionality.
2. Use mock cart data for testing.

### **Git Commit**:
```bash
git add src/pages/cart.jsx
git commit -m "Implement functionality for Shopping Cart Page with mock cart data"
```

---

## **Step 5: Implement Checkout Page Functionality**

### **Instructions**:
1. Add functionality to collect checkout information and save it to localStorage.
2. Use a mock JSON object for order details.

### **Git Commit**:
```bash
git add src/pages/checkout.jsx
git commit -m "Implement functionality for Checkout Page with mock order data"
```

---

## **Step 6: Implement Login and Register Pages**

### **Instructions**:
1. Replace the stub functions in `registerUser` and `signInUser` with functionality to save user data to localStorage.
2. Validate user credentials on the login page using mock data.

### **Git Commit**:
```bash
git add src/pages/signup.jsx src/pages/login.jsx
git commit -m "Implement functionality for Login and Register Pages with localStorage"
```

---

## **Optional: Implement CRUD Operations for Products**

### **Pages to Update**:
1. **Product Create Page**:
   - Implement `createProduct(productData)` to add a product to mock data.
2. **Product Update Page**:
   - Implement `updateProduct(product)` to modify an existing product in mock data.
   - Include a delete confirmation popup for `deleteProduct(product)`.

### **Git Commit**:
```bash
git add src/pages/admin/
git commit -m "Implement CRUD operations for Product Create and Update Pages"
```

---

## **Utility Functions in `/src/util/index.js`**

### **Shared Functions**:
1. `loadProductsFromLocalStorage()`: Returns products.
2. `loadCartFromLocalStorage()`: Returns the cart.
3. `saveCartToLocalStorage(cart)`: Saves cart to localStorage.
4. `saveProductsToLocalStorage(products)`: Saves products to localStorage.
5. `saveUserToLocalStorage(user)`: Saves user to localStorage.
6. `loginUserToLocalStorage(user)`: Saves logged-in user to localStorage.

### **Git Commit**:
```bash
git add src/util/index.js
git commit -m "Add utility functions for handling localStorage operations"
```

---

## **Final Steps: Review and Push**
1. Test all pages and functionality.
2. Push changes to your Git repository.

### **Git Push**:
```bash
git push origin main
```

### Submit Your Work

Submit the GitHub repository link for the project. Be sure to include the link to the deployed app on Vercel.

Check that your deployment on Vercel still works. If your deploy stops working, take a screenshot of the error message on Vercel and submit it with your repository link. Give a brief description of the work that you completed after the deploy stopped working.

---

By the end of Day 3, your Coffee Shop frontend will have functional mock interactions, a dynamic UI, and persistent mock data via localStorage. This foundation prepares you for future lessons on backend integration.

---

## **Appendix: Mock Data and Utility Functions**

Examples of utility functions:
```javascript
// src/util/index.js
export const loadProductsFromLocalStorage = () => {
  const products = localStorage.getItem('products');
  return products ? JSON.parse(products) : [];
};

export const loadCartFromLocalStorage = () => {
  const cart = localStorage.getItem('cart');
  return cart ? JSON.parse(cart) : [];
};

export const saveCartToLocalStorage = (cart) => {
  localStorage.setItem('cart', JSON.stringify(cart));
};

export const saveProductsToLocalStorage = (products) => {
  localStorage.setItem('products', JSON.stringify(products));
};

export const saveUserToLocalStorage = (user) => {
  localStorage.setItem('user', JSON.stringify(user));
};

export const loginUserToLocalStorage = (user) => {
  localStorage.setItem('loggedInUser', JSON.stringify(user));
};

import CartData from '@/mocks/cart.json';

export function populateCartFromMockData() {
  if (!loadCartFromLocalStorage()){
    saveCartToLocalStorage(CartData);
  }
}

import ProductData from '@/mocks/products.json';

export function populateProductsFromMockData() {
  if (!loadProductsFromLocalStorage()){
    saveProductsToLocalStorage(ProductData);
  }
}
```

Create a new file named `util/mockData.js` and add the following code:
```javascript
// mockData.js
import CartData from '@/mocks/cart.json';
import ProductData from '@/mocks/products.json';
import { loadCartFromLocalStorage, loadProductsFromLocalStorage, saveCartToLocalStorage, saveProductsToLocalStorage } from './util'

if(!loadCartFromLocalStorage()){
  saveCartToLocalStorage(CartData);
}

if(!loadProductsFromLocalStorage()){
  saveProductsToLocalStorage(ProductData);
}
```

In one of your pages, (for example `index.js`), simply import the mockData file to populate the localStorage with mock data. Remember to remove the import once you have connected to a real backend:

```javascript
// index.js
import '@/util/mockData';
```

---

Examples of using utility functions:
```javascript
// src/pages/cart.jsx
import cartData from '@/mocks/cart.json';
import { loadCartFromLocalStorage, saveCartToLocalStorage } from '@/util';

export default function CartPage() {
  const [cart, setCart] = useState([]);

  const loadCart = () => {
    const cart = loadCartFromLocalStorage();
    setCart(cart);
  };

  const addToCart = (product) => {
    const updatedCart = [...cart, product];
    setCart(updatedCart);
    saveCartToLocalStorage(updatedCart);
  };

  const removeFromCart = (product) => {
    const updatedCart = cart.filter((item) => item.id !== product.id);
    setCart(updatedCart);
    saveCartToLocalStorage(updatedCart);
  };

  useEffect(() => {
    loadCart();
  }, []);
}
```
