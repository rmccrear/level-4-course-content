# Day 1: Setting Up the Frontend and Basic Components

## Introduction

Today, we will set up our Next.js frontend project for the coffee shop e-commerce site. We will create basic components, set up routing, and clear out the default styles.

## Steps

1. **Set Up a New Next.js Project**

   - Initialize a new Next.js project with Tailwind CSS enabled.

   ```bash
   npx create-next-app@latest coffee-shop --tailwind
   cd coffee-shop
   ```

2. **Clear Out Default Styles**

   - Remove default fonts and styles from `layout.js`:

   ```jsx
   // app/layout.js
   export const metadata = {
     title: 'Coffee Shop',
     description: 'Best coffee products online',
   };

   export default function RootLayout({ children }) {
     return (
       <html lang="en">
         <body>{children}</body>
       </html>
     );
   }
   ```

   - Clear out styles from `globals.css`:

   ```css
   /* globals.css */
   @tailwind base;
   @tailwind components;
   @tailwind utilities;
   ```

   - Clear out styles from `page.js`:

   ```jsx
   // app/page.js
   export default function HomePage() {
     return (
       <div>
         <h1 className="text-2xl font-bold">Welcome to the Coffee Shop</h1>
         <p>
           Discover our amazing products and enjoy a great shopping experience.
         </p>
       </div>
     );
   }
   ```

3. **Create Basic Components**

### 3.1. Create Header Component

- Create a new file named `Header.jsx` in the `components` directory and input the following content:

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
          </ul>
        </nav>
      </header>
    );
  };

  export default Header;
  ```

### 3.2. Create Footer Component

- Create a new file named `Footer.jsx` in the `components` directory and input the following content:

  ```jsx
  // components/Footer.jsx
  const Footer = () => {
    return (
      <footer className="bg-gray-800 text-white p-4 mt-8">
        <p className="text-center">
          &copy; 2024 Coffee Shop. All rights reserved.
        </p>
      </footer>
    );
  };

  export default Footer;
  ```

### 3.3. Create Layout Component

- Update the `layout.js` file to use the `Header` and `Footer` components:

  ```jsx
  // app/layout.js
  import Header from '../components/Header';
  import Footer from '../components/Footer';

  export const metadata = {
    title: 'Coffee Shop',
    description: 'Best coffee products online',
  };

  export default function RootLayout({ children }) {
    return (
      <html lang="en">
        <body>
          <Header />
          <main className="container mx-auto p-4">{children}</main>
          <Footer />
        </body>
      </html>
    );
  }
  ```

4. **Set Up Routing and Basic Pages**

### 4.1. Create Home Page

- Update the `page.js` file to include a welcome message:

  ```jsx
  // app/page.js
  export default function HomePage() {
    return (
      <div>
        <h1 className="text-2xl font-bold">Welcome to the Coffee Shop</h1>
        <p>
          Discover our amazing products and enjoy a great shopping experience.
        </p>
      </div>
    );
  }
  ```

### 4.2. Create Products Page

- Create a new directory `products` with an `index.js` file in the `app` directory and input the following content:

  ```jsx
  // app/products/page.js
  export default function ProductsPage() {
    return (
      <div>
        <h1 className="text-2xl font-bold">Products</h1>
        <p>Explore our range of coffee products.</p>
      </div>
    );
  }
  ```

### 4.3. Create About and Contact Pages

- Create `about` and `contact` directories with a `page.js` file in the `app` directory:

  ```jsx
  // app/about/page.js
  export default function AboutPage() {
    return (
      <div>
        <h1 className="text-2xl font-bold">About Us</h1>
        <p>Learn more about our coffee shop and our story.</p>
      </div>
    );
  }

  // app/contact/page.js
  export default function ContactPage() {
    return (
      <div>
        <h1 className="text-2xl font-bold">Contact Us</h1>
        <p>Get in touch with us for any queries or feedback.</p>
      </div>
    );
  }
  ```

## Conclusion

We have set up the basic structure of our Next.js frontend application, created key components, and established initial routing. Tomorrow, we will start integrating our API to display products and handle user interactions.

## Instructions

Work on replicating the steps demonstrated during the session. Create the necessary components and pages to match the structure we have built today.
