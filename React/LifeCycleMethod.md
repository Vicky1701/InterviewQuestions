# React Class Component Lifecycle Methods - Essential Interview Questions

## 🎯 Core Questions

### Q1: What are the main lifecycle methods?

**Answer:**

React Class Component Lifecycle Methods are **special methods that automatically get called at specific points** during a component's existence. They provide **hooks into the component's lifecycle** allowing you to execute code at crucial moments.

**Core Concept:** Every React component goes through three distinct phases:
1. **Mounting** (Birth) - Component is being created and inserted into DOM
2. **Updating** (Life) - Component is being re-rendered due to prop/state changes  
3. **Unmounting** (Death) - Component is being removed from DOM

**Essential Methods by Phase:**

```javascript
class MyComponent extends React.Component {
  // MOUNTING PHASE
  constructor(props) { 
    super(props); 
    // Initialize state, bind methods
  }
  
  componentDidMount() { 
    // API calls, subscriptions, DOM operations
    // Called ONCE after first render
  }
  
  // UPDATING PHASE  
  componentDidUpdate(prevProps, prevState) { 
    // React to prop/state changes
    // Called after every re-render (except first)
  }
  
  // UNMOUNTING PHASE
  componentWillUnmount() { 
    // Cleanup: remove listeners, cancel requests
    // Called just before component destruction
  }
  
  render() { 
    // Must return JSX - called in ALL phases
    return <div>Hello</div>; 
  }
}
```

**Why Important for Interviews:**
- **Memory Management**: Shows you understand preventing memory leaks
- **Performance**: Demonstrates knowledge of when to trigger expensive operations
- **Architecture**: Proves understanding of component lifecycle flow
- **Modern React**: Essential for comparing with Hooks equivalents

---

### Q2: When do you use componentDidMount?

**Answer:**
```javascript
componentDidMount() {
  // 1. API calls
  fetch('/api/data')
    .then(res => res.json())
    .then(data => this.setState({ data }));
  
  // 2. Event listeners
  window.addEventListener('scroll', this.handleScroll);
  
  // 3. Timers
  this.timer = setInterval(this.updateTime, 1000);
}
```

**Use for:** Data fetching, subscriptions, DOM manipulation after mount.

---

### Q3: What's componentDidUpdate used for?

**Answer:**
```javascript
componentDidUpdate(prevProps, prevState) {
  // Fetch new data when props change
  if (prevProps.userId !== this.props.userId) {
    this.fetchUserData();
  }
  
  // Update DOM when state changes
  if (prevState.count !== this.state.count) {
    document.title = `Count: ${this.state.count}`;
  }
}
```

**Use for:** Responding to prop/state changes, conditional API calls.

---

### Q4: Why is componentWillUnmount important?

**Answer:**
```javascript
componentWillUnmount() {
  // 1. Remove event listeners
  window.removeEventListener('scroll', this.handleScroll);
  
  // 2. Clear timers
  clearInterval(this.timer);
  
  // 3. Cancel API requests
  this.apiController.abort();
}
```

**Critical for:** Preventing memory leaks and cleanup.

---

### Q5: What's the complete lifecycle order?

**Answer:**

Understanding the **exact execution order** is crucial for interviews. React follows a predictable sequence:

**MOUNTING PHASE (Component Birth):**
```javascript
// Order: constructor → render → componentDidMount
1. constructor(props)           // Initialize state, bind methods
2. render()                     // Return JSX (first time)
3. componentDidMount()          // DOM is ready, make API calls
```

**UPDATING PHASE (Props/State Change):**
```javascript
// Order: render → componentDidUpdate
1. render()                     // Re-render with new data
2. componentDidUpdate(prevProps, prevState)  // Handle side effects
```

**UNMOUNTING PHASE (Component Death):**
```javascript
// Order: componentWillUnmount → removal from DOM
1. componentWillUnmount()       // Final cleanup chance
2. [Component removed from DOM]
```

**Interview Pro Tip:** 
- **Mounting**: Constructor runs first, render creates virtual DOM, componentDidMount confirms real DOM exists
- **Updating**: Render always runs first, componentDidUpdate handles post-render logic  
- **Unmounting**: componentWillUnmount is your last chance to prevent memory leaks

**Memory Trick:** **"C-R-M, R-U, U"** → Constructor-Render-Mount, Render-Update, Unmount

---

### Q6: How to handle errors in lifecycle methods?

**Answer:**
```javascript
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    console.log('Error:', error);
    console.log('Error Info:', errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    return this.props.children;
  }
}
```

**Use for:** Catching JavaScript errors in component tree.

---

### Q7: Lifecycle methods vs React Hooks equivalent?

**Answer:**
```javascript
// Class Component
class MyComponent extends React.Component {
  componentDidMount() {
    fetchData();
  }
  
  componentWillUnmount() {
    cleanup();
  }
  
  render() {
    return <div>{this.state.data}</div>;
  }
}

// Functional Component with Hooks
function MyComponent() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    fetchData(); // componentDidMount
    
    return () => {
      cleanup(); // componentWillUnmount
    };
  }, []); // Empty dependency array = componentDidMount
  
  return <div>{data}</div>;
}
```

**Key mapping:**
- `componentDidMount` → `useEffect(() => {}, [])`
- `componentDidUpdate` → `useEffect(() => {})`
- `componentWillUnmount` → `useEffect` cleanup function

---

### Q8: Common lifecycle method mistakes?

**Answer:**

**1. Calling setState in render:**
```javascript
// WRONG
render() {
  this.setState({ count: 1 }); // Infinite loop!
  return <div>{this.state.count}</div>;
}

// CORRECT
componentDidMount() {
  this.setState({ count: 1 });
}
```

**2. Not cleaning up in componentWillUnmount:**
```javascript
// WRONG
componentDidMount() {
  this.timer = setInterval(this.tick, 1000);
}
// No cleanup = memory leak!

// CORRECT
componentWillUnmount() {
  clearInterval(this.timer);
}
```

**3. Infinite loops in componentDidUpdate:**
```javascript
// WRONG
componentDidUpdate() {
  this.setState({ count: this.state.count + 1 }); // Infinite!
}

// CORRECT
componentDidUpdate(prevProps) {
  if (prevProps.userId !== this.props.userId) {
    this.fetchData(); // Only update when props actually change
  }
}
```

---

## 📝 Quick Reference

**Mounting (Birth):**
- `constructor()` - Initialize state
- `render()` - Return JSX
- `componentDidMount()` - API calls, subscriptions

**Updating (Life):**
- `render()` - Re-render with new data
- `componentDidUpdate()` - Handle changes

**Unmounting (Death):**
- `componentWillUnmount()` - Cleanup everything

**Golden Rules:**
1. API calls in `componentDidMount`
2. Always cleanup in `componentWillUnmount`
3. Compare prev/current in `componentDidUpdate`
4. Never call `setState` in `render`