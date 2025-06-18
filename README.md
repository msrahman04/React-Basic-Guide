# The React foundations link :
https://nextjs.org/learn/react-foundations

# What is React?
React is a JavaScript library for building interactive user interfaces.

# What is Next.js?
Next.js is a flexible React framework that gives you building blocks to create fast, full-stack web applications.

You can use React to build your UI, then incrementally adopt Next.js features to solve common application requirements such as routing, data fetching, and caching - all while improving the developer and end-user experience.


# What is the DOM?
The DOM is an object representation of the HTML elements. It acts as a bridge between your code and the user interface, and has a tree-like structure with parent and child relationships.

# What is JSX?
JSX is a syntax extension for JavaScript that allows you to describe your UI in a familiar HTML-like syntax. The nice thing about JSX is that apart from following three JSX rules, you don't need to learn any new symbols or syntax outside of HTML and JavaScript.

# What is react components?
A component is a function that returns UI elements. Inside the return statement of the function, you can write JSX:
User interfaces can be broken down into smaller building blocks called components.
Components allow you to build self-contained, reusable snippets of code. If you think of components as LEGO bricks, you can take these individual bricks and combine them together to form larger structures. If you need to update a piece of the UI, you can update the specific component or brick.
s
This modularity allows your code to be more maintainable as it grows because you can add, update, and delete components without touching the rest of our application.

The nice thing about React components is that they are just JavaScript.





# React Fundamentals Guide

## Table of Contents
- [Introduction](#introduction)
- [Components](#components)
- [Props](#props)
- [State](#state)
- [Hooks](#hooks)
- [Event Handling](#event-handling)
- [Conditional Rendering](#conditional-rendering)
- [Lists and Keys](#lists-and-keys)
- [Modern React Features](#modern-react-features)
- [Best Practices](#best-practices)

## Introduction

React is a JavaScript library for building user interfaces, particularly web applications. It uses a component-based architecture where UIs are built by composing reusable components. React follows a declarative paradigm - you describe what the UI should look like for any given state, and React handles updating the DOM when state changes.

## Components

Components are the building blocks of React applications. They are reusable pieces of UI that can accept inputs (props) and return JSX elements describing what should appear on the screen.

### Function Components

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Arrow function syntax
const Welcome = (props) => {
  return <h1>Hello, {props.name}!</h1>;
};
```

### Class Components (Legacy)

```jsx
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

**Note:** Function components are now the preferred approach as they're simpler and work better with React Hooks.

### JSX

JSX is a syntax extension that allows you to write HTML-like code in JavaScript:

```jsx
const element = <h1>Hello, world!</h1>;

// JSX with expressions
const name = 'John';
const element = <h1>Hello, {name}!</h1>;

// JSX with attributes
const element = <img src={user.avatarUrl} alt={user.name} />;
```

## Props

Props (short for properties) are how components receive data from their parent components. Props are read-only and should never be modified by the receiving component.

```jsx
// Parent component
function App() {
  return (
    <div>
      <Welcome name="Alice" age={25} />
      <Welcome name="Bob" age={30} />
    </div>
  );
}

// Child component
function Welcome(props) {
  return (
    <div>
      <h1>Hello, {props.name}!</h1>
      <p>You are {props.age} years old.</p>
    </div>
  );
}
```

### Destructuring Props

```jsx
// Instead of using props.name and props.age
function Welcome({ name, age }) {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>You are {age} years old.</p>
    </div>
  );
}
```

### Default Props

```jsx
function Welcome({ name = "Guest", age = 18 }) {
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>You are {age} years old.</p>
    </div>
  );
}
```

## State

State represents data that can change over time and affects what gets rendered. Unlike props, state is managed within the component and can be updated.

### useState Hook

```jsx
import { useState } from 'react';

function Counter() {
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

### State with Objects

```jsx
function UserProfile() {
  const [user, setUser] = useState({
    name: 'John',
    email: 'john@example.com',
    age: 25
  });

  const updateName = (newName) => {
    setUser(prevUser => ({
      ...prevUser,
      name: newName
    }));
  };

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
      <p>Age: {user.age}</p>
      <button onClick={() => updateName('Jane')}>
        Change Name
      </button>
    </div>
  );
}
```

## Hooks

Hooks are functions that let you use state and other React features in function components.

### useState

For managing component state:

```jsx
const [state, setState] = useState(initialValue);
```

### useEffect

For side effects like data fetching, subscriptions, or manually changing the DOM:

```jsx
import { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    // Effect runs after component mounts and when userId changes
    fetchUser(userId)
      .then(userData => {
        setUser(userData);
        setLoading(false);
      });
  }, [userId]); // Dependency array

  useEffect(() => {
    // Effect with cleanup
    const timer = setInterval(() => {
      console.log('Timer tick');
    }, 1000);

    return () => {
      clearInterval(timer); // Cleanup function
    };
  }, []); // Empty dependency array = runs once on mount

  if (loading) return <div>Loading...</div>;
  return <div>Welcome, {user.name}!</div>;
}
```

### useContext

For consuming context values:

```jsx
import { createContext, useContext } from 'react';

const ThemeContext = createContext();

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Header />
    </ThemeContext.Provider>
  );
}

function Header() {
  const theme = useContext(ThemeContext);
  return <h1 className={theme}>Welcome!</h1>;
}
```

### useReducer

For complex state logic:

```jsx
import { useReducer } from 'react';

const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      Count: {state.count}
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}
```

### Custom Hooks

Create reusable stateful logic:

```jsx
// Custom hook for fetching data
function useApi(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetch(url)
      .then(response => response.json())
      .then(data => {
        setData(data);
        setLoading(false);
      })
      .catch(error => {
        setError(error);
        setLoading(false);
      });
  }, [url]);

  return { data, loading, error };
}

// Using the custom hook
function UserList() {
  const { data: users, loading, error } = useApi('/api/users');

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

## Event Handling

React uses SyntheticEvents, which wrap native events and provide consistent behavior across browsers:

```jsx
function Button() {
  const handleClick = (event) => {
    event.preventDefault();
    console.log('Button clicked!');
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    const formData = new FormData(event.target);
    console.log(Object.fromEntries(formData));
  };

  return (
    <form onSubmit={handleSubmit}>
      <input name="username" type="text" />
      <button type="submit" onClick={handleClick}>
        Submit
      </button>
    </form>
  );
}
```

## Conditional Rendering

Different ways to conditionally render components:

```jsx
function Greeting({ isLoggedIn, user }) {
  // Using if statements
  if (isLoggedIn) {
    return <h1>Welcome back, {user.name}!</h1>;
  }
  return <h1>Please sign in.</h1>;

  // Using ternary operator
  return (
    <div>
      {isLoggedIn ? (
        <h1>Welcome back, {user.name}!</h1>
      ) : (
        <h1>Please sign in.</h1>
      )}
    </div>
  );

  // Using logical AND operator
  return (
    <div>
      {isLoggedIn && <h1>Welcome back, {user.name}!</h1>}
      {!isLoggedIn && <h1>Please sign in.</h1>}
    </div>
  );
}
```

## Lists and Keys

When rendering lists, each item needs a unique key:

```jsx
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <span className={todo.completed ? 'completed' : ''}>
            {todo.text}
          </span>
        </li>
      ))}
    </ul>
  );
}
```

**Important:** Keys should be stable, predictable, and unique. Avoid using array indices as keys when the list can change.

## Modern React Features

### Server Components

React Server Components allow components to render on the server, reducing bundle size and improving performance:

```jsx
// Server Component (runs on server)
async function BlogPost({ id }) {
  // This data fetching happens on the server
  const post = await fetchBlogPost(id);
  
  return (
    <article>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
      <Comments postId={id} /> {/* Client Component */}
    </article>
  );
}

// Client Component (runs in browser)
'use client';

function Comments({ postId }) {
  const [comments, setComments] = useState([]);
  
  useEffect(() => {
    fetchComments(postId).then(setComments);
  }, [postId]);

  return (
    <div>
      {comments.map(comment => (
        <div key={comment.id}>{comment.text}</div>
      ))}
    </div>
  );
}
```

### Suspense

Suspense lets you display a fallback UI while waiting for components to load:

```jsx
import { Suspense, lazy } from 'react';

// Lazy load components
const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <h1>My App</h1>
      <Suspense fallback={<div>Loading component...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}
```

### Suspense with Data Fetching

```jsx
function App() {
  return (
    <div>
      <h1>User Profile</h1>
      <Suspense fallback={<ProfileSkeleton />}>
        <UserProfile userId="123" />
        <Suspense fallback={<PostsSkeleton />}>
          <UserPosts userId="123" />
        </Suspense>
      </Suspense>
    </div>
  );
}
```

### Concurrent Features

React 18 introduced concurrent features that make apps more responsive:

```jsx
import { startTransition, useDeferredValue } from 'react';

function SearchResults() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);

  const handleInputChange = (e) => {
    // Mark as transition for lower priority
    startTransition(() => {
      setQuery(e.target.value);
    });
  };

  return (
    <div>
      <input onChange={handleInputChange} />
      <Results query={deferredQuery} />
    </div>
  );
}
```

### Error Boundaries

Handle JavaScript errors in component trees:

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    console.log('Error caught:', error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary>
      <MyComponent />
    </ErrorBoundary>
  );
}
```

## Best Practices

### Component Design

- Keep components small and focused on a single responsibility
- Use descriptive names for components and props
- Prefer function components over class components
- Extract reusable logic into custom hooks

### State Management

- Keep state as close to where it's used as possible
- Use multiple useState calls for unrelated state variables
- Consider useReducer for complex state logic
- Use context sparingly - not everything needs to be global

### Performance

- Use React.memo for expensive components that don't change often
- Optimize expensive calculations with useMemo
- Cache functions with useCallback when passing to child components
- Use code splitting with lazy loading for large components

```jsx
import { memo, useMemo, useCallback } from 'react';

const ExpensiveComponent = memo(function ExpensiveComponent({ data, onUpdate }) {
  const expensiveValue = useMemo(() => {
    return data.reduce((sum, item) => sum + item.value, 0);
  }, [data]);

  const handleClick = useCallback((id) => {
    onUpdate(id);
  }, [onUpdate]);

  return (
    <div>
      <p>Total: {expensiveValue}</p>
      <button onClick={() => handleClick(data.id)}>Update</button>
    </div>
  );
});
```

### Code Organization

- Group related components in folders
- Use index.js files for clean imports
- Separate business logic from UI components
- Use TypeScript for better type safety and developer experience

### Testing

- Write tests for component behavior, not implementation details
- Use React Testing Library for integration-style tests
- Test user interactions and edge cases
- Mock external dependencies appropriately

This guide covers the essential concepts you need to understand React. As you build more applications, you'll discover additional patterns and libraries that complement React's core functionality.
