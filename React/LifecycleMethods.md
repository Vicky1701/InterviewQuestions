# React Lifecycle Methods - Most Common Interview Questions
*For 2 Years Experience - Quick Interview Prep*

## 1. What are React Lifecycle Methods?
**Most Asked:** Basic definition and purpose
**Quick Answer:** Methods that get called at different phases of a component's life - mounting, updating, and unmounting.

## 2. What are the three phases of React component lifecycle?
**Must Know:**
1. **Mounting** - Component is being created and inserted into DOM
2. **Updating** - Component is being re-rendered due to props/state changes
3. **Unmounting** - Component is being removed from DOM

## 3. What is componentDidMount and when is it used?
**Most Common Use Cases:**
```javascript
componentDidMount() {
  // API calls
  fetch('/api/data')
    .then(response => response.json())
    .then(data => this.setState({ data }));
  
  // DOM manipulation
  this.inputRef.focus();
  
  // Set up subscriptions/timers
  this.timer = setInterval(() => {
    this.setState({ time: new Date() });
  }, 1000);
}
```

## 4. What is componentWillUnmount and why is it important?
**Critical for Memory Leaks:**
```javascript
componentWillUnmount() {
  // Clear timers
  clearInterval(this.timer);
  
  // Remove event listeners
  window.removeEventListener('resize', this.handleResize);
  
  // Cancel API calls
  this.abortController.abort();
  
  // Clean up subscriptions
  this.subscription.unsubscribe();
}
```

## 5. What is componentDidUpdate and when do you use it?
**Common Pattern:**
```javascript
componentDidUpdate(prevProps, prevState) {
  // Only update if props changed
  if (prevProps.userId !== this.props.userId) {
    this.fetchUserData(this.props.userId);
  }
  
  // Compare state
  if (prevState.count !== this.state.count) {
    console.log('Count updated');
  }
}
```

## 6. What is shouldComponentUpdate and how does it optimize performance?
**Performance Optimization:**
```javascript
shouldComponentUpdate(nextProps, nextState) {
  // Only re-render if specific props changed
  return nextProps.name !== this.props.name || 
         nextState.count !== this.state.count;
}
```

## 7. What is the difference between componentDidMount and constructor?
**Common Comparison:**
- **Constructor:** Initialize state, bind methods
- **componentDidMount:** Side effects, API calls, DOM access

```javascript
constructor(props) {
  super(props);
  this.state = { count: 0 }; // Initialize state
  this.handleClick = this.handleClick.bind(this); // Bind methods
}

componentDidMount() {
  // API calls, subscriptions
  this.fetchData();
}
```

## 8. How do React Hooks relate to lifecycle methods?
**Modern Replacement:**
```javascript
// Class component
componentDidMount() {
  fetchData();
}
componentWillUnmount() {
  cleanup();
}

// Hooks equivalent
useEffect(() => {
  fetchData();
  return () => cleanup(); // Cleanup function
}, []); // Empty dependency array = componentDidMount
```

## 9. What is getSnapshotBeforeUpdate?
**Less Common but Asked:**
```javascript
getSnapshotBeforeUpdate(prevProps, prevState) {
  // Capture scroll position before update
  if (prevState.list.length < this.state.list.length) {
    return this.listRef.scrollHeight;
  }
  return null;
}

componentDidUpdate(prevProps, prevState, snapshot) {
  if (snapshot !== null) {
    // Restore scroll position
    this.listRef.scrollTop = snapshot;
  }
}
```

## 10. What happens if you call setState in different lifecycle methods?
**Critical Understanding:**
```javascript
// ✅ Safe
componentDidMount() {
  this.setState({ data: 'loaded' }); // OK
}

// ❌ Dangerous - Infinite loop
componentDidUpdate() {
  this.setState({ data: 'updated' }); // Infinite loop!
}

// ✅ Safe with condition
componentDidUpdate(prevProps) {
  if (prevProps.id !== this.props.id) {
    this.setState({ data: null }); // OK with condition
  }
}
```

## 11. What is componentDidCatch and how do you handle errors?
**Error Handling:**
```javascript
componentDidCatch(error, errorInfo) {
  // Log error
  console.error('Component caught error:', error);
  
  // Update state to show error UI
  this.setStat
  e({ hasError: true, error });
  
  // Send to error reporting service
  errorReporting.captureException(error);
}
```

## 12. What are the deprecated lifecycle methods?
**Must Know - Avoid These:**
- ❌ componentWillMount
- ❌ componentWillReceiveProps  
- ❌ componentWillUpdate

**Why deprecated:** Unsafe for async rendering and concurrent mode

## Quick Lifecycle Flow Diagram:
```
Mounting:
constructor → componentDidMount → render

Updating:
shouldComponentUpdate → render → componentDidUpdate

Unmounting:
componentWillUnmount
```

## Most Important Points for Interview:
1. **componentDidMount** - API calls, subscriptions
2. **componentWillUnmount** - Cleanup to prevent memory leaks
3. **componentDidUpdate** - Handle prop/state changes
4. **shouldComponentUpdate** - Performance optimization
5. **Hooks are modern replacement** - useEffect replaces most lifecycle methods

## Common Patterns to Master:
```javascript
// Data fetching pattern
componentDidMount() {
  this.fetchData();
}

// Cleanup pattern
componentWillUnmount() {
  this.cleanup();
}

// Conditional update pattern
componentDidUpdate(prevProps) {
  if (prevProps.id !== this.props.id) {
    this.fetchData();
  }
}
```

## Interview Tips:
- ✅ Always mention cleanup in componentWillUnmount
- ✅ Show you know hooks are the modern way
- ✅ Understand the order of lifecycle methods
- ✅ Know which methods are deprecated
- ✅ Practice the common patterns above
