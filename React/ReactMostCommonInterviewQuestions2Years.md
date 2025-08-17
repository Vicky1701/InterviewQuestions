# React Interview Questions with Confident Answers & Examples

## 🔹 React Basics

### 1. What is React? Why is it used?

**Confident Answer:** React is a JavaScript library developed by Facebook for building user interfaces, especially for web applications. It focuses on creating reusable UI components.

**Why React is Popular:**
- **Component-Based:** Build encapsulated components that manage their own state
- **Virtual DOM:** Efficient updates and rendering for better performance
- **Reusability:** Write once, use anywhere
- **Large Ecosystem:** Huge community support and third-party libraries
- **Easy Learning Curve:** Especially if you know JavaScript

**Example:**
```jsx
// Simple React component
function Welcome() {
  return <h1>Hello, Welcome to React!</h1>;
}
```

---

### 2. What are Components in React? Difference between Functional and Class Components.

**Confident Answer:** Components are the building blocks of React applications. They are like JavaScript functions that return JSX elements describing what should appear on screen.

**Functional Components (Modern Approach):**
```jsx
// Functional Component with Hooks
function UserProfile({ name, age }) {
  const [isOnline, setIsOnline] = useState(false);
  
  useEffect(() => {
    // Component logic here
  }, []);
  
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      <p>Status: {isOnline ? 'Online' : 'Offline'}</p>
    </div>
  );
}
```

**Class Components (Traditional Approach):**
```jsx
// Class Component
class UserProfile extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isOnline: false };
  }
  
  componentDidMount() {
    // Component logic here
  }
  
  render() {
    return (
      <div>
        <h2>{this.props.name}</h2>
        <p>Age: {this.props.age}</p>
        <p>Status: {this.state.isOnline ? 'Online' : 'Offline'}</p>
      </div>
    );
  }
}
```

**Key Differences:**
| Functional | Class |
|------------|-------|
| Uses Hooks for state | Uses this.state |
| Simpler syntax | More verbose |
| Better performance | Lifecycle methods |
| Modern preferred approach | Legacy approach |

---

### 3. What is JSX? Why do we use it?

**Confident Answer:** JSX (JavaScript XML) is a syntax extension that allows you to write HTML-like code inside JavaScript. It makes React code more readable and easier to write.

**Without JSX (Pure JavaScript):**
```javascript
const element = React.createElement(
  'h1',
  { className: 'greeting' },
  'Hello, World!'
);
```

**With JSX (Much Cleaner):**
```jsx
const element = <h1 className="greeting">Hello, World!</h1>;
```

**JSX Rules:**
- Must return single parent element
- Use `className` instead of `class`
- Use `{}` for JavaScript expressions
- Self-closing tags must end with `/>`

**Example:**
```jsx
function App() {
  const name = "John";
  const isLoggedIn = true;
  
  return (
    <div>
      <h1>Hello, {name}!</h1>
      {isLoggedIn && <p>Welcome back!</p>}
      <img src="photo.jpg" alt="Profile" />
    </div>
  );
}
```

---

### 4. What is the Virtual DOM? How is it different from the real DOM?

**Confident Answer:** Virtual DOM is a JavaScript representation of the actual DOM (Document Object Model). React creates a virtual copy of the DOM in memory, compares changes, and updates only the parts that actually changed.

**How it Works:**
1. **State Change:** Component state changes
2. **Virtual DOM Update:** React creates new Virtual DOM tree
3. **Diffing:** Compares new tree with previous tree
4. **Reconciliation:** Updates only changed elements in real DOM

**Example:**
```jsx
function Counter() {
  const [count, setCount] = useState(0);
  
  // When count changes, only the <span> text updates in real DOM
  // Not the entire component
  return (
    <div>
      <h1>Counter App</h1>
      <p>Count: <span>{count}</span></p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

**Benefits:**
- **Performance:** Batch updates, minimal DOM manipulation
- **Predictability:** Easier to understand what changes when
- **Cross-browser:** React handles browser differences

---

### 5. What are props and state? Difference between them.

**Confident Answer:** Props and State are two ways to manage data in React components.

**Props (Properties):**
- Data passed from parent to child
- Read-only (immutable)
- Used for component communication

**State:**
- Internal component data
- Mutable (can be changed)
- Triggers re-render when updated

**Example:**
```jsx
// Parent Component
function App() {
  const [user, setUser] = useState({ name: "John", age: 25 });
  
  return (
    <div>
      <UserProfile 
        name={user.name}        // Props
        age={user.age}          // Props
        onUpdate={setUser}      // Props (function)
      />
    </div>
  );
}

// Child Component
function UserProfile({ name, age, onUpdate }) {
  const [isEditing, setIsEditing] = useState(false); // State
  
  const handleEdit = () => {
    setIsEditing(!isEditing); // Changing state
  };
  
  return (
    <div>
      <h2>{name}</h2>         {/* Using props */}
      <p>Age: {age}</p>       {/* Using props */}
      {isEditing && <input />} {/* Using state */}
      <button onClick={handleEdit}>
        {isEditing ? 'Save' : 'Edit'}
      </button>
    </div>
  );
}
```

**Key Differences:**
| Props | State |
|-------|-------|
| External data | Internal data |
| Read-only | Mutable |
| Passed from parent | Managed within component |
| Cannot trigger re-render | Triggers re-render |

---

### 6. What is the difference between controlled and uncontrolled components?

**Confident Answer:** This refers to how form elements handle their data.

**Controlled Components:**
- Form data handled by React state
- React controls the input value
- Recommended approach

**Uncontrolled Components:**
- Form data handled by DOM
- Access data using refs
- Used when you need direct DOM access

**Controlled Component Example:**
```jsx
function ControlledForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Email:', email, 'Password:', password);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        value={email}                              // Controlled by state
        onChange={(e) => setEmail(e.target.value)} // Update state
        placeholder="Email"
      />
      <input
        type="password"
        value={password}                              // Controlled by state
        onChange={(e) => setPassword(e.target.value)} // Update state
        placeholder="Password"
      />
      <button type="submit">Login</button>
    </form>
  );
}
```

**Uncontrolled Component Example:**
```jsx
function UncontrolledForm() {
  const emailRef = useRef(null);
  const passwordRef = useRef(null);
  
  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Email:', emailRef.current.value);
    console.log('Password:', passwordRef.current.value);
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        ref={emailRef}      // Access via ref
        type="email"
        placeholder="Email"
      />
      <input
        ref={passwordRef}   // Access via ref
        type="password"
        placeholder="Password"
      />
      <button type="submit">Login</button>
    </form>
  );
}
```

---

## 🔹 React Hooks (Most Important for 2 Years Experience)

### 7. What are React Hooks? Why were they introduced?

**Confident Answer:** Hooks are functions that let you use state and other React features in functional components. They were introduced in React 16.8 to solve several problems.

**Why Hooks were Introduced:**
- **Complex Class Components:** Classes were hard to understand and optimize
- **Logic Reuse:** Difficult to share stateful logic between components
- **Lifecycle Confusion:** Related logic scattered across different lifecycle methods

**Rules of Hooks:**
1. Only call Hooks at the top level (not inside loops, conditions, or nested functions)
2. Only call Hooks from React functions

**Example:**
```jsx
// Before Hooks (Class Component)
class OldCounter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
  
  componentDidMount() {
    document.title = `Count: ${this.state.count}`;
  }
  
  componentDidUpdate() {
    document.title = `Count: ${this.state.count}`;
  }
  
  render() {
    return (
      <div>
        <p>{this.state.count}</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          +
        </button>
      </div>
    );
  }
}

// With Hooks (Functional Component)
function NewCounter() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);
  
  return (
    <div>
      <p>{count}</p>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  );
}
```

---

### 8. Explain useState with example.

**Confident Answer:** useState is a Hook that allows you to add state to functional components. It returns an array with the current state value and a function to update it.

**Syntax:** `const [state, setState] = useState(initialValue);`

**Simple Example:**
```jsx
function Counter() {
  const [count, setCount] = useState(0); // Initial value: 0
  
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

**Multiple State Variables:**
```jsx
function UserForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [age, setAge] = useState(0);
  
  return (
    <form>
      <input 
        value={name}
        onChange={(e) => setName(e.target.value)}
        placeholder="Name"
      />
      <input 
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
      />
      <input 
        type="number"
        value={age}
        onChange={(e) => setAge(parseInt(e.target.value))}
        placeholder="Age"
      />
    </form>
  );
}
```

**Object State:**
```jsx
function UserProfile() {
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0
  });
  
  const updateUser = (field, value) => {
    setUser(prevUser => ({
      ...prevUser,      // Spread previous state
      [field]: value    // Update specific field
    }));
  };
  
  return (
    <div>
      <input 
        value={user.name}
        onChange={(e) => updateUser('name', e.target.value)}
        placeholder="Name"
      />
      <p>Hello, {user.name}!</p>
    </div>
  );
}
```

---

### 9. Explain useEffect with example. What is the dependency array?

**Confident Answer:** useEffect is a Hook that lets you perform side effects in functional components. It serves the same purpose as componentDidMount, componentDidUpdate, and componentWillUnmount combined.

**Basic Syntax:** `useEffect(() => { /* effect */ }, [dependencies]);`

**Component Mount & Unmount:**
```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    console.log('Component mounted');
    
    // Cleanup function (componentWillUnmount)
    return () => {
      console.log('Component unmounted');
    };
  }, []); // Empty dependency array = run once on mount
  
  return <div>{user?.name}</div>;
}
```

**Effect with Dependencies:**
```jsx
function UserData({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(false);
  
  useEffect(() => {
    setLoading(true);
    
    // Fetch user data when userId changes
    fetch(`/api/users/${userId}`)
      .then(response => response.json())
      .then(userData => {
        setUser(userData);
        setLoading(false);
      });
  }, [userId]); // Run when userId changes
  
  if (loading) return <div>Loading...</div>;
  return <div>{user?.name}</div>;
}
```

**Dependency Array Explained:**
- **No array:** Effect runs after every render
- **Empty array `[]`:** Effect runs once on mount
- **With dependencies `[value]`:** Effect runs when dependencies change

**Timer Example:**
```jsx
function Timer() {
  const [seconds, setSeconds] = useState(0);
  
  useEffect(() => {
    const interval = setInterval(() => {
      setSeconds(prev => prev + 1);
    }, 1000);
    
    // Cleanup: clear interval when component unmounts
    return () => clearInterval(interval);
  }, []); // Empty dependency - set up once
  
  return <div>Timer: {seconds} seconds</div>;
}
```

---

### 10. How do you clean up inside useEffect?

**Confident Answer:** Cleanup in useEffect is done by returning a function from the effect. This function runs before the component unmounts or before the effect runs again.

**Common Cleanup Scenarios:**

**1. Timer Cleanup:**
```jsx
function Timer() {
  const [time, setTime] = useState(0);
  
  useEffect(() => {
    const timer = setInterval(() => {
      setTime(prev => prev + 1);
    }, 1000);
    
    // Cleanup: Clear timer
    return () => {
      clearInterval(timer);
      console.log('Timer cleared');
    };
  }, []);
  
  return <div>Time: {time}</div>;
}
```

**2. Event Listener Cleanup:**
```jsx
function WindowSize() {
  const [windowWidth, setWindowWidth] = useState(window.innerWidth);
  
  useEffect(() => {
    const handleResize = () => {
      setWindowWidth(window.innerWidth);
    };
    
    window.addEventListener('resize', handleResize);
    
    // Cleanup: Remove event listener
    return () => {
      window.removeEventListener('resize', handleResize);
      console.log('Event listener removed');
    };
  }, []);
  
  return <div>Window width: {windowWidth}px</div>;
}
```

**3. API Request Cleanup:**
```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    let cancelled = false; // Flag to prevent state update
    
    const fetchUser = async () => {
      try {
        const response = await fetch(`/api/users/${userId}`);
        const userData = await response.json();
        
        // Only update state if component is still mounted
        if (!cancelled) {
          setUser(userData);
        }
      } catch (error) {
        if (!cancelled) {
          console.error('Failed to fetch user:', error);
        }
      }
    };
    
    fetchUser();
    
    // Cleanup: Mark as cancelled
    return () => {
      cancelled = true;
    };
  }, [userId]);
  
  return <div>{user?.name || 'Loading...'}</div>;
}
```

**Why Cleanup is Important:**
- **Memory Leaks:** Prevent timers, listeners from running after unmount
- **Performance:** Clean up unnecessary operations
- **Bugs:** Prevent state updates on unmounted components

---

### 11. Difference between useState and useRef.

**Confident Answer:** Both store data, but they work very differently:

**useState:**
- Triggers re-render when value changes
- Used for component state that affects UI
- Returns current value and setter function

**useRef:**
- Does NOT trigger re-render when value changes
- Used for accessing DOM elements or storing mutable values
- Returns object with `.current` property

**Comparison Example:**
```jsx
function ComparisonDemo() {
  const [count, setCount] = useState(0);        // State - triggers re-render
  const renderCount = useRef(0);                // Ref - no re-render
  const inputRef = useRef(null);                // Ref for DOM access
  
  // This runs every render
  renderCount.current = renderCount.current + 1;
  
  const handleClick = () => {
    setCount(count + 1);                        // Triggers re-render
    console.log('Render count:', renderCount.current);
  };
  
  const focusInput = () => {
    inputRef.current.focus();                   // Direct DOM manipulation
  };
  
  return (
    <div>
      <p>Count: {count}</p>
      <p>Component rendered {renderCount.current} times</p>
      
      <input ref={inputRef} placeholder="Focus me!" />
      
      <button onClick={handleClick}>Increment Count</button>
      <button onClick={focusInput}>Focus Input</button>
    </div>
  );
}
```

**When to Use Each:**
- **useState:** Form inputs, toggles, counters, any UI state
- **useRef:** DOM access, timers, previous values, any non-UI data

**useRef for Previous Value:**
```jsx
function PreviousValue() {
  const [count, setCount] = useState(0);
  const prevCount = useRef();
  
  useEffect(() => {
    prevCount.current = count;  // Store previous value
  });
  
  return (
    <div>
      <p>Current: {count}</p>
      <p>Previous: {prevCount.current}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

---

### 12. What are custom hooks? Have you created one?

**Confident Answer:** Custom hooks are JavaScript functions that start with "use" and can call other hooks. They let you extract component logic into reusable functions.

**Simple Custom Hook Example:**
```jsx
// Custom hook for counter logic
function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(prev => prev + 1);
  const decrement = () => setCount(prev => prev - 1);
  const reset = () => setCount(initialValue);
  
  return { count, increment, decrement, reset };
}

// Using the custom hook
function CounterApp() {
  const { count, increment, decrement, reset } = useCounter(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}
```

**API Fetching Custom Hook:**
```jsx
// Custom hook for API calls
function useApi(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        
        if (!response.ok) {
          throw new Error(`Error: ${response.status}`);
        }
        
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchData();
  }, [url]);
  
  return { data, loading, error };
}

// Using the API hook
function UserProfile({ userId }) {
  const { data: user, loading, error } = useApi(`/api/users/${userId}`);
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
    </div>
  );
}
```

**Local Storage Custom Hook:**
```jsx
// Custom hook for localStorage
function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.error('Error reading localStorage:', error);
      return initialValue;
    }
  });
  
  const setValue = (value) => {
    try {
      setStoredValue(value);
      window.localStorage.setItem(key, JSON.stringify(value));
    } catch (error) {
      console.error('Error setting localStorage:', error);
    }
  };
  
  return [storedValue, setValue];
}

// Using localStorage hook
function Settings() {
  const [theme, setTheme] = useLocalStorage('theme', 'light');
  
  return (
    <div>
      <p>Current theme: {theme}</p>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </div>
  );
}
```

**Benefits of Custom Hooks:**
- **Reusability:** Share logic across components
- **Separation of Concerns:** Keep components clean
- **Testing:** Easier to test isolated logic
- **Community:** Share with other developers

---

## 🔹 Component Lifecycle & Rendering

### 13. What are React component lifecycle methods? (mounting, updating, unmounting)

**Confident Answer:** Component lifecycle refers to the different phases a component goes through from creation to destruction.

**Class Component Lifecycle:**

**Mounting (Component Created):**
```jsx
class UserProfile extends React.Component {
  constructor(props) {
    super(props);
    this.state = { user: null };
    console.log('1. Constructor - Component being created');
  }
  
  componentDidMount() {
    console.log('2. ComponentDidMount - Component mounted, perfect for API calls');
    fetch('/api/user')
      .then(response => response.json())
      .then(user => this.setState({ user }));
  }
  
  render() {
    console.log('3. Render - Component rendering');
    return <div>{this.state.user?.name}</div>;
  }
}
```

**Updating (Props or State Change):**
```jsx
componentDidUpdate(prevProps, prevState) {
  console.log('4. ComponentDidUpdate - Component updated');
  
  // Only fetch if userId changed
  if (prevProps.userId !== this.props.userId) {
    this.fetchUserData(this.props.userId);
  }
}
```

**Unmounting (Component Removed):**
```jsx
componentWillUnmount() {
  console.log('5. ComponentWillUnmount - Cleanup before removal');
  // Clear timers, cancel API requests, remove event listeners
  clearInterval(this.timer);
}
```

**Complete Lifecycle Example:**
```jsx
class LifecycleDemo extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
    this.timer = null;
  }
  
  componentDidMount() {
    // Set up timer when component mounts
    this.timer = setInterval(() => {
      this.setState(prev => ({ count: prev.count + 1 }));
    }, 1000);
  }
  
  componentDidUpdate(prevProps, prevState) {
    if (prevState.count !== this.state.count) {
      document.title = `Count: ${this.state.count}`;
    }
  }
  
  componentWillUnmount() {
    // Clean up timer when component unmounts
    clearInterval(this.timer);
  }
  
  render() {
    return <div>Count: {this.state.count}</div>;
  }
}
```

---

### 14. How do hooks replace lifecycle methods in functional components?

**Confident Answer:** Hooks allow functional components to have the same capabilities as class components, but with cleaner syntax.

**Lifecycle to Hooks Conversion:**

**componentDidMount → useEffect with empty dependency:**
```jsx
// Class Component
componentDidMount() {
  fetchData();
}

// Functional Component
useEffect(() => {
  fetchData();
}, []); // Empty array = runs once on mount
```

**componentDidUpdate → useEffect with dependencies:**
```jsx
// Class Component
componentDidUpdate(prevProps) {
  if (prevProps.userId !== this.props.userId) {
    fetchUserData(this.props.userId);
  }
}

// Functional Component
useEffect(() => {
  fetchUserData(userId);
}, [userId]); // Runs when userId changes
```

**componentWillUnmount → useEffect cleanup:**
```jsx
// Class Component
componentWillUnmount() {
  clearInterval(this.timer);
}

// Functional Component
useEffect(() => {
  const timer = setInterval(() => {
    // Timer logic
  }, 1000);
  
  return () => {
    clearInterval(timer); // Cleanup function
  };
}, []);
```

**Complete Conversion Example:**
```jsx
// Class Component
class UserData extends React.Component {
  constructor(props) {
    super(props);
    this.state = { user: null, loading: true };
  }
  
  componentDidMount() {
    this.fetchUser();
  }
  
  componentDidUpdate(prevProps) {
    if (prevProps.userId !== this.props.userId) {
      this.fetchUser();
    }
  }
  
  fetchUser = () => {
    this.setState({ loading: true });
    fetch(`/api/users/${this.props.userId}`)
      .then(response => response.json())
      .then(user => this.setState({ user, loading: false }));
  }
  
  render() {
    if (this.state.loading) return <div>Loading...</div>;
    return <div>{this.state.user?.name}</div>;
  }
}

// Functional Component with Hooks
function UserData({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    setLoading(true);
    fetch(`/api/users/${userId}`)
      .then(response => response.json())
      .then(userData => {
        setUser(userData);
        setLoading(false);
      });
  }, [userId]); // Runs on mount and when userId changes
  
  if (loading) return <div>Loading...</div>;
  return <div>{user?.name}</div>;
}
```

---

### 15. What is React.memo and why do we use it?

**Confident Answer:** React.memo is a higher-order component that memoizes the result of a component. It only re-renders if props have changed, improving performance.

**Without React.memo (Always Re-renders):**
```jsx
function ExpensiveChild({ name, age }) {
  console.log('Child component re-rendered'); // This runs on every parent render
  
  // Expensive calculation
  const expensiveValue = useMemo(() => {
    let result = 0;
    for (let i = 0; i < 1000000; i++) {
      result += i;
    }
    return result;
  }, []);
  
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
      <p>Expensive calculation: {expensiveValue}</p>
    </div>
  );
}

function Parent() {
  const [count, setCount] = useState(0);
  const [user] = useState({ name: 'John', age: 25 });
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>
      <ExpensiveChild name={user.name} age={user.age} />
    </div>
  );
}
```

**With React.memo (Only Re-renders When Props Change):**
```jsx
const ExpensiveChild = React.memo(function ExpensiveChild({ name, age }) {
  console.log('Child component re-rendered'); // Only runs when name or age changes
  
  const expensiveValue = useMemo(() => {
    let result = 0;
    for (let i = 0; i < 1000000; i++) {
      result += i;
    }
    return result;
  }, []);
  
  return (
    <div>
      <p>Name: {name}</p>
      <p>Age: {age}</p>
      <p>Expensive calculation: {expensiveValue}</p>
    </div>
  );
});
```

**Custom Comparison with React.memo:**
```jsx
const UserCard = React.memo(function UserCard({ user, theme }) {
  return (
    <div className={theme}>
      <h3>{user.name}</h3>
      <p>{user.email}</p>
    </div>
  );
}, (prevProps, nextProps) => {
  // Custom comparison function
  // Return true if props are equal (skip re-render)
  // Return false if props are different (re-render)
  return (
    prevProps.user.id === nextProps.user.id &&
    prevProps.theme === nextProps.theme
  );
});
```

**When to Use React.memo:**
- Component re-renders frequently with same props
- Component has expensive calculations
- Component is deep in the component tree
- **Don't overuse:** Only use when you have performance issues

---

### 16. What is the difference between React.Fragment and a div wrapper?

**Confident Answer:** React.Fragment lets you group multiple elements without adding extra DOM nodes, while div creates an actual DOM element.

**Problem with Extra div:**
```jsx
function UserInfo() {
  return (
    <div> {/* Extra div wrapper */}
      <h1>John Doe</h1>
      <p>Software Developer</p>
    </div>
  );
}

function App() {
  return (
    <div className="container">
      <UserInfo />
    </div>
  );
}

// Resulting DOM:
// <div class="container">
//   <div> <!-- Extra unnecessary div -->
//     <h1>John Doe</h1>
//     <p>Software Developer</p>
//   </div>
// </div>
```

**Solution with React.Fragment:**
```jsx
function UserInfo() {
  return (
    <React.Fragment> {/* No extra DOM node */}
      <h1>John Doe</h1>
      <p>Software Developer</p>
    </React.Fragment>
  );
}

// Short syntax
function UserInfo() {
  return (
    <>
      <h1>John Doe</h1>
      <p>Software Developer</p>
    </>
  );
}

// Resulting DOM:
// <div class="container">
//   <h1>John Doe</h1>
//   <p>Software Developer</p>
// </div>
```

**Fragment with Key (for lists):**
```jsx
function UserList({ users }) {
  return (
    <ul>
      {users.map(user => (
        <React.Fragment key={user.id}>
          <li>{user.name}</li>
          <li>{user.email}</li>
        </React.Fragment>
      ))}
    </ul>
  );
}
```

**Benefits of Fragment:**
- **Cleaner DOM:** No extra wrapper elements
- **Better Performance:** Fewer DOM nodes
- **CSS-friendly:** Avoids layout issues
- **Semantic HTML:** Maintains proper HTML structure

---

## 🔹 State Management

### 17. How do you share state between components?

**Confident Answer:** There are several ways to share state between components depending on the relationship and complexity.

**1. Props (Parent to Child):**
```jsx
function Parent() {
  const [user, setUser] = useState({ name: 'John', age: 25 });
  
  return (
    <div>
      <ChildA user={user} />
      <ChildB user={user} />
    </div>
  );
}

function ChildA({ user }) {
  return <h1>Hello, {user.name}!</h1>;
}

function ChildB({ user }) {
  return <p>Age: {user.age}</p>;
}
```

**2. Callback Functions (Child to Parent):**
```jsx
function Parent() {
  const [message, setMessage] = useState('');
  
  const handleMessage = (msg) => {
    setMessage(msg);
  };
  
  return (
    <div>
      <p>Message from child: {message}</p>
      <Child onMessage={handleMessage} />
    </div>
  );
}

function Child({ onMessage }) {
  const sendMessage = () => {
    onMessage('Hello from child!');
  };
  
  return <button onClick={sendMessage}>Send Message</button>;
}
```

**3. Lifting State Up (Sibling Components):**
```jsx
function Parent() {
  const [sharedData, setSharedData] = useState('');
  
  return (
    <div>
      <SiblingA data={sharedData} onDataChange={setSharedData} />
      <SiblingB data={sharedData} />
    </div>
  );
}

function SiblingA({ data, onDataChange }) {
  return (
    <input 
      value={data}
      onChange={(e) => onDataChange(e.target.value)}
      placeholder="Type something..."
    />
  );
}

function SiblingB({ data }) {
  return <p>You typed: {data}</p>;
}
```

**4. Context API (Deep Component Tree):**
```jsx
// Create Context
const UserContext = React.createContext();

// Provider Component
function UserProvider({ children }) {
  const [user, setUser] = useState({ name: 'John', age: 25 });
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
}

// Deep Child Component
function DeepChild() {
  const { user, setUser } = useContext(UserContext);
  
  return (
    <div>
      <p>Hello, {user.name}!</p>
      <button onClick={() => setUser({ ...user, age: user.age + 1 })}>
        Age: {user.age}
      </button>
    </div>
  );
}

// App
function App() {
  return (
    <UserProvider>
      <div>
        <Header />
        <Main>
          <DeepChild />
        </Main>
      </div>
    </UserProvider>
  );
}
```

---

### 18. What is Context API? When to use it?

**Confident Answer:** Context API provides a way to pass data through the component tree without having to pass props down manually at every level. It's React's built-in state management solution.

**Creating and Using Context:**
```jsx
// 1. Create Context
const ThemeContext = React.createContext();

// 2. Create Provider Component
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// 3. Use Context in Components
function Header() {
  const { theme, toggleTheme } = useContext(ThemeContext);
  
  return (
    <header className={`header-${theme}`}>
      <h1>My App</h1>
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'dark' : 'light'} mode
      </button>
    </header>
  );
}

function Content() {
  const { theme } = useContext(ThemeContext);
  
  return (
    <main className={`content-${theme}`}>
      <p>This content adapts to the theme!</p>
    </main>
  );
}

// 4. App Component
function App() {
  return (
    <ThemeProvider>
      <Header />
      <Content />
    </ThemeProvider>
  );
}
```

**Multiple Contexts Example:**
```jsx
// User Context
const UserContext = React.createContext();
const CartContext = React.createContext();

function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  );
}

function CartProvider({ children }) {
  const [cart, setCart] = useState([]);
  
  const addToCart = (item) => {
    setCart(prev => [...prev, item]);
  };
  
  return (
    <CartContext.Provider value={{ cart, addToCart }}>
      {children}
    </CartContext.Provider>
  );
}

function App() {
  return (
    <UserProvider>
      <CartProvider>
        <Header />
        <ProductList />
        <Cart />
      </CartProvider>
    </UserProvider>
  );
}
```

**When to Use Context API:**
- **Theme switching** (light/dark mode)
- **User authentication** state
- **Language/localization** settings
- **Shopping cart** data
- **Avoid prop drilling** through many levels

**When NOT to Use Context:**
- **Simple parent-child** communication (use props)
- **Frequently changing** data (performance issues)
- **Complex state logic** (consider Redux instead)

---

### 19. What is Redux? Core concepts: store, reducer, actions.

**Confident Answer:** Redux is a predictable state container for JavaScript applications. It helps manage application state in a centralized store with a unidirectional data flow.

**Core Concepts:**

**1. Store - Single Source of Truth:**
```jsx
import { createStore } from 'redux';

// The store holds the entire application state
const store = createStore(rootReducer);

// Access state
const currentState = store.getState();

// Subscribe to changes
store.subscribe(() => {
  console.log('State changed:', store.getState());
});
```

**2. Actions - What Happened:**
```jsx
// Action Types (constants)
const ADD_TODO = 'ADD_TODO';
const TOGGLE_TODO = 'TOGGLE_TODO';
const DELETE_TODO = 'DELETE_TODO';

// Action Creators (functions that return actions)
const addTodo = (text) => ({
  type: ADD_TODO,
  payload: {
    id: Date.now(),
    text,
    completed: false
  }
});

const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
  payload: { id }
});

const deleteTodo = (id) => ({
  type: DELETE_TODO,
  payload: { id }
});
```

**3. Reducers - How State Changes:**
```jsx
// Initial State
const initialState = {
  todos: [],
  filter: 'ALL'
};

// Reducer Function (pure function)
function todoReducer(state = initialState, action) {
  switch (action.type) {
    case ADD_TODO:
      return {
        ...state,
        todos: [...state.todos, action.payload]
      };
      
    case TOGGLE_TODO:
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload.id
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      };
      
    case DELETE_TODO:
      return {
        ...state,
        todos: state.todos.filter(todo => todo.id !== action.payload.id)
      };
      
    default:
      return state;
  }
}
```

**Complete Redux Example:**
```jsx
import React from 'react';
import { createStore } from 'redux';
import { Provider, useSelector, useDispatch } from 'react-redux';

// Component using Redux
function TodoApp() {
  const todos = useSelector(state => state.todos);
  const dispatch = useDispatch();
  const [inputText, setInputText] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (inputText.trim()) {
      dispatch(addTodo(inputText));
      setInputText('');
    }
  };
  
  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input
          value={inputText}
          onChange={(e) => setInputText(e.target.value)}
          placeholder="Add a todo..."
        />
        <button type="submit">Add</button>
      </form>
      
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <span
              style={{
                textDecoration: todo.completed ? 'line-through' : 'none'
              }}
              onClick={() => dispatch(toggleTodo(todo.id))}
            >
              {todo.text}
            </span>
            <button onClick={() => dispatch(deleteTodo(todo.id))}>
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}

// App with Redux Provider
function App() {
  const store = createStore(todoReducer);
  
  return (
    <Provider store={store}>
      <TodoApp />
    </Provider>
  );
}
```

**Redux Data Flow:**
1. Component dispatches an action
2. Store passes action to reducer
3. Reducer returns new state
4. Store updates and notifies subscribers
5. Component re-renders with new state

---

### 20. Difference between Context API and Redux.

**Confident Answer:** Both manage state, but they serve different purposes and have different strengths.

**Context API:**

**Pros:**
- Built into React (no extra dependency)
- Simple setup for basic state sharing
- Great for theme, auth, language switching
- No boilerplate code

**Cons:**
- Can cause performance issues with frequent updates
- No time-travel debugging
- Limited tools for complex state logic

**Context API Example:**
```jsx
// Simple and lightweight
const UserContext = React.createContext();

function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  
  const login = (userData) => setUser(userData);
  const logout = () => setUser(null);
  
  return (
    <UserContext.Provider value={{ user, login, logout }}>
      {children}
    </UserContext.Provider>
  );
}
```

**Redux:**

**Pros:**
- Predictable state updates
- Excellent developer tools (time-travel debugging)
- Middleware support (logging, async actions)
- Great for complex applications

**Cons:**
- More boilerplate code
- Additional dependency
- Learning curve
- Can be overkill for simple apps

**Redux Example:**
```jsx
// More structured but verbose
const initialState = { user: null };

function userReducer(state = initialState, action) {
  switch (action.type) {
    case 'LOGIN':
      return { ...state, user: action.payload };
    case 'LOGOUT':
      return { ...state, user: null };
    default:
      return state;
  }
}

const login = (userData) => ({ type: 'LOGIN', payload: userData });
const logout = () => ({ type: 'LOGOUT' });
```

**When to Use Each:**

**Use Context API for:**
- Small to medium applications
- Simple state sharing (theme, auth status)
- Component-specific state
- Learning React state management

**Use Redux for:**
- Large, complex applications
- Complex state interactions
- Need for debugging tools
- Team development with strict patterns

**Comparison Table:**

| Feature | Context API | Redux |
|---------|-------------|-------|
| Setup | Simple | Complex |
| Learning Curve | Easy | Moderate |
| Performance | Can be slow | Optimized |
| Developer Tools | Basic | Excellent |
| Middleware | No | Yes |
| Community | Smaller | Large |
| Bundle Size | 0 (built-in) | ~8KB |

**Can Use Both Together:**
```jsx
// Use Context for simple state
function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Use Redux for complex state
function App() {
  return (
    <Provider store={reduxStore}>
      <ThemeProvider>
        <AppContent />
      </ThemeProvider>
    </Provider>
  );
}
```

---

## 🔹 React Routing

### 21. What is React Router?

**Confident Answer:** React Router is a standard library for routing in React applications. It enables navigation between different components/pages while maintaining the SPA (Single Page Application) behavior.

**Installation and Basic Setup:**
```bash
npm install react-router-dom
```

**Basic Router Setup:**
```jsx
import React from 'react';
import {
  BrowserRouter as Router,
  Routes,
  Route,
  Link,
  useNavigate
} from 'react-router-dom';

// Page Components
function Home() {
  return <h1>Home Page</h1>;
}

function About() {
  return <h1>About Page</h1>;
}

function Contact() {
  return <h1>Contact Page</h1>;
}

function NotFound() {
  return <h1>404 - Page Not Found</h1>;
}

// App with Routing
function App() {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link> | 
        <Link to="/about">About</Link> | 
        <Link to="/contact">Contact</Link>
      </nav>
      
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </Router>
  );
}
```

**Dynamic Routes with Parameters:**
```jsx
function UserProfile() {
  const { userId } = useParams();
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    fetch(`/api/users/${userId}`)
      .then(response => response.json())
      .then(setUser);
  }, [userId]);
  
  return (
    <div>
      <h1>User Profile</h1>
      {user ? (
        <div>
          <p>Name: {user.name}</p>
          <p>Email: {user.email}</p>
        </div>
      ) : (
        <p>Loading...</p>
      )}
    </div>
  );
}

// Route with parameter
<Route path="/users/:userId" element={<UserProfile />} />
```

**Programmatic Navigation:**
```jsx
function LoginForm() {
  const navigate = useNavigate();
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  
  const handleLogin = (e) => {
    e.preventDefault();
    
    // Simulate login
    if (email && password) {
      // Navigate to dashboard after successful login
      navigate('/dashboard');
    }
  };
  
  return (
    <form onSubmit={handleLogin}>
      <input
        type="email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Email"
        required
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        placeholder="Password"
        required
      />
      <button type="submit">Login</button>
    </form>
  );
}
```

---

### 22. How do you implement navigation between pages?

**Confident Answer:** React Router provides several ways to navigate between pages:

**1. Link Component (Declarative):**
```jsx
import { Link } from 'react-router-dom';

function Navigation() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/products">Products</Link>
      <Link to="/about">About</Link>
      
      {/* Link with state */}
      <Link 
        to="/products" 
        state={{ fromNavbar: true }}
      >
        Products with State
      </Link>
      
      {/* Link with query parameters */}
      <Link to="/search?q=react&category=tutorials">
        Search React Tutorials
      </Link>
    </nav>
  );
}
```

**2. useNavigate Hook (Programmatic):**
```jsx
import { useNavigate } from 'react-router-dom';

function ProductCard({ product }) {
  const navigate = useNavigate();
  
  const handleViewDetails = () => {
    // Navigate with state
    navigate(`/products/${product.id}`, {
      state: { product }
    });
  };
  
  const handleEditProduct = () => {
    // Navigate and replace current entry in history
    navigate(`/products/${product.id}/edit`, { replace: true });
  };
  
  const goBack = () => {
    // Go back in history
    navigate(-1);
  };
  
  return (
    <div className="product-card">
      <h3>{product.name}</h3>
      <p>{product.description}</p>
      <button onClick={handleViewDetails}>View Details</button>
      <button onClick={handleEditProduct}>Edit</button>
      <button onClick={goBack}>Go Back</button>
    </div>
  );
}
```

**3. Nested Routing:**
```jsx
function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      <nav>
        <Link to="profile">Profile</Link>
        <Link to="settings">Settings</Link>
        <Link to="orders">Orders</Link>
      </nav>
      
      {/* Nested routes render here */}
      <Outlet />
    </div>
  );
}

function Profile() {
  return <h2>User Profile</h2>;
}

function Settings() {
  return <h2>User Settings</h2>;
}

// Route Configuration
function App() {
  return (
    <Router>
      <Routes>
        <Route path="/dashboard" element={<Dashboard />}>
          <Route path="profile" element={<Profile />} />
          <Route path="settings" element={<Settings />} />
          <Route path="orders" element={<Orders />} />
        </Route>
      </Routes>
    </Router>
  );
}
```

**4. Protected Routes:**
```jsx
function ProtectedRoute({ children }) {
  const isAuthenticated = useAuth(); // Custom hook for auth
  
  if (!isAuthenticated) {
    // Redirect to login page
    return <Navigate to="/login" replace />;
  }
  
  return children;
}

// Usage
<Route 
  path="/dashboard" 
  element={
    <ProtectedRoute>
      <Dashboard />
    </ProtectedRoute>
  } 
/>
```

---

### 23. Difference between <Link> and <a> tag?

**Confident Answer:** Both create clickable links, but they work very differently in React applications.

**<a> Tag (Traditional HTML):**
- Causes full page reload
- Loses React component state
- Slower navigation
- Breaks SPA experience

**<Link> Component (React Router):**
- Client-side navigation (no page reload)
- Preserves React component state
- Faster navigation
- Maintains SPA experience

**Example Comparison:**
```jsx
function NavigationComparison() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <h1>Count: {count}</h1>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
      
      <nav>
        {/* ❌ BAD: Full page reload, loses count state */}
        <a href="/about">About (Full Reload)</a>
        
        {/* ✅ GOOD: Client-side navigation, preserves count state */}
        <Link to="/about">About (SPA Navigation)</Link>
      </nav>
    </div>
  );
}
```

**When to Use Each:**

**Use <Link> for:**
- Internal navigation within your React app
- Preserving component state
- Better performance and user experience

**Use <a> for:**
- External links to other websites
- Downloading files
- Email links (mailto:)
- Phone links (tel:)

**Example:**
```jsx
function Navigation() {
  return (
    <nav>
      {/* Internal navigation - use Link */}
      <Link to="/home">Home</Link>
      <Link to="/products">Products</Link>
      
      {/* External links - use a tag */}
      <a href="https://google.com" target="_blank" rel="noopener noreferrer">
        Google
      </a>
      <a href="mailto:contact@example.com">Contact Us</a>
      <a href="tel:+1234567890">Call Us</a>
      
      {/* File download - use a tag */}
      <a href="/assets/brochure.pdf" download>
        Download Brochure
      </a>
    </nav>
  );
}
```

---

## 🔹 Performance & Optimization

### 24. What is lazy loading in React?

**Confident Answer:** Lazy loading is a technique to load components only when they're needed, rather than loading everything upfront. This improves initial page load performance.

**React.lazy() for Component Lazy Loading:**
```jsx
import React, { Suspense, lazy } from 'react';

// Lazy load components
const Home = lazy(() => import('./components/Home'));
const About = lazy(() => import('./components/About'));
const Dashboard = lazy(() => import('./components/Dashboard'));

function App() {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/dashboard">Dashboard</Link>
      </nav>
      
      <Suspense fallback={<div>Loading page...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/dashboard" element={<Dashboard />} />
        </Routes>
      </Suspense>
    </Router>
  );
}
```

**Custom Loading Component:**
```jsx
function LoadingSpinner() {
  return (
    <div className="loading-container">
      <div className="spinner"></div>
      <p>Loading...</p>
    </div>
  );
}

// Usage with custom fallback
<Suspense fallback={<LoadingSpinner />}>
  <LazyComponent />
</Suspense>
```

**Lazy Loading with Error Boundaries:**
```jsx
class LazyErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Lazy loading error:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <div>Something went wrong while loading the component.</div>;
    }
    
    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <LazyErrorBoundary>
      <Suspense fallback={<LoadingSpinner />}>
        <LazyComponent />
      </Suspense>
    </LazyErrorBoundary>
  );
}
```

---

### 25. What is code splitting?

**Confident Answer:** Code splitting is the practice of splitting your bundle into smaller chunks that can be loaded on demand. This reduces the initial bundle size and improves loading performance.

**Route-based Code Splitting:**
```jsx
import { lazy, Suspense } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';

// Split each route into separate chunks
const Home = lazy(() => import('./pages/Home'));
const Products = lazy(() => import('./pages/Products'));
const Dashboard = lazy(() => import('./pages/Dashboard'));
const AdminPanel = lazy(() => import('./pages/AdminPanel'));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<div>Loading...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/products" element={<Products />} />
          <Route path="/dashboard" element={<Dashboard />} />
          <Route path="/admin" element={<AdminPanel />} />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

**Component-based Code Splitting:**
```jsx
function ProductDetails({ productId }) {
  const [showReviews, setShowReviews] = useState(false);
  const [ReviewsComponent, setReviewsComponent] = useState(null);
  
  const loadReviews = async () => {
    if (!ReviewsComponent) {
      // Load reviews component only when needed
      const { default: Reviews } = await import('./Reviews');
      setReviewsComponent(() => Reviews);
    }
    setShowReviews(true);
  };
  
  return (
    <div>
      <h1>Product Details</h1>
      <button onClick={loadReviews}>
        Show Reviews
      </button>
      
      {showReviews && ReviewsComponent && (
        <Suspense fallback={<div>Loading reviews...</div>}>
          <ReviewsComponent productId={productId} />
        </Suspense>
      )}
    </div>
  );
}
```

**Library Code Splitting:**
```jsx
function ChartComponent({ data }) {
  const [ChartLib, setChartLib] = useState(null);
  
  useEffect(() => {
    // Load heavy charting library only when component mounts
    const loadChart = async () => {
      const { Chart } = await import('chart.js');
      setChartLib(Chart);
    };
    
    loadChart();
  }, []);
  
  if (!ChartLib) {
    return <div>Loading chart...</div>;
  }
  
  return <ChartLib data={data} />;
}
```

---

### 26. How to improve performance in React? (memoization, useCallback, useMemo, key props)

**Confident Answer:** React performance optimization involves preventing unnecessary re-renders and expensive calculations.

**1. React.memo() - Component Memoization:**
```jsx
// Expensive component that should only re-render when props change
const ExpensiveComponent = React.memo(function ExpensiveComponent({ data, theme }) {
  console.log('Expensive component rendered');
  
  // Expensive calculation
  const processedData = data.map(item => ({
    ...item,
    processed: item.value * 100
  }));
  
  return (
    <div className={`component-${theme}`}>
      {processedData.map(item => (
        <div key={item.id}>{item.processed}</div>
      ))}
    </div>
  );
});

function Parent() {
  const [count, setCount] = useState(0);
  const [data] = useState([{ id: 1, value: 10 }]);
  const [theme] = useState('light');
  
  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>
      {/* ExpensiveComponent only re-renders when data or theme changes */}
      <ExpensiveComponent data={data} theme={theme} />
    </div>
  );
}
```

**2. useMemo() - Value Memoization:**
```jsx
function ProductList({ products, filters }) {
  // Expensive filtering operation - only recalculate when dependencies change
  const filteredProducts = useMemo(() => {
    console.log('Filtering products...');
    return products.filter(product => {
      return (
        product.price >= filters.minPrice &&
        product.price <= filters.maxPrice &&
        product.category === filters.category
      );
    });
  }, [products, filters.minPrice, filters.maxPrice, filters.category]);
  
  // Expensive calculation - only recalculate when filtered products change
  const averagePrice = useMemo(() => {
    console.log('Calculating average price...');
    if (filteredProducts.length === 0) return 0;
    
    const total = filteredProducts.reduce((sum, product) => sum + product.price, 0);
    return total / filteredProducts.length;
  }, [filteredProducts]);
  
  return (
    <div>
      <p>Average Price: ${averagePrice.toFixed(2)}</p>
      {filteredProducts.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}
```

**3. useCallback() - Function Memoization:**
```jsx
function TodoList({ todos }) {
  const [filter, setFilter] = useState('all');
  
  // Memoize callback to prevent child re-renders
  const handleToggle = useCallback((todoId) => {
    setTodos(prevTodos =>
      prevTodos.map(todo =>
        todo.id === todoId 
          ? { ...todo, completed: !todo.completed }
          : todo
      )
    );
  }, []); // No dependencies since we use functional update
  
  const handleDelete = useCallback((todoId) => {
    setTodos(prevTodos => prevTodos.filter(todo => todo.id !== todoId));
  }, []);
  
  const filteredTodos = useMemo(() => {
    switch (filter) {
      case 'active':
        return todos.filter(todo => !todo.completed);
      case 'completed':
        return todos.filter(todo => todo.completed);
      default:
        return todos;
    }
  }, [todos, filter]);
  
  return (
    <div>
      <FilterButtons filter={filter} setFilter={setFilter} />
      {filteredTodos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={handleToggle}  // Memoized callback
          onDelete={handleDelete} // Memoized callback
        />
      ))}
    </div>
  );
}

// Child component with React.memo
const TodoItem = React.memo(function TodoItem({ todo, onToggle, onDelete }) {
  console.log(`TodoItem ${todo.id} rendered`);
  
  return (
    <div>
      <span onClick={() => onToggle(todo.id)}>
        {todo.text}
      </span>
      <button onClick={() => onDelete(todo.id)}>Delete</button>
    </div>
  );
});
```

**4. Proper Key Props:**
```jsx
function MessageList({ messages }) {
  return (
    <div>
      {messages.map(message => (
        // ✅ GOOD: Stable, unique key
        <MessageItem key={message.id} message={message} />
        
        // ❌ BAD: Index as key (causes issues when list changes)
        // <MessageItem key={index} message={message} />
      ))}
    </div>
  );
}
```

**5. Virtualization for Large Lists:**
```jsx
import { FixedSizeList as List } from 'react-window';

function LargeList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style}>
      Item {items[index].name}
    </div>
  );
  
  return (
    <List
      height={400}        // Container height
      itemCount={items.length}
      itemSize={50}       // Each item height
      width="100%"
    >
      {Row}
    </List>
  );
}
```

**Performance Optimization Checklist:**
- ✅ Use React.memo for expensive components
- ✅ Use useMemo for expensive calculations
- ✅ Use useCallback for event handlers passed to memoized components
- ✅ Use proper keys in lists
- ✅ Avoid creating objects/functions in render
- ✅ Consider virtualization for large lists
- ✅ Use React DevTools Profiler to identify performance bottlenecks

---

## 🔹 Other Practical Topics

### 27. What is the difference between SSR (Server-Side Rendering) and CSR (Client-Side Rendering)?

**Confident Answer:** SSR and CSR are different approaches to rendering React applications, each with specific advantages.

**Client-Side Rendering (CSR) - Traditional React:**

**How it works:**
1. Server sends minimal HTML with JavaScript bundle
2. JavaScript executes in browser
3. React components render on client side

**Example CSR Flow:**
```jsx
// index.html (minimal)
<!DOCTYPE html>
<html>
<head>
    <title>My React App</title>
</head>
<body>
    <div id="root"></div> <!-- Empty initially -->
    <script src="/bundle.js"></script>
</body>
</html>

// React renders everything client-side
function App() {
  const [users, setUsers] = useState([]);
  
  useEffect(() => {
    // API call happens in browser
    fetch('/api/users')
      .then(response => response.json())
      .then(setUsers);
  }, []);
  
  return (
    <div>
      <h1>User List</h1>
      {users.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
}
```

**Server-Side Rendering (SSR) - Next.js Example:**

**How it works:**
1. Server renders React components to HTML
2. Full HTML sent to browser
3. JavaScript hydrates the static HTML

**Example SSR with Next.js:**
```jsx
// pages/users.js (Next.js)
function UsersPage({ users }) {
  return (
    <div>
      <h1>User List</h1>
      {users.map(user => (
        <UserCard key={user.id} user={user} />
      ))}
    </div>
  );
}

// This runs on the server
export async function getServerSideProps() {
  // API call happens on server
  const response = await fetch('http://localhost:3000/api/users');
  const users = await response.json();
  
  return {
    props: { users } // Pre-rendered with data
  };
}

export default UsersPage;
```

**Comparison Table:**

| Feature | CSR | SSR |
|---------|-----|-----|
| **Initial Load** | Slower (needs JS) | Faster (pre-rendered) |
| **SEO** | Poor | Excellent |
| **Time to Interactive** | Slower | Faster |
| **Server Load** | Lower | Higher |
| **Caching** | Easy | Complex |
| **Development** | Simpler | More Complex |

**When to Use Each:**

**CSR is good for:**
- Dashboard/admin panels
- Apps behind authentication
- Interactive applications
- Lower server costs

**SSR is good for:**
- E-commerce sites
- Blogs/content sites
- SEO-critical applications
- Better initial user experience

---

### 28. What are higher-order components (HOC)?

**Confident Answer:** HOC is a pattern where a function takes a component and returns a new component with additional functionality. It's a way to reuse component logic.

**Basic HOC Example:**
```jsx
// HOC function
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const [isAuthenticated, setIsAuthenticated] = useState(false);
    const [loading, setLoading] = useState(true);
    
    useEffect(() => {
      // Check authentication
      const checkAuth = async () => {
        try {
          const response = await fetch('/api/auth/check');
          setIsAuthenticated(response.ok);
        } catch (error) {
          setIsAuthenticated(false);
        } finally {
          setLoading(false);
        }
      };
      
      checkAuth();
    }, []);
    
    if (loading) {
      return <div>Loading...</div>;
    }
    
    if (!isAuthenticated) {
      return <div>Please log in to access this page.</div>;
    }
    
    // Render original component with all props
    return <WrappedComponent {...props} />;
  };
}

// Regular component
function Dashboard({ user }) {
  return (
    <div>
      <h1>Dashboard</h1>
      <p>Welcome, {user.name}!</p>
    </div>
  );
}

// Enhanced component with authentication
const AuthenticatedDashboard = withAuth(Dashboard);

// Usage
function App() {
  return (
    <div>
      <AuthenticatedDashboard user={{ name: 'John' }} />
    </div>
  );
}
```

**HOC with Configuration:**
```jsx
// HOC with options
function withLoading(loadingMessage = 'Loading...') {
  return function(WrappedComponent) {
    return function LoadingComponent({ isLoading, ...props }) {
      if (isLoading) {
        return <div>{loadingMessage}</div>;
      }
      
      return <WrappedComponent {...props} />;
    };
  };
}

// Usage with different loading messages
const UserListWithLoading = withLoading('Loading users...')(UserList);
const ProductListWithLoading = withLoading('Loading products...')(ProductList);

function App() {
  const [usersLoading, setUsersLoading] = useState(true);
  const [productsLoading, setProductsLoading] = useState(true);
  
  return (
    <div>
      <UserListWithLoading isLoading={usersLoading} users={users} />
      <ProductListWithLoading isLoading={productsLoading} products={products} />
    </div>
  );
}
```

**Multiple HOCs Composition:**
```jsx
// Multiple HOCs
const withAuth = (Component) => (props) => {
  // Authentication logic
  return <Component {...props} />;
};

const withLogging = (Component) => (props) => {
  useEffect(() => {
    console.log('Component mounted:', Component.name);
  }, []);
  
  return <Component {...props} />;
};

const withErrorBoundary = (Component) => {
  return class extends React.Component {
    state = { hasError: false };
    
    componentDidCatch(error) {
      this.setState({ hasError: true });
      console.error('Error caught:', error);
    }
    
    render() {
      if (this.state.hasError) {
        return <div>Something went wrong!</div>;
      }
      
      return <Component {...this.props} />;
    }
  };
};

// Compose multiple HOCs
const enhance = (Component) => 
  withErrorBoundary(
    withAuth(
      withLogging(Component)
    )
  );

const EnhancedDashboard = enhance(Dashboard);
```

**HOC vs Hooks (Modern Approach):**
```jsx
// Old way: HOC
const withWindowSize = (Component) => {
  return function WindowSizeComponent(props) {
    const [windowSize, setWindowSize] = useState({
      width: window.innerWidth,
      height: window.innerHeight
    });
    
    useEffect(() => {
      const handleResize = () => {
        setWindowSize({
          width: window.innerWidth,
          height: window.innerHeight
        });
      };
      
      window.addEventListener('resize', handleResize);
      return () => window.removeEventListener('resize', handleResize);
    }, []);
    
    return <Component {...props} windowSize={windowSize} />;
  };
};

// Modern way: Custom Hook
function useWindowSize() {
  const [windowSize, setWindowSize] = useState({
    width: window.innerWidth,
    height: window.innerHeight
  });
  
  useEffect(() => {
    const handleResize = () => {
      setWindowSize({
        width: window.innerWidth,
        height: window.innerHeight
      });
    };
    
    window.addEventListener('resize', handleResize);
    return () => window.removeEventListener('resize', handleResize);
  }, []);
  
  return windowSize;
}

// Usage with custom hook (preferred)
function ResponsiveComponent() {
  const { width, height } = useWindowSize();
  
  return (
    <div>
      <p>Width: {width}, Height: {height}</p>
    </div>
  );
}
```

---

### 29. How do you handle forms in React?

**Confident Answer:** React provides multiple ways to handle forms, with controlled components being the recommended approach.

**Controlled Form (Recommended):**
```jsx
function ContactForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: '',
    subscribe: false,
    country: ''
  });
  
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  // Handle input changes
  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    
    setFormData(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
    
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: ''
      }));
    }
  };
  
  // Form validation
  const validateForm = () => {
    const newErrors = {};
    
    if (!formData.name.trim()) {
      newErrors.name = 'Name is required';
    }
    
    if (!formData.email.trim()) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email is invalid';
    }
    
    if (!formData.message.trim()) {
      newErrors.message = 'Message is required';
    } else if (formData.message.length < 10) {
      newErrors.message = 'Message must be at least 10 characters';
    }
    
    return newErrors;
  };
  
  // Handle form submission
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    const newErrors = validateForm();
    
    if (Object.keys(newErrors).length > 0) {
      setErrors(newErrors);
      return;
    }
    
    setIsSubmitting(true);
    
    try {
      const response = await fetch('/api/contact', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(formData),
      });
      
      if (response.ok) {
        alert('Message sent successfully!');
        setFormData({
          name: '',
          email: '',
          message: '',
          subscribe: false,
          country: ''
        });
      } else {
        throw new Error('Failed to send message');
      }
    } catch (error) {
      alert('Error: ' + error.message);
    } finally {
      setIsSubmitting(false);
    }
  };
  
  return (
    <form onSubmit={handleSubmit} className="contact-form">
      {/* Text Input */}
      <div className="form-group">
        <label htmlFor="name">Name:</label>
        <input
          type="text"
          id="name"
          name="name"
          value={formData.name}
          onChange={handleChange}
          className={errors.name ? 'error' : ''}
          disabled={isSubmitting}
        />
        {errors.name && <span className="error-message">{errors.name}</span>}
      </div>
      
      {/* Email Input */}
      <div className="form-group">
        <label htmlFor="email">Email:</label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          className={errors.email ? 'error' : ''}
          disabled={isSubmitting}
        />
        {errors.email && <span className="error-message">{errors.email}</span>}
      </div>
      
      {/* Textarea */}
      <div className="form-group">
        <label htmlFor="message">Message:</label>
        <textarea
          id="message"
          name="message"
          value={formData.message}
          onChange={handleChange}
          rows="5"
          className={errors.message ? 'error' : ''}
          disabled={isSubmitting}
        />
        {errors.message && <span className="error-message">{errors.message}</span>}
      </div>
      
      {/* Select */}
      <div className="form-group">
        <label htmlFor="country">Country:</label>
        <select
          id="country"
          name="country"
          value={formData.country}
          onChange={handleChange}
          disabled={isSubmitting}
        >
          <option value="">Select a country</option>
          <option value="US">United States</option>
          <option value="CA">Canada</option>
          <option value="UK">United Kingdom</option>
        </select>
      </div>
      
      {/* Checkbox */}
      <div className="form-group">
        <label>
          <input
            type="checkbox"
            name="subscribe"
            checked={formData.subscribe}
            onChange={handleChange}
            disabled={isSubmitting}
          />
          Subscribe to newsletter
        </label>
      </div>
      
      {/* Submit Button */}
      <button 
        type="submit" 
        disabled={isSubmitting}
        className="submit-button"
      >
        {isSubmitting ? 'Sending...' : 'Send Message'}
      </button>
    </form>
  );
}
```

**Custom Form Hook for Reusability:**
```jsx
// Custom hook for form handling
function useForm(initialValues, validationRules) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    
    setValues(prev => ({
      ...prev,
      [name]: type === 'checkbox' ? checked : value
    }));
    
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: ''
      }));
    }
  };
  
  const validate = () => {
    const newErrors = {};
    
    Object.keys(validationRules).forEach(field => {
      const rule = validationRules[field];
      const value = values[field];
      
      if (rule.required && !value?.trim()) {
        newErrors[field] = `${field} is required`;
      } else if (rule.minLength && value?.length < rule.minLength) {
        newErrors[field] = `${field} must be at least ${rule.minLength} characters`;
      } else if (rule.pattern && !rule.pattern.test(value)) {
        newErrors[field] = rule.message || `${field} is invalid`;
      }
    });
    
    return newErrors;
  };
  
  const handleSubmit = (onSubmit) => async (e) => {
    e.preventDefault();
    
    const newErrors = validate();
    
    if (Object.keys(newErrors).length > 0) {
      setErrors(newErrors);
      return;
    }
    
    setIsSubmitting(true);
    
    try {
      await onSubmit(values);
    } catch (error) {
      console.error('Form submission error:', error);
    } finally {
      setIsSubmitting(false);
    }
  };
  
  const reset = () => {
    setValues(initialValues);
    setErrors({});
    setIsSubmitting(false);
  };
  
  return {
    values,
    errors,
    isSubmitting,
    handleChange,
    handleSubmit,
    reset
  };
}

// Usage of custom hook
function LoginForm() {
  const {
    values,
    errors,
    isSubmitting,
    handleChange,
    handleSubmit,
    reset
  } = useForm(
    { email: '', password: '' },
    {
      email: {
        required: true,
        pattern: /\S+@\S+\.\S+/,
        message: 'Please enter a valid email'
      },
      password: {
        required: true,
        minLength: 6
      }
    }
  );
  
  const onSubmit = async (formData) => {
    const response = await fetch('/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(formData)
    });
    
    if (response.ok) {
      console.log('Login successful');
      reset();
    } else {
      throw new Error('Login failed');
    }
  };
  
  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input
        type="email"
        name="email"
        value={values.email}
        onChange={handleChange}
        placeholder="Email"
      />
      {errors.email && <span>{errors.email}</span>}
      
      <input
        type="password"
        name="password"
        value={values.password}
        onChange={handleChange}
        placeholder="Password"
      />
      {errors.password && <span>{errors.password}</span>}
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Logging in...' : 'Login'}
      </button>
    </form>
  );
}
```

---

### 30. How do you handle async tasks like API calls in React? (fetch/axios, useEffect)

**Confident Answer:** React handles async operations primarily through useEffect hook with proper error handling and loading states.

**Basic API Call with Fetch:**
```jsx
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchUser = async () => {
      try {
        setLoading(true);
        setError(null);
        
        const response = await fetch(`/api/users/${userId}`);
        
        if (!response.ok) {
          throw new Error(`Error: ${response.status} ${response.statusText}`);
        }
        
        const userData = await response.json();
        setUser(userData);
      } catch (err) {
        setError(err.message);
        console.error('Failed to fetch user:', err);
      } finally {
        setLoading(false);
      }
    };
    
    fetchUser();
  }, [userId]); // Re-fetch when userId changes
  
  if (loading) return <div>Loading user...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return <div>User not found</div>;
  
  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
      <p>Role: {user.role}</p>
    </div>
  );
}
```

**Custom Hook for API Calls:**
```jsx
// Reusable hook for API calls
function useApi(url, options = {}) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    let cancelled = false;
    
    const fetchData = async () => {
      try {
        setLoading(true);
        setError(null);
        
        const response = await fetch(url, {
          headers: {
            'Content-Type': 'application/json',
            ...options.headers
          },
          ...options
        });
        
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const result = await response.json();
        
        // Only update state if component is still mounted
        if (!cancelled) {
          setData(result);
        }
      } catch (err) {
        if (!cancelled) {
          setError(err.message);
        }
      } finally {
        if (!cancelled) {
          setLoading(false);
        }
      }
    };
    
    fetchData();
    
    // Cleanup function to prevent memory leaks
    return () => {
      cancelled = true;
    };
  }, [url]);
  
  return { data, loading, error };
}

// Usage of custom API hook
function ProductList() {
  const { data: products, loading, error } = useApi('/api/products');
  
  if (loading) return <div>Loading products...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      <h1>Products</h1>
      {products?.map(product => (
        <ProductCard key={product.id} product={product} />
      ))}
    </div>
  );
}
```

**Handling Multiple API Calls:**
```jsx
function Dashboard() {
  const [user, setUser] = useState(null);
  const [orders, setOrders] = useState([]);
  const [notifications, setNotifications] = useState([]);
  const [loading, setLoading] = useState(true);
  const [errors, setErrors] = useState({});
  
  useEffect(() => {
    const fetchDashboardData = async () => {
      setLoading(true);
      
      try {
        // Fetch multiple endpoints in parallel
        const [userResponse, ordersResponse, notificationsResponse] = await Promise.all([
          fetch('/api/user'),
          fetch('/api/orders'),
          fetch('/api/notifications')
        ]);
        
        // Check if all requests were successful
        if (!userResponse.ok) throw new Error('Failed to fetch user data');
        if (!ordersResponse.ok) throw new Error('Failed to fetch orders');
        if (!notificationsResponse.ok) throw new Error('Failed to fetch notifications');
        
        // Parse all responses
        const [userData, ordersData, notificationsData] = await Promise.all([
          userResponse.json(),
          ordersResponse.json(),
          notificationsResponse.json()
        ]);
        
        // Update all state at once
        setUser(userData);
        setOrders(ordersData);
        setNotifications(notificationsData);
      } catch (error) {
        setErrors(prev => ({
          ...prev,
          dashboard: error.message
        }));
      } finally {
        setLoading(false);
      }
    };
    
    fetchDashboardData();
  }, []);
  
  if (loading) return <div>Loading dashboard...</div>;
  if (errors.dashboard) return <div>Error: {errors.dashboard}</div>;
  
  return (
    <div>
      <UserSection user={user} />
      <OrdersSection orders={orders} />
      <NotificationsSection notifications={notifications} />
    </div>
  );
}
```

**POST Requests with Error Handling:**
```jsx
function CreatePost() {
  const [title, setTitle] = useState('');
  const [content, setContent] = useState('');
  const [isSubmitting, setIsSubmitting] = useState(false);
  const [submitError, setSubmitError] = useState(null);
  const [success, setSuccess] = useState(false);
  
  const handleSubmit = async (e) => {
    e.preventDefault();
    
    setIsSubmitting(true);
    setSubmitError(null);
    setSuccess(false);
    
    try {
      const response = await fetch('/api/posts', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'Authorization': `Bearer ${localStorage.getItem('token')}`
        },
        body: JSON.stringify({
          title,
          content
        })
      });
      
      if (!response.ok) {
        // Handle different error status codes
        if (response.status === 401) {
          throw new Error('Unauthorized. Please log in again.');
        } else if (response.status === 400) {
          const errorData = await response.json();
          throw new Error(errorData.message || 'Invalid input data');
        } else {
          throw new Error(`Server error: ${response.status}`);
        }
      }
      
      const newPost = await response.json();
      console.log('Post created:', newPost);
      
      // Reset form and show success
      setTitle('');
      setContent('');
      setSuccess(true);
      
      // Hide success message after 3 seconds
      setTimeout(() => setSuccess(false), 3000);
      
    } catch (error) {
      setSubmitError(error.message);
    } finally {
      setIsSubmitting(false);
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      {success && (
        <div className="success-message">
          Post created successfully!
        </div>
      )}
      
      {submitError && (
        <div className="error-message">
          Error: {submitError}
        </div>
      )}
      
      <input
        type="text"
        value={title}
        onChange={(e) => setTitle(e.target.value)}
        placeholder="Post Title"
        required
        disabled={isSubmitting}
      />
      
      <textarea
        value={content}
        onChange={(e) => setContent(e.target.value)}
        placeholder="Post Content"
        rows="6"
        required
        disabled={isSubmitting}
      />
      
      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Creating...' : 'Create Post'}
      </button>
    </form>
  );
}
```

**Using Axios (Alternative to Fetch):**
```jsx
import axios from 'axios';

// Create axios instance with base configuration
const api = axios.create({
  baseURL: '/api',
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json'
  }
});

// Add request interceptor for auth token
api.interceptors.request.use(
  (config) => {
    const token = localStorage.getItem('token');
    if (token) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// Add response interceptor for error handling
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      // Redirect to login on unauthorized
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

// Using axios in component
function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchUsers = async () => {
      try {
        setLoading(true);
        const response = await api.get('/users');
        setUsers(response.data);
      } catch (err) {
        setError(err.response?.data?.message || err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchUsers();
  }, []);
  
  const deleteUser = async (userId) => {
    try {
      await api.delete(`/users/${userId}`);
      setUsers(users.filter(user => user.id !== userId));
    } catch (err) {
      alert('Failed to delete user: ' + err.message);
    }
  };
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      {users.map(user => (
        <div key={user.id}>
          <span>{user.name}</span>
          <button onClick={() => deleteUser(user.id)}>Delete</button>
        </div>
      ))}
    </div>
  );
}
```

---

### 31. What is error boundary in React?

**Confident Answer:** Error boundaries are React components that catch JavaScript errors anywhere in their child component tree, log those errors, and display a fallback UI instead of crashing the whole application.

**Creating an Error Boundary:**
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { 
      hasError: false, 
      error: null,
      errorInfo: null
    };
  }
  
  // This method is called when an error occurs
  static getDerivedStateFromError(error) {
    // Update state to show fallback UI
    return { hasError: true };
  }
  
  // This method is called after an error has been thrown
  componentDidCatch(error, errorInfo) {
    // Log error details
    console.error('Error caught by boundary:', error);
    console.error('Error info:', errorInfo);
    
    // Store error details in state
    this.setState({
      error,
      errorInfo
    });
    
    // You can also log error to external service
    // logErrorToService(error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      // Fallback UI
      return (
        <div className="error-boundary">
          <h2>Something went wrong!</h2>
          <details style={{ whiteSpace: 'pre-wrap' }}>
            <summary>Error Details (click to expand)</summary>
            <p><strong>Error:</strong> {this.state.error?.toString()}</p>
            <p><strong>Stack Trace:</strong></p>
            <pre>{this.state.errorInfo?.componentStack}</pre>
          </details>
          <button onClick={() => window.location.reload()}>
            Reload Page
          </button>
        </div>
      );
    }
    
    // Render children normally
    return this.props.children;
  }
}
```

**Using Error Boundary:**
```jsx
// Problematic component that might throw errors
function ProblematicComponent({ data }) {
  if (!data) {
    throw new Error('Data is required but not provided!');
  }
  
  return (
    <div>
      <h3>{data.title}</h3>
      <p>{data.description}</p>
    </div>
  );
}

// App with Error Boundary
function App() {
  const [data, setData] = useState(null);
  const [showProblematic, setShowProblematic] = useState(false);
  
  return (
    <div>
      <h1>My React App</h1>
      
      <button onClick={() => setShowProblematic(!showProblematic)}>
        Toggle Problematic Component
      </button>
      
      <ErrorBoundary>
        <div>
          <h2>Safe Content</h2>
          <p>This content is always safe</p>
        </div>
        
        {showProblematic && (
          <ProblematicComponent data={data} />
        )}
      </ErrorBoundary>
      
      <div>
        <h2>Content Outside Error Boundary</h2>
        <p>This content continues to work even if above components crash</p>
      </div>
    </div>
  );
}
```

**Custom Error Boundary with Reset:**
```jsx
class ErrorBoundaryWithReset extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Error caught:', error, errorInfo);
    
    // Call onError prop if provided
    if (this.props.onError) {
      this.props.onError(error, errorInfo);
    }
  }
  
  handleReset = () => {
    this.setState({ hasError: false, error: null });
    
    // Call onReset prop if provided
    if (this.props.onReset) {
      this.props.onReset();
    }
  }
  
  render() {
    if (this.state.hasError) {
      // Use custom fallback if provided
      if (this.props.fallback) {
        return this.props.fallback(this.state.error, this.handleReset);
      }
      
      // Default fallback UI
      return (
        <div className="error-boundary">
          <h2>Oops! Something went wrong</h2>
          <p>{this.state.error?.message}</p>
          <button onClick={this.handleReset}>
            Try Again
          </button>
        </div>
      );
    }
    
    return this.props.children;
  }
}

// Usage with custom fallback
function App() {
  const handleError = (error, errorInfo) => {
    // Send error to logging service
    console.log('Logging error:', error);
  };
  
  const handleReset = () => {
    // Reset any global state if needed
    console.log('Resetting after error');
  };
  
  const customFallback = (error, reset) => (
    <div className="custom-error">
      <h2>🔥 Application Error</h2>
      <p>Error: {error.message}</p>
      <div>
        <button onClick={reset}>🔄 Retry</button>
        <button onClick={() => window.location.reload()}>
          🏠 Reload App
        </button>
      </div>
    </div>
  );
  
  return (
    <ErrorBoundaryWithReset
      onError={handleError}
      onReset={handleReset}
      fallback={customFallback}
    >
      <MyApp />
    </ErrorBoundaryWithReset>
  );
}
```

**What Error Boundaries Don't Catch:**
- Errors in event handlers
- Errors in async code (setTimeout, promises)
- Errors during server-side rendering
- Errors thrown in error boundary itself

**Handling Event Handler Errors:**
```jsx
function ButtonWithErrorHandling() {
  const [error, setError] = useState(null);
  
  const handleClick = () => {
    try {
      // Potentially problematic code
      throw new Error('Something went wrong in event handler!');
    } catch (err) {
      setError(err.message);
      console.error('Event handler error:', err);
    }
  };
  
  if (error) {
    return (
      <div>
        <p>Error: {error}</p>
        <button onClick={() => setError(null)}>Clear Error</button>
      </div>
    );
  }
  
  return <button onClick={handleClick}>Click Me</button>;
}
```

---

### 32. What testing libraries are used in React? (Jest, React Testing Library)

**Confident Answer:** React testing primarily uses Jest (test runner) and React Testing Library (testing utilities) to write comprehensive tests for components and functionality.

**Basic Component Testing with React Testing Library:**
```jsx
// UserProfile.js
function UserProfile({ user, onEdit }) {
  if (!user) {
    return <div>Loading...</div>;
  }
  
  return (
    <div>
      <h1>{user.name}</h1>
      <p>Email: {user.email}</p>
      <p>Role: {user.role}</p>
      <button onClick={() => onEdit(user.id)}>
        Edit Profile
      </button>
    </div>
  );
}

// UserProfile.test.js
import { render, screen, fireEvent } from '@testing-library/react';
import UserProfile from './UserProfile';

describe('UserProfile Component', () => {
  const mockUser = {
    id: 1,
    name: 'John Doe',
    email: 'john@example.com',
    role: 'Developer'
  };
  
  test('renders user information correctly', () => {
    render(<UserProfile user={mockUser} onEdit={() => {}} />);
    
    // Test if elements are rendered
    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('Email: john@example.com')).toBeInTheDocument();
    expect(screen.getByText('Role: Developer')).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /edit profile/i })).toBeInTheDocument();
  });
  
  test('shows loading when user is not provided', () => {
    render(<UserProfile user={null} onEdit={() => {}} />);
    
    expect(screen.getByText('Loading...')).toBeInTheDocument();
  });
  
  test('calls onEdit when edit button is clicked', () => {
    const mockOnEdit = jest.fn();
    render(<UserProfile user={mockUser} onEdit={mockOnEdit} />);
    
    const editButton = screen.getByRole('button', { name: /edit profile/i });
    fireEvent.click(editButton);
    
    expect(mockOnEdit).toHaveBeenCalledWith(1);
    expect(mockOnEdit).toHaveBeenCalledTimes(1);
  });
});
```

**Testing Forms and User Interactions:**
```jsx
// LoginForm.js
import { useState } from 'react';

function LoginForm({ onLogin }) {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [error, setError] = useState('');
  
  const handleSubmit = (e) => {
    e.preventDefault();
    
    if (!email || !password) {
      setError('Both email and password are required');
      return;
    }
    
    onLogin({ email, password });
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="email">Email:</label>
        <input
          id="email"
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
      </div>
      
      <div>
        <label htmlFor="password">Password:</label>
        <input
          id="password"
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
      </div>
      
      {error && <div role="alert">{error}</div>}
      
      <button type="submit">Login</button>
    </form>
  );
}

// LoginForm.test.js
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import LoginForm from './LoginForm';

describe('LoginForm', () => {
  test('renders form fields', () => {
    render(<LoginForm onLogin={() => {}} />);
    
    expect(screen.getByLabelText(/email/i)).toBeInTheDocument();
    expect(screen.getByLabelText(/password/i)).toBeInTheDocument();
    expect(screen.getByRole('button', { name: /login/i })).toBeInTheDocument();
  });
  
  test('shows error when submitting empty form', async () => {
    render(<LoginForm onLogin={() => {}} />);
    
    const submitButton = screen.getByRole('button', { name: /login/i });
    fireEvent.click(submitButton);
    
    await waitFor(() => {
      expect(screen.getByRole('alert')).toHaveTextContent(
        'Both email and password are required'
      );
    });
  });
  
  test('calls onLogin with correct data when form is valid', async () => {
    const user = userEvent.setup();
    const mockOnLogin = jest.fn();
    
    render(<LoginForm onLogin={mockOnLogin} />);
    
    // Fill out form
    await user.type(screen.getByLabelText(/email/i), 'test@example.com');
    await user.type(screen.getByLabelText(/password/i), 'password123');
    
    // Submit form
    await user.click(screen.getByRole('button', { name: /login/i }));
    
    expect(mockOnLogin).toHaveBeenCalledWith({
      email: 'test@example.com',
      password: 'password123'
    });
  });
});
```

**Testing Hooks:**
```jsx
// useCounter.js - Custom Hook
import { useState } from 'react';

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(prev => prev + 1);
  const decrement = () => setCount(prev => prev - 1);
  const reset = () => setCount(initialValue);
  
  return { count, increment, decrement, reset };
}

// useCounter.test.js
import { renderHook, act } from '@testing-library/react';
import useCounter from './useCounter';

describe('useCounter Hook', () => {
  test('initializes with default value', () => {
    const { result } = renderHook(() => useCounter());
    
    expect(result.current.count).toBe(0);
  });
  
  test('initializes with custom value', () => {
    const { result } = renderHook(() => useCounter(10));
    
    expect(result.current.count).toBe(10);
  });
  
  test('increments counter', () => {
    const { result } = renderHook(() => useCounter());
    
    act(() => {
      result.current.increment();
    });
    
    expect(result.current.count).toBe(1);
  });
  
  test('decrements counter', () => {
    const { result } = renderHook(() => useCounter(5));
    
    act(() => {
      result.current.decrement();
    });
    
    expect(result.current.count).toBe(4);
  });
  
  test('resets counter to initial value', () => {
    const { result } = renderHook(() => useCounter(3));
    
    // Change the value
    act(() => {
      result.current.increment();
      result.current.increment();
    });
    
    expect(result.current.count).toBe(5);
    
    // Reset
    act(() => {
      result.current.reset();
    });
    
    expect(result.current.count).toBe(3);
  });
});
```

**Testing API Calls with Mocking:**
```jsx
// UserList.js
import { useState, useEffect } from 'react';

function UserList() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchUsers = async () => {
      try {
        const response = await fetch('/api/users');
        if (!response.ok) {
          throw new Error('Failed to fetch users');
        }
        const userData = await response.json();
        setUsers(userData);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchUsers();
  }, []);
  
  if (loading) return <div>Loading users...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <ul>
      {users.map(user => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

// UserList.test.js
import { render, screen, waitFor } from '@testing-library/react';
import UserList from './UserList';

// Mock fetch globally
global.fetch = jest.fn();

describe('UserList', () => {
  beforeEach(() => {
    fetch.mockClear();
  });
  
  test('displays loading initially', () => {
    fetch.mockResolvedValueOnce({
      ok: true,
      json: async () => []
    });
    
    render(<UserList />);
    
    expect(screen.getByText('Loading users...')).toBeInTheDocument();
  });
  
  test('displays users after successful fetch', async () => {
    const mockUsers = [
      { id: 1, name: 'John Doe' },
      { id: 2, name: 'Jane Smith' }
    ];
    
    fetch.mockResolvedValueOnce({
      ok: true,
      json: async () => mockUsers
    });
    
    render(<UserList />);
    
    await waitFor(() => {
      expect(screen.getByText('John Doe')).toBeInTheDocument();
      expect(screen.getByText('Jane Smith')).toBeInTheDocument();
    });
    
    expect(fetch).toHaveBeenCalledWith('/api/users');
  });
  
  test('displays error message when fetch fails', async () => {
    fetch.mockRejectedValueOnce(new Error('Network error'));
    
    render(<UserList />);
    
    await waitFor(() => {
      expect(screen.getByText('Error: Network error')).toBeInTheDocument();
    });
  });
  
  test('displays error when response is not ok', async () => {
    fetch.mockResolvedValueOnce({
      ok: false,
      status: 500
    });
    
    render(<UserList />);
    
    await waitFor(() => {
      expect(screen.getByText('Error: Failed to fetch users')).toBeInTheDocument();
    });
  });
});
```

**Common Testing Patterns:**

**1. Testing Custom Providers:**
```jsx
// test-utils.js - Custom render with providers
import { render } from '@testing-library/react';
import { BrowserRouter } from 'react-router-dom';
import { UserProvider } from './UserContext';

const AllTheProviders = ({ children }) => {
  return (
    <BrowserRouter>
      <UserProvider>
        {children}
      </UserProvider>
    </BrowserRouter>
  );
};

const customRender = (ui, options) =>
  render(ui, { wrapper: AllTheProviders, ...options });

export * from '@testing-library/react';
export { customRender as render };
```

**2. Jest Configuration (jest.config.js):**
```javascript
module.exports = {
  testEnvironment: 'jsdom',
  setupFilesAfterEnv: ['<rootDir>/src/setupTests.js'],
  moduleNameMapping: {
    '\\.(css|less|scss|sass)$': 'identity-obj-proxy',
    '\\.(jpg|jpeg|png|gif|eot|otf|webp|svg|ttf|woff|woff2)$': '<rootDir>/__mocks__/fileMock.js'
  },
  collectCoverageFrom: [
    'src/**/*.{js,jsx}',
    '!src/index.js',
    '!src/reportWebVitals.js'
  ],
  coverageThreshold: {
    global: {
      branches: 80,
      functions: 80,
      lines: 80,
      statements: 80
    }
  }
};
```

**Benefits of Testing:**
- **Confidence:** Know your code works as expected
- **Regression Prevention:** Catch bugs early
- **Documentation:** Tests serve as living documentation
- **Refactoring Safety:** Make changes without fear
- **Better Design:** Tests encourage better component design

---

## 🎯 **FINAL INTERVIEW PREPARATION TIPS**

### **Last-Minute Confidence Boosters:**

**1. Practice Explaining Out Loud:**
- Pick 5 random questions and explain them aloud
- Practice drawing component hierarchies
- Explain data flow in React apps

**2. Prepare Your Experience Stories:**
- "In my current project, I used React hooks to..."
- "I faced a challenge with state management when..."
- "I improved performance by implementing..."

**3. Technical Confidence Phrases:**
```
"Based on my experience with React..."
"In my project, I implemented this by..."
"React's virtual DOM helps us because..."
"The key benefit of this approach is..."
"I would handle this scenario by..."
```

**4. Questions to Ask Interviewer:**
- "What React patterns does your team follow?"
- "How do you handle state management in your application?"
- "What's your approach to testing React components?"
- "What challenges is the team currently facing with React?"

### **Remember:**
✅ **Be honest** about your experience level
✅ **Show enthusiasm** for learning and growing
✅ **Focus on understanding** rather than memorization
✅ **Practice coding examples** before the interview
✅ **Prepare questions** about their React tech stack

### **You've Got This! 🚀**

Your 6 months of hands-on React experience combined with this preparation gives you a solid foundation. Focus on the concepts you understand well, be honest about areas you're still learning, and show your passion for React development.

**Good luck with your interview!** 💪