# Singleton Design Pattern - Interview Questions (2 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is Singleton pattern and why use it?
**Answer:** Singleton ensures only one instance of a class exists throughout application lifetime. Benefits: controlled object creation, global access point, memory efficiency, shared resources management.

### Q2: How many ways can you implement Singleton?
**Answer:** 5 main approaches:
| Method | Thread Safety | Performance | Lazy Loading |
|--------|---------------|-------------|--------------|
| Eager Initialization | ✅ | Fast | ❌ |
| Lazy Initialization | ❌ | Fast | ✅ |
| Synchronized Method | ✅ | Slow | ✅ |
| Double-Checked Locking | ✅ | Fast | ✅ |
| Enum Singleton | ✅ | Fast | ❌ |

### Q3: What is Eager Initialization Singleton?
**Answer:** Instance created at class loading time, before any request.
```java
class EagerSingleton {
    // Instance created immediately when class loads
    private static final EagerSingleton INSTANCE = new EagerSingleton();
    
    private EagerSingleton() {} // Private constructor
    
    public static EagerSingleton getInstance() {
        return INSTANCE; // Simply return pre-created instance
    }
}
```
**Pros:** Simple, thread-safe by JVM  
**Cons:** Memory waste if never used

### Q4: What is Lazy Initialization Singleton?
**Answer:** Instance created only when first requested (on-demand).
```java
class LazySingleton {
    private static LazySingleton instance;
    
    private LazySingleton() {}
    
    public static LazySingleton getInstance() {
        if (instance == null) {
            instance = new LazySingleton(); // Create only when needed
        }
        return instance;
    }
}
```
**Pros:** Memory efficient, lazy loading  
**Cons:** Not thread-safe, race condition possible

### Q5: Why is Lazy Singleton not thread-safe?
**Answer:** Multiple threads can enter if condition simultaneously and create multiple instances.
```java
// Thread 1: checks (instance == null) → true
// Thread 2: checks (instance == null) → true (before Thread 1 creates)
// Both threads create separate instances → Singleton violated!

// Race condition scenario:
Thread t1 = new Thread(() -> {
    LazySingleton obj1 = LazySingleton.getInstance();
});
Thread t2 = new Thread(() -> {
    LazySingleton obj2 = LazySingleton.getInstance();
});
// obj1 and obj2 might be different objects!
```

### Q6: How to make Singleton thread-safe?
**Answer:** Synchronize the getInstance() method to allow only one thread at a time.
```java
class ThreadSafeSingleton {
    private static ThreadSafeSingleton instance;
    
    private ThreadSafeSingleton() {}
    
    public static synchronized ThreadSafeSingleton getInstance() {
        if (instance == null) {
            instance = new ThreadSafeSingleton();
        }
        return instance; // Only one thread can execute this method
    }
}
```
**Pros:** Thread-safe, lazy loading  
**Cons:** Performance hit due to synchronization overhead

### Q7: What is Double-Checked Locking?
**Answer:** Optimized approach that checks twice to minimize synchronization overhead.
```java
class DoubleCheckedSingleton {
    private static volatile DoubleCheckedSingleton instance;
    
    private DoubleCheckedSingleton() {}
    
    public static DoubleCheckedSingleton getInstance() {
        if (instance == null) {          // First check (no sync)
            synchronized (DoubleCheckedSingleton.class) {
                if (instance == null) {  // Second check (synchronized)
                    instance = new DoubleCheckedSingleton();
                }
            }
        }
        return instance;
    }
}
```
**Key Points:** `volatile` prevents instruction reordering, double-check minimizes sync overhead

### Q8: Why volatile keyword in Double-Checked Locking?
**Answer:** Prevents instruction reordering that could cause incomplete object initialization.
```java
// Without volatile, JVM might reorder:
// 1. Allocate memory
// 2. Set instance reference (before initialization!)
// 3. Initialize object

// Another thread might see non-null reference to uninitialized object
// volatile ensures proper happens-before relationship
```

### Q9: What is Enum Singleton?
**Answer:** Using enum to implement Singleton - considered best practice.
```java
enum EnumSingleton {
    INSTANCE;
    
    public void doSomething() {
        System.out.println("Doing work...");
    }
}

// Usage:
EnumSingleton singleton = EnumSingleton.INSTANCE;
singleton.doSomething();
```
**Pros:** Thread-safe, serialization-safe, reflection-proof, simple  
**Cons:** Not lazy, slight memory overhead

### Q10: What is Bill Pugh Singleton (Initialization-on-demand)?
**Answer:** Uses static inner class for lazy loading with thread safety.
```java
class BillPughSingleton {
    private BillPughSingleton() {}
    
    // Static inner class - loaded only when accessed
    private static class SingletonHelper {
        private static final BillPughSingleton INSTANCE = new BillPughSingleton();
    }
    
    public static BillPughSingleton getInstance() {
        return SingletonHelper.INSTANCE; // Thread-safe by JVM class loading
    }
}
```
**Best of all worlds:** Thread-safe, lazy loading, high performance, no synchronization

---

## 🔥 Tricky Interview Questions (High Probability)

### Q11: Can you break Singleton pattern? How?
**Answer:** YES, Singleton can be broken in 3 ways:

**1. Reflection Attack:**
```java
// Breaking through reflection
Constructor<Singleton> constructor = Singleton.class.getDeclaredConstructor();
constructor.setAccessible(true);
Singleton instance1 = constructor.newInstance();
Singleton instance2 = constructor.newInstance();
// Two different instances created!

// Protection: Throw exception in constructor
private Singleton() {
    if (instance != null) {
        throw new RuntimeException("Use getInstance() method");
    }
}
```

**2. Serialization Attack:**
```java
// Breaking through serialization
Singleton instance1 = Singleton.getInstance();
// Serialize
ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("singleton.ser"));
out.writeObject(instance1);

// Deserialize
ObjectInputStream in = new ObjectInputStream(new FileInputStream("singleton.ser"));
Singleton instance2 = (Singleton) in.readObject();
// Different instance created!

// Protection: Implement readResolve()
private Object readResolve() {
    return getInstance();
}
```

**3. Cloning Attack:**
```java
// Breaking through cloning
Singleton instance1 = Singleton.getInstance();
Singleton instance2 = (Singleton) instance1.clone();
// Different instance if clone() is implemented

// Protection: Override clone() to throw exception
@Override
protected Object clone() throws CloneNotSupportedException {
    throw new CloneNotSupportedException("Cloning not allowed");
}
```

### Q12: Singleton vs Static Class - When to use what?
**Answer:**
| Singleton | Static Class |
|-----------|--------------|
| Can implement interfaces | Cannot implement interfaces |
| Can be subclassed | Cannot be subclassed |
| Can be passed as parameter | Cannot be passed around |
| Lazy initialization possible | All methods loaded at class loading |
| Can maintain state | Stateless utility methods |
| Object-oriented approach | Procedural approach |

```java
// Use Singleton for:
class DatabaseConnection {
    private Connection conn;
    // Can implement Closeable, can be passed to methods
}

// Use Static for:
class MathUtils {
    public static int add(int a, int b) { return a + b; }
    // Pure utility functions, no state needed
}
```

### Q13: What's the output?
```java
class TestSingleton {
    private static TestSingleton instance;
    private static int counter = 0;
    
    private TestSingleton() {
        counter++;
        System.out.println("Constructor called. Counter: " + counter);
    }
    
    public static TestSingleton getInstance() {
        if (instance == null) {
            instance = new TestSingleton();
        }
        return instance;
    }
}

public class Test {
    public static void main(String[] args) {
        TestSingleton s1 = TestSingleton.getInstance();
        TestSingleton s2 = TestSingleton.getInstance();
        TestSingleton s3 = TestSingleton.getInstance();
        
        System.out.println("s1 == s2: " + (s1 == s2));
        System.out.println("s2 == s3: " + (s2 == s3));
    }
}
```
**Answer:**
```
Constructor called. Counter: 1
s1 == s2: true
s2 == s3: true
```
**Explanation:** Constructor called only once, all references point to same instance.

### Q14: Memory allocation of Singleton - Where is it stored?
**Answer:** 
- **Instance:** Stored in **Heap memory**
- **Static reference:** Stored in **Method Area** (Metaspace)
- **Class metadata:** Stored in **Method Area**

```java
// Memory layout:
Method Area: [static instance reference] → points to Heap
Heap: [Actual Singleton object with instance variables]
```

### Q15: Singleton with generic types?
**Answer:** Generic Singleton for type-safe implementation.
```java
class GenericSingleton<T> {
    private static volatile Object instance;
    
    @SuppressWarnings("unchecked")
    public static <T> GenericSingleton<T> getInstance() {
        if (instance == null) {
            synchronized (GenericSingleton.class) {
                if (instance == null) {
                    instance = new GenericSingleton<T>();
                }
            }
        }
        return (GenericSingleton<T>) instance;
    }
    
    private T data;
    
    public void setData(T data) { this.data = data; }
    public T getData() { return data; }
}
```

---

## 💡 Real-World Applications

### Q16: When to use Singleton in real projects?
**Answer:** Common use cases in enterprise applications:

**1. Database Connection Pool:**
```java
class ConnectionPool {
    private static volatile ConnectionPool instance;
    private List<Connection> connections;
    
    public static ConnectionPool getInstance() {
        if (instance == null) {
            synchronized (ConnectionPool.class) {
                if (instance == null) {
                    instance = new ConnectionPool();
                }
            }
        }
        return instance;
    }
    
    public Connection getConnection() {
        // Return available connection from pool
    }
}
```

**2. Configuration Manager:**
```java
class ConfigManager {
    private static final ConfigManager INSTANCE = new ConfigManager();
    private Properties config;
    
    private ConfigManager() {
        config = loadConfigFile();
    }
    
    public static ConfigManager getInstance() {
        return INSTANCE;
    }
    
    public String getProperty(String key) {
        return config.getProperty(key);
    }
}
```

**3. Logger Service:**
```java
class Logger {
    private static volatile Logger instance;
    private FileWriter writer;
    
    public static Logger getInstance() {
        if (instance == null) {
            synchronized (Logger.class) {
                if (instance == null) {
                    instance = new Logger();
                }
            }
        }
        return instance;
    }
    
    public synchronized void log(String message) {
        // Write to log file
    }
}
```

### Q17: Singleton in multithreaded environment - Best practices?
**Answer:**
1. **Use Enum or Bill Pugh** for new code
2. **Avoid lazy initialization** unless memory is critical
3. **Use volatile** with double-checked locking
4. **Consider ThreadLocal** for per-thread singletons
5. **Implement readResolve()** if serializable

```java
// Thread-safe lazy Singleton with all protections
class ProductionSingleton implements Serializable {
    private static volatile ProductionSingleton instance;
    
    private ProductionSingleton() {
        // Reflection protection
        if (instance != null) {
            throw new RuntimeException("Use getInstance()");
        }
    }
    
    public static ProductionSingleton getInstance() {
        if (instance == null) {
            synchronized (ProductionSingleton.class) {
                if (instance == null) {
                    instance = new ProductionSingleton();
                }
            }
        }
        return instance;
    }
    
    // Serialization protection
    private Object readResolve() {
        return getInstance();
    }
    
    // Cloning protection
    @Override
    protected Object clone() throws CloneNotSupportedException {
        throw new CloneNotSupportedException();
    }
}
```

---

## 🚀 Anti-Patterns and Common Mistakes

### Q18: What are common Singleton anti-patterns?
**Answer:**

**1. God Object Anti-pattern:**
```java
// ❌ DON'T DO THIS - Singleton doing everything
class GodSingleton {
    public void handleDatabase() { /* ... */ }
    public void manageFiles() { /* ... */ }
    public void sendEmails() { /* ... */ }
    public void processPayments() { /* ... */ }
    // Violates Single Responsibility Principle
}

// ✅ DO THIS - Separate responsibilities
class DatabaseManager { /* ... */ }
class FileManager { /* ... */ }
class EmailService { /* ... */ }
```

**2. Hidden Dependencies:**
```java
// ❌ Hard to test and maintain
class OrderService {
    public void processOrder(Order order) {
        Logger.getInstance().log("Processing order"); // Hidden dependency
        DatabaseManager.getInstance().save(order);    // Hard to mock
    }
}

// ✅ Dependency Injection approach
class OrderService {
    private Logger logger;
    private DatabaseManager dbManager;
    
    public OrderService(Logger logger, DatabaseManager dbManager) {
        this.logger = logger;
        this.dbManager = dbManager;
    }
}
```

**3. Global State Management:**
```java
// ❌ Shared mutable state - threading issues
class GlobalState {
    private Map<String, Object> state = new HashMap<>();
    
    public void setState(String key, Object value) {
        state.put(key, value); // Race condition possible
    }
}

// ✅ Immutable or thread-safe approach
class SafeGlobalState {
    private final ConcurrentHashMap<String, Object> state = new ConcurrentHashMap<>();
    
    public void setState(String key, Object value) {
        state.put(key, value); // Thread-safe
    }
}
```

### Q19: How to test Singleton classes?
**Answer:** Testing challenges and solutions:

**Problem:** Hard to test due to global state and hidden dependencies.

**Solutions:**
```java
// 1. Reset method for testing
class TestableSingleton {
    private static TestableSingleton instance;
    
    public static TestableSingleton getInstance() { /* ... */ }
    
    // For testing only
    public static void resetInstance() {
        instance = null;
    }
}

// 2. Interface-based Singleton
interface DatabaseService {
    void save(Object data);
}

class DatabaseSingleton implements DatabaseService {
    // Singleton implementation
}

// 3. Dependency Injection
class OrderService {
    private DatabaseService dbService;
    
    // Constructor injection for testing
    public OrderService(DatabaseService dbService) {
        this.dbService = dbService;
    }
    
    // Default constructor uses Singleton
    public OrderService() {
        this.dbService = DatabaseSingleton.getInstance();
    }
}
```

### Q20: Singleton vs Dependency Injection?
**Answer:**
| Singleton | Dependency Injection |
|-----------|---------------------|
| Hard-coded dependencies | Flexible dependencies |
| Difficult to test | Easy to test and mock |
| Tight coupling | Loose coupling |
| Global access | Controlled access |
| Single implementation | Multiple implementations |

**Modern Approach:** Use DI frameworks (Spring) instead of manual Singletons:
```java
// Spring-managed Singleton
@Component
@Scope("singleton") // Default scope
class OrderService {
    // Spring manages instance lifecycle
}

// Better testability and maintainability
```

---

## 🚀 Last Minute Quick Review

### One-Liner Answers:
1. **What is Singleton?** → Ensures only one instance exists globally
2. **Thread-safe approaches?** → Synchronized method, Double-checked locking, Enum, Bill Pugh
3. **Why volatile in DCL?** → Prevents instruction reordering, ensures proper initialization
4. **Best Singleton approach?** → Enum or Bill Pugh (static inner class)
5. **Breaking Singleton?** → Reflection, Serialization, Cloning attacks
6. **Singleton vs Static?** → Singleton for stateful objects, Static for utilities
7. **Real-world usage?** → DB connections, Config managers, Loggers, Caches
8. **Testing challenges?** → Global state, hidden dependencies, hard to mock
9. **Memory location?** → Instance in Heap, reference in Method Area
10. **Anti-patterns?** → God object, Hidden dependencies, Global mutable state

### Memory Tips:
- **Enum = Best practice** (thread-safe, serialization-safe, reflection-proof)
- **Bill Pugh = Performance king** (lazy + thread-safe + no synchronization)
- **DCL needs volatile** (prevent instruction reordering)
- **Eager = Simple but wasteful** (immediate loading)

### Code Templates:
```java
// Production-ready Singleton
class ProductionSingleton {
    private ProductionSingleton() {}
    
    private static class Holder {
        private static final ProductionSingleton INSTANCE = new ProductionSingleton();
    }
    
    public static ProductionSingleton getInstance() {
        return Holder.INSTANCE;
    }
}

// Enum Singleton (Recommended)
enum ConfigManager {
    INSTANCE;
    
    public String getConfig(String key) {
        // Implementation
    }
}
```

### Common Interview Traps:
- **"Why not just use static methods?"** → Need object behavior, interfaces, inheritance
- **"How to handle inheritance?"** → Singleton + inheritance = complex, usually avoid
- **"Performance of synchronized method?"** → Every call synchronized, DCL better
- **"Singleton in distributed systems?"** → Each JVM has separate instance, need coordination

---

## 🎯 Final Tips

### During Interview:
- **Start with simple Eager implementation** then discuss advanced approaches
- **Always mention thread-safety concerns** and solutions
- **Discuss trade-offs** of each approach (memory vs performance vs complexity)
- **Give real-world examples** from your experience
- **Show awareness of modern alternatives** (DI frameworks)

### Red Flags to Avoid:
- Saying "Singleton is always thread-safe" (Lazy isn't!)
- Forgetting to make constructor private
- Not knowing about reflection/serialization attacks
- Overusing Singleton pattern (anti-pattern)

### Pro Tips:
- **Prefer Enum or Bill Pugh** for new implementations
- **Consider alternatives** like Dependency Injection
- **Be aware of testing challenges** and solutions
- **Understand memory implications** and JVM behavior

---

*🎯 Perfect for 2-year experience Singleton pattern interviews!*
