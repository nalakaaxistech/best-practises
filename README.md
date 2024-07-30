# 🌟 Developer's Guide to React/Next.js

Hi everyone! 👋 This guide will help you write clean, efficient, and maintainable code in our React/Next.js projects using Nx monorepo. Let's make your coding journey awesome! 🚀

## 📚 Table of Contents

1. [Project Structure and Component Library](#1-project-structure-and-component-library)
2. [Clean Code Principles](#2-clean-code-principles)
3. [TypeScript Best Practices](#3-typescript-best-practices)
4. [React Component Best Practices](#4-react-component-best-practices)
5. [State Management Tips](#5-state-management-tips)
6. [Styling and CSS](#6-styling-and-css)
7. [Performance Optimization](#7-performance-optimization)

## 1. Project Structure and Component Library

### 📁 Folder Structure

Organize your project with a clear and consistent folder structure:

```
src/
├── components/
├── constants/
├── contexts/
├── lib/
├── utils/
└── hooks/
```

### 🧩 Component Library

Create a reusable component library based on the Atomic Design principle. For more details on component classification, refer to our [UI Component Classification Guide](./ui-classification-guide.md).

### 📦 Component Structure

For each component, create a dedicated folder with related files:

```
Button/
├── Button.tsx
├── styles.ts
├── types.ts
└── index.ts
```

Use `index.ts` to export the component and its types:

```typescript
// Button/index.ts
export { default as Button } from "./Button";
export { ButtonProps } from "./types";
```

### 🔍 Example: Button Component

```typescript
// Button/Button.tsx
import {ReactNode, FC} from 'react';
import { ButtonProps } from './types';
import * as Styles from './styles';

const Button: FC<ButtonProps> = ({ children, onClick, variant = 'primary' }) => {
  return (
    <StyledButton onClick={onClick} variant={variant}>
      {children}
    </StyledButton>
  );
};

export default Button;

// Button/types.ts
export interface ButtonProps {
  children: ReactNode;
  onClick: () => void;
  variant?: 'primary' | 'secondary';
}

// Button/styles.ts
import styled from 'styled-components';
import { ButtonProps } from './types';

export const Styles.StyledButton = styled.button<Pick<ButtonProps, 'variant'>>`
  // Styles here
`;
```

## 🎨 Atomic Design Implementation

Organize your components based on the Atomic Design principle:

- Atoms: Basic UI elements (e.g., Button, Input, Icon)
- Molecules: Simple combinations of atoms (e.g., SearchBar, FormField)
- Organisms: Complex UI sections (e.g., Header, ProductCard)
- Templates: Page layouts without specific content
- Pages: Specific instances of templates with real content

For a detailed guide on component classification, please refer to our Cardinal UI Component Classification Guide.

Remember, a well-organized project structure and component library will make your development process smoother and more efficient. Keep your components modular, reusable, and well-documented! 🚀💻

## 2. Clean Code Principles

### 🧹 Keep It Simple, Silly (KISS)

Always strive for simplicity in your code. It's easier to read, maintain, and debug!

✅ Good:

```typescript
function isEven(number) {
  return number % 2 === 0;
}
```

❌ Avoid:

```typescript
function isEven(number) {
  if (number % 2 === 0) {
    return true;
  } else {
    return false;
  }
}
```

### 📏 Follow Consistent Naming Conventions

Use clear, descriptive names for variables, functions, and components.

✅ Good:

```typescript
const getUserProfile = (userId) => {
  // function scope
};

const UserProfileCard = ({ user }) => {
  // component scope
};
```

❌ Avoid:

```typescript
const get = (id) => {
  // ...
};

const Card = ({ u }) => {
  // ...
};
```

### 🎯 Single Responsibility Principle

Each function or component should do one thing and do it well.

✅ Good:

```typescript
const formatDate = (date) => {
  // Format the date
};

const fetchUserData = (userId) => {
  // Fetch user data
};

const UserProfile = ({ userId }) => {
  const userData = fetchUserData(userId);
  return <div>{formatDate(userData.joinDate)}</div>;
};
```

❌ Avoid:

```typescript
const UserProfile = ({ userId }) => {
  // Fetches data, formats date, and renders all in one component
};
```

# 3. TypeScript Best Practices

## 🔍 Types vs Interfaces

Use types for unions, intersections, or when you need to use mapped types. Use interfaces for object shapes that you might want to extend later.

✅ Good use of types:

```typescript
type ButtonVariant = "primary" | "secondary" | "tertiary";

type Theme = "light" | "dark";

type Coordinates = {
  x: number;
  y: number;
};
```

✅ Good use of interfaces:

```typescript
interface User {
  id: string;
  name: string;
  email: string;
}

interface AdminUser extends User {
  role: "admin";
  permissions: string[];
}
```

## 📚 Use Type Inference

Let TypeScript infer types when it's clear what the type should be:

✅ Good:

```typescript
const numbers = [1, 2, 3, 4]; // inferred as number[]
const user = {
  name: "John",
  age: 30,
}; // inferred as { name: string; age: number; }
```

## ❌ Avoid unnecessary annotations:

```typescript
const numbers: number[] = [1, 2, 3, 4];
const user: { name: string; age: number } = {
  name: "John",
  age: 30,
};
```

## 🚫 Avoid **any**

Use **unknown** instead of **any** when you're unsure of a type. This forces you to perform type checking before using the value:

✅ Good:

```typescript
function processValue(value: unknown) {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else if (typeof value === "number") {
    console.log(value.toFixed(2));
  }
}
```

❌ Avoid:

```typescript
function processValue(value: any) {
  console.log(value.toUpperCase()); // No error, but might crash at runtime
}
```

## 🔒 Use Readonly for Immutable Data

When you have data that shouldn't be modified, use **Readonly** or **readonly**:

✅ Good:

```typescript
interface Config {
  readonly apiUrl: string;
  readonly timeout: number;
}

const config: Readonly<Config> = {
  apiUrl: "https://api.example.com",
  timeout: 5000,
};
```

## 🧩 Utilize Utility Types

TypeScript provides several utility types that can help you manipulate types:

✅ Good:

```typescript
interface User {
  id: string;
  name: string;
  email: string;
}

type UserWithoutId = Omit<User, "id">;
type PartialUser = Partial<User>;
```

## 🎭 Use Discriminated Unions for Complex State

When dealing with complex state that can be in different modes, use discriminated unions:

✅ Good:

```typescript
type LoadingState = { status: "loading" };
type SuccessState = { status: "success"; data: string };
type ErrorState = { status: "error"; error: Error };

type State = LoadingState | SuccessState | ErrorState;

function renderState(state: State) {
  switch (state.status) {
    case "loading":
      return <LoadingSpinner />;
    case "success":
      return <div>{state.data}</div>;
    case "error":
      return <div>{state.error.message}</div>;
  }
}
```

Remember, TypeScript is a powerful tool that can help you write more robust and self-documenting code. Use its features wisely to improve your code quality and catch errors early in development.💻

# 4. React Component Best Practices

## 🧩 Use Functional Components and Hooks

Functional components with hooks are now the preferred way to write React components.

✅ Good:

```typescript
import React, { useState, useEffect } from "react";

const UserProfile = ({ userId }) => {
  const [user, setUser] = useState(null);

  useEffect(() => {
    fetchUser(userId).then(setUser);
  }, [userId]);

  if (!user) return <div>Loading...</div>;

  return <div>{user.name}</div>;
};
```

❌ Avoid:

```typescript
class UserProfile extends React.Component {
  constructor(props) {
    super(props);
    this.state = { user: null };
  }

  componentDidMount() {
    fetchUser(this.props.userId).then((user) => this.setState({ user }));
  }

  render() {
    if (!this.state.user) return <div>Loading...</div>;
    return <div>{this.state.user.name}</div>;
  }
}
```

## 🎭 Prop Types for Type Checking

Use PropTypes to catch bugs early by type-checking props.

✅ Good:

```typescript
// types.ts
export type PropTypes = {
  text: string.isRequired;
  onClick: func.isRequired;
};

// YourComponent.tsx
import { PropTypes } from "./types.ts";

const Button = ({ text, onClick }: PropTypes) => (
  <button onClick={onClick}>{text}</button>
);
```

## 🔄 Keys in Lists

Always use unique keys when rendering lists of elements.

✅ Good:

```typescript
const TodoList = ({ todos }) => (
  <ul>
    {todos.map((todo) => (
      <li key={todo.id}>{todo.text}</li>
    ))}
  </ul>
);
```

❌ Avoid:

```typescript
const TodoList = ({ todos }) => (
  <ul>
    {todos.map((todo, index) => (
      <li key={index}>{todo.text}</li>
    ))}
  </ul>
);
```

### 🎭 Using Optional Props

When defining component props, use optional props to make your components more flexible:

✅ Good:

```tsx
interface ButtonProps {
  text: string;
  onClick: () => void;
  variant?: "primary" | "secondary";
  disabled?: boolean;
}

const Button: React.FC<ButtonProps> = ({
  text,
  onClick,
  variant = "primary",
  disabled = false,
}) => (
  <button onClick={onClick} className={variant} disabled={disabled}>
    {text}
  </button>
);
```

### 🔄 Default Props vs Default Parameters

For functional components, prefer default parameters over the defaultProps object:

✅ Good:

```typescript
const Button = ({ text, onClick, variant = "primary" }: ButtonProps) => {
  // ...
};
```

❌ Avoid:

```typescript
const Button: React.FC<ButtonProps> = (props) => {
  // ...
};

Button.defaultProps = {
  variant: "primary",
};
```

## 🎨 Use Enum for Predefined Options

When a prop has a fixed set of possible values, use an enum:

✅ Good:

```typescript
enum ButtonSize {
  Small = "small",
  Medium = "medium",
  Large = "large",
}

interface ButtonProps {
  size: ButtonSize;
  // ...
}

const Button: React.FC<ButtonProps> = ({ size, ...props }) => {
  // ...
};
```

# 5. State Management Tips

## 🎣 Use Hooks for Local State

For component-level state, use the **useState** hook.

✅ Good:

```typescript
const Counter = () => {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};
```

## 🌳 Context API for Shared State

Use Context for state that needs to be accessed by multiple components.

✅ Good:

```typescript
const ThemeContext = React.createContext("light");

const App = () => (
  <ThemeContext.Provider value="dark">
    <ThemedButton />
  </ThemeContext.Provider>
);

const ThemedButton = () => {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Themed Button</button>;
};
```

# 6. Styling and CSS

## 💅 CSS-in-JS with Styled Components

Use Styled Components for component-specific styles.

✅ Good:

```typescript
// styles.ts
import styled from "styled-components";

const Button = styled.button`
  background-color: blue;
  color: white;
  padding: 10px 20px;
`;

// YourComponent.tsx
import * as Styles from "./styles";
const MyComponent = () => <Button>Click me</Button>;
```

## 🎨 Use CSS Modules for Local Scoping

CSS Modules help avoid naming conflicts and provide local scoping.

✅ Good:

```typescript
import styles from "./Button.module.css";

const Button = ({ children }) => (
  <button className={styles.button}>{children}</button>
);
```

# 7. Performance Optimization

## 🏃‍♂️ Use React.memo for Pure Components

Wrap pure functional components with React.memo to prevent unnecessary re-renders.

✅ Good:

```typescript
const ExpensiveComponent = React.memo(({ data }) => {
  // Expensive rendering logic
});
```

## 🔍 Use the React DevTools Profiler

Regularly profile your app to identify performance bottlenecks.

Remember, becoming a great developer is a journey! 🌟 Keep practicing, stay curious, and don't be afraid to ask questions. Happy coding! 💻🎉
