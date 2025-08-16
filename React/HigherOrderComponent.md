# Higher Order Components (HOC) - Most Common Interview Questions
*For 2 Years Experience - Quick Interview Prep*

## 1. What is a Higher Order Component (HOC) in React?
**Most Asked:** Define HOC and explain why it's used.
**Quick Answer:** A function that takes a component and returns a new component with additional props or functionality.

## 2. Can you write a simple HOC example?
**Must Know:** Basic HOC implementation
```javascript
const withLoading = (WrappedComponent) => {
  return (props) => {
    if (props.isLoading) {
      return <div>Loading...</div>;
    }
    return <WrappedComponent {...props} />;
  };
};
```

## 3. What are the advantages of using HOCs?
**Common Answer Points:**
- Code reusability
- Separation of concerns
- Cross-cutting functionality
- Props manipulation

## 4. What are the disadvantages/problems with HOCs?
**Critical Points:**
- Wrapper hell
- Props collision
- Static methods not copied
- Refs not passed through

## 5. How do you pass refs through HOCs?
**Must Know:** React.forwardRef usage
```javascript
const withAuth = React.forwardRef((props, ref) => {
  return <WrappedComponent {...props} ref={ref} />;
});
```

## 6. What's the difference between HOCs and Render Props?
**Common Comparison Question:**
- HOCs: Component wrapping pattern
- Render Props: Function as children pattern
- Both solve same problems differently

## 7. What's the difference between HOCs and React Hooks?
**Modern React Question:**
- HOCs: Class component era solution
- Hooks: Modern functional component solution
- Hooks are preferred now for state logic reuse

## 8. Can you create an HOC for authentication?
**Practical Example:**
```javascript
const withAuth = (WrappedComponent) => {
  return (props) => {
    const isAuthenticated = checkAuth(); // your auth logic
    
    if (!isAuthenticated) {
      return <LoginComponent />;
    }
    
    return <WrappedComponent {...props} />;
  };
};
```

## 9. How do you handle static methods in HOCs?
**Technical Detail:**
```javascript
// Copy static methods
const EnhancedComponent = withHOC(MyComponent);
EnhancedComponent.staticMethod = MyComponent.staticMethod;
```

## 10. What are some common use cases for HOCs?
**Real-world Examples:**
- Authentication/Authorization
- Loading states
- Error handling
- Data fetching
- Logging/Analytics

## 11. Can you explain the naming convention for HOCs?
**Best Practice:**
- Start with "with" prefix (withAuth, withLoading)
- Use descriptive names
- Follow camelCase convention

## 12. What's the problem with using HOCs inside render method?
**Performance Issue:**
- Creates new component on every render
- Breaks React's reconciliation
- Always remounts the component

## Quick Tips for Interview:
1. **Always mention** HOCs are functions that take and return components
2. **Show practical examples** - authentication, loading states
3. **Know the problems** - wrapper hell, props collision
4. **Mention modern alternatives** - React Hooks are preferred now
5. **Code confidently** - practice writing basic HOC syntax

## Most Important Points to Remember:
- ✅ HOC = Function that takes component, returns enhanced component
- ✅ Used for cross-cutting concerns
- ✅ Can cause wrapper hell
- ✅ Hooks are modern replacement
- ✅ Always use React.forwardRef for refs
