# React.js Interview Questions (1-30) - Fundamentals & Core Concepts

## 1. What is React.js?

**Answer:** React.js is a JavaScript library developed by Facebook for building user interfaces, particularly single-page applications (SPAs). It allows developers to create reusable UI components and manage application state efficiently.

**Key Points:**
- Component-based architecture
- Virtual DOM for performance optimization
- Declarative programming paradigm
- Maintained by Facebook (Meta)
- Used for building interactive UIs

---

## 2. What are the major features of React?

**Answer:** React has several key features that make it popular:

1. **Virtual DOM**: Improves performance by minimizing direct DOM manipulation
2. **Component-Based**: Encapsulated components that manage their own state
3. **JSX**: JavaScript syntax extension for writing HTML-like code
4. **One-way Data Binding**: Data flows in one direction, making apps predictable
5. **Reusability**: Components can be reused across different parts of the application
6. **Developer Tools**: Excellent debugging tools and browser extensions

---

## 3. What is Virtual DOM and how it works?

**Answer:** Virtual DOM is a JavaScript representation of the actual DOM kept in memory. It's a programming concept where a "virtual" representation of UI is kept in memory and synced with the "real" DOM.

**How it works:**
1. When state changes, React creates a new Virtual DOM tree
2. React compares (diffs) the new tree with the previous Virtual DOM tree
3. React calculates the minimum changes needed
4. React updates only the changed elements in the real DOM

**Benefits:**
- Faster updates
- Better performance
- Batch updates
- Cross-browser compatibility

---

## 4. What are components in React?

**Answer:** Components are the building blocks of React applications. They are reusable pieces of code that return JSX elements to describe what should appear on the screen.

**Types of Components:**
1. **Functional Components**: JavaScript functions that return JSX
2. **Class Components**: ES6 classes that extend React.Component

**Characteristics:**
- Encapsulated and reusable
- Can accept inputs (props)
- Can manage their own state
- Follow a hierarchy

---

## 5. Explain Class components with example

**Answer:** Class components are ES6 classes that extend React.Component and must have a render() method.

```jsx
import React, { Component } from 'react';

class Welcome extends Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  handleIncrement = () => {
    this.setState({ count: this.state.count + 1 });
  }

  render() {
    return (
      <div>
        <h1>Hello, {this.props.name}!</h1>
        <p>Count: {this.state.count}</p>
        <button onClick={this.handleIncrement}>
          Increment
        </button>
      </div>
    );
  }
}

export default Welcome;
```

**Key Features:**
- Have access to lifecycle methods
- Can have state using this.state
- Use this.setState() to update state

---

## 6. Explain functional components with example

**Answer:** Functional components are JavaScript functions that return JSX. With hooks, they can also manage state and side effects.

```jsx
import React, { useState } from 'react';

const Welcome = ({ name }) => {
  const [count, setCount] = useState(0);

  const handleIncrement = () => {
    setCount(count + 1);
  };

  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>
        Increment
      </button>
    </div>
  );
};

export default Welcome;
```

**Advantages:**
- Simpler syntax
- Better performance
- Easier to test
- Hooks enable state and lifecycle features

---

## 7. What is JSX?

**Answer:** JSX (JavaScript XML) is a syntax extension for JavaScript that allows you to write HTML-like code within JavaScript. It makes React components more readable and easier to write.

```jsx
// JSX
const element = <h1>Hello, World!</h1>;

// Equivalent JavaScript
const element = React.createElement('h1', null, 'Hello, World!');
```

**JSX Rules:**
- Must return a single parent element
- Use className instead of class
- Use camelCase for attributes
- Close all tags
- Use {} for JavaScript expressions

---

## 8. How to export and import components?

**Answer:** There are two main ways to export/import components:

**Default Export/Import:**
```jsx
// Component.js
import React from 'react';

const MyComponent = () => {
  return <div>Hello World</div>;
};

export default MyComponent;

// App.js
import MyComponent from './Component';
```

**Named Export/Import:**
```jsx
// Components.js
export const Header = () => <header>Header</header>;
export const Footer = () => <footer>Footer</footer>;

// App.js
import { Header, Footer } from './Components';
// or
import * as Components from './Components';
```

---

## 9. How to use nested components?

**Answer:** Nested components allow you to compose complex UIs by combining simpler components.

```jsx
// Child Component
const Button = ({ text, onClick }) => (
  <button onClick={onClick}>{text}</button>
);

// Parent Component
const UserCard = ({ user }) => (
  <div className="user-card">
    <h2>{user.name}</h2>
    <p>{user.email}</p>
    <Button text="Edit" onClick={() => console.log('Edit clicked')} />
    <Button text="Delete" onClick={() => console.log('Delete clicked')} />
  </div>
);

// Main App
const App = () => {
  const user = { name: 'John Doe', email: 'john@example.com' };
  
  return (
    <div>
      <UserCard user={user} />
    </div>
  );
};
```

---

## 10. What is state in React?

**Answer:** State is a built-in object that stores property values that belong to a component. When state changes, the component re-renders.

**Characteristics:**
- Private to the component
- Can be changed using setState() or useState()
- Triggers re-render when updated
- Should be treated as immutable

```jsx
// Class Component State
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
}

// Functional Component State
const Counter = () => {
  const [count, setCount] = useState(0);
  return <div>Count: {count}</div>;
};
```

---

## 11. How to update state in React?

**Answer:** State should never be updated directly. Use the appropriate methods:

**Class Components:**
```jsx
// Wrong
this.state.count = 1;

// Correct
this.setState({ count: 1 });

// With previous state
this.setState(prevState => ({
  count: prevState.count + 1
}));
```

**Functional Components:**
```jsx
const [count, setCount] = useState(0);

// Direct update
setCount(1);

// With previous state
setCount(prevCount => prevCount + 1);
```

---

## 12. What is setState callback?

**Answer:** setState callback is a function that executes after setState completes and the component re-renders.

```jsx
class MyComponent extends React.Component {
  handleClick = () => {
    this.setState(
      { count: this.state.count + 1 },
      () => {
        // This callback runs after state is updated
        console.log('State updated:', this.state.count);
      }
    );
  };
}
```

**Use Cases:**
- Performing actions after state update
- Making API calls after state change
- Focusing elements after render

---

## 13. Why you should not update state directly? Explain with example

**Answer:** Direct state mutation doesn't trigger re-renders and can cause bugs.

```jsx
// WRONG - Don't do this
class BadExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = { items: ['apple', 'banana'] };
  }

  addItem = () => {
    // This won't trigger re-render
    this.state.items.push('orange');
    this.forceUpdate(); // Bad practice
  };
}

// CORRECT - Do this
class GoodExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = { items: ['apple', 'banana'] };
  }

  addItem = () => {
    // This triggers re-render
    this.setState({
      items: [...this.state.items, 'orange']
    });
  };
}
```

**Reasons:**
- Doesn't trigger re-render
- Breaks React's reconciliation
- Can cause unpredictable behavior
- Makes debugging difficult

---

## 14. What are props in React?

**Answer:** Props (properties) are read-only inputs passed from parent to child components. They allow data to flow down the component tree.

```jsx
// Parent Component
const App = () => {
  return (
    <UserProfile 
      name="John Doe" 
      age={30}
      isActive={true}
    />
  );
};

// Child Component
const UserProfile = ({ name, age, isActive }) => {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
      {isActive && <span>Active User</span>}
    </div>
  );
};
```

**Characteristics:**
- Read-only (immutable)
- Passed from parent to child
- Can be any data type
- Help make components reusable

---

## 15. What is difference between state and props?

**Answer:** 

| **State** | **Props** |
|-----------|-----------|
| Mutable (can be changed) | Immutable (read-only) |
| Managed within component | Passed from parent |
| Triggers re-render when changed | Component re-renders when props change |
| Private to component | Shared between components |
| Used with useState/setState | Received as function parameters |

```jsx
// State example
const [count, setCount] = useState(0); // Can be changed

// Props example
const Child = ({ title }) => <h1>{title}</h1>; // Cannot be changed
```

---

## 16. What is lifting state up in React?

**Answer:** Lifting state up means moving state from child components to their common parent component to share data between siblings.

```jsx
// Before - State in individual components
const ComponentA = () => {
  const [value, setValue] = useState('');
  // ...
};

const ComponentB = () => {
  const [value, setValue] = useState('');
  // ...
};

// After - State lifted to parent
const Parent = () => {
  const [sharedValue, setSharedValue] = useState('');
  
  return (
    <div>
      <ComponentA value={sharedValue} onChange={setSharedValue} />
      <ComponentB value={sharedValue} onChange={setSharedValue} />
    </div>
  );
};
```

**When to use:**
- Sharing data between siblings
- Synchronizing components
- Managing form data

---

## 17. What is children prop in React?

**Answer:** The children prop allows components to receive and render nested content passed between opening and closing tags.

```jsx
// Container component
const Card = ({ children, title }) => {
  return (
    <div className="card">
      <h2>{title}</h2>
      <div className="card-content">
        {children}
      </div>
    </div>
  );
};

// Usage
const App = () => {
  return (
    <Card title="User Info">
      <p>Name: John Doe</p>
      <p>Email: john@example.com</p>
      <button>Edit Profile</button>
    </Card>
  );
};
```

**Benefits:**
- Component composition
- Flexible layouts
- Reusable wrappers

---

## 18. What is defaultProps in React?

**Answer:** defaultProps allows you to define default values for props in case they are not provided.

```jsx
// Class Component
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}

Greeting.defaultProps = {
  name: 'Guest'
};

// Functional Component
const Greeting = ({ name = 'Guest' }) => {
  return <h1>Hello, {name}!</h1>;
};

// Or using defaultProps
Greeting.defaultProps = {
  name: 'Guest'
};

// Usage
<Greeting />          // Renders: Hello, Guest!
<Greeting name="John" /> // Renders: Hello, John!
```

---

## 19. What are fragments in React and its advantages?

**Answer:** React Fragments allow you to group multiple elements without adding extra DOM nodes.

```jsx
// Using React.Fragment
const MyComponent = () => {
  return (
    <React.Fragment>
      <h1>Title</h1>
      <p>Description</p>
    </React.Fragment>
  );
};

// Using short syntax
const MyComponent = () => {
  return (
    <>
      <h1>Title</h1>
      <p>Description</p>
    </>
  );
};

// With key (for lists)
const List = ({ items }) => {
  return (
    <>
      {items.map(item => (
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </>
  );
};
```

**Advantages:**
- Cleaner DOM structure
- Better performance
- No extra wrapper elements
- Improved accessibility

---

## 20. How to use styling in React.js?

**Answer:** There are several ways to style React components:

**1. Inline Styles:**
```jsx
const MyComponent = () => {
  const styles = {
    backgroundColor: 'blue',
    color: 'white',
    padding: '10px'
  };
  
  return <div style={styles}>Styled Component</div>;
};
```

**2. CSS Classes:**
```jsx
// styles.css
.my-component {
  background-color: blue;
  color: white;
  padding: 10px;
}

// Component
import './styles.css';

const MyComponent = () => {
  return <div className="my-component">Styled Component</div>;
};
```

**3. CSS Modules:**
```jsx
// styles.module.css
.myComponent {
  background-color: blue;
}

// Component
import styles from './styles.module.css';

const MyComponent = () => {
  return <div className={styles.myComponent}>Styled Component</div>;
};
```

**4. Styled Components:**
```jsx
import styled from 'styled-components';

const StyledDiv = styled.div`
  background-color: blue;
  color: white;
  padding: 10px;
`;

const MyComponent = () => {
  return <StyledDiv>Styled Component</StyledDiv>;
};
```

---

## 21. How can you conditionally render components in React?

**Answer:** There are several ways to conditionally render components:

**1. If-Else Statements:**
```jsx
const MyComponent = ({ isLoggedIn }) => {
  if (isLoggedIn) {
    return <Dashboard />;
  } else {
    return <LoginForm />;
  }
};
```

**2. Ternary Operator:**
```jsx
const MyComponent = ({ isLoggedIn }) => {
  return (
    <div>
      {isLoggedIn ? <Dashboard /> : <LoginForm />}
    </div>
  );
};
```

**3. Logical AND (&&):**
```jsx
const MyComponent = ({ user, notifications }) => {
  return (
    <div>
      {user && <Welcome user={user} />}
      {notifications.length > 0 && <NotificationsList />}
    </div>
  );
};
```

**4. Switch Statement:**
```jsx
const StatusComponent = ({ status }) => {
  const renderContent = () => {
    switch (status) {
      case 'loading':
        return <Spinner />;
      case 'success':
        return <SuccessMessage />;
      case 'error':
        return <ErrorMessage />;
      default:
        return <DefaultContent />;
    }
  };

  return <div>{renderContent()}</div>;
};
```

---

## 22. How to render list of data in React?

**Answer:** Use the map() method to render arrays of data:

```jsx
const TodoList = () => {
  const todos = [
    { id: 1, text: 'Learn React', completed: false },
    { id: 2, text: 'Build a project', completed: true },
    { id: 3, text: 'Get a job', completed: false }
  ];

  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <span style={{ 
            textDecoration: todo.completed ? 'line-through' : 'none' 
          }}>
            {todo.text}
          </span>
        </li>
      ))}
    </ul>
  );
};

// With filtering
const CompletedTodos = () => {
  return (
    <ul>
      {todos
        .filter(todo => todo.completed)
        .map(todo => (
          <li key={todo.id}>{todo.text}</li>
        ))
      }
    </ul>
  );
};
```

---

## 23. What is key prop?

**Answer:** The key prop is a special attribute that helps React identify which list items have changed, been added, or removed.

```jsx
// Good - Using unique id
const TodoList = ({ todos }) => {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
};

// Bad - Using index
const TodoList = ({ todos }) => {
  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index}>{todo.text}</li>
      ))}
    </ul>
  );
};
```

**Key Rules:**
- Must be unique among siblings
- Should be stable (not change between renders)
- Should be predictable
- Helps React optimize re-renders

---

## 24. Why indexes for keys are not recommended?

**Answer:** Using array indexes as keys can cause performance issues and bugs when the list order changes.

```jsx
// Problems with index keys
const items = ['Apple', 'Banana', 'Cherry'];

// If we remove 'Apple', indexes shift:
// Before: Apple(0), Banana(1), Cherry(2)
// After:  Banana(0), Cherry(1)

// This causes React to:
// 1. Think Banana is a new item at index 0
// 2. Update all subsequent items
// 3. Potentially lose component state

// Better approach
const items = [
  { id: 1, name: 'Apple' },
  { id: 2, name: 'Banana' },
  { id: 3, name: 'Cherry' }
];

return (
  <ul>
    {items.map(item => (
      <li key={item.id}>{item.name}</li>
    ))}
  </ul>
);
```

**Issues with index keys:**
- Performance problems
- Loss of component state
- Incorrect re-rendering
- UI bugs with forms

---

## 25. How to handle buttons in React?

**Answer:** Handle button clicks using event handlers:

```jsx
const ButtonExample = () => {
  const [count, setCount] = useState(0);

  // Simple click handler
  const handleClick = () => {
    setCount(count + 1);
  };

  // Handler with parameters
  const handleClickWithParam = (value) => {
    setCount(count + value);
  };

  // Handler with event object
  const handleClickWithEvent = (event) => {
    console.log('Button clicked:', event.target.textContent);
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      
      {/* Simple handler */}
      <button onClick={handleClick}>
        Increment
      </button>
      
      {/* Handler with parameter */}
      <button onClick={() => handleClickWithParam(5)}>
        Add 5
      </button>
      
      {/* Handler with event */}
      <button onClick={handleClickWithEvent}>
        Click me
      </button>
      
      {/* Inline handler */}
      <button onClick={() => setCount(0)}>
        Reset
      </button>
    </div>
  );
};
```

---

## 26. How to handle inputs in React?

**Answer:** Handle inputs using controlled components:

```jsx
const FormExample = () => {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    age: '',
    gender: '',
    terms: false
  });

  // Handle text inputs
  const handleInputChange = (event) => {
    const { name, value, type, checked } = event.target;
    setFormData(prevState => ({
      ...prevState,
      [name]: type === 'checkbox' ? checked : value
    }));
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log('Form submitted:', formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Text input */}
      <input
        type="text"
        name="name"
        placeholder="Name"
        value={formData.name}
        onChange={handleInputChange}
      />

      {/* Email input */}
      <input
        type="email"
        name="email"
        placeholder="Email"
        value={formData.email}
        onChange={handleInputChange}
      />

      {/* Number input */}
      <input
        type="number"
        name="age"
        placeholder="Age"
        value={formData.age}
        onChange={handleInputChange}
      />

      {/* Select dropdown */}
      <select
        name="gender"
        value={formData.gender}
        onChange={handleInputChange}
      >
        <option value="">Select Gender</option>
        <option value="male">Male</option>
        <option value="female">Female</option>
      </select>

      {/* Checkbox */}
      <label>
        <input
          type="checkbox"
          name="terms"
          checked={formData.terms}
          onChange={handleInputChange}
        />
        Accept Terms
      </label>

      <button type="submit">Submit</button>
    </form>
  );
};
```

---

## 27. Explain lifecycle methods in React

**Answer:** Lifecycle methods are special methods that run at different phases of a component's life.

**Class Component Lifecycle:**

```jsx
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { data: null };
    console.log('1. Constructor');
  }

  // Mounting Phase
  componentDidMount() {
    console.log('3. Component Did Mount');
    // Good for: API calls, subscriptions, timers
    fetch('/api/data')
      .then(response => response.json())
      .then(data => this.setState({ data }));
  }

  // Updating Phase
  componentDidUpdate(prevProps, prevState) {
    console.log('4. Component Did Update');
    // Good for: responding to prop/state changes
    if (prevProps.userId !== this.props.userId) {
      this.fetchUserData(this.props.userId);
    }
  }

  // Unmounting Phase
  componentWillUnmount() {
    console.log('5. Component Will Unmount');
    // Good for: cleanup, removing listeners, canceling timers
    clearInterval(this.timer);
    document.removeEventListener('scroll', this.handleScroll);
  }

  render() {
    console.log('2. Render');
    return <div>{this.state.data ? 'Data loaded' : 'Loading...'}</div>;
  }
}
```

**Functional Component with Hooks:**
```jsx
const MyComponent = ({ userId }) => {
  const [data, setData] = useState(null);

  // Equivalent to componentDidMount and componentDidUpdate
  useEffect(() => {
    console.log('Component mounted or userId changed');
    
    fetch(`/api/users/${userId}`)
      .then(response => response.json())
      .then(data => setData(data));

    // Cleanup function (componentWillUnmount)
    return () => {
      console.log('Cleanup');
    };
  }, [userId]); // Dependency array

  return <div>{data ? 'Data loaded' : 'Loading...'}</div>;
};
```

---

## 28. What are the popular hooks in React and explain its usage?

**Answer:** Here are the most commonly used React hooks:

**1. useState - State Management:**
```jsx
const [count, setCount] = useState(0);
const [user, setUser] = useState({ name: '', email: '' });
```

**2. useEffect - Side Effects:**
```jsx
// Component did mount
useEffect(() => {
  console.log('Component mounted');
}, []);

// Component did update
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```

**3. useContext - Context Consumption:**
```jsx
const theme = useContext(ThemeContext);
const user = useContext(UserContext);
```

**4. useReducer - Complex State:**
```jsx
const [state, dispatch] = useReducer(reducer, initialState);
```

**5. useRef - DOM References:**
```jsx
const inputRef = useRef(null);
const focusInput = () => inputRef.current.focus();
```

**6. useMemo - Memoization:**
```jsx
const expensiveValue = useMemo(() => {
  return computeExpensiveValue(a, b);
}, [a, b]);
```

**7. useCallback - Function Memoization:**
```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

---

## 29. What is useState and how to manage state using it?

**Answer:** useState is a Hook that allows you to add state to functional components.

```jsx
import React, { useState } from 'react';

const StateExample = () => {
  // Basic usage
  const [count, setCount] = useState(0);
  
  // With object
  const [user, setUser] = useState({
    name: '',
    email: '',
    age: 0
  });

  // With array
  const [items, setItems] = useState([]);

  // Update simple state
  const increment = () => {
    setCount(count + 1);
    // or with previous state
    setCount(prevCount => prevCount + 1);
  };

  // Update object state
  const updateUser = (field, value) => {
    setUser(prevUser => ({
      ...prevUser,
      [field]: value
    }));
  };

  // Update array state
  const addItem = (newItem) => {
    setItems(prevItems => [...prevItems, newItem]);
  };

  const removeItem = (index) => {
    setItems(prevItems => prevItems.filter((_, i) => i !== index));
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      
      <input
        placeholder="Name"
        value={user.name}
        onChange={(e) => updateUser('name', e.target.value)}
      />
      
      <button onClick={() => addItem(`Item ${items.length + 1}`)}>
        Add Item
      </button>
    </div>
  );
};
```

**Key Points:**
- Returns array with [state, setter]
- State updates are asynchronous
- Use functional updates for dependent updates
- State should be treated as immutable

---

## 30. What is useEffect hook and how to manage side effects?

**Answer:** useEffect allows you to perform side effects in functional components. It combines componentDidMount, componentDidUpdate, and componentWillUnmount.

```jsx
import React, { useState, useEffect } from 'react';

const EffectExample = () => {
  const [count, setCount] = useState(0);
  const [data, setData] = useState(null);

  // 1. Effect without dependencies (runs after every render)
  useEffect(() => {
    console.log('Component rendered');
  });

  // 2. Effect with empty dependency array (runs only on mount)
  useEffect(() => {
    console.log('Component mounted');
    
    // Cleanup function (runs on unmount)
    return () => {
      console.log('Component will unmount');
    };
  }, []);

  // 3. Effect with dependencies (runs when dependencies change)
  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  // 4. Data fetching example
  useEffect(() => {
    const fetchData = async () => {
      try {
        const response = await fetch('/api/data');
        const result = await response.json();
        setData(result);
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    };

    fetchData();
  }, []); // Empty dependency - fetch only once

  // 5. Subscription example with cleanup
  useEffect(() => {
    const timer = setInterval(() => {
      setCount(prevCount => prevCount + 1);
    }, 1000);

    // Cleanup timer on unmount
    return () => {
      clearInterval(timer);
    };
  }, []);

  // 6. Event listener example
  useEffect(() => {
    const handleResize = () => {
      console.log('Window resized');
    };

    window.addEventListener('resize', handleResize);

    // Cleanup event listener
    return () => {
      window.removeEventListener('resize', handleResize);
    };
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <p>Data: {data ? JSON.stringify(data) : 'Loading...'}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
};
```

**Common Use Cases:**
- Data fetching
- Setting up subscriptions
- Manually changing DOM
- Cleanup (timers, subscriptions)
- Setting document title

**Dependency Array Rules:**
- No array: runs after every render
- Empty array []: runs only on mount/unmount
- With dependencies [dep1, dep2]: runs when dependencies change
