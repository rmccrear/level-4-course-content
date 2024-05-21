# Day 3: Finishing the Application

## Introduction

Today, we will finish our coffee shop e-commerce application by adding cart functionality, user registration, and refining the user interface.

## Steps

1. **Implement Cart Functionality**

### 1.1. Create Cart Context

- Create a new file named `CartContext.js` in the `context` directory and input the following content:

  ```jsx
  // context/CartContext.js
  import { createContext, useContext, useState } from 'react';

  const CartContext = createContext();

  export function CartProvider({ children }) {
    const [cart, setCart] = useState([]);

    const addToCart = (product) => {
      setCart((prevCart) => [...prevCart, product]);
    };

    const removeFromCart = (productId) => {
      setCart((prevCart) =>
        prevCart.filter((product) => product._id !== productId),
      );
    };

    return (
      <CartContext.Provider value={{ cart, addToCart, removeFromCart }}>
        {children}
      </CartContext.Provider>
    );
  }

  export function useCart() {
    return useContext(CartContext);
  }
  ```

### 1.2. Wrap Application with CartProvider

- Update the `layout.js` file to wrap the application with `CartProvider`:

  ```jsx
  // app/layout.js
  import Header from '../components/Header';
  import Footer from '../components/Footer';
  import { CartProvider } from '../context/CartContext';

  export const metadata = {
    title: 'Coffee Shop',
    description: 'Best coffee products online',
  };

  export default function RootLayout({ children }) {
    return (
      <html lang="en">
        <body>
          <CartProvider>
            <Header />
            <main className="container mx-auto p-4">{children}</main>
            <Footer />
          </CartProvider>
        </body>
      </html>
    );
  }
  ```

### 1.3. Add Cart Button to Product Cards

- Update the `ProductsPage` to include an "Add to Cart" button:

  ```jsx
  // app/products/page.js
  import { useEffect, useState } from 'react';
  import { getProducts } from '../../lib/api';
  import { useCart } from '../../context/CartContext';

  export default function ProductsPage() {
    const [products, setProducts] = useState([]);
    const [loading, setLoading] = useState(true);
    const { addToCart } = useCart();

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
              <button
                className="mt-2 bg-blue-500 text-white py-1 px-4 rounded"
                onClick={() => addToCart(product)}
              >
                Add to Cart
              </button>
            </div>
          ))}
        </div>
      </div>
    );
  }
  ```

2. **Implement User Registration**

### 2.1. Create Register Page

- Create a new directory `register` with a `page.js` file in the `app` directory and input the following content:

  ```jsx
  // app/register/page.js
  import { useState } from 'react';
  import axios from 'axios';

  export default function RegisterPage() {
    const [email, setEmail] = useState('');
    const [password, setPassword] = useState('');

    const handleSubmit = async (e) => {
      e.preventDefault();
      try {
        await axios.post('https://your-api-url.onrender.com/auth/register', {
          email,
          password,
        });
        alert('Registration successful');
      } catch (error) {
        console.error('Error registering:', error);
        alert('Failed to register');
      }
    };

    return (
      <div className="max-w-md mx-auto mt-8">
        <h1 className="text-2xl font-bold mb-4">Register</h1>
        <form onSubmit={handleSubmit}>
          <div className="mb-4">
            <label
              className="block text-sm font-medium mb-1"
              htmlFor="email"
            >
              Email
            </label>
            <input
              className="w-full px-3 py-2 border rounded"
              type="email"
              id="email"
              value={email}
              onChange={(e) => setEmail(e.target.value)}
              required
            />
          </div>
          <div className="mb-4">
            <label
              className="block text-sm font-medium mb-1"
              htmlFor="password"
            >
              Password
            </label>
            <input
              className="w-full px-3 py-2 border rounded"
              type="password"
              id="password"
              value={password}
              onChange={(e) => setPassword(e.target.value)}
              required
            />
          </div>
          <button
            className="w-full bg-blue-500 text-white py-2 rounded"
            type="submit"
          >
            Register
          </button>
        </form>
      </div>
    );
  }
  ```

3. **Create Cart Page**

### 3.1. Create Cart Page

- Create a new directory `cart` with a `page.js` file in the `app` directory and input the following content:

  ```jsx
  // app/cart/page.js
  import { useCart } from '../../context/CartContext';

  export default function CartPage() {
    const { cart, removeFromCart } = useCart();

    if (cart.length === 0) {
      return <p>Your cart is empty.</p>;
    }

    return (
      <div>
        <h1 className="text-2xl font-bold mb-4">Your Cart</h1>
        <div className="grid grid-cols-1 gap-4">
          {cart.map((product) => (
            <div
              key={product._id}
              className="border p-4 rounded shadow-sm hover:shadow-lg transition-shadow"
            >
              <h2 className="text-xl font-semibold">{product.name}</h2>
              <p>{product.description}</p>
              <p className="text-lg font-bold">${product.price}</p>
              <button
                className="mt-2 bg-red-500 text-white py-1 px-4 rounded"
                onClick={() => removeFromCart(product._id)}
              >
                Remove from Cart
              </button>
            </div>
          ))}
        </div>
      </div>
    );
  }
  ```

### 3.2. Add Link to Cart in Header

- Update the `Header` component to include a link to the cart page:

  ```jsx
  // components/Header.jsx
  import Link from 'next/link';

  const Header = () => {
    return (
      <header className="bg-gray-800 text-white p-4">
        <nav className="container mx-auto flex justify-between">
          <h1 className="text-xl font-bold">Coffee Shop</h1>
          <ul className="flex space-x-4">
            <li>
              <Link href="/">Home</Link>
            </li>
            <li>
              <Link href="/products">Products</Link>
            </li>
            <li>
              <Link href="/about">About</Link>
            </li>
            <li>
              <Link href="/contact">Contact</Link>
            </li>
            <li>
              <Link href="/cart">Cart</Link>
            </li>
            <li>
              <Link href="/login">Login</Link>
            </li>
            <li>
              <Link href="/register">Register</Link>
            </li>
          </ul>
        </nav>
      </header>
    );
  };

  export default Header;
  ```

## Conclusion

We have successfully implemented cart functionality, user registration, and refined our user interface. Tomorrow, we will prepare for presentations and ensure everything is ready for deployment.

## Instructions

Work on replicating the steps demonstrated during the session. Ensure that you can register, log in, add products to the cart, and view the cart.
