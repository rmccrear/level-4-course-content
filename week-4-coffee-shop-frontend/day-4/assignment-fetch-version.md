# Day 4 Guide: Connecting to the Coffee Shop Backend (fetch version)

## Objective:  
Integrate the Next.js Coffee Shop frontend with the backend API as defined by the Swagger documentation. You will use **Fetch API** for HTTP requests and secure the backend URL in a `.env` file.

---

## **Step 1: Set Up the Backend URL**

### **Instructions**:

1. Find the `.env.local` file in the root of your project.
2. Add the backend base URL from the Swagger documentation:
   ```plaintext
   NEXT_PUBLIC_API_BASE_URL=https://coffee-shop-backend-12345.render.com-or-whatever-your-url-is
   ```

---

## **Step 2: Fetch Data from the Backend**

### **Instructions**:

We will first use the fetch API to replace the mock data in `/products/index.js` with real data from the backend.

1. Open `/pages/products/index.js`.

It should look something like this.

```jsx
import ProductCard from '../components/ProductCard';
import products from '../mocks/products.json';
import '@/styles/products.css';

export default function ProductsPage() {
  const productsJSX = products.map((product) => {
    return (<ProductCard key={product._id} product={product} />)
  });
  return (
    <div className="products-grid">
      {productsJSX}
    </div>
  );
}
```

2. Replace the `products` import with a fetch request to the backend wrapped in a useEffect. The `useEffect` hook with a `[]` dependency array will run the fetch request whenever the component mounts.

```jsx

import ProductCard from '../components/ProductCard';

import { useEffect, useState } from 'react';

import '@/styles/products.css';

export default function ProductsPage() {
  const [products, setProducts] = useState([]);

  useEffect(() => {
    async function fetchProducts() {
      const res = await fetch(`${process.env.NEXT_PUBLIC_API_BASE_URL}/products`);
      const data = await res.json();
      setProducts(data);
    }
    fetchProducts();
  }, []);

  const productsJSX = products.map((product) => {
    return (<ProductCard key={product._id} product={product} />)
  });

  return (
    <div className="products-grid">
      {productsJSX}
    </div>
  );
}
```

---

## **Step 3: Test the Integration**

### **Instructions**:

1. Start the frontend server with `npm run dev`.
2. Open the browser and navigate to `http://localhost:3000/products`.
3. You should see the products fetched from the backend displayed on the page.

---

## **Step 4: GET a Single Product**

You challenge is to fetch a single product from the backend and display it on the page.

Start with `/pages/products/[id].js`.

Follow the same steps as above to fetch the product data for a single from the backend.

---