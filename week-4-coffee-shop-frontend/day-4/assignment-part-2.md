# Updated Guide: Handling Authentication Tokens

In this update, you will enhance the **register** and **login** functions to store authentication tokens in `localStorage` and automatically include them in subsequent Axios requests.

---

## **Step 1: Update Register and Login Functions**

### **Instructions**:
1. Modify the `/src/util/api.js` file:
   - After a successful `registerUser` or `loginUser` call, save the returned token to `localStorage`.
   - Update the Axios instance to include the token in the `Authorization` header.

### **Code Update**:
```javascript
import api from './axiosInstance';

// Authentication
export const registerUser = async (userData) => {
  const { data } = await api.post('/auth/register', userData);
  localStorage.setItem('authToken', data.token); // Save token to localStorage
  return data;
};

export const loginUser = async (credentials) => {
  const { data } = await api.post('/auth/login', credentials);
  localStorage.setItem('authToken', data.token); // Save token to localStorage
  return data;
};
```

---

## **Step 2: Add Token to Axios Requests**

### **Instructions**:
1. Modify `/src/util/axiosInstance.js` to include the token in the `Authorization` header.
2. Use Axios interceptors to dynamically add the token to all requests.

### **Code Update**:
```javascript
import axios from 'axios';

const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_BASE_URL,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Add interceptor to include token in Authorization header
api.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('authToken'); // Get token from localStorage
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

export default api;
```

---

## **Step 3: Test Authentication**

### **Register Page**:
1. Update `/src/pages/signup.jsx` to use the updated `registerUser` function:
   ```javascript
   import { useState } from 'react';
   import { registerUser } from '../../util/api';

   export default function SignupPage() {
     const [formData, setFormData] = useState({ name: '', email: '', password: '' });

     const handleSubmit = async (e) => {
       e.preventDefault();
       try {
         const response = await registerUser(formData);
         console.log('Registration successful:', response);
       } catch (error) {
         console.error('Error registering user:', error);
       }
     };

     return (
       <form onSubmit={handleSubmit}>
         <input
           type="text"
           placeholder="Name"
           value={formData.name}
           onChange={(e) => setFormData({ ...formData, name: e.target.value })}
         />
         <input
           type="email"
           placeholder="Email"
           value={formData.email}
           onChange={(e) => setFormData({ ...formData, email: e.target.value })}
         />
         <input
           type="password"
           placeholder="Password"
           value={formData.password}
           onChange={(e) => setFormData({ ...formData, password: e.target.value })}
         />
         <button type="submit">Register</button>
       </form>
     );
   }
   ```

### **Login Page**:
1. Update `/src/pages/login.jsx` to use the updated `loginUser` function:
   ```javascript
   import { useState } from 'react';
   import { loginUser } from '../../util/api';

   export default function LoginPage() {
     const [formData, setFormData] = useState({ email: '', password: '' });

     const handleSubmit = async (e) => {
       e.preventDefault();
       try {
         const response = await loginUser(formData);
         console.log('Login successful:', response);
       } catch (error) {
         console.error('Error logging in:', error);
       }
     };

     return (
       <form onSubmit={handleSubmit}>
         <input
           type="email"
           placeholder="Email"
           value={formData.email}
           onChange={(e) => setFormData({ ...formData, email: e.target.value })}
         />
         <input
           type="password"
           placeholder="Password"
           value={formData.password}
           onChange={(e) => setFormData({ ...formData, password: e.target.value })}
         />
         <button type="submit">Login</button>
       </form>
     );
   }
   ```

---

## **Step 4: Verify Token Usage**

### **Testing**:
1. **Register** a new user:
   - Ensure the token is stored in `localStorage`.
2. **Login** with an existing user:
   - Confirm the token is updated in `localStorage`.
3. Check any other Axios requests (e.g., fetching products) to ensure the `Authorization` header includes the token.

---

## **Step 5: Git Commit**

### **Commit Updated Code**:
```bash
git add src/util/ src/pages/signup.jsx src/pages/login.jsx
git commit -m "Store auth token in localStorage and include it in Axios requests"
```

---

## **Step 6: Push Changes**

### **Git Push**:
```bash
git push origin main
```

---

By following this update, your application will securely handle authentication tokens, allowing authenticated requests to the backend while ensuring the token is properly included in every request.