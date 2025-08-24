# State Management - Essential Interview Questions

## 🎯 Core Questions

### Q1: What's the difference between State and Props?

**Answer:**

**State and Props are the fundamental data management concepts in React** - understanding their differences is crucial for building scalable applications.

**🎯 Interview-Ready Definition:**

**State** is **component-owned, mutable data** that represents the component's internal condition and can trigger re-renders when changed.

**Props** are **immutable data passed from parent to child** components, enabling unidirectional data flow and component communication.

**📊 Comprehensive Comparison:**

| Aspect | State | Props |
|--------|-------|-------|
| **Ownership** | Belongs to component itself | Passed from parent component |
| **Mutability** | Mutable (can be changed) | Immutable (read-only in child) |
| **Scope** | Local to component | Flows down from parent |
| **Purpose** | Internal component logic | Component communication |
| **Re-render Trigger** | Yes, when setState is called | Yes, when parent re-renders |
| **Initialization** | useState() or constructor | Received as function parameters |

**🚀 Real-World Examples:**

**State - Component's Own Data:**
```javascript
// Class Component State
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,        // Component owns this data
      isVisible: true, // Component controls this
      userInput: ''    // Local state for form inputs
    };
  }
  
  // Component can modify its own state
  increment = () => {
    this.setState(prevState => ({ 
      count: prevState.count + 1 
    }));
  }
  
  render() {
    return (
      <div>
        <h2>Count: {this.state.count}</h2>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}

// Functional Component with Hooks
function Counter() {
  const [count, setCount] = useState(0);           // Local state
  const [isLoading, setIsLoading] = useState(false); // Component owns this
  
  const handleIncrement = () => {
    setCount(prev => prev + 1); // Component updates its own state
  };
  
  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={handleIncrement}>Increment</button>
    </div>
  );
}
```

**Props - Data Passed from Parent:**
```javascript
// Props come from parent component
function UserCard({ name, email, age, onEdit }) { // These are PROPS
  return (
    <div className="user-card">
      <h3>{name}</h3>           {/* Using props */}
      <p>Email: {email}</p>     {/* Props are read-only */}
      <p>Age: {age}</p>
      <button onClick={onEdit}>Edit</button>  {/* Function prop */}
    </div>
  );
}

// Parent component passes props
function UserList() {
  const [users, setUsers] = useState([
    { id: 1, name: 'John', email: 'john@example.com', age: 25 }
  ]);
  
  return (
    <div>
      {users.map(user => (
        <UserCard 
          key={user.id}
          name={user.name}        // Passing props to child
          email={user.email}      // Parent's data becomes child's props
          age={user.age}
          onEdit={() => editUser(user.id)}
        />
      ))}
    </div>
  );
}
```

**Key Differences:**

| Aspect | State | Props |
|--------|-------|-------|
| **Ownership** | Belongs to component | Passed from parent |
| **Mutability** | Can be changed by component | Read-only in child |
| **Purpose** | Component's internal data | Communication from parent |
| **Scope** | Local to component | Flows down from parent |

**Data Flow Example:**
```javascript
function App() {
  const [user, setUser] = useState({ name: 'John', age: 25 }); // STATE in App
  
  return (
    <UserProfile 
      userName={user.name}    // App's STATE becomes child's PROPS
      userAge={user.age}
      onUpdate={setUser}      // Function to update parent's STATE
    />
  );
}

function UserProfile({ userName, userAge, onUpdate }) { // Receiving PROPS
  const [isEditing, setIsEditing] = useState(false);    // Local STATE
  
  return (
    <div>
      <h2>{userName}</h2>     {/* Using PROPS from parent */}
      <p>Age: {userAge}</p>
      {isEditing && (         {/* Using local STATE */}
        <EditForm 
          name={userName}     // Passing PROPS down further
          onSave={onUpdate}   
        />
      )}
    </div>
  );
}
```

---

### Q2: What is Props Drilling and why is it a problem?

**Answer:**

**Props Drilling** is an **anti-pattern** where you pass props through multiple component layers, even when intermediate components don't need those props. It's one of the most common scalability issues in React applications.

**🎯 Interview Definition:**
Props Drilling occurs when **data needs to travel through multiple component levels** to reach deeply nested components, forcing intermediate components to act as mere "prop couriers."

**🚨 Real-World Problem Example:**

```javascript
// App component has user data
function App() {
  const [user, setUser] = useState({
    name: 'John Doe',
    email: 'john@example.com',
    preferences: { theme: 'dark', language: 'en' }
  });
  
  return (
    <Dashboard 
      user={user}           // Level 1: Passing to Dashboard
      onUpdateUser={setUser}
    />
  );
}

// Dashboard doesn't need user data, just passes it down
function Dashboard({ user, onUpdateUser }) {
  return (
    <div className="dashboard">
      <Sidebar />
      <MainContent 
        user={user}         // Level 2: Dashboard becomes prop courier
        onUpdateUser={onUpdateUser}
      />
    </div>
  );
}

// MainContent also just passes it down (another unnecessary middleman)
function MainContent({ user, onUpdateUser }) {
  return (
    <div className="main-content">
      <Header />
      <UserSection 
        user={user}         // Level 3: Still just passing through
        onUpdateUser={onUpdateUser}
      />
    </div>
  );
}

// UserSection also passes it down (getting ridiculous)
function UserSection({ user, onUpdateUser }) {
  return (
    <div className="user-section">
      <UserProfile 
        user={user}         // Level 4: Finally reaches destination!
        onUpdateUser={onUpdateUser}
      />
    </div>
  );
}

// UserProfile actually uses the data (4 levels deep!)
function UserProfile({ user, onUpdateUser }) {
  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
      <button onClick={() => onUpdateUser({...user, name: 'Jane'})}>
        Update Name
      </button>
    </div>
  );
}
```

**💥 Why Props Drilling is Problematic:**

**1. Maintenance Nightmare:**
- Adding new props requires updating ALL intermediate components
- Removing props means touching multiple files
- Refactoring becomes exponentially difficult

**2. Unclear Dependencies:**
- Hard to determine which components actually need which props
- Component interfaces become bloated with irrelevant props
- Difficult to understand data flow

**3. Tight Coupling:**
- Intermediate components become tightly coupled to child requirements
- Changes in deep components force changes in unrelated parent components
- Violates Single Responsibility Principle

**4. Testing Complexity:**
- Must mock all props for every intermediate component
- Test setup becomes verbose and brittle

**🚨 When Props Drilling Becomes a Problem:**
- **More than 2-3 levels** of prop passing
- **Multiple unrelated props** being passed through same path
- **Frequent changes** to the passed data structure
- **Many components** in the middle that don't use the props

**💡 Interview Pro Tips:**
- **Recognize the Pattern**: "I see intermediate components acting as prop couriers"
- **Quantify the Problem**: "This creates tight coupling and maintenance overhead"
- **Propose Solutions**: "We can solve this with Context API, state management libraries, or component composition"

**🔧 Solutions to Props Drilling:**
1. **React Context API** (built-in solution)
2. **State Management Libraries** (Redux, Zustand, Recoil)
3. **Component Composition** (render props, children)
4. **Custom Hooks** (for sharing logic without prop drilling)
// Components become tightly coupled to their parent's data structure
function UserProfile({ user }) {
  return <div>{user.profile.personal.name}</div>; // Deeply nested dependency
}

// If parent changes user structure, this component breaks
```

**When Props Drilling Becomes a Problem:**
- More than 2-3 levels of passing
- Multiple props being passed through
- Intermediate components don't use the props
- Components become hard to test in isolation

---

### Q3: What is React Context and how does it solve Props Drilling?

**Answer:**

**React Context** is a built-in **state management solution** that enables components to **share data without explicit prop passing through intermediate components**. It creates a "data tunnel" that bypasses the component tree.

**🎯 Interview Definition:**
Context provides **global-like state management** within a component subtree, solving props drilling by allowing any descendant component to directly access shared data.

**📊 How Context Solves Props Drilling:**

**Before (Props Drilling):**
```
App (user data) → Dashboard → MainContent → UserSection → UserProfile
    ↓ props         ↓ props    ↓ props      ↓ props       ↑ uses data
```

**After (Context):**
```
App (Provider wraps all)
├── Dashboard
├── MainContent  
├── UserSection
└── UserProfile ← directly accesses context (no prop chain!)
```

**🚀 Complete Context Implementation:**

**Step 1: Create Context & Custom Hook**
```javascript
// Create context with default value and type safety
const UserContext = React.createContext({
  user: null,
  updateUser: () => {},
  isLoading: false
});

// Custom hook for consuming context (Best Practice)
function useUser() {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error('useUser must be used within UserProvider');
  }
  return context;
}
```

**Step 2: Create Provider Component**
```javascript
function UserProvider({ children }) {
  const [user, setUser] = useState({
    name: 'John',
    email: 'john@example.com',
    preferences: { theme: 'dark' }
  });
  
  const updateUser = (newUserData) => {
    setUser(prevUser => ({ ...prevUser, ...newUserData }));
  };
  
  const contextValue = {
    user,
    updateUser
  };
  
  return (
    <UserContext.Provider value={contextValue}>
      {children}
    </UserContext.Provider>
  );
}
```

**Step 3: Wrap App with Provider**
```javascript
function App() {
  return (
    <UserProvider>
      <Dashboard />  {/* No need to pass user prop! */}
    </UserProvider>
  );
}
```

**Step 4: Consume Context in Components**
```javascript
// Dashboard - doesn't need to know about user
function Dashboard() {
  return (
    <div className="dashboard">
      <Sidebar />
      <MainContent />  {/* No props to pass! */}
    </div>
  );
}

// MainContent - also doesn't need user props
function MainContent() {
  return (
    <div className="main-content">
      <Header />
      <UserSection />  {/* No props here either! */}
    </div>
  );
}

// UserProfile - directly accesses user data from context
function UserProfile() {
  const { user, updateUser } = useUser(); // Direct access to context!
  
  return (
    <div>
      <h2>{user.name}</h2>
      <p>{user.email}</p>
      <button onClick={() => updateUser({ name: 'Jane' })}>
        Update Name
      </button>
    </div>
  );
}
```

**Comparison - Before and After Context:**
```javascript
// BEFORE (Props Drilling)
<App>
  <Dashboard user={user}>           // Props drilling starts
    <MainContent user={user}>       // Unnecessary prop passing
      <UserProfile user={user} />   // Finally used here
    </MainContent>
  </Dashboard>
</App>

// AFTER (Context)
<UserProvider>
  <App>
    <Dashboard>                     // Clean, no props
      <MainContent>                 // Clean, no props
        <UserProfile />             // Uses context directly
      </MainContent>
    </Dashboard>
  </App>
</UserProvider>
```

**Multiple Contexts Example:**
```javascript
// You can have multiple contexts
function App() {
  return (
    <UserProvider>
      <ThemeProvider>
        <NotificationProvider>
          <Dashboard />
        </NotificationProvider>
      </ThemeProvider>
    </UserProvider>
  );
}

// Component using multiple contexts
function UserProfile() {
  const { user, updateUser } = useUser();
  const { theme, toggleTheme } = useTheme();
  const { showNotification } = useNotification();
  
  const handleSave = () => {
    updateUser({ name: 'New Name' });
    showNotification('User updated!');
  };
  
  return (
    <div style={{ backgroundColor: theme.background }}>
      <h2>{user.name}</h2>
      <button onClick={handleSave}>Save</button>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
}
```

---

### Q4: When should you use Context vs Props vs State Management Libraries?

**Answer:**

**Use Local State when:**
```javascript
// Simple component-specific data
function Counter() {
  const [count, setCount] = useState(0); // Perfect for local state
  
  return (
    <div>
      <span>{count}</span>
      <button onClick={() => setCount(count + 1)}>+</button>
    </div>
  );
}

// Form inputs, toggles, temporary UI state
function LoginForm() {
  const [formData, setFormData] = useState({ email: '', password: '' });
  const [isSubmitting, setIsSubmitting] = useState(false);
  
  // This data doesn't need to be shared - perfect for local state
}
```

**Use Props when:**
```javascript
// Parent-child communication (1-2 levels)
function UserList({ users }) {                    // Props from parent
  return (
    <div>
      {users.map(user => (
        <UserCard                                  // Passing props down 1 level
          key={user.id}
          name={user.name}
          email={user.email}
          onEdit={() => handleEdit(user.id)}
        />
      ))}
    </div>
  );
}

// Configuration or styling props
function Button({ variant, size, onClick, children }) {
  const className = `btn btn-${variant} btn-${size}`;
  return <button className={className} onClick={onClick}>{children}</button>;
}
```

**Use Context when:**
```javascript
// Data needed by many components at different levels
const ThemeContext = createContext();

function App() {
  const [theme, setTheme] = useState('light');
  
  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Header />        {/* Header needs theme */}
      <Dashboard>
        <Sidebar />     {/* Sidebar needs theme */}
        <Content>
          <UserCard />  {/* UserCard needs theme - 3 levels deep! */}
        </Content>
      </Dashboard>
      <Footer />        {/* Footer needs theme */}
    </ThemeContext.Provider>
  );
}

// User authentication state
const AuthContext = createContext();

function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [isAuthenticated, setIsAuthenticated] = useState(false);
  
  // Login, logout, etc.
  
  return (
    <AuthContext.Provider value={{ user, isAuthenticated, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}
```

**Use State Management Libraries (Redux, Zustand) when:**
```javascript
// Complex state with many updates from different components
// Shopping cart example
const cartSlice = createSlice({
  name: 'cart',
  initialState: { items: [], total: 0 },
  reducers: {
    addItem: (state, action) => {
      // Complex logic for adding items
      // Updating totals, checking inventory, etc.
    },
    removeItem: (state, action) => {
      // Complex removal logic
    },
    applyDiscount: (state, action) => {
      // Business logic for discounts
    }
  }
});

// Multiple components need to update cart:
// - ProductList (add items)
// - Cart (remove items, update quantities)
// - Checkout (apply discounts)
// - Header (show cart count)
```

**Decision Tree:**
```javascript
// Ask these questions:
1. "Is this data used by only one component?"
   → YES: Use local state

2. "Is this data passed down 1-2 levels?"
   → YES: Use props

3. "Is this data needed by many components at different levels?"
   → YES: Consider Context

4. "Do you have complex state logic with many actions?"
   → YES: Consider state management library

5. "Do you need time-travel debugging, middleware, or devtools?"
   → YES: Use Redux

6. "Do you want something simpler than Redux?"
   → YES: Use Zustand or Context
```

**Real Project Example:**
```javascript
function EcommerceApp() {
  return (
    <ReduxProvider store={store}>          {/* Complex app state */}
      <AuthProvider>                       {/* User authentication */}
        <ThemeProvider>                    {/* UI theming */}
          <Router>
            <Header />                     {/* Uses all 3 contexts */}
            <ProductList />                {/* Uses Redux for products */}
            <ShoppingCart />               {/* Uses Redux for cart */}
          </Router>
        </ThemeProvider>
      </AuthProvider>
    </ReduxProvider>
  );
}

// Individual components use what they need:
function Header() {
  const { user } = useAuth();           // Context
  const { theme } = useTheme();         // Context  
  const cartItems = useSelector(state => state.cart.items); // Redux
  
  return <header>...</header>;
}
```

---

### Q5: How do you implement Context with useReducer for complex state?

**Answer:**

For complex state management, combining Context with useReducer provides Redux-like patterns without the library overhead.

**Step 1: Define State and Actions**
```javascript
// Define your state shape
const initialState = {
  todos: [],
  filter: 'all', // 'all', 'active', 'completed'
  loading: false,
  error: null
};

// Define action types
const actionTypes = {
  ADD_TODO: 'ADD_TODO',
  TOGGLE_TODO: 'TOGGLE_TODO',
  DELETE_TODO: 'DELETE_TODO',
  SET_FILTER: 'SET_FILTER',
  SET_LOADING: 'SET_LOADING',
  SET_ERROR: 'SET_ERROR',
  CLEAR_COMPLETED: 'CLEAR_COMPLETED'
};
```

**Step 2: Create Reducer Function**
```javascript
function todosReducer(state, action) {
  switch (action.type) {
    case actionTypes.ADD_TODO:
      return {
        ...state,
        todos: [
          ...state.todos,
          {
            id: Date.now(),
            text: action.payload,
            completed: false,
            createdAt: new Date().toISOString()
          }
        ]
      };
      
    case actionTypes.TOGGLE_TODO:
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      };
      
    case actionTypes.DELETE_TODO:
      return {
        ...state,
        todos: state.todos.filter(todo => todo.id !== action.payload)
      };
      
    case actionTypes.SET_FILTER:
      return {
        ...state,
        filter: action.payload
      };
      
    case actionTypes.CLEAR_COMPLETED:
      return {
        ...state,
        todos: state.todos.filter(todo => !todo.completed)
      };
      
    case actionTypes.SET_LOADING:
      return {
        ...state,
        loading: action.payload
      };
      
    case actionTypes.SET_ERROR:
      return {
        ...state,
        error: action.payload,
        loading: false
      };
      
    default:
      return state;
  }
}
```

**Step 3: Create Context and Provider**
```javascript
const TodoContext = createContext();

function TodoProvider({ children }) {
  const [state, dispatch] = useReducer(todosReducer, initialState);
  
  // Action creators (optional, but makes components cleaner)
  const actions = {
    addTodo: (text) => {
      if (text.trim()) {
        dispatch({ type: actionTypes.ADD_TODO, payload: text.trim() });
      }
    },
    
    toggleTodo: (id) => {
      dispatch({ type: actionTypes.TOGGLE_TODO, payload: id });
    },
    
    deleteTodo: (id) => {
      dispatch({ type: actionTypes.DELETE_TODO, payload: id });
    },
    
    setFilter: (filter) => {
      dispatch({ type: actionTypes.SET_FILTER, payload: filter });
    },
    
    clearCompleted: () => {
      dispatch({ type: actionTypes.CLEAR_COMPLETED });
    },
    
    // Async action example
    saveTodoToServer: async (todoData) => {
      dispatch({ type: actionTypes.SET_LOADING, payload: true });
      try {
        const response = await fetch('/api/todos', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify(todoData)
        });
        
        if (!response.ok) throw new Error('Failed to save todo');
        
        const savedTodo = await response.json();
        dispatch({ type: actionTypes.ADD_TODO, payload: savedTodo });
        dispatch({ type: actionTypes.SET_LOADING, payload: false });
      } catch (error) {
        dispatch({ type: actionTypes.SET_ERROR, payload: error.message });
      }
    }
  };
  
  // Computed values (selectors)
  const selectors = {
    getFilteredTodos: () => {
      const { todos, filter } = state;
      switch (filter) {
        case 'active':
          return todos.filter(todo => !todo.completed);
        case 'completed':
          return todos.filter(todo => todo.completed);
        default:
          return todos;
      }
    },
    
    getTodoStats: () => {
      const { todos } = state;
      return {
        total: todos.length,
        completed: todos.filter(todo => todo.completed).length,
        active: todos.filter(todo => !todo.completed).length
      };
    }
  };
  
  const contextValue = {
    state,
    actions,
    selectors
  };
  
  return (
    <TodoContext.Provider value={contextValue}>
      {children}
    </TodoContext.Provider>
  );
}

// Custom hook for using the context
function useTodos() {
  const context = useContext(TodoContext);
  if (!context) {
    throw new Error('useTodos must be used within TodoProvider');
  }
  return context;
}
```

**Step 4: Use in Components**
```javascript
// TodoList component
function TodoList() {
  const { state, actions, selectors } = useTodos();
  const filteredTodos = selectors.getFilteredTodos();
  
  if (state.loading) {
    return <div>Loading todos...</div>;
  }
  
  if (state.error) {
    return <div>Error: {state.error}</div>;
  }
  
  return (
    <ul>
      {filteredTodos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={actions.toggleTodo}
          onDelete={actions.deleteTodo}
        />
      ))}
    </ul>
  );
}

// TodoItem component
function TodoItem({ todo, onToggle, onDelete }) {
  return (
    <li>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => onToggle(todo.id)}
      />
      <span style={{ 
        textDecoration: todo.completed ? 'line-through' : 'none' 
      }}>
        {todo.text}
      </span>
      <button onClick={() => onDelete(todo.id)}>Delete</button>
    </li>
  );
}

// AddTodo component
function AddTodo() {
  const [text, setText] = useState('');
  const { actions } = useTodos();
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (text.trim()) {
      actions.addTodo(text);
      setText('');
    }
  };
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Add a todo..."
      />
      <button type="submit">Add</button>
    </form>
  );
}

// TodoStats component
function TodoStats() {
  const { selectors } = useTodos();
  const stats = selectors.getTodoStats();
  
  return (
    <div>
      <p>Total: {stats.total}</p>
      <p>Active: {stats.active}</p>
      <p>Completed: {stats.completed}</p>
    </div>
  );
}
```

**Benefits of Context + useReducer:**
1. **Predictable State Updates:** All state changes go through reducer
2. **Centralized Logic:** Business logic lives in one place
3. **Easy Testing:** Reducer is pure function, easy to test
4. **Better Organization:** Actions, state, and selectors are clearly separated
5. **Redux-like Patterns:** Familiar to developers who know Redux

---

### Q6: What are common Context performance pitfalls and how to avoid them?

**Answer:**

Context can cause unnecessary re-renders if not implemented carefully. Here are the main performance issues and solutions:

**Problem 1: Recreating Context Value Object**
```javascript
// BAD - Creates new object every render
function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  
  return (
    <UserContext.Provider 
      value={{ user, setUser }} // New object every time!
    >
      {children}
    </UserContext.Provider>
  );
}

// Result: ALL consumers re-render even if user didn't change
```

**Solution: useMemo for Context Value**
```javascript
// GOOD - Memoized context value
function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  
  const contextValue = useMemo(() => ({
    user,
    setUser
  }), [user]); // Only recreate if user changes
  
  return (
    <UserContext.Provider value={contextValue}>
      {children}
    </UserContext.Provider>
  );
}
```

**Problem 2: Large Context with Mixed Concerns**
```javascript
// BAD - Everything in one context
const AppContext = createContext();

function AppProvider({ children }) {
  const [user, setUser] = useState(null);
  const [theme, setTheme] = useState('light');
  const [notifications, setNotifications] = useState([]);
  const [cart, setCart] = useState([]);
  
  const value = useMemo(() => ({
    user, setUser,
    theme, setTheme,
    notifications, setNotifications,
    cart, setCart
  }), [user, theme, notifications, cart]);
  
  return <AppContext.Provider value={value}>{children}</AppContext.Provider>;
}

// Problem: Any change causes ALL consumers to re-render
```

**Solution: Split Contexts by Concern**
```javascript
// GOOD - Separate contexts
function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  const value = useMemo(() => ({ user, setUser }), [user]);
  return <UserContext.Provider value={value}>{children}</UserContext.Provider>;
}

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  const value = useMemo(() => ({ theme, setTheme }), [theme]);
  return <ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>;
}

function CartProvider({ children }) {
  const [cart, setCart] = useState([]);
  const value = useMemo(() => ({ cart, setCart }), [cart]);
  return <CartContext.Provider value={value}>{children}</CartContext.Provider>;
}

// Usage: Components only re-render when their needed context changes
function App() {
  return (
    <UserProvider>
      <ThemeProvider>
        <CartProvider>
          <Dashboard />
        </CartProvider>
      </ThemeProvider>
    </UserProvider>
  );
}
```

**Problem 3: Not Optimizing Consumer Components**
```javascript
// BAD - Component re-renders unnecessarily
function UserProfile() {
  const { user, theme, cart } = useAppContext(); // Gets all context data
  
  return (
    <div>
      <h2>{user.name}</h2>  {/* Only uses user, but re-renders when theme/cart change */}
    </div>
  );
}
```

**Solution: Selective Context Usage + React.memo**
```javascript
// GOOD - Only subscribe to what you need
const UserProfile = React.memo(function UserProfile() {
  const { user } = useUser(); // Only user context
  
  return (
    <div>
      <h2>{user.name}</h2>
    </div>
  );
});

// Advanced: Custom selector hook
function useUserSelector(selector) {
  const { user } = useUser();
  return useMemo(() => selector(user), [user, selector]);
}

// Usage
function UserProfile() {
  const userName = useUserSelector(user => user.name);
  
  return <h2>{userName}</h2>; // Only re-renders when name changes
}
```

**Problem 4: Functions in Context Value**
```javascript
// BAD - Functions recreated every render
function TodoProvider({ children }) {
  const [todos, setTodos] = useState([]);
  
  const value = useMemo(() => ({
    todos,
    addTodo: (text) => setTodos(prev => [...prev, { id: Date.now(), text }]),
    deleteTodo: (id) => setTodos(prev => prev.filter(t => t.id !== id))
  }), [todos]); // Functions recreated when todos change!
  
  return <TodoContext.Provider value={value}>{children}</TodoContext.Provider>;
}
```

**Solution: useCallback for Stable Functions**
```javascript
// GOOD - Stable function references
function TodoProvider({ children }) {
  const [todos, setTodos] = useState([]);
  
  const addTodo = useCallback((text) => {
    setTodos(prev => [...prev, { id: Date.now(), text }]);
  }, []); // No dependencies, function never changes
  
  const deleteTodo = useCallback((id) => {
    setTodos(prev => prev.filter(t => t.id !== id));
  }, []);
  
  const value = useMemo(() => ({
    todos,
    addTodo,
    deleteTodo
  }), [todos, addTodo, deleteTodo]);
  
  return <TodoContext.Provider value={value}>{children}</TodoContext.Provider>;
}
```

**Performance Testing Example:**
```javascript
// Add this to see re-renders
function UserProfile() {
  const { user } = useUser();
  
  console.log('UserProfile rendered'); // Check console for unnecessary renders
  
  return <h2>{user.name}</h2>;
}

// Use React DevTools Profiler to measure performance:
// 1. Open React DevTools
// 2. Go to Profiler tab
// 3. Click record
// 4. Interact with your app
// 5. Stop recording
// 6. See which components re-rendered and why
```

**Best Practices Summary:**
1. **Use useMemo** for context values
2. **Split contexts** by concern
3. **Use React.memo** for consumer components
4. **Use useCallback** for functions in context
5. **Only consume what you need** from context
6. **Consider state management libraries** for complex scenarios
7. **Profile your app** to identify performance issues
