# React State Management - Most Common Interview Questions
*For 2 Years Experience - Quick Interview Prep*

## 1. What is the difference between state and props?
**Most Asked Comparison:**
- **State:** Internal data that component owns and can change
- **Props:** External data passed from parent, read-only in child

```javascript
// State - component owns it
const [name, setName] = useState('John');

// Props - received from parent
function Child({ name, age }) {
  return <div>{name} is {age} years old</div>;
}
```

## 2. Can you modify props directly? Why or why not?
**Critical Rule:** 
**Answer:** No! Props are read-only. Modifying props breaks React's one-way data flow.

```javascript
// ❌ Wrong - Never do this
function Child({ user }) {
  user.name = 'Modified'; // Never modify props!
  return <div>{user.name}</div>;
}

// ✅ Correct - Use callback to parent
function Child({ user, onUserUpdate }) {
  const handleUpdate = () => {
    onUserUpdate({ ...user, name: 'Modified' });
  };
  return <button onClick={handleUpdate}>Update</button>;
}
```

## 3. What is props drilling and why is it a problem?
**Definition:** Passing props through multiple levels of components that don't need them.

```javascript
// Props drilling example
function App() {
  const [user, setUser] = useState({ name: 'John' });
  return <Level1 user={user} />;
}

function Level1({ user }) {
  return <Level2 user={user} />; // Just passing through
}

function Level2({ user }) {
  return <Level3 user={user} />; // Just passing through
}

function Level3({ user }) {
  return <div>{user.name}</div>; // Finally uses it
}
```

**Problems:**
- Hard to maintain
- Components receive props they don't use
- Refactoring becomes difficult

## 4. How does Context API solve props drilling?
**Modern Solution:**
```javascript
// Create Context
const UserContext = createContext();

// Provider at top level
function App() {
  const [user, setUser] = useState({ name: 'John' });
  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Level1 />
    </UserContext.Provider>
  );
}

// Skip intermediate levels
function Level1() {
  return <Level2 />; // No props needed
}

function Level2() {
  return <Level3 />; // No props needed
}

// Consume directly where needed
function Level3() {
  const { user } = useContext(UserContext);
  return <div>{user.name}</div>;
}
```

## 5. How do you update state properly in React?
**State Update Patterns:**
```javascript
// ❌ Wrong - Direct mutation
const [user, setUser] = useState({ name: 'John', age: 25 });
user.name = 'Jane'; // Never mutate directly!

// ✅ Correct - Create new object
setUser({ ...user, name: 'Jane' });

// ✅ Correct - Functional update
setUser(prevUser => ({ ...prevUser, name: 'Jane' }));

// ✅ Array updates
const [items, setItems] = useState([1, 2, 3]);
setItems([...items, 4]); // Add item
setItems(items.filter(item => item !== 2)); // Remove item
```

## 6. What is the difference between controlled and uncontrolled components?
**Form Handling:**
```javascript
// Controlled Component - React controls the value
function ControlledInput() {
  const [value, setValue] = useState('');
  
  return (
    <input
      value={value}
      onChange={(e) => setValue(e.target.value)}
    />
  );
}

// Uncontrolled Component - DOM controls the value
function UncontrolledInput() {
  const inputRef = useRef();
  
  const handleSubmit = () => {
    console.log(inputRef.current.value); // Access DOM directly
  };
  
  return <input ref={inputRef} />;
}
```

## 7. When should you use Context vs props?
**Decision Guide:**
- **Use Props:** For direct parent-child communication
- **Use Context:** For data needed by many components at different levels

```javascript
// ✅ Good for Context - Global data
const ThemeContext = createContext();
const UserContext = createContext();
const LanguageContext = createContext();

// ✅ Good for Props - Direct communication
function Parent() {
  const handleClick = () => console.log('clicked');
  return <Child onButtonClick={handleClick} />;
}
```

## 8. How do you handle multiple contexts?
**Multiple Context Providers:**
```javascript
function App() {
  return (
    <ThemeProvider>
      <UserProvider>
        <LanguageProvider>
          <MainApp />
        </LanguageProvider>
      </UserProvider>
    </ThemeProvider>
  );
}

// Using multiple contexts
function Component() {
  const { theme } = useContext(ThemeContext);
  const { user } = useContext(UserContext);
  const { language } = useContext(LanguageContext);
  
  return <div>Component content</div>;
}
```

## 9. What are the performance implications of Context?
**Important Considerations:**
```javascript
// ❌ Problem - Creates new object every render
function App() {
  const [user, setUser] = useState({ name: 'John' });
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Child />
    </UserContext.Provider>
  );
}

// ✅ Solution - Memoize the value
function App() {
  const [user, setUser] = useState({ name: 'John' });
  
  const contextValue = useMemo(() => ({
    user,
    setUser
  }), [user]);
  
  return (
    <UserContext.Provider value={contextValue}>
      <Child />
    </UserContext.Provider>
  );
}
```

## 10. How do you lift state up?
**Common Pattern:**
```javascript
// Before - State in child
function Child() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}

// After - Lifted to parent
function Parent() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <Child count={count} onIncrement={() => setCount(count + 1)} />
      <AnotherChild count={count} />
    </div>
  );
}

function Child({ count, onIncrement }) {
  return <button onClick={onIncrement}>{count}</button>;
}
```

## 11. What is state initialization and lazy initial state?
**Performance Optimization:**
```javascript
// Normal initialization
const [state, setState] = useState(expensiveCalculation());

// ✅ Lazy initialization - runs only once
const [state, setState] = useState(() => expensiveCalculation());

// Use case example
const [user, setUser] = useState(() => {
  const saved = localStorage.getItem('user');
  return saved ? JSON.parse(saved) : null;
});
```

## 12. How do you handle form state efficiently?
**Common Patterns:**
```javascript
// Single state object for form
const [formData, setFormData] = useState({
  name: '',
  email: '',
  age: ''
});

const handleChange = (e) => {
  const { name, value } = e.target;
  setFormData(prev => ({
    ...prev,
    [name]: value
  }));
};

// Or custom hook for reusability
function useForm(initialValues) {
  const [values, setValues] = useState(initialValues);
  
  const handleChange = (e) => {
    setValues({
      ...values,
      [e.target.name]: e.target.value
    });
  };
  
  return [values, handleChange];
}
```

## Quick Decision Tree:
```
Need to share data?
├── Between parent-child only? → Use Props
├── Multiple levels down? → Consider Context
├── Global state (theme, auth)? → Context
└── Complex state logic? → useReducer + Context
```

## Most Important Points for Interview:
1. **Never mutate state directly** - Always create new objects/arrays
2. **Props are read-only** - Use callbacks to communicate back to parent
3. **Context solves props drilling** - But don't overuse it
4. **Lift state up** when multiple components need same data
5. **Controlled vs Uncontrolled** components for forms

## Common Patterns to Master:
```javascript
// State update pattern
setUser(prev => ({ ...prev, name: 'New Name' }));

// Context pattern
const value = useMemo(() => ({ user, setUser }), [user]);

// Form handling pattern
const handleChange = (e) => {
  setForm({ ...form, [e.target.name]: e.target.value });
};
```

## Interview Tips:
- ✅ Always mention immutability when updating state
- ✅ Show you understand when to use Context vs props
- ✅ Know the performance implications of Context
- ✅ Practice lifting state up scenarios
- ✅ Understand controlled component benefits
