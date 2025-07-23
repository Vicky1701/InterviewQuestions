# React.js Interview Questions - Complete Study Guide

## 🔥 MUST-KNOW Questions (High Priority)

### Core Concepts
**Q1: What is React? Why is it popular?**

**Q2: What is JSX? Can you write React without JSX?**

**Q3: What are React components? Difference between functional and class components?**

**Q4: What is the Virtual DOM? How does it work?**

**Q5: What are props? How do you pass data between components?**

**Q6: What is state in React? How do you manage state?**

**Q7: What is the difference between state and props?**

**Q8: What are React Hooks? Why were they introduced?**

### Essential Hooks
**Q9: Explain useState hook with an example.**

**Q10: Explain useEffect hook. What are its use cases?**

**Q11: What is the dependency array in useEffect?**

**Q12: How do you handle events in React?**

---

## 💡 LIKELY Questions (Medium Priority)

### Advanced Hooks
**Q13: What is useContext hook and when would you use it?**

**Q14: What is useReducer hook? When would you prefer it over useState?**

**Q15: What are useMemo and useCallback hooks? When to use them?**

**Q16: What is useRef hook and its use cases?**

**Q17: How do you create custom hooks?**

### Component Lifecycle & Patterns
**Q18: What is component lifecycle? Explain lifecycle methods.**

**Q19: How do you handle component mounting and unmounting in hooks?**

**Q20: What are controlled vs uncontrolled components?**

**Q21: What is prop drilling? How can you avoid it?**

**Q22: What are Higher-Order Components (HOCs)?**

**Q23: What is render props pattern?**

**Q24: What are React fragments? Why are they useful?**

### State Management
**Q25: What is Context API? How does it work?**

**Q26: When would you use Redux vs Context API?**

**Q27: What is the useReducer pattern?**

---

## 🎯 SCENARIO-BASED Questions (Medium Priority)

### Performance & Optimization
**Q28: How do you optimize React application performance?**

**Q29: What is React.memo? When would you use it?**

**Q30: How do you prevent unnecessary re-renders?**

**Q31: What causes components to re-render?**

**Q32: How do you handle large lists efficiently in React?**

**Q33: What is code splitting and lazy loading in React?**

### Practical Scenarios
**Q34: How do you handle forms in React?**

**Q35: How do you make API calls in React?**

**Q36: How do you handle error boundaries in React?**

**Q37: How would you implement authentication in a React app?**

**Q38: How do you handle routing in React applications?**

**Q39: How would you share state between sibling components?**

---

## 📝 CODING Questions (Be Ready to Write)

**Q40: Create a simple counter component using useState.**

**Q41: Create a component that fetches data on mount using useEffect.**

**Q42: Implement a toggle button component.**

**Q43: Create a simple todo list application.**

**Q44: Implement a search/filter functionality.**

**Q45: Create a custom hook for API calls.**

---

## 🔧 ADVANCED Questions (Lower Priority)

### Modern React Features
**Q46: What are React Portals? When would you use them?**

**Q47: What is React Suspense? How does it work?**

**Q48: What are Server Components in React?**

**Q49: What is Concurrent Rendering in React 18?**

**Q50: What are the new features in React 18?**

### Testing & DevTools
**Q51: How do you test React components?**

**Q52: What testing libraries do you use with React?**

**Q53: How do you debug React applications?**

**Q54: What are React Developer Tools?**

---

## ⚡ QUICK-FIRE Questions (Know the Answers)

**Q55: What is the default port for React development server?**
*Answer: 3000*

**Q56: Can you use multiple useState hooks in a single component?**
*Answer: Yes*

**Q57: What is the difference between React and ReactDOM?**
*Answer: React is the library, ReactDOM is for DOM manipulation*

**Q58: What is the difference between onClick and onclick?**
*Answer: onClick is React event, onclick is HTML attribute*

**Q59: What is the key prop in React lists?**
*Answer: Unique identifier for list items to help with efficient re-rendering*

**Q60: Can you update state directly in React?**
*Answer: No, always use setState or state setter functions*

---

## 🎪 ECOSYSTEM Questions (If Time Permits)

### Popular Libraries & Tools
**Q61: What is Create React App? What are its alternatives?**

**Q62: What is Next.js? How is it different from React?**

**Q63: What is React Router? How do you implement routing?**

**Q64: What are popular state management libraries for React?**

**Q65: What is Styled Components? How do you style React components?**

**Q66: What build tools do you use with React?**

### Architecture & Best Practices
**Q67: How do you structure a large React application?**

**Q68: What are React design patterns you should know?**

**Q69: How do you handle environment variables in React?**

**Q70: What are the best practices for React development?**

---

## 📋 PRIORITY STUDY ORDER

### Week 1 (Essential - Focus Here First)
- Q1-Q12 (Core concepts, JSX, components, hooks basics)
- Practice writing simple components with useState and useEffect
- Understand Virtual DOM and component lifecycle

### Week 2 (If You Have More Time)
- Q13-Q27 (Advanced hooks, patterns, state management)
- Q28-Q39 (Performance optimization and practical scenarios)
- Practice coding questions Q40-Q45

### Additional Time
- Q46-Q70 (Advanced features, testing, ecosystem)

---

## 🚀 INTERVIEW SURVIVAL TIPS

### Core Concepts You Must Explain Clearly:
1. **What React is**: JavaScript library for building user interfaces
2. **Virtual DOM**: Efficient way to update UI by comparing virtual representations
3. **Components**: Reusable pieces of UI that accept props and return JSX
4. **State vs Props**: Internal data vs external data passed down
5. **Hooks**: Functions that let you use state and lifecycle in functional components

### Safe Answers to Have Ready:
- "React is a JavaScript library for building user interfaces with a component-based architecture"
- "I understand React's declarative approach and virtual DOM concepts"
- "I'm comfortable with functional components, hooks, and state management"
- "I know how to handle component lifecycle and optimize performance"

### When Asked About Experience:
- "I have experience building React applications with modern hooks"
- "I understand React fundamentals and have worked with components and state management"
- "I'm familiar with React ecosystem including routing and state management patterns"

### Code Writing Strategy:
- Start with functional component structure
- Use modern hooks (useState, useEffect) instead of class components
- Explain your approach as you write
- Focus on clean, readable code with proper naming

---

## 🎯 MINIMUM VIABLE PREPARATION

**If you have LIMITED time, focus ONLY on these 15 questions:**
Q1, Q2, Q3, Q4, Q5, Q6, Q7, Q9, Q10, Q11, Q12, Q20, Q28, Q40, Q41

**Master these and you can handle 70% of React interviews!**

---

## 📚 QUICK REFERENCE - KEY DEFINITIONS

**React**: JavaScript library for building user interfaces
**JSX**: Syntax extension that allows writing HTML-like code in JavaScript
**Component**: Independent, reusable piece of UI
**Props**: Properties passed to components from parent
**State**: Internal data that determines component's behavior and rendering
**Hook**: Special function that lets you use React features in functional components
**Virtual DOM**: JavaScript representation of the real DOM for efficient updates
**Event Handling**: How React components respond to user interactions

---

## 🔑 ESSENTIAL CODE PATTERNS TO KNOW

### Basic Functional Component
```jsx
function MyComponent({ name }) {
  return <h1>Hello, {name}!</h1>;
}
```

### useState Hook
```jsx
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

### useEffect Hook
```jsx
function DataFetcher() {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    fetchData().then(setData);
  }, []); // Empty dependency array = run once on mount
  
  return <div>{data ? data.name : 'Loading...'}</div>;
}
```

### Event Handling
```jsx
function Button() {
  const handleClick = (e) => {
    e.preventDefault();
    console.log('Button clicked!');
  };
  
  return <button onClick={handleClick}>Click me</button>;
}
```

---

## 🎲 COMMON REACT PATTERNS TO PRACTICE

### Conditional Rendering
```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
{error && <ErrorMessage />}
```

### List Rendering
```jsx
{items.map(item => (
  <li key={item.id}>{item.name}</li>
))}
```

### Form Handling
```jsx
const [value, setValue] = useState('');
<input 
  value={value} 
  onChange={(e) => setValue(e.target.value)} 
/>
```

---

## 🏗️ COMPONENT LIFECYCLE MAPPING (Class to Hooks)

| Class Component | Hooks Equivalent |
|----------------|------------------|
| componentDidMount | useEffect(() => {}, []) |
| componentDidUpdate | useEffect(() => {}) |
| componentWillUnmount | useEffect(() => { return () => {} }, []) |
| shouldComponentUpdate | React.memo() |
| getDerivedStateFromProps | useState + useEffect |

---

## ⚠️ COMMON INTERVIEW MISTAKES TO AVOID

- Don't mutate state directly - always use setState or state setters
- Remember that useEffect without dependency array runs on every render
- Don't forget keys when rendering lists
- Understand that props are read-only
- Know that useState updates are asynchronous
- Remember that functional components re-run entirely on each render
- Don't confuse controlled vs uncontrolled components
- Understand when to use useCallback vs useMemo
- Know that useEffect cleanup functions prevent memory leaks

---

## 🔥 TRENDING REACT TOPICS (2024-2025)

- **React 18 Features**: Concurrent rendering, automatic batching, Suspense improvements
- **Server Components**: Rendering components on the server
- **React Query/TanStack Query**: Data fetching and caching
- **Zustand/Valtio**: Lightweight state management alternatives
- **React Hook Form**: Efficient form handling
- **Next.js 13+**: App Router, Server Actions
- **TypeScript with React**: Type-safe React development
