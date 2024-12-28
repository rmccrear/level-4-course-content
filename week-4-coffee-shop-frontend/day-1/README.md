# Coffee Shop Frontend

## **Project Overview**

A modern, styled, and component-driven frontend for a coffee shop application.

---

## **1. Objective**

Provide a foundation for the Coffee Shop frontend using **Next.js**, **Storybook**, **Tailwind CSS with DaisyUI**, and backend integration. The project employs best practices for rapid development and scalability.

---

## **2. Features**

- Next.js framework with Tailwind CSS.
- Component-driven development using Storybook.
- Fully customizable theme with DaisyUI.
- Backend connectivity via `.env` configuration.
- Prebuilt components such as Navbar, Footer, and ProductCard.
- Testing environment for rapid UI iteration.

---

## **3. Prerequisites**

- Node.js v14 or later.
- npm v6 or later.
- Git installed locally.

---

## **4. Setup Instructions**

### 4.1 Clone Repository  
```bash
git clone <your-repo-url>
cd coffee-shop-frontend
```

### 4.2 Install Dependencies  
```bash
npm install
```

### 4.3 Environment Configuration  
Create a `.env.local` file:  
```plaintext
NEXT_PUBLIC_API_BASE_URL=http://localhost:3000
NEXT_PUBLIC_API_BASE_URL_PROD=https://api.onrender.com
```

### 4.4 Run Development Server  
```bash
npm run dev
```  
Access the app at [http://localhost:3000](http://localhost:3000).

**Screenshot Placeholder**  
*(Add a screenshot of the running application)*  

---

## **5. Storybook Setup**

### 5.1 Start Storybook  
```bash
npm run storybook
```  
Access Storybook at [http://localhost:6006](http://localhost:6006).

**Screenshot Placeholder**  
*(Add a screenshot of Storybook's running interface)*  

### 5.2 Test Components  
1. Add a new component in `/components/`.
2. Create stories in `/components/*.stories.js`.
3. Verify in Storybook.

**Screenshot Placeholder**  
*(Add a screenshot of a sample component displayed in Storybook)*  

---

## **6. DaisyUI Integration**

### 6.1 Install DaisyUI  
```bash
npm install daisyui
```

### 6.2 Configure Tailwind CSS  
Update `tailwind.config.js`:  
```javascript
module.exports = {
  content: ["./pages/**/*.{js,ts,jsx,tsx}", "./components/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [require("daisyui")],
};
```

**Screenshot Placeholder**  
*(Add a screenshot of DaisyUI applied to a component)*  

### 6.3 Apply Custom Theme  
Add custom theme in `tailwind.config.js`:  
```javascript
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
};
```

**Screenshot Placeholder**  
*(Add a screenshot of the theme applied to your app interface)*  

---

## **7. Component Architecture**

### Core Components  
- Navbar
- Footer
- Button
- Loader
- Toast
- ProductCard
- CartItem
- CartSummary

### Example: Navbar Component  
```javascript
// components/Navbar.js
export default function Navbar() {
  return <div className="navbar">Navbar Component</div>;
}
```

**Screenshot Placeholder**  
*(Add a screenshot of the Navbar component rendered in the browser)*  

---

## **8. Backend Connectivity**

### Test Connection with Backend  
Example component:  
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

**Screenshot Placeholder**  
*(Add a screenshot of the console log showing backend connectivity)*  

---

## **9. Testing**

1. Verify all components in Storybook.
2. Confirm app functionality at [http://localhost:3000](http://localhost:3000).
3. Check backend response in browser console.

**Screenshot Placeholder**  
*(Add a screenshot of the final application with all integrated features)*  

---

## **10. Customization**

### Custom CSS Example  
```css
/* styles/navbar.css */
.navbar {
  background-color: #6b4f4f;
  color: white;
}
```

Import in `Navbar.js`:  
```javascript
import '../styles/navbar.css';
```

**Screenshot Placeholder**  
*(Add a screenshot showing the applied custom styling to Navbar)*  

---

## **11. Contributions**

- Fork the repository.
- Create a feature branch: `git checkout -b feature-name`.
- Commit changes: `git commit -m "Description of changes"`.
- Push branch: `git push origin feature-name`.
- Submit a Pull Request.

---