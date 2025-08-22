# React Interview Questions - Theory Only with Confident Answers

## 🔹 React Basics

### **What is React? Why is it used?**

**Confident Answer:** React is a JavaScript library developed by Facebook for building user interfaces, particularly for web applications. It's component-based and focuses on creating reusable UI components.

**Why React is Popular:**
- **Component-Based Architecture:** Build encapsulated components that manage their own state
- **Virtual DOM:** Provides efficient updates and better performance  
- **Reusability:** Write once, use anywhere approach
- **Large Ecosystem:** Huge community support and third-party libraries
- **Easy Learning Curve:** Especially if you already know JavaScript
- **Backed by Facebook:** Strong corporate support and continuous development

---

### **What are Components in React? Difference between Functional and Class Components**

**Confident Answer:** Components are the building blocks of React applications. Think of them as reusable pieces of UI that can be combined to create complex interfaces.

**Functional Components (Modern Approach):**
- Use React Hooks for state management
- Simpler and cleaner syntax
- Better performance optimization
- Preferred modern approach
- Easier to test and debug

**Class Components (Traditional Approach):**
- Use this.state for state management
- More verbose syntax with lifecycle methods
- Legacy approach, still supported
- Required for error boundaries (currently)

**Key Differences:**
- Functional components use hooks, class components use this.state
- Functional components are simpler to write and understand
- Modern React development favors functional components

---

### **What is JSX? Why do we use it?**

**Confident Answer:** JSX (JavaScript XML) is a syntax extension that allows you to write HTML-like code inside JavaScript. It makes React code more readable and intuitive.

**Why JSX is Beneficial:**
- **Readability:** HTML-like syntax is familiar to developers
- **Integration:** Seamlessly combines JavaScript logic with markup
- **Type Safety:** Better error checking during development
- **Tooling Support:** Better IDE support and debugging

**JSX Rules to Remember:**
- Must return a single parent element (or use React.Fragment)
- Use `className` instead of `class`
- Use camelCase for HTML attributes
- JavaScript expressions go inside curly braces {}

---

### **What is the Virtual DOM? How is it different from the real DOM?**

**Confident Answer:** Virtual DOM is a JavaScript representation of the actual DOM kept in memory. React uses it to optimize performance by minimizing expensive DOM operations.

**How Virtual DOM Works:**
1. When state changes, React creates a new Virtual DOM tree
2. It compares (diffs) the new tree with the previous Virtual DOM tree
3. Calculates the minimum changes needed
4. Updates only the changed parts in the real DOM

**Benefits of Virtual DOM:**
- **Performance:** Batch updates and minimal DOM manipulation
- **Predictability:** Easier to understand what changes when
- **Cross-browser Compatibility:** React handles browser differences
- **Developer Experience:** Declarative programming model

---

### **What are props and state? Difference between them**

**Confident Answer:** Props and state are two ways to manage data in React components, but they serve different purposes.

**Props (Properties):**
- Data passed from parent component to child component
- Read-only and immutable
- Used for component communication
- Like function parameters in regular programming

**State:**
- Internal data managed within a component
- Mutable and can be changed
- Triggers re-render when updated
- Like instance variables in a class

**Key Differences:**
- Props come from outside, state is internal
- Props are read-only, state can be modified
- Props are passed down, state is managed locally
- Changing props doesn't trigger re-render, changing state does

---

### **Controlled vs Uncontrolled Components - What is the difference?**

**Confident Answer:** This refers to how form elements manage their data.

**Controlled Components (Recommended):**
- Form data is handled by React state
- Input values are controlled by React
- onChange handlers update state
- Single source of truth
- Better for validation and form submission

**Uncontrolled Components:**
- Form data is handled by the DOM itself
- Access values using refs when needed
- Less React involvement
- Useful for simple forms or integrating with non-React libraries

**Why Controlled is Preferred:**
- Better data validation
- Easier form submission handling
- More React-like approach
- Better testing capabilities

---

## 🔹 React Hooks (Important for 2 Years Experience)

### **What are React Hooks? Why were they introduced?**

**Confident Answer:** React Hooks are functions that let you use state and other React features in functional components. They were introduced in React 16.8 to solve several problems with class components.

**Problems Hooks Solved:**
- **Complex Class Components:** Classes were hard to understand and optimize
- **Logic Reuse:** Difficult to share stateful logic between components  
- **Lifecycle Confusion:** Related logic was scattered across different lifecycle methods
- **Performance Issues:** Classes didn't optimize as well

**Rules of Hooks:**
1. Only call hooks at the top level (never inside loops, conditions, or nested functions)
2. Only call hooks from React functions (components or custom hooks)

---

### **useState - Explain with example**

**Confident Answer:** useState is the most basic hook that adds state to functional components. It returns an array with the current state value and a function to update it.

**How useState Works:**
- Takes initial value as parameter
- Returns array with [currentValue, setterFunction]
- Setter function triggers component re-render
- Can store any type of data (string, number, object, array)

**Key Points for Interview:**
- Always use destructuring to get state and setter
- State updates are asynchronous
- For objects/arrays, always create new instances (immutability)
- Can have multiple useState calls in one component

---

### **useEffect - Explain with example. What is the dependency array?**

**Confident Answer:** useEffect lets you perform side effects in functional components. It's like componentDidMount, componentDidUpdate, and componentWillUnmount combined.

**Dependency Array Explained:**
- **No array:** Effect runs after every render (not recommended)
- **Empty array []:** Effect runs only once after initial mount
- **With dependencies [value1, value2]:** Effect runs when any dependency changes

**Common Use Cases:**
- API calls and data fetching
- Setting up subscriptions or timers
- Manually changing DOM
- Cleanup operations

**Best Practices:**
- Always include dependencies that are used inside effect
- Use multiple useEffect for different concerns
- Return cleanup function when needed

---

### **Cleanup in useEffect - How do you clean up inside useEffect?**

**Confident Answer:** Cleanup is done by returning a function from useEffect. This cleanup function runs before the component unmounts or before the effect runs again.

**When Cleanup is Needed:**
- Timers and intervals (clearInterval, clearTimeout)
- Event listeners (removeEventListener)
- API request cancellation
- WebSocket connections
- Subscriptions

**Why Cleanup is Important:**
- Prevents memory leaks
- Avoids updating state on unmounted components
- Improves application performance
- Prevents unexpected behavior

---

### **useContext Hook - What is useContext? How does it solve prop drilling?**

**Confident Answer:** useContext allows you to access context values directly in functional components without prop drilling.

**Prop Drilling Problem:**
- Passing data through many component levels
- Components that don't need data still receive props
- Makes code harder to maintain and understand

**useContext Solution:**
- Direct access to context values from any component
- No need to pass props through intermediate components
- Cleaner component tree structure

**When to Use useContext:**
- Global data like user authentication, theme, language
- Avoiding prop drilling through 3+ component levels
- Shared configuration across app

---

### **useContext vs Props - When would you use Context API over props?**

**Confident Answer:** Use Context API when data needs to be accessed by many components at different nesting levels.

**Use Props When:**
- Direct parent-child communication
- Data is only needed by immediate children
- Simple component hierarchies

**Use Context When:**
- Data needed by components far down the tree
- Global state like theme, user info, language
- Multiple components need same data
- Avoiding prop drilling

---

### **useReducer Hook - What is useReducer? When to use it over useState?**

**Confident Answer:** useReducer is a hook for managing complex state logic. It's similar to Redux patterns but built into React.

**How useReducer Works:**
- Takes a reducer function and initial state
- Returns current state and dispatch function
- State updates happen through dispatched actions
- Reducer function determines how state changes

**Use useReducer When:**
- Complex state logic with multiple sub-values
- Next state depends on previous state
- Managing state transitions with specific actions
- Need predictable state updates

**Use useState When:**
- Simple state (strings, numbers, booleans)
- Independent state variables
- Direct state updates

---

### **useReducer vs useState - What are the differences?**

**Confident Answer:**

**useReducer:**
- Better for complex state logic
- Centralized state updates
- Predictable state transitions
- Good for multiple related state values
- Similar to Redux pattern

**useState:**
- Better for simple state
- Direct state updates
- Easier to understand
- Good for independent values
- Less boilerplate

**Practical Example When to Choose:**
- Form with multiple fields and validation → useReducer
- Simple toggle or counter → useState
- Shopping cart with add/remove/update → useReducer
- Loading state → useState

---

### **useReducer with useContext - How do you combine them for global state?**

**Confident Answer:** Combining useReducer with useContext creates a powerful global state management solution, similar to Redux but with less boilerplate.

**Benefits of This Combination:**
- Global state management without Redux
- Predictable state updates
- Better performance than useState with Context
- Type-safe state transitions

**Use Cases:**
- Application-wide state (user, cart, preferences)
- Complex state that multiple components need
- When you need Redux-like patterns but want to stay with React

---

### **useMemo Hook - What is useMemo? When and why do we use it?**

**Confident Answer:** useMemo is a performance optimization hook that memoizes expensive calculations and only recalculates when dependencies change.

**When to Use useMemo:**
- Expensive calculations or computations
- Creating objects/arrays that are dependencies for other hooks
- Filtering or sorting large datasets
- Preventing unnecessary re-renders of child components

**When NOT to Use useMemo:**
- Simple calculations (addition, string concatenation)
- Values that change on every render anyway
- When memoization overhead is more than the calculation cost

**Performance Benefits:**
- Avoids expensive recalculations
- Reduces unnecessary child component re-renders
- Improves app responsiveness

---

### **useMemo vs useCallback - What's the difference between them?**

**Confident Answer:**

**useMemo:**
- Memoizes computed values
- Returns the memoized value directly
- Used for expensive calculations
- Example: `useMemo(() => expensiveCalculation(a, b), [a, b])`

**useCallback:**
- Memoizes functions
- Returns the memoized function
- Used for event handlers and callback functions
- Example: `useCallback((id) => handleClick(id), [dependency])`

**When to Use Each:**
- useMemo for values and calculations
- useCallback for functions passed to child components
- Both help prevent unnecessary re-renders

---

### **useMemo Performance - How does useMemo help with performance optimization?**

**Confident Answer:** useMemo optimizes performance by preventing expensive recalculations and unnecessary re-renders.

**Performance Benefits:**
1. **Expensive Calculations:** Only recalculates when dependencies change
2. **Reference Equality:** Maintains same object reference if dependencies haven't changed
3. **Child Re-renders:** Prevents child components from re-rendering unnecessarily
4. **Memory Usage:** Balances computation vs memory tradeoffs

**Real-world Example:**
- Filtering large lists of data
- Complex mathematical calculations
- Processing API responses
- Creating derived state from props

---

### **useCallback Hook - What is useCallback? When do you use it?**

**Confident Answer:** useCallback memoizes functions to prevent unnecessary re-creation on every render, which helps optimize performance.

**Problem it Solves:**
- Functions are recreated on every component render
- Child components re-render when they receive new function references
- Performance issues in large applications

**When to Use useCallback:**
- Functions passed as props to memoized child components
- Functions used as dependencies in other hooks (useEffect, useMemo)
- Event handlers in lists
- Callback functions that trigger expensive operations

---

### **useCallback with Dependencies - How do dependencies work in useCallback?**

**Confident Answer:** Dependencies in useCallback work similar to useEffect - the function is recreated only when dependencies change.

**Dependency Guidelines:**
- Include all values from component scope used inside the callback
- Use empty array [] if function doesn't depend on any values
- Use functional updates to reduce dependencies
- ESLint plugin helps identify missing dependencies

**Common Mistakes:**
- Missing dependencies (stale closure)
- Including unnecessary dependencies
- Not using functional updates when possible

---

### **useCallback vs Regular Function - Why not just define functions inside component?**

**Confident Answer:** Regular functions defined inside components are recreated on every render, which can cause performance issues.

**Problems with Regular Functions:**
- New function reference on every render
- Breaks React.memo optimization in child components
- Triggers unnecessary re-renders
- Can cause infinite loops in useEffect

**Benefits of useCallback:**
- Stable function reference when dependencies don't change
- Optimizes child component performance
- Prevents unnecessary effect executions
- Better for performance-critical applications

---

### **useRef Hook - What is useRef? What are its use cases?**

**Confident Answer:** useRef returns a mutable ref object whose .current property can hold any value. It doesn't trigger re-renders when the value changes.

**Main Use Cases:**
1. **DOM Access:** Focus inputs, scroll to elements, measure dimensions
2. **Storing Mutable Values:** Timers, counters, previous values
3. **Avoiding Re-renders:** Store values without triggering updates
4. **Instance Variables:** Similar to instance variables in class components

**Key Characteristics:**
- Value persists across re-renders
- Changing .current doesn't trigger re-render
- Initialized once and maintained throughout component lifecycle

---

### **useRef vs useState - When to use useRef over useState?**

**Confident Answer:**

**Use useRef When:**
- You need to access DOM elements directly
- Storing values that shouldn't trigger re-renders
- Keeping track of previous values
- Timers and intervals that need cleanup
- Mutable values that don't affect UI

**Use useState When:**
- Value changes should trigger re-render
- Data affects what's displayed to user
- Form inputs and user interactions
- Any UI state

**Key Difference:** useRef doesn't trigger re-renders, useState does.

---

### **useRef for DOM Access - How do you access DOM elements using useRef?**

**Confident Answer:** useRef is the React way to directly access DOM elements, similar to document.querySelector but in a React-compliant manner.

**Common DOM Operations:**
- Focus inputs programmatically
- Scroll to specific elements
- Measure element dimensions
- Integrate with third-party DOM libraries
- Trigger imperative animations

**Best Practices:**
- Always check if .current exists before using
- Prefer React patterns over direct DOM manipulation
- Use sparingly and only when necessary
- Clean up any DOM listeners in useEffect cleanup

---

### **useRef Current Property - Why do we use .current property in useRef?**

**Confident Answer:** The .current property is how React maintains the reference while keeping the useRef object stable across re-renders.

**Why .current Design:**
- Provides mutable container for the actual value
- Allows React to maintain object identity across renders
- Enables changing the value without triggering re-renders
- Consistent API pattern across React hooks

**Important Notes:**
- Always access via .current property
- .current can be null initially
- Changing .current doesn't notify React

---

### **Custom Hooks - What are they? Have you created one?**

**Confident Answer:** Custom hooks are JavaScript functions that start with "use" and can call other hooks. They extract component logic into reusable functions.

**Benefits of Custom Hooks:**
- **Code Reuse:** Share logic across multiple components
- **Separation of Concerns:** Keep components focused on UI
- **Easier Testing:** Test logic independently
- **Better Organization:** Cleaner component code

**Examples of Custom Hooks:**
- useApi for data fetching
- useLocalStorage for browser storage
- useDebounce for input debouncing
- useWindowSize for responsive design

**Interview Tip:** Even if you haven't created one, you can describe a simple example like useCounter or useToggle.

---

## 🔹 Advanced React Hooks Scenarios (1-2 Years Experience)

### **Hook Rules - What are the rules of hooks? Why can't we call hooks inside loops or conditions?**

**Confident Answer:** React hooks must follow specific rules to ensure consistent behavior across re-renders.

**The Two Rules:**
1. **Top Level Only:** Never call hooks inside loops, conditions, or nested functions
2. **React Functions Only:** Only call hooks from React function components or custom hooks

**Why These Rules Exist:**
- React uses call order to track hook state
- Conditional hooks would break this internal tracking
- Ensures hooks are called in same order every time
- Prevents bugs and inconsistent behavior

**How React Tracks Hooks:**
- React maintains an internal list of hooks per component
- Uses array index to match hooks with their state
- Changing order breaks this mapping

---

### **Multiple useEffect - Can you have multiple useEffect in one component? When would you do this?**

**Confident Answer:** Yes, you can and should have multiple useEffect hooks in one component for separation of concerns.

**Benefits of Multiple useEffect:**
- **Separation of Concerns:** Each effect handles one specific task
- **Different Dependencies:** Each effect can have its own dependency array
- **Easier Debugging:** Isolate different side effects
- **Better Code Organization:** Related logic stays together

**Common Scenarios:**
- One for data fetching, another for window resize listeners
- Separate effects for different API calls
- One for timers, another for subscriptions
- Different cleanup requirements

---

### **useEffect Dependencies - What happens if you miss a dependency in useEffect array?**

**Confident Answer:** Missing dependencies can cause stale closures and bugs where your effect uses outdated values.

**Problems with Missing Dependencies:**
- **Stale Closures:** Effect captures old values
- **Bugs:** Unexpected behavior with outdated data
- **Infinite Loops:** Sometimes effects run more than expected
- **Performance Issues:** Effects might not update when they should

**Solutions:**
- Use ESLint plugin react-hooks/exhaustive-deps
- Include all values from component scope used in effect
- Use functional updates to reduce dependencies
- Split effects if they have different concerns

---

### **Performance Optimization - How do useMemo and useCallback help in avoiding unnecessary re-renders?**

**Confident Answer:** Both hooks help maintain referential equality, which is crucial for React's optimization mechanisms.

**How They Prevent Re-renders:**
- **Stable References:** Same object/function reference when dependencies unchanged
- **React.memo Optimization:** Memoized components only re-render when props actually change
- **Child Component Optimization:** Prevents cascading re-renders down the tree

**When to Use for Performance:**
- Large lists with many items
- Complex calculations in parent components
- Expensive child components
- Deeply nested component trees

---

### **Context Performance - How can useContext cause performance issues? How to solve them?**

**Confident Answer:** useContext can cause performance issues because all consuming components re-render when context value changes.

**Performance Problems:**
- All context consumers re-render on any context change
- Large objects in context cause unnecessary re-renders
- Frequent context updates impact performance

**Solutions:**
- **Split Contexts:** Separate frequently changing values
- **Memoize Context Values:** Use useMemo for context provider values
- **Component Optimization:** Use React.memo for context consumers
- **Selective Subscriptions:** Only subscribe to needed parts of context

---

### **useReducer Complex State - When would you choose useReducer over multiple useState hooks?**

**Confident Answer:** Choose useReducer when state logic becomes complex and multiple state variables are interdependent.

**Scenarios for useReducer:**
- **Related State Values:** Multiple pieces of state that change together
- **Complex State Transitions:** State changes follow specific business rules
- **Multiple Actions:** Different ways to update the same state
- **Validation Logic:** Complex validation that affects multiple fields

**Examples:**
- Form with validation and multiple fields
- Shopping cart with add/remove/update operations
- Game state with multiple interconnected values
- Complex UI state machines

---

### **Refs vs State - Give 3 practical examples where you'd use useRef instead of useState**

**Confident Answer:**

**1. Timer IDs and Cleanup:**
- Store setInterval/setTimeout IDs
- Don't need re-render when timer ID changes
- Need to clear timer on component unmount

**2. Previous Values Tracking:**
- Store previous props or state values
- Compare current vs previous without triggering re-render
- Useful for detecting changes

**3. DOM Element References:**
- Focus management (input focus on button click)
- Scroll to element functionality
- Measuring element dimensions
- Integration with third-party DOM libraries

**Key Principle:** Use useRef for values that don't affect what's rendered to the user.

---

### **Memory Leaks - How can improper use of useEffect cause memory leaks?**

**Confident Answer:** Memory leaks in useEffect typically happen when cleanup is not properly implemented.

**Common Memory Leak Scenarios:**
- **Uncleaned Timers:** setInterval/setTimeout not cleared
- **Event Listeners:** Not removing event listeners on unmount
- **API Subscriptions:** WebSocket connections not closed
- **State Updates:** Updating state after component unmounts

**Prevention Strategies:**
- Always return cleanup function from useEffect
- Use flags to prevent state updates on unmounted components
- Cancel API requests in cleanup
- Remove all event listeners
- Clear all timers and intervals

---

### **Testing Hooks - How do you test custom hooks?**

**Confident Answer:** Custom hooks can be tested using React Testing Library's renderHook utility.

**Testing Approach:**
- Use renderHook to test hook in isolation
- Test hook behavior, not implementation details
- Mock dependencies like API calls
- Test different scenarios and edge cases

**What to Test:**
- Initial hook state
- State changes after actions
- Side effects (API calls, localStorage)
- Error handling
- Cleanup functionality

**Best Practices:**
- Test hooks separately from components
- Mock external dependencies
- Test both success and error scenarios

---

## 🔹 Component Lifecycle & Rendering

### **Lifecycle Methods - What are React component lifecycle methods?**

**Confident Answer:** Component lifecycle methods are special methods that run at specific times during a component's life cycle.

**Three Main Phases:**

**1. Mounting (Component Creation):**
- constructor(): Initialize state and bind methods
- componentDidMount(): After component is rendered, perfect for API calls

**2. Updating (Props or State Changes):**
- componentDidUpdate(): After component updates, compare prev props/state

**3. Unmounting (Component Removal):**
- componentWillUnmount(): Cleanup before component is removed

**Modern Approach:** Functional components with hooks have largely replaced class component lifecycle methods.

---

### **Hooks vs Lifecycle Methods - How do hooks replace lifecycle methods in functional components?**

**Confident Answer:** Hooks provide equivalent functionality to class lifecycle methods but with more flexibility.

**Lifecycle to Hooks Mapping:**

**componentDidMount** → `useEffect(() => {}, [])`
- Empty dependency array runs effect once after mount

**componentDidUpdate** → `useEffect(() => {}, [dependency])`
- Effect with dependencies runs when dependencies change

**componentWillUnmount** → `useEffect(() => { return () => cleanup(); }, [])`
- Cleanup function in useEffect

**Advantages of Hooks:**
- Can have multiple effects for different concerns
- More flexible dependency tracking
- Easier to reuse logic between components
- Less verbose than class components

---

### **React.memo - What is it and why do we use it?**

**Confident Answer:** React.memo is a higher-order component that memoizes the rendered output of a component, preventing unnecessary re-renders when props haven't changed.

**How React.memo Works:**
- Compares current props with previous props (shallow comparison)
- Only re-renders if props have actually changed
- Can provide custom comparison function for complex props

**When to Use React.memo:**
- Component re-renders frequently with same props
- Component has expensive rendering logic
- Child components in lists
- Performance optimization is needed

**When NOT to Use:**
- Props change frequently anyway
- Component is already fast to render
- Over-optimization can hurt performance

---

### **React.Fragment vs div Wrapper - What is the difference?**

**Confident Answer:** React.Fragment allows grouping multiple elements without adding extra DOM nodes, while div creates an actual HTML element.

**Problems with Extra Divs:**
- Unnecessary DOM nodes affect performance
- Can break CSS layouts (flexbox, grid)
- Semantic HTML structure issues
- Larger DOM tree size

**Benefits of Fragment:**
- No extra DOM nodes
- Cleaner HTML output
- Better performance
- Maintains proper semantic structure
- CSS layouts work as expected

**Fragment Syntax Options:**
- `<React.Fragment>`: Full syntax, allows key prop
- `<>`: Short syntax, more common
- React.Fragment with key for list items

---

## 🔹 State Management

### **Sharing State - How do you share state between components?**

**Confident Answer:** There are several patterns for sharing state depending on component relationship and complexity.

**State Sharing Methods:**

**1. Props Down, Callbacks Up:**
- Pass data down via props
- Pass callback functions for child to update parent
- Good for direct parent-child communication

**2. Lifting State Up:**
- Move state to closest common ancestor
- Share state between sibling components
- Pass down state and update functions

**3. Context API:**
- For global or deeply nested state
- Avoid prop drilling
- Good for theme, auth, user preferences

**4. External State Management:**
- Redux, Zustand, or other libraries
- For complex applications
- Centralized state management

---

### **Context API - What is it? When to use it?**

**Confident Answer:** Context API is React's built-in solution for sharing data across component tree without prop drilling.

**How Context Works:**
1. Create context with createContext()
2. Wrap components with Provider
3. Consume context with useContext()

**Perfect Use Cases:**
- User authentication status
- Theme preferences (dark/light mode)
- Language/localization settings
- Global app configuration
- Shopping cart state

**When to Avoid Context:**
- Simple parent-child communication
- Frequently changing data (performance issues)
- Local component state
- When props are sufficient

---

### **Redux - Core concepts: store, reducer, actions**

**Confident Answer:** Redux is a predictable state container that implements a unidirectional data flow pattern.

**Core Concepts:**

**1. Store:**
- Single source of truth for application state
- Holds the entire state tree
- Only way to change state is by dispatching actions

**2. Actions:**
- Plain JavaScript objects describing what happened
- Must have a 'type' property
- Can carry additional data in 'payload'

**3. Reducers:**
- Pure functions that specify how state changes
- Take current state and action, return new state
- Never mutate existing state, always return new state

**Redux Flow:**
Component → Action → Reducer → Store → Component

---

### **Context API vs Redux - Difference between them**

**Confident Answer:**

**Context API:**
- **Pros:** Built into React, no extra dependencies, simple setup
- **Cons:** Performance issues with frequent updates, limited dev tools
- **Best for:** Small to medium apps, simple global state

**Redux:**
- **Pros:** Excellent dev tools, predictable updates, great for complex apps
- **Cons:** More boilerplate, learning curve, additional dependency
- **Best for:** Large applications, complex state logic, team development

**Decision Factors:**
- **App Size:** Context for small/medium, Redux for large
- **State Complexity:** Context for simple, Redux for complex
- **Team Size:** Redux better for large teams
- **Dev Tools:** Redux has superior debugging tools

---

## 🔹 React Routing

### **React Router - What is it?**

**Confident Answer:** React Router is the standard routing library for React applications that enables navigation between different components while maintaining SPA behavior.

**Key Features:**
- Declarative routing configuration
- Dynamic route matching
- Nested routing support
- Browser history management
- Route parameters and query strings

**Main Components:**
- **BrowserRouter:** Uses HTML5 history API
- **Routes:** Container for route definitions
- **Route:** Maps URL paths to components
- **Link:** Navigation component
- **useNavigate:** Hook for programmatic navigation

---

### **Navigation - How do you implement navigation between pages?**

**Confident Answer:** React Router provides multiple ways to handle navigation.

**Declarative Navigation:**
- Use Link component for clickable navigation
- Use NavLink for navigation with active states
- Better than anchor tags for SPA behavior

**Programmatic Navigation:**
- Use useNavigate hook for navigation based on user actions
- Navigate after form submissions
- Redirect based on authentication status
- Handle navigation in event handlers

**Route Configuration:**
- Define routes with path and element props
- Support for nested routes and layouts
- Dynamic routes with parameters
- Protected routes with conditional rendering

---

### **Link vs a Tag - Difference between them**

**Confident Answer:**

**<a> Tag (Traditional HTML):**
- Causes full page reload
- Loses all React component state
- Slower navigation experience
- Breaks single-page application behavior
- Triggers server request

**<Link> Component (React Router):**
- Client-side navigation only
- Preserves React application state
- Faster navigation (no page reload)
- Maintains SPA experience
- Updates URL without server request

**When to Use Each:**
- **Link:** Internal navigation within React app
- **<a> tag:** External links, file downloads, email links

**Performance Impact:**
- Link components provide much better user experience
- No loading states between pages
- Instant navigation feel

---