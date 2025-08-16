# React Routing - Most Common Interview Questions
*For 2 Years Experience - Quick Interview Prep*

## 1. What is React Router and why do we need it?
**Most Asked:** SPA routing basics
**Quick Answer:** React Router enables navigation between different components in a Single Page Application (SPA) without full page refresh, maintaining browser history and URL synchronization.

**Benefits:**
- Client-side routing
- Browser history management
- URL synchronization
- Nested routing support
- Code splitting integration

## 2. How do you set up basic routing in React?
**Must Know Setup:**
```javascript
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/contact">Contact</Link>
      </nav>
      
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
        <Route path="*" element={<NotFound />} /> {/* 404 route */}
      </Routes>
    </BrowserRouter>
  );
}
```

## 3. What's the difference between BrowserRouter and HashRouter?
**Common Comparison:**
```javascript
// BrowserRouter (Recommended)
// URL: https://example.com/about
// - Clean URLs
// - Requires server configuration
// - Uses HTML5 History API

<BrowserRouter>
  <App />
</BrowserRouter>

// HashRouter
// URL: https://example.com/#/about
// - Hash-based routing
// - Works without server configuration
// - Fallback for older browsers

<HashRouter>
  <App />
</HashRouter>
```

## 4. How do you implement Dynamic Routes with parameters?
**Essential Pattern:**
```javascript
import { useParams, useNavigate } from 'react-router-dom';

function App() {
  return (
    <Routes>
      <Route path="/user/:id" element={<UserProfile />} />
      <Route path="/post/:postId/comments/:commentId" element={<Comment />} />
      <Route path="/category/:category" element={<CategoryPage />} />
    </Routes>
  );
}

// Access route parameters
function UserProfile() {
  const { id } = useParams();
  const navigate = useNavigate();
  
  useEffect(() => {
    fetchUser(id).then(setUser);
  }, [id]);
  
  const goBack = () => navigate(-1);
  const goToEdit = () => navigate(`/user/${id}/edit`);
  
  return (
    <div>
      <h1>User Profile: {id}</h1>
      <button onClick={goBack}>Go Back</button>
      <button onClick={goToEdit}>Edit Profile</button>
    </div>
  );
}
```

## 5. How do you implement Protected Routes?
**Critical Security Pattern:**
```javascript
import { Navigate, useLocation } from 'react-router-dom';

// Protected Route Component
function ProtectedRoute({ children }) {
  const isAuthenticated = useAuth(); // Your auth logic
  const location = useLocation();
  
  if (!isAuthenticated) {
    // Redirect to login with return path
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  return children;
}

// Usage in App
function App() {
  return (
    <Routes>
      <Route path="/login" element={<Login />} />
      <Route path="/public" element={<PublicPage />} />
      
      {/* Protected Routes */}
      <Route 
        path="/dashboard" 
        element={
          <ProtectedRoute>
            <Dashboard />
          </ProtectedRoute>
        } 
      />
      
      <Route 
        path="/profile" 
        element={
          <ProtectedRoute>
            <Profile />
          </ProtectedRoute>
        } 
      />
    </Routes>
  );
}

// Advanced: Role-based protection
function RoleProtectedRoute({ children, allowedRoles }) {
  const { user } = useAuth();
  
  if (!user || !allowedRoles.includes(user.role)) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return children;
}
```

## 6. How do you handle programmatic navigation?
**Navigation Hooks:**
```javascript
import { useNavigate, useLocation } from 'react-router-dom';

function MyComponent() {
  const navigate = useNavigate();
  const location = useLocation();
  
  // Different navigation methods
  const handleNavigation = () => {
    // Navigate to specific route
    navigate('/dashboard');
    
    // Navigate with state
    navigate('/user/123', { state: { from: 'dashboard' } });
    
    // Navigate and replace current entry
    navigate('/login', { replace: true });
    
    // Go back/forward
    navigate(-1); // Go back
    navigate(1);  // Go forward
    
    // Conditional navigation
    if (user.isAdmin) {
      navigate('/admin');
    } else {
      navigate('/user');
    }
  };
  
  // Access current location
  console.log('Current path:', location.pathname);
  console.log('Query params:', location.search);
  console.log('State:', location.state);
  
  return <button onClick={handleNavigation}>Navigate</button>;
}
```

## 7. How do you implement Nested Routes?
**Hierarchical Routing:**
```javascript
import { Outlet } from 'react-router-dom';

function App() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Home />} /> {/* Default child */}
        <Route path="about" element={<About />} />
        
        {/* Nested routes */}
        <Route path="dashboard" element={<Dashboard />}>
          <Route index element={<DashboardHome />} />
          <Route path="analytics" element={<Analytics />} />
          <Route path="settings" element={<Settings />} />
        </Route>
      </Route>
    </Routes>
  );
}

// Layout component with Outlet
function Layout() {
  return (
    <div>
      <header>My App Header</header>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        <Link to="/dashboard">Dashboard</Link>
      </nav>
      <main>
        <Outlet /> {/* Child routes render here */}
      </main>
    </div>
  );
}

// Dashboard with nested navigation
function Dashboard() {
  return (
    <div>
      <h1>Dashboard</h1>
      <nav>
        <Link to="/dashboard">Overview</Link>
        <Link to="/dashboard/analytics">Analytics</Link>
        <Link to="/dashboard/settings">Settings</Link>
      </nav>
      <Outlet /> {/* Nested routes render here */}
    </div>
  );
}
```

## 8. How do you handle Query Parameters and Search?
**URL Search Handling:**
```javascript
import { useSearchParams } from 'react-router-dom';

function ProductList() {
  const [searchParams, setSearchParams] = useSearchParams();
  
  // Read query parameters
  const category = searchParams.get('category') || 'all';
  const page = parseInt(searchParams.get('page')) || 1;
  const sortBy = searchParams.get('sort') || 'name';
  
  // Update query parameters
  const updateFilters = (newFilters) => {
    const params = new URLSearchParams(searchParams);
    
    Object.entries(newFilters).forEach(([key, value]) => {
      if (value) {
        params.set(key, value);
      } else {
        params.delete(key);
      }
    });
    
    setSearchParams(params);
  };
  
  // URL: /products?category=electronics&page=2&sort=price
  
  return (
    <div>
      <div>
        <select 
          value={category} 
          onChange={(e) => updateFilters({ category: e.target.value, page: 1 })}
        >
          <option value="all">All Categories</option>
          <option value="electronics">Electronics</option>
          <option value="clothing">Clothing</option>
        </select>
        
        <select 
          value={sortBy} 
          onChange={(e) => updateFilters({ sort: e.target.value })}
        >
          <option value="name">Sort by Name</option>
          <option value="price">Sort by Price</option>
        </select>
      </div>
      
      <div>
        {/* Product list based on filters */}
        <ProductGrid category={category} page={page} sortBy={sortBy} />
      </div>
      
      <div>
        <button onClick={() => updateFilters({ page: page - 1 })}>Previous</button>
        <span>Page {page}</span>
        <button onClick={() => updateFilters({ page: page + 1 })}>Next</button>
      </div>
    </div>
  );
}
```

## 9. How do you implement Route Guards and Authentication?
**Advanced Protection Patterns:**
```javascript
// Auth Context
const AuthContext = createContext();

function AuthProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  
  useEffect(() => {
    // Check if user is logged in
    checkAuthStatus().then(user => {
      setUser(user);
      setLoading(false);
    });
  }, []);
  
  return (
    <AuthContext.Provider value={{ user, setUser, loading }}>
      {children}
    </AuthContext.Provider>
  );
}

// Route Guard Hook
function useAuth() {
  const context = useContext(AuthContext);
  if (!context) {
    throw new Error('useAuth must be used within AuthProvider');
  }
  return context;
}

// Protected Route with Loading
function ProtectedRoute({ children, requiredRole }) {
  const { user, loading } = useAuth();
  const location = useLocation();
  
  if (loading) {
    return <div>Loading...</div>;
  }
  
  if (!user) {
    return <Navigate to="/login" state={{ from: location }} replace />;
  }
  
  if (requiredRole && user.role !== requiredRole) {
    return <Navigate to="/unauthorized" replace />;
  }
  
  return children;
}

// Usage
function App() {
  return (
    <AuthProvider>
      <BrowserRouter>
        <Routes>
          <Route path="/login" element={<Login />} />
          
          <Route 
            path="/admin" 
            element={
              <ProtectedRoute requiredRole="admin">
                <AdminPanel />
              </ProtectedRoute>
            } 
          />
        </Routes>
      </BrowserRouter>
    </AuthProvider>
  );
}
```

## 10. How do you handle 404 and Error Routes?
**Error Handling:**
```javascript
import { useRouteError } from 'react-router-dom';

function App() {
  return (
    <Routes>
      <Route path="/" element={<Home />} />
      <Route path="/about" element={<About />} />
      
      {/* Catch-all route for 404 */}
      <Route path="*" element={<NotFound />} />
    </Routes>
  );
}

// 404 Component
function NotFound() {
  const navigate = useNavigate();
  
  return (
    <div>
      <h1>404 - Page Not Found</h1>
      <p>The page you're looking for doesn't exist.</p>
      <button onClick={() => navigate('/')}>Go Home</button>
      <button onClick={() => navigate(-1)}>Go Back</button>
    </div>
  );
}

// Error Boundary for Route Errors
function ErrorBoundary() {
  const error = useRouteError();
  
  return (
    <div>
      <h1>Oops! Something went wrong</h1>
      <p>{error.statusText || error.message}</p>
    </div>
  );
}
```

## 11. How do you implement Lazy Loading with Routes?
**Code Splitting with Routes:**
```javascript
import { lazy, Suspense } from 'react';

// Lazy load route components
const Home = lazy(() => import('./pages/Home'));
const Dashboard = lazy(() => import('./pages/Dashboard'));
const Profile = lazy(() => import('./pages/Profile'));

function App() {
  return (
    <BrowserRouter>
      <Suspense fallback={<div>Loading page...</div>}>
        <Routes>
          <Route path="/" element={<Home />} />
          
          <Route 
            path="/dashboard" 
            element={
              <ProtectedRoute>
                <Dashboard />
              </ProtectedRoute>
            } 
          />
          
          <Route 
            path="/profile" 
            element={
              <ProtectedRoute>
                <Profile />
              </ProtectedRoute>
            } 
          />
        </Routes>
      </Suspense>
    </BrowserRouter>
  );
}
```

## 12. How do you test React Router components?
**Testing Patterns:**
```javascript
import { render, screen } from '@testing-library/react';
import { BrowserRouter } from 'react-router-dom';
import { MemoryRouter } from 'react-router-dom';

// Test with MemoryRouter
function renderWithRouter(component, initialEntries = ['/']) {
  return render(
    <MemoryRouter initialEntries={initialEntries}>
      {component}
    </MemoryRouter>
  );
}

// Test protected routes
test('redirects to login when not authenticated', () => {
  renderWithRouter(<ProtectedRoute><Dashboard /></ProtectedRoute>);
  expect(screen.getByText(/login/i)).toBeInTheDocument();
});

// Test route parameters
test('displays user profile with correct ID', () => {
  renderWithRouter(<UserProfile />, ['/user/123']);
  expect(screen.getByText(/User Profile: 123/)).toBeInTheDocument();
});
```

## Quick Router Hooks Reference:
```javascript
// Navigation
const navigate = useNavigate();

// Route parameters
const { id, category } = useParams();

// Current location
const location = useLocation();

// Query parameters
const [searchParams, setSearchParams] = useSearchParams();

// Match current route
const match = useMatch('/user/:id');
```

## Most Important Points for Interview:
1. **BrowserRouter vs HashRouter** - Know when to use each
2. **Dynamic routes with useParams** - Essential for data fetching
3. **Protected Routes pattern** - Critical for authentication
4. **Programmatic navigation** - useNavigate hook usage
5. **Nested routing with Outlet** - Component composition

## Common Patterns to Master:
```javascript
// Protected route wrapper
<ProtectedRoute><Dashboard /></ProtectedRoute>

// Dynamic route with params
<Route path="/user/:id" element={<UserProfile />} />

// Nested routes structure
<Route path="dashboard" element={<Dashboard />}>
  <Route path="settings" element={<Settings />} />
</Route>
```

## Interview Tips:
- ✅ Always mention protected routes for authentication
- ✅ Show you understand dynamic routing with parameters
- ✅ Know the difference between navigation methods
- ✅ Understand nested routing and Outlet concept
- ✅ Practice query parameter handling patterns
