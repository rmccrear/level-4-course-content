# Day 4 Guide: Connecting to the Coffee Shop Backend

## Objective:  
Integrate the Coffee Shop frontend with the backend API as defined by the Swagger documentation. Use **Axios** for HTTP requests, configure an **Axios instance**, and use **React Query** for efficient data fetching. Secure the backend URL in a `.env` file.

- [Axios and ReactQuery](https://www.youtube.com/watch?v=lLWfZL-Y8lM)

---

## **Step 1: Set Up the Backend URL**

### **Instructions**:
1. Create a `.env.local` file in the root of your project.
2. Add the backend base URL from the Swagger documentation:
   ```plaintext
   NEXT_PUBLIC_API_BASE_URL=http://localhost:3000
   ```
3. Restart your development server to ensure the environment variable is loaded.

---

## **Step 2: Install Required Dependencies**

### **Install Axios and React Query**:
```bash
npm install axios @tanstack/react-query
```

### **Git Commit**:
```bash
git add package.json package-lock.json
git commit -m "Install Axios and React Query for backend integration"
```

---

## **Step 3: Configure Axios Instance**

### **Instructions**:
1. Create `/src/util/axiosInstance.js`:
   ```javascript
   import axios from 'axios';

   const api = axios.create({
     baseURL: process.env.NEXT_PUBLIC_API_BASE_URL,
     headers: {
       'Content-Type': 'application/json',
     },
   });

   export default api;
   ```

### **Git Commit**:
```bash
git add src/util/axiosInstance.js
git commit -m "Configure Axios instance with baseURL from .env file"
```

---

## **Step 4: Set Up React Query**

### **Instructions**:
1. Wrap your app in a React Query provider in `_app.js`:
   ```javascript
   import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
   import '../styles/globals.css';

   const queryClient = new QueryClient();

   export default function MyApp({ Component, pageProps }) {
     return (
       <QueryClientProvider client={queryClient}>
         <Component {...pageProps} />
       </QueryClientProvider>
     );
   }
   ```

### **Git Commit**:
```bash
git add pages/_app.js
git commit -m "Set up React Query provider in _app.js"
```

---

## **Step 5: Implement API Functions**

### **Create a Utility File for API Calls**:
1. Create `/src/util/api.js` and implement API functions for the endpoints:

   ```javascript
   import api from './axiosInstance';

   // Authentication
   export const registerUser = (userData) => api.post('/auth/register', userData);
   export const loginUser = (credentials) => api.post('/auth/login', credentials);

   // Products
   export const fetchProducts = (params) => api.get('/products', { params });
   export const fetchProduct = (id) => api.get(`/products/${id}`);
   export const createProduct = (productData) => api.post('/products', productData);
   export const updateProduct = (id, productData) => api.put(`/products/${id}`, productData);
   export const deleteProduct = (id) => api.delete(`/products/${id}`);
   ```

### **Git Commit**:
```bash
git add src/util/api.js
git commit -m "Implement API utility functions for authentication and products"
```

---

## **Step 6: Fetch Products**

### **Instructions**:
1. Update `/src/pages/products/index.jsx`:
   ```javascript
   import { useQuery } from '@tanstack/react-query';
   import { fetchProducts } from '../../util/api';
   import ProductCard from '../../components/ProductCard';

   export default function ProductsPage() {
     const { data, isLoading, error } = useQuery(['products'], () => fetchProducts({ page: 1, limit: 10 }));

     if (isLoading) return <p>Loading...</p>;
     if (error) return <p>Error: {error.message}</p>;

     const { products } = data;

     return (
       <div className="products-grid">
         {products.map((product) => (
           <ProductCard key={product.id} product={product} />
         ))}
       </div>
     );
   }
   ```

2. Test by navigating to `/products` and confirming the data loads.

### **Git Commit**:
```bash
git add src/pages/products/index.jsx
git commit -m "Fetch and display products using React Query and backend integration"
```

---

## **Step 7: Fetch a Single Product**

### **Instructions**:
1. Update `/src/pages/products/[id].jsx`:
   ```javascript
   import { useQuery } from '@tanstack/react-query';
   import { fetchProduct } from '../../util/api';
   import ProductCard from '../../components/ProductCard';
   import { useRouter } from 'next/router';

   export default function ProductPage() {
     const router = useRouter();
     const { id } = router.query;

     const { data: product, isLoading, error } = useQuery(['product', id], () => fetchProduct(id), {
       enabled: !!id, // Ensure query only runs when `id` is available
     });

     if (isLoading) return <p>Loading...</p>;
     if (error) return <p>Error: {error.message}</p>;

     return <ProductCard product={product} />;
   }
   ```

2. Test by navigating to `/products/{id}`.

### **Git Commit**:
```bash
git add src/pages/products/[id].jsx
git commit -m "Fetch and display a single product by ID using React Query"
```

---

## **Step 8: Register and Login Users**

### **Register Page**:
1. Update `/src/pages/signup.jsx`:
   ```javascript
   import { registerUser } from '../../util/api';

   const handleRegister = async (userData) => {
     try {
       const response = await registerUser(userData);
       console.log('User registered:', response.data);
     } catch (error) {
       console.error('Error registering user:', error);
     }
   };
   ```

### **Login Page**:
1. Update `/src/pages/login.jsx`:
   ```javascript
   import { loginUser } from '../../util/api';

   const handleLogin = async (credentials) => {
     try {
       const response = await loginUser(credentials);
       console.log('User logged in:', response.data);
     } catch (error) {
       console.error('Error logging in:', error);
     }
   };
   ```

### **Git Commit**:
```bash
git add src/pages/signup.jsx src/pages/login.jsx
git commit -m "Integrate user registration and login with backend"
```

---

## **Step 9: Test All Pages**

1. Test the following features:
   - Product list loads from `/products`.
   - Single product details load from `/products/{id}`.
   - User registration and login work correctly with the backend.

2. Debug any issues.

---

## **Final Steps: Push Changes**

### **Git Push**:
```bash
git push origin main
```

---

By the end of Day 4, your Coffee Shop frontend will successfully integrate with the backend API, fetching and posting data seamlessly. Move on to the second part to implement user authentication and protected routes. 