# Day 2: Integrating the Backend API

## Introduction

Today, we will integrate our frontend application with the backend API we deployed to Render.com. We will fetch products from the API and display them on the Products page.

## Steps

1. **Set Up Axios for API Requests**

   - Install Axios if you haven't already:

   ```bash
   npm install axios
   ```

2. **Create API Utility**

### 2.1. Create api.js

- Create a new file named `api.js` in the `lib` directory and input the following content:

  ```jsx
  // lib/api.js
  import axios from 'axios';

  const api = axios.create({
    baseURL: 'https://your-api-url.onrender.com',
  });

  export const getProducts = async () => {
    try {
      const response = await api.get('/products');
      return response.data;
    } catch (error) {
      console.error('Error fetching products:', error);
      throw error;
    }
  };
  ```

3. **Fetch Products and Display Them**

### 3.1. Update Products Page

- Update the `ProductsPage` to fetch and display products:

  ```jsx
  // app/products/page.js
  import { useEffect, useState } from 'react';
  import { getProducts } from '../../lib/api';

  export default function ProductsPage() {
    const [products, setProducts] = useState([]);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
      async function fetchProducts() {
        try {
          const data = await getProducts();
          setProducts(data);
        } catch (error) {
          console.error('Failed to fetch products:', error);
        } finally {
          setLoading(false);
        }
      }

      fetchProducts();
    }, []);

    if (loading) {
      return <p>Loading...</p>;
    }

    return (
      <div>
        <h1 className="text-2xl font-bold">Products</h1>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
          {products.map((product) => (
            <div
              key={product._id}
              className="border p-4 rounded"
            >
              <h2 className="text-xl font-semibold">{product.name}</h2>
              <p>{product.description}</p>
              <p className="text-lg font-bold">${product.price}</p>
            </div>
          ))}
        </div>
      </div>
    );
  }
  ```

4. **Styling the Product Cards**

### 4.1. Add Tailwind Classes

- Add basic Tailwind CSS styles to the product cards:

  ```jsx
  // app/products/page.js (Updated)
  import { useEffect, useState } from 'react';
  import { getProducts } from '../../lib/api';

  export default function ProductsPage() {
    const [products, setProducts] = useState([]);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
      async function fetchProducts() {
        try {
          const data = await getProducts();
          setProducts(data);
        } catch (error) {
          console.error('Failed to fetch products:', error);
        } finally {
          setLoading(false);
        }
      }

      fetchProducts();
    }, []);

    if (loading) {
      return <p>Loading...</p>;
    }

    return (
      <div>
        <h1 className="text-2xl font-bold mb-4">Products</h1>
        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
          {products.map((product) => (
            <div
              key={product._id}
              className="border p-4 rounded shadow-sm hover:shadow-lg transition-shadow"
            >
              <h2 className="text-xl font-semibold">{product.name}</h2>
              <p>{product.description}</p>
              <p className="text-lg font-bold">${product.price}</p>
            </div>
          ))}
        </div>
      </div>
    );
  }
  ```

## Conclusion

We have successfully integrated our frontend application with the backend API. We can now fetch and display products on the Products page. Tomorrow, we will add more features to our application, such as product detail pages and user authentication.

## Instructions

Work on replicating the steps demonstrated during the session. Ensure that you can fetch products from the API and display them on the Products page.
