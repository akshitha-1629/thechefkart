# thechefkart
Nxtwave assignment

# TheChefkart Assignment Reference Document

## Party Menu Selection App with ReactJS

## 1. Project Overview

The goal of this assignment is to build a "Party Menu Selection App". This is a web application where a user can browse a categorized menu of dishes, filter them based on different criteria, and select their desired items for a party.

## 2. Core ReactJS Concepts

Before we start building, let's understand the basic building blocks of React.

### a. Components

Components are like independent, reusable building blocks for your UI. A React app is essentially a tree of components. For example, you can have a `SearchBar` component, a `DishCard` component, and a `Button` component. We will use **functional components**.

### b. JSX (JavaScript XML)

JSX allows you to write HTML-like code directly inside your JavaScript files. This makes it very easy to visualize the UI structure within the component's logic.

```
// This is JSX
function Greeting() {
  return <h1>Hello, World!</h1>;
}

```

### c. Props (Properties)

Props are used to pass data from a parent component to a child component. This is how components communicate with each other.

```
// Passing a 'name' prop to the Greeting component
<Greeting name="Alice" />

// Inside the Greeting component, we can access it
function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

```

### d. State (`useState` Hook)

State is data that a component "remembers" or manages. When the state of a component changes, React automatically re-renders the component to reflect those changes. The `useState` hook is how we add state to a functional component.

```
import React, { useState } from 'react';

function Counter() {
  // 'count' is our state variable, 'setCount' is the function to update it
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

```

### e. Conditional Rendering

This allows you to render different JSX elements based on certain conditions. You can use standard JavaScript `if` statements or the ternary operator for this.

```
function LoginButton({ isLoggedIn }) {
  if (isLoggedIn) {
    return <button>Logout</button>;
  } else {
    return <button>Login</button>;
  }
}

```

### f. Rendering Lists (`.map()`)

You can render a list of items by taking an array of data and mapping over it to return a JSX element for each item. It's crucial to provide a unique `key` prop for each item in the list.

```
function DishList({ dishes }) {
  return (
    <ul>
      {dishes.map(dish => (
        <li key={dish.id}>{dish.name}</li>
      ))}
    </ul>
  );
}

```

## 3. Project Setup

We will use **Create React App**, the standard way to set up a new single-page React application.

1. **Open your terminal or command prompt.**
2. **Run the following command:** This will create a new folder named `party-menu-app` with a complete React project setup.
    
    ```
    npx create-react-app party-menu-app
    
    ```
    
3. **Navigate into your new project folder:**
    
    ```
    cd party-menu-app
    
    ```
    
4. **Start the development server:**
    
    ```
    npm start
    
    ```
    
    This will open a new tab in your browser at `http://localhost:3000` showing your new React app.
    

## 4. Step-by-Step Implementation Guide

### Step 1: Create Your Project Structure

Inside the `src/` folder, create the following subfolders and files to keep your code organized:

```
party-menu-app/
└── src/
    ├── components/
    │   ├── DishCard.js
    │   ├── DishList.js
    │   ├── Filters.js
    │   └── IngredientModal.js
    ├── data/
    │   └── mockDishes.js
    ├── App.css
    └── App.js

```

### Step 2: Prepare Mock Data

Create the file `src/data/mockDishes.js`. Copy the JSON data from the provided assignment PDF and export it as a JavaScript array.

**`src/data/mockDishes.js`**

```
// A sample structure based on your PDF. Add all dishes here.
export const dishes = [
  {
    "id": 1,
    "name": "Kadhai Paneer 1",
    "description": "Paneer cubes in spicy onion gravy with onions and capsicum cubes.",
    "image": "https://placehold.co/300x200/F7D0B3/422402?text=Kadhai+Paneer",
    "mealType": "MAIN COURSE",
    "type": "VEG",
    "ingredients": [
        { "name": "Paneer", "quantity": "200g" },
        { "name": "Onion", "quantity": "2 large" },
        { "name": "Capsicum", "quantity": "1 large" },
        { "name": "Tomato Puree", "quantity": "1 cup" }
    ]
  },
  // ... add more dishes here for STARTER, DESSERT, SIDES and NON-VEG options
];

```

*Note: I've added a placeholder image URL and an `ingredients` array to the data structure to fulfill all requirements.*

### Step 3: Build the Main App Component (`App.js`)

This will be your main component that manages the overall state of the application.

**`src/App.js` Logic:**

1. **Import `useState` and your `dishes` data.**
2. **Create state variables:**
    - `selectedCategory`: To track the active tab ('STARTER', 'MAIN COURSE', etc.).
    - `searchTerm`: To hold the value from the search bar.
    - `vegOnly`: A boolean to track the veg filter.
    - `selectedDishes`: An array to store the IDs of the dishes the user has added.
    - `isModalOpen` and `currentDish`: To manage the ingredient details modal.
3. **Write the filtering logic:** Create a new variable (`filteredDishes`) that filters the main `dishes` array based on the current state of `selectedCategory`, `searchTerm`, and `vegOnly`.
4. **Render the child components:** Pass the necessary data and functions down to them as props.

### Step 4: Create the UI Components

### `Filters.js`

This component will contain the category tabs, search bar, and veg/non-veg toggle.

- **Props:** `activeCategory`, `onCategoryChange`, `searchTerm`, `onSearchChange`, `vegOnly`, `onVegOnlyChange`.
- **Functionality:**
    - Renders buttons for each category. The active one should have a different style. Clicking a button calls `onCategoryChange`.
    - Renders an `<input type="text">` for search. Its `onChange` event calls `onSearchChange`.
    - Renders a checkbox or toggle for the "Veg Only" filter.

### `DishList.js`

This component maps over the filtered dishes and renders a `DishCard` for each one.

- **Props:** `dishes`, `onAddDish`, `onRemoveDish`, `selectedDishes`, `onViewIngredients`.
- **Functionality:**
    - Takes the array of `filteredDishes`.
    - Uses `.map()` to render a `<DishCard>` for each dish.
    - Passes all necessary props down to each `DishCard`.

### `DishCard.js`

This component displays the information for a single dish.

- **Props:** `dish` (the dish object), `onAddDish`, `onRemoveDish`, `isSelected`, `onViewIngredients`.
- **Functionality:**
    - Displays the dish name, description, and image.
    - Has an "Add" or "Remove" button based on the `isSelected` prop. Clicking the button calls `onAddDish(dish.id)` or `onRemoveDish(dish.id)`.
    - Has an "Ingredients" button that calls `onViewIngredients(dish)`.

### `IngredientModal.js` (Replaces Ingredient Detail Screen)

For a web app, a modal is a better user experience than navigating to a new page for this feature.

- **Props:** `dish`, `onClose`.
- **Functionality:**
    - If `dish` is not null, it displays a modal overlay.
    - Shows the dish name, description, and a list of ingredients.
    - Has a "Close" button which calls the `onClose` function.

### Step 5: Add Styling (`App.css`)

Add some basic CSS to make the application look clean and modern. Use Flexbox or Grid for layout.

**Example `App.css` Snippets:**

```
body {
  font-family: Arial, sans-serif;
  background-color: #f4f4f4;
}

.app-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

.dish-card {
  border: 1px solid #ddd;
  border-radius: 8px;
  padding: 15px;
  margin: 10px;
  background-color: white;
  box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

.dish-list {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

```

## 5. Putting It All Together

Your `App.js` will orchestrate the entire application.

1. A user clicks a category tab in the `Filters` component.
2. The `Filters` component calls the `onCategoryChange` function passed from `App.js`.
3. `App.js` updates its `selectedCategory` state.
4. React re-renders `App.js`. The filtering logic runs again with the new category, creating a new `filteredDishes` array.
5. The `DishList` component receives the new array as a prop and re-renders, displaying only the dishes from the selected category.
6. The same process happens for search and the veg-only filter.
