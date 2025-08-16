# React Hooks - Most Common Interview Questions
*For 2 Years Experience - Quick Interview Prep*

## 1. What are React Hooks and why were they introduced?
**Most Asked:** Simple definition and main benefits
**Quick Answer:** Functions that let you use state and lifecycle features in functional components. Introduced to avoid class component complexity.

## 2. What is useState and how do you use it?
**Must Know Code Example:**
```javascript
const [count, setCount] = useState(0);
const [user, setUser] = useState({ name: '', email: '' });

// Update state
setCount(count + 1);
setUser({ ...user, name: 'John' });
```

## 3. What is useEffect and what are dependency arrays?
**Critical Understanding:**
```javascript
// Run on every render
useEffect(() => {
  console.log('Every render');
});

// Run only once (componentDidMount)
useEffect(() => {
  console.log('Only once');
}, []);

// Run when dependency changes
useEffect(() => {
  console.log('When count changes');
}, [count]);
```

## 4. How do you cleanup in useEffect?
**Must Know Pattern:**
```javascript
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Timer');
  }, 1000);

  // Cleanup function
  return () => {
    clearInterval(timer);
  };
}, []);
```

## 5. What's the difference between useState and useRef?
**Common Comparison:**
- useState: Triggers re-render when changed
- useRef: Doesn't trigger re-render, persists across renders

```javascript
const [count, setCount] = useState(0); // Re-renders
const countRef = useRef(0); // No re-render
```

## 6. What is useContext and how does it work?
**Practical Example:**
```javascript
// Create context
const UserContext = createContext();

// Provider
<UserContext.Provider value={user}>
  <ChildComponent />
</UserContext.Provider>

// Consumer
const user = useContext(UserContext);
```

## 7. When would you use useCallback?
**Performance Question:**
```javascript
// Without useCallback - creates new function every render
const handleClick = () => {
  console.log('clicked');
};

// With useCallback - memoized function
const handleClick = useCallback(() => {
  console.log('clicked');
}, [dependency]);
```

## 8. What is useMemo and when to use it?
**Optimization Hook:**
```javascript
const expensiveValue = useMemo(() => {
  return heavyComputation(data);
}, [data]);
```

## 9. What are the Rules of Hooks?
**Critical Rules:**
1. Only call hooks at the top level
2. Only call hooks from React functions
3. Don't call hooks inside loops, conditions, or nested functions

```javascript
// ❌ Wrong
if (condition) {
  const [state, setState] = useState(0);
}

// ✅ Correct
const [state, setState] = useState(0);
if (condition) {
  // use state here
}
```

## 10. How do you handle forms with hooks?
**Practical Example:**
```javascript
const [formData, setFormData] = useState({
  name: '',
  email: ''
});

const handleChange = (e) => {
  setFormData({
    ...formData,
    [e.target.name]: e.target.value
  });
};
```

## 11. What is useReducer and when to use it?
**Alternative to useState:**
```javascript
const [state, dispatch] = useReducer(reducer, initialState);

const reducer = (state, action) => {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    default:
      return state;
  }
};
```

## 12. How do you fetch data with hooks?
**Most Common Pattern:**
```javascript
const [data, setData] = useState(null);
const [loading, setLoading] = useState(true);
const [error, setError] = useState(null);

useEffect(() => {
  fetch('/api/data')
    .then(response => response.json())
    .then(data => {
      setData(data);
      setLoading(false);
    })
    .catch(err => {
      setError(err);
      setLoading(false);
    });
}, []);
```

## Quick Tips for Interview:
1. **Always mention** functional components over class components
2. **Show practical examples** - forms, API calls, cleanup
3. **Know the rules** - top level only, no conditions
4. **Understand dependencies** - empty array vs dependency array
5. **Practice cleanup** - timers, subscriptions, event listeners

## Most Important Hooks to Master:
- ✅ useState - State management
- ✅ useEffect - Side effects & lifecycle
- ✅ useContext - Avoid prop drilling
- ✅ useCallback - Function memoization
- ✅ useMemo - Value memoization
- ✅ useRef - DOM references & mutable values

## Common Mistakes to Avoid:
- ❌ Calling hooks conditionally
- ❌ Forgetting dependency arrays in useEffect
- ❌ Not cleaning up subscriptions
- ❌ Overusing useCallback/useMemo
- ❌ Mutating state directly