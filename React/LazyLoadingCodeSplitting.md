# React Lazy Loading & Code Splitting - Most Common Interview Questions
*For 2 Years Experience - Quick Interview Prep*

## 1. What is Code Splitting and why is it important?
**Most Asked:** Performance optimization concept
**Quick Answer:** Breaking your app into smaller chunks that load on-demand, reducing initial bundle size and improving performance.

**Benefits:**
- Faster initial load time
- Better user experience
- Reduced bandwidth usage
- Load only what's needed

## 2. What is React.lazy() and how do you use it?
**Must Know Pattern:**
```javascript
// Before - Normal import (loads immediately)
import Dashboard from './Dashboard';

// After - Lazy import (loads when needed)
const Dashboard = React.lazy(() => import('./Dashboard'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading Dashboard...</div>}>
        <Dashboard />
      </Suspense>
    </div>
  );
}
```

## 3. What is React Suspense and how does it work with lazy loading?
**Critical Understanding:**
```javascript
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <h1>My App</h1>
      <Suspense fallback={<LoadingSpinner />}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}

// Custom loading component
function LoadingSpinner() {
  return (
    <div className="spinner">
      <div>Loading...</div>
    </div>
  );
}
```

## 4. How do you implement route-based code splitting?
**Most Common Use Case:**
```javascript
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { Suspense, lazy } from 'react';

// Lazy load route components
const Home = lazy(() => import('./pages/Home'));
const About = lazy(() => import('./pages/About'));
const Dashboard = lazy(() => import('./pages/Dashboard'));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<div>Loading page...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/about" element={<About />} />
          <Route path="/dashboard" element={<Dashboard />} />
        </Routes>
      </Suspense>
    </div>
  );
}
```

## 5. What is dynamic import() and how is it different from static import?
**Fundamental Difference:**
```javascript
// Static import - loaded at build time
import Dashboard from './Dashboard';

// Dynamic import - loaded at runtime
const Dashboard = React.lazy(() => import('./Dashboard'));

// Dynamic import with conditions
const loadComponent = async (componentName) => {
  if (componentName === 'dashboard') {
    const { default: Dashboard } = await import('./Dashboard');
    return Dashboard;
  }
};
```

## 6. How do you handle loading states and errors in lazy loading?
**Error Handling Pattern:**
```javascript
import { Suspense, lazy } from 'react';
import ErrorBoundary from './ErrorBoundary';

const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
  return (
    <ErrorBoundary>
      <Suspense 
        fallback={
          <div className="loading">
            <div>Loading component...</div>
            <div className="spinner"></div>
          </div>
        }
      >
        <LazyComponent />
      </Suspense>
    </ErrorBoundary>
  );
}

// Error Boundary for failed lazy loads
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  render() {
    if (this.state.hasError) {
      return <div>Something went wrong loading the component.</div>;
    }
    return this.props.children;
  }
}
```

## 7. What is component-based code splitting?
**Conditional Loading:**
```javascript
import { useState, Suspense, lazy } from 'react';

const HeavyChart = lazy(() => import('./HeavyChart'));
const HeavyTable = lazy(() => import('./HeavyTable'));

function Dashboard() {
  const [activeTab, setActiveTab] = useState('overview');

  return (
    <div>
      <nav>
        <button onClick={() => setActiveTab('overview')}>Overview</button>
        <button onClick={() => setActiveTab('charts')}>Charts</button>
        <button onClick={() => setActiveTab('data')}>Data</button>
      </nav>

      <div className="content">
        {activeTab === 'overview' && <div>Overview content</div>}
        
        {activeTab === 'charts' && (
          <Suspense fallback={<div>Loading charts...</div>}>
            <HeavyChart />
          </Suspense>
        )}
        
        {activeTab === 'data' && (
          <Suspense fallback={<div>Loading table...</div>}>
            <HeavyTable />
          </Suspense>
        )}
      </div>
    </div>
  );
}
```

## 8. How do you preload components for better UX?
**Optimization Technique:**
```javascript
// Preload on hover
const Dashboard = lazy(() => import('./Dashboard'));

function Navigation() {
  const preloadDashboard = () => {
    // Triggers the import but doesn't render
    import('./Dashboard');
  };

  return (
    <nav>
      <Link 
        to="/dashboard"
        onMouseEnter={preloadDashboard} // Preload on hover
      >
        Dashboard
      </Link>
    </nav>
  );
}

// Preload after initial load
useEffect(() => {
  // Preload important components after main app loads
  setTimeout(() => {
    import('./Dashboard');
    import('./Profile');
  }, 2000);
}, []);
```

## 9. What are webpack chunks and how do they relate to code splitting?
**Build Process Understanding:**
```javascript
// Webpack automatically creates chunks for dynamic imports

// This creates a separate chunk file
const Dashboard = lazy(() => import('./Dashboard'));

// Named chunks (optional)
const Dashboard = lazy(() => 
  import(/* webpackChunkName: "dashboard" */ './Dashboard')
);

// Chunk files generated:
// main.js - Main application bundle
// dashboard.chunk.js - Dashboard component chunk
// vendor.chunk.js - Third-party libraries
```

## 10. How do you implement lazy loading for large lists?
**Virtual Scrolling Concept:**
```javascript
import { useState, useEffect } from 'react';

function LazyList({ items }) {
  const [visibleItems, setVisibleItems] = useState([]);
  const [loading, setLoading] = useState(false);

  const loadMore = async () => {
    setLoading(true);
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    const nextItems = items.slice(visibleItems.length, visibleItems.length + 20);
    setVisibleItems(prev => [...prev, ...nextItems]);
    setLoading(false);
  };

  useEffect(() => {
    loadMore(); // Initial load
  }, []);

  return (
    <div>
      {visibleItems.map(item => (
        <div key={item.id}>{item.name}</div>
      ))}
      
      {loading && <div>Loading more...</div>}
      
      <button onClick={loadMore} disabled={loading}>
        Load More
      </button>
    </div>
  );
}
```

## 11. What are the best practices for code splitting?
**Optimization Guidelines:**
```javascript
// ✅ Good - Route level splitting
const Home = lazy(() => import('./pages/Home'));
const Dashboard = lazy(() => import('./pages/Dashboard'));

// ✅ Good - Feature level splitting
const ChatWidget = lazy(() => import('./features/Chat'));

// ❌ Avoid - Too granular splitting
const Button = lazy(() => import('./Button')); // Too small

// ✅ Good - Bundle related components
const AdminPanel = lazy(() => import('./admin/AdminPanel'));

// ✅ Good - Third-party library splitting
const ChartComponent = lazy(() => 
  import('./charts/ChartComponent') // Includes chart.js
);
```

## 12. How do you measure the impact of code splitting?
**Performance Monitoring:**
```javascript
// Bundle analyzer (webpack-bundle-analyzer)
// Shows chunk sizes and dependencies

// Performance API
const observer = new PerformanceObserver((list) => {
  for (const entry of list.getEntries()) {
    if (entry.entryType === 'navigation') {
      console.log('Page load time:', entry.loadEventEnd - entry.fetchStart);
    }
  }
});

observer.observe({ entryTypes: ['navigation'] });

// Lazy loading analytics
const trackLazyLoad = (componentName) => {
  const startTime = performance.now();
  
  return import(`./components/${componentName}`)
    .then(component => {
      const loadTime = performance.now() - startTime;
      console.log(`${componentName} loaded in ${loadTime}ms`);
      return component;
    });
};
```

## Quick Implementation Checklist:
```javascript
// 1. Basic lazy loading setup
const Component = lazy(() => import('./Component'));

// 2. Wrap with Suspense
<Suspense fallback={<Loading />}>
  <Component />
</Suspense>

// 3. Add Error Boundary
<ErrorBoundary>
  <Suspense fallback={<Loading />}>
    <Component />
  </Suspense>
</ErrorBoundary>

// 4. Route-based splitting
const routes = [
  { path: '/', component: lazy(() => import('./Home')) },
  { path: '/about', component: lazy(() => import('./About')) }
];
```

## Most Important Points for Interview:
1. **Code splitting reduces initial bundle size** - Faster first load
2. **React.lazy + Suspense** - Standard pattern for lazy loading
3. **Route-based splitting** - Most common and effective approach
4. **Error boundaries** - Handle failed lazy loads gracefully
5. **Preloading strategies** - Improve perceived performance

## Common Patterns to Master:
- Route-based code splitting with React Router
- Component-based splitting for heavy features
- Error handling with Error Boundaries
- Loading states with Suspense fallback
- Preloading on user interaction

## Interview Tips:
- ✅ Always mention performance benefits
- ✅ Show you understand Suspense + Error Boundary pattern
- ✅ Know when NOT to split (small components)
- ✅ Understand webpack chunking concepts
- ✅ Practice route-based splitting example
