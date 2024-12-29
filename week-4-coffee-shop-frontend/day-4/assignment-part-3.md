# Protecting Routes and Redirecting If Not Logged In

To protect routes and ensure users are redirected if they are not logged in, we will create a **Route Guard**. This guard checks if the user is authenticated (i.e., a token exists in `localStorage`) before allowing access to protected pages. If the user is not authenticated, they will be redirected to the login page.

---

## **Step 1: Create a Higher-Order Component (HOC) for Route Protection**

1. Create a new file: `/src/components/ProtectedRoute.jsx`.
2. Add the following code to check authentication and handle redirection:

```javascript
import { useEffect } from 'react';
import { useRouter } from 'next/router';

const ProtectedRoute = ({ children }) => {
  const router = useRouter();

  useEffect(() => {
    const token = localStorage.getItem('authToken');
    if (!token) {
      router.push('/login'); // Redirect to login if no token is found
    }
  }, [router]);

  return <>{children}</>;
};

export default ProtectedRoute;
```

---

## **Step 2: Wrap Protected Pages with the HOC**

1. Go to the protected pages you want to secure (e.g., `/cart.jsx`, `/checkout.jsx`).
2. Wrap the page content in the `ProtectedRoute` component.

### Example: Protecting the Cart Page
Update `/src/pages/cart.jsx`:
```javascript
import ProtectedRoute from '../components/ProtectedRoute';

export default function CartPage() {
  return (
    <ProtectedRoute>
      <div>Your Cart</div>
      {/* Add your cart content here */}
    </ProtectedRoute>
  );
}
```

---

## **Step 3: Create a Reusable Authentication Hook**

To simplify the process of checking if a user is authenticated, create a hook.

1. Create `/src/hooks/useAuth.js`:
   ```javascript
   import { useState, useEffect } from 'react';

   const useAuth = () => {
     const [isAuthenticated, setIsAuthenticated] = useState(false);

     useEffect(() => {
       const token = localStorage.getItem('authToken');
       setIsAuthenticated(!!token); // Set true if token exists
     }, []);

     return isAuthenticated;
   };

   export default useAuth;
   ```

2. Update `ProtectedRoute` to use this hook:
   ```javascript
   import { useEffect } from 'react';
   import { useRouter } from 'next/router';
   import useAuth from '../hooks/useAuth';

   const ProtectedRoute = ({ children }) => {
     const router = useRouter();
     const isAuthenticated = useAuth();

     useEffect(() => {
       if (!isAuthenticated) {
         router.push('/login'); // Redirect to login if not authenticated
       }
     }, [isAuthenticated, router]);

     if (!isAuthenticated) return null; // Optionally render nothing while redirecting

     return <>{children}</>;
   };

   export default ProtectedRoute;
   ```

---

## **Step 4: Redirect If Already Logged In**

1. Add logic to prevent logged-in users from accessing the login and signup pages.

### Example: Redirect on the Login Page
Update `/src/pages/login.jsx`:
```javascript
import { useEffect } from 'react';
import { useRouter } from 'next/router';

export default function LoginPage() {
  const router = useRouter();

  useEffect(() => {
    const token = localStorage.getItem('authToken');
    if (token) {
      router.push('/'); // Redirect to home if already logged in
    }
  }, [router]);

  const handleLogin = async (e) => {
    e.preventDefault();
    // Handle login logic
  };

  return (
    <form onSubmit={handleLogin}>
      <input type="email" placeholder="Email" />
      <input type="password" placeholder="Password" />
      <button type="submit">Login</button>
    </form>
  );
}
```

---

## **Step 5: Test the Route Protection**

1. **Test without a token**:
   - Navigate to a protected route (e.g., `/cart`).
   - Verify you are redirected to `/login`.

2. **Test with a token**:
   - Login to generate a token in `localStorage`.
   - Navigate to the protected route and confirm access.

3. **Test redirect from `/login` or `/signup`**:
   - With a token in `localStorage`, navigate to `/login` or `/signup`.
   - Verify you are redirected to `/`.

---

## **Step 6: Git Commit**

### **Commit Updated Code**:
```bash
git add src/components/ProtectedRoute.jsx src/hooks/useAuth.js src/pages/cart.jsx src/pages/login.jsx
git commit -m "Add route protection and redirection for authenticated users"
```

---

## **Step 7: Push Changes**

### **Git Push**:
```bash
git push origin main
```

---

By following these steps, your Coffee Shop frontend will enforce route protection, ensuring only authenticated users can access specific pages while redirecting unauthenticated users to the login page. 🎉