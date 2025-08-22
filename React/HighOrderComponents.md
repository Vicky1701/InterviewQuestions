# Higher Order Components - Essential Interview Questions

## 🎯 Core Concepts (Must Know)

### Q1: What is a Higher Order Component?
**Answer:** 

A Higher Order Component (HOC) is a **design pattern** in React that allows you to reuse component logic. It's essentially a **pure function that takes a component as an argument and returns a new, enhanced component** with additional functionality.

**Key Characteristics:**
- **Pure Function**: Takes a component, returns a component
- **Cross-cutting Concerns**: Perfect for handling authentication, logging, data fetching, etc.
- **Composition over Inheritance**: React's way of sharing code between components
- **No DOM Manipulation**: Works purely with React components

**Real-world Analogy**: Think of it like a decorator pattern - you're wrapping your component with additional behavior without modifying the original component.

```javascript
const withGreeting = (WrappedComponent) => {
  return (props) => (
    <div>
      <h2>Hello from HOC!</h2>
      <WrappedComponent {...props} />
    </div>
  );
};
```

**Why use HOCs:**
- **Code Reusability**: Share common logic across multiple components
- **Separation of Concerns**: Keep business logic separate from UI logic
- **Conditional Rendering**: Easily add/remove features based on conditions
- **Props Manipulation**: Transform or add props before passing to wrapped component

---

### Q2: Create a simple HOC for loading states
**Answer:**
```javascript
const withLoading = (WrappedComponent) => {
  return ({ isLoading, ...props }) => {
    if (isLoading) return <div>Loading...</div>;
    return <WrappedComponent {...props} />;
  };
};

// Usage
const UserList = ({ users }) => <div>{users.map(u => u.name)}</div>;
const UserListWithLoading = withLoading(UserList);
```

---

### Q3: How do you pass props through a HOC?
**Answer:** Always use spread operator `{...props}` to pass all original props.

```javascript
const withTimestamp = (WrappedComponent) => {
  return (props) => (
    <WrappedComponent 
      {...props} 
      timestamp={new Date().toISOString()} 
    />
  );
};
```

---

## 🔧 Practical Questions

### Q4: Create an authentication HOC
**Answer:**
```javascript
const withAuth = (WrappedComponent) => {
  return (props) => {
    const isLoggedIn = localStorage.getItem('token');
    
    if (!isLoggedIn) {
      return <div>Please login</div>;
    }
    
    return <WrappedComponent {...props} />;
  };
};
```

---

### Q5: HOCs vs Custom Hooks - When to use which?
**Answer:**

**Use HOCs when:**
- Need to wrap with JSX (modals, borders)
- Conditional rendering of different components
- Working with class components

**Use Hooks when:**
- Only sharing logic (no JSX wrapping)
- Better performance
- Functional components only

---

### Q6: How to use multiple HOCs?
**Answer:**
```javascript
// Method 1: Nesting
const Enhanced = withAuth(withLoading(MyComponent));

// Method 2: Compose (better)
const compose = (...hocs) => (Component) => 
  hocs.reduceRight((acc, hoc) => hoc(acc), Component);

const Enhanced = compose(withAuth, withLoading)(MyComponent);
```

---

## ⚠️ Common Mistakes

### Q7: What are 3 main HOC pitfalls?
**Answer:**

**1. Not forwarding refs:**
```javascript
// Wrong
const withLogging = (Component) => (props) => <Component {...props} />;

// Correct
const withLogging = (Component) => 
  React.forwardRef((props, ref) => <Component {...props} ref={ref} />);
```

**2. Creating objects in render:**
```javascript
// Wrong - creates new object every render
const withData = (Component) => (props) => (
  <Component {...props} config={{ api: 'url' }} />
);

// Correct - create outside
const config = { api: 'url' };
const withData = (Component) => (props) => (
  <Component {...props} config={config} />
);
```

**3. No display name for debugging:**
```javascript
const withLogging = (Component) => {
  const Enhanced = (props) => <Component {...props} />;
  Enhanced.displayName = `WithLogging(${Component.name})`;
  return Enhanced;
};
```

---

## 🚀 Quick Practice

### Q8: Create a HOC that adds a border and click counter
**Answer:**
```javascript
const withBorderAndCounter = (WrappedComponent) => {
  return (props) => {
    const [clicks, setClicks] = useState(0);
    
    return (
      <div 
        style={{ border: '2px solid blue', padding: '10px' }}
        onClick={() => setClicks(clicks + 1)}
      >
        <p>Clicks: {clicks}</p>
        <WrappedComponent {...props} />
      </div>
    );
  };
};
```

---

## 📝 Interview Cheat Sheet

**Definition:** Function that takes component → returns enhanced component

**Basic Pattern:**
```javascript
const withSomething = (WrappedComponent) => {
  return (props) => (
    <WrappedComponent {...props} />
  );
};
```

**Key Points:**
- Always spread props: `{...props}`
- Add display names for debugging
- Use `React.forwardRef` when needed
- Compose multiple HOCs for clean code
- Consider hooks for logic-only sharing

**Common Use Cases:**
- Authentication checks
- Loading states  
- Error boundaries
- Theme providers
- Analytics tracking