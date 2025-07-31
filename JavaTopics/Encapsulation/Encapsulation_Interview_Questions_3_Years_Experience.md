# Encapsulation - Interview Questions (3 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is Encapsulation and why is it important?

**Answer:** Encapsulation is the bundling of data (variables) and methods that operate on that data into a single unit (class), while hiding internal implementation details from outside world.

**Benefits:**
- **Data Protection**: Prevents unauthorized access and modification
- **Maintainability**: Internal implementation can change without affecting clients
- **Validation**: Control how data is accessed and modified
- **Modularity**: Self-contained units of functionality
- **Security**: Sensitive data remains hidden

### Q2: How is Encapsulation achieved in Java?

**Answer:** Encapsulation is achieved through:

1. **Private variables** - Hide data from outside access
2. **Public getter/setter methods** - Controlled access to data
3. **Access modifiers** - Control visibility levels
4. **Method overloading** - Multiple ways to access data

```java
public class Student {
    private String name;        // Encapsulated data
    private int age;           // Hidden from outside
    
    // Controlled access through methods
    public String getName() { return name; }
    public void setName(String name) {
        if (name != null && !name.trim().isEmpty()) {
            this.name = name;   // Validation before setting
        }
    }
    
    public int getAge() { return age; }
    public void setAge(int age) {
        if (age > 0 && age < 120) {
            this.age = age;     // Business rule validation
        }
    }
}
```

### Q3: What are Access Modifiers and their scope?

**Answer:**

| Access Modifier | Same Class | Same Package | Subclass | Different Package |
|----------------|------------|--------------|----------|-------------------|
| private | ✅ | ❌ | ❌ | ❌ |
| default (package) | ✅ | ✅ | ❌ | ❌ |
| protected | ✅ | ✅ | ✅ | ❌ |
| public | ✅ | ✅ | ✅ | ✅ |

```java
public class AccessDemo {
    private int privateVar;      // Only within same class
    int packageVar;              // Same package only
    protected int protectedVar;  // Subclasses + same package
    public int publicVar;        // Everywhere
}
```

### Q4: Difference between Encapsulation and Data Hiding?

**Answer:**

| Encapsulation | Data Hiding |
|---------------|-------------|
| Broader concept - bundling data + methods | Specific aspect of encapsulation |
| Includes both data and behavior | Focuses only on hiding data |
| Achieved via classes and access modifiers | Achieved via private access modifier |
| Promotes modularity and maintainability | Promotes security and integrity |

**Example:**
```java
// Encapsulation - bundling data + methods
class BankAccount {
    private double balance;              // Data hiding
    
    public void deposit(double amount) { // Method bundled with data
        if (amount > 0) balance += amount;
    }
    
    public double getBalance() { return balance; } // Controlled access
}
```

### Q5: What are Getter and Setter methods?

**Answer:** Getter and Setter methods provide controlled access to private variables.

**Naming Conventions:**
- Getter: `getPropertyName()` or `isPropertyName()` for boolean
- Setter: `setPropertyName(Type value)`

```java
public class Person {
    private String name;
    private boolean active;
    
    // Getter methods
    public String getName() { return name; }
    public boolean isActive() { return active; } // boolean getter
    
    // Setter methods
    public void setName(String name) {
        this.name = (name != null) ? name.trim() : "";
    }
    
    public void setActive(boolean active) {
        this.active = active;
    }
}
```

### Q6: Can we have validation in getters and setters?

**Answer:** YES. Setters commonly include validation, getters can include formatting or computed values.

```java
public class Employee {
    private String email;
    private double salary;
    private Date birthDate;
    
    public void setEmail(String email) {
        if (email != null && email.contains("@")) {
            this.email = email.toLowerCase();
        } else {
            throw new IllegalArgumentException("Invalid email");
        }
    }
    
    public void setSalary(double salary) {
        if (salary >= 0) {
            this.salary = salary;
        } else {
            throw new IllegalArgumentException("Salary cannot be negative");
        }
    }
    
    // Computed property in getter
    public int getAge() {
        return Period.between(birthDate.toLocalDate(), LocalDate.now()).getYears();
    }
}
```

### Q7: What is the difference between public variables and getters/setters?

**Answer:**

| Public Variables | Getters/Setters |
|------------------|-----------------|
| Direct access | Controlled access |
| No validation possible | Validation can be added |
| Cannot track access | Can log access |
| Breaking changes if modified | Internal changes don't affect clients |
| No computed properties | Can return computed values |

```java
// ❌ Public variable approach
class BadDesign {
    public String name; // Direct access, no control
}

// ✅ Encapsulated approach
class GoodDesign {
    private String name;
    
    public String getName() { 
        logAccess("name getter called");
        return name; 
    }
    
    public void setName(String name) {
        validateName(name);
        this.name = name;
        notifyPropertyChanged("name");
    }
}
```

### Q8: Can constructors break encapsulation?

**Answer:** YES, if not designed carefully. Constructors should validate input and maintain object invariants.

```java
public class Rectangle {
    private double width;
    private double height;
    
    // Good constructor - validates input
    public Rectangle(double width, double height) {
        if (width <= 0 || height <= 0) {
            throw new IllegalArgumentException("Dimensions must be positive");
        }
        this.width = width;
        this.height = height;
    }
    
    // Avoid direct field assignment in constructor
    // this.width = width; // Bad - no validation
}
```

### Q9: What is Immutable class and how does it relate to encapsulation?

**Answer:** Immutable class is one whose objects cannot be modified after creation. It's the strongest form of encapsulation.

```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final List<String> hobbies;
    
    public ImmutablePerson(String name, int age, List<String> hobbies) {
        this.name = name;
        this.age = age;
        // Defensive copying for mutable objects
        this.hobbies = new ArrayList<>(hobbies);
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
    
    // Return defensive copy
    public List<String> getHobbies() {
        return new ArrayList<>(hobbies);
    }
    
    // No setter methods - object is immutable
}
```

### Q10: How does encapsulation support inheritance?

**Answer:** Encapsulation works with inheritance through protected access and method overriding.

```java
public class Vehicle {
    private String brand;           // Private - only Vehicle class
    protected int maxSpeed;         // Protected - subclasses can access
    
    protected void startEngine() { // Protected method for subclasses
        System.out.println("Starting " + brand + " engine");
    }
    
    public final String getBrand() { return brand; } // Public interface
}

public class Car extends Vehicle {
    public void accelerate() {
        startEngine();              // Can access protected method
        // brand = "Toyota";        // Cannot access private field
        maxSpeed = 200;             // Can access protected field
    }
}
```

---

## 🔥 Tricky Questions (High Probability)

### Q11: Can private methods be overridden?

```java
class Parent {
    private void privateMethod() {
        System.out.println("Parent private");
    }
    
    public void callPrivate() {
        privateMethod();
    }
}

class Child extends Parent {
    private void privateMethod() { // Override?
        System.out.println("Child private");
    }
}

new Child().callPrivate(); // Output?
```

**Answer:** "Parent private" - Private methods cannot be overridden, only hidden. Child's method is a new method, not an override.

### Q12: Encapsulation with static variables?

```java
public class Counter {
    private static int count = 0;
    
    public static int getCount() { return count; }
    public static void increment() { count++; }
}

// Multiple threads accessing Counter.increment()
// Is this properly encapsulated?
```

**Answer:** NOT thread-safe. Static variables are shared among all instances and threads. Need synchronization for proper encapsulation in multi-threaded environment.

### Q13: Breaking encapsulation with mutable objects?

```java
public class Department {
    private List<Employee> employees = new ArrayList<>();
    
    public List<Employee> getEmployees() {
        return employees; // Problem?
    }
}

// Client code
Department dept = new Department();
List<Employee> empList = dept.getEmployees();
empList.clear(); // What happens to department's employees?
```

**Answer:** Encapsulation BROKEN. Client can modify internal list. Should return defensive copy:

```java
public List<Employee> getEmployees() {
    return new ArrayList<>(employees); // Defensive copy
}
```

### Q14: Package-private constructors?

```java
public class DatabaseConnection {
    private static DatabaseConnection instance;
    
    // Package-private constructor
    DatabaseConnection() { }
    
    public static DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }
}
```

**Answer:** Package-private constructor allows classes in same package to create instances, breaking singleton pattern. Should be private for proper encapsulation.

### Q15: Encapsulation with inheritance and method overriding?

```java
class Parent {
    private int value = 10;
    
    protected int getValue() { return value; }
    protected void setValue(int value) { this.value = value; }
}

class Child extends Parent {
    protected void setValue(int value) {
        if (value > 0) {
            super.setValue(value);
        }
    }
}

Child child = new Child();
child.setValue(-5); // What happens?
```

**Answer:** Child's validation prevents setting negative value. Encapsulation allows subclass to add stricter validation while maintaining parent's interface.

### Q16: Reflection breaking encapsulation?

```java
public class SecureClass {
    private String secret = "top-secret";
    
    private void secretMethod() {
        System.out.println("Secret operation");
    }
}

// Using reflection
SecureClass obj = new SecureClass();
Field field = SecureClass.class.getDeclaredField("secret");
field.setAccessible(true); // Breaks encapsulation
String value = (String) field.get(obj);
```

**Answer:** Reflection CAN break encapsulation by accessing private members. However, it requires explicit permission and is detectable by security managers.

### Q17: Inner classes and encapsulation?

```java
public class Outer {
    private String outerField = "outer";
    
    class InnerClass {
        void accessOuter() {
            System.out.println(outerField); // Can access private field?
        }
    }
    
    static class StaticInnerClass {
        void accessOuter() {
            // System.out.println(outerField); // Can access?
        }
    }
}
```

**Answer:** 
- **Inner class**: CAN access private members of outer class
- **Static inner class**: CANNOT access non-static private members of outer class

### Q18: Method chaining and encapsulation?

```java
public class FluentAPI {
    private String name;
    private int age;
    
    public FluentAPI setName(String name) {
        this.name = name;
        return this; // Returns this for chaining
    }
    
    public FluentAPI setAge(int age) {
        this.age = age;
        return this;
    }
}

// Usage: new FluentAPI().setName("John").setAge(25);
// Does this maintain encapsulation?
```

**Answer:** YES, encapsulation is maintained. Returning `this` allows method chaining while still controlling access through setter methods with validation.

### Q19: Serialization and encapsulation?

```java
public class Employee implements Serializable {
    private String name;
    private transient String password; // transient field
    
    // Custom serialization
    private void writeObject(ObjectOutputStream out) throws IOException {
        out.defaultWriteObject();
        // Custom logic for sensitive data
    }
    
    private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {
        in.defaultReadObject();
        // Validation after deserialization
    }
}
```

**Answer:** Serialization can break encapsulation by exposing internal state. Use `transient` keyword and custom serialization methods to maintain encapsulation.

### Q20: Generics and encapsulation?

```java
public class Container<T> {
    private List<T> items = new ArrayList<>();
    
    public void add(T item) { items.add(item); }
    
    // Which return type maintains better encapsulation?
    public List<T> getItems() { return items; }              // Option 1
    public List<T> getItems() { return new ArrayList<>(items); } // Option 2
    public Iterator<T> getIterator() { return items.iterator(); } // Option 3
}
```

**Answer:** 
- Option 1: Breaks encapsulation (direct access)
- Option 2: Maintains encapsulation (defensive copy)
- Option 3: Best balance (controlled iteration without full access)

---

## 💡 Expert Level Concepts (3+ Years)

### Q21: Encapsulation in microservices architecture?

**Answer:** Encapsulation principles apply at service level:

```java
// Service encapsulation
@RestController
public class UserController {
    @Autowired
    private UserService userService; // Encapsulated business logic
    
    @GetMapping("/users/{id}")
    public ResponseEntity<UserDTO> getUser(@PathVariable Long id) {
        // DTO pattern - encapsulates data transfer
        UserDTO user = userService.getUserById(id);
        return ResponseEntity.ok(user);
    }
}

// Internal implementation hidden from other services
@Service
class UserService {
    private UserRepository repository; // Encapsulated data access
    
    public UserDTO getUserById(Long id) {
        // Business logic encapsulated within service
        User user = repository.findById(id);
        return convertToDTO(user);
    }
}
```

### Q22: Defensive programming and encapsulation?

**Answer:** Defensive programming strengthens encapsulation by protecting against misuse:

```java
public class BankAccount {
    private BigDecimal balance;
    private final Object lock = new Object();
    
    public void withdraw(BigDecimal amount) {
        // Defensive checks
        if (amount == null) {
            throw new IllegalArgumentException("Amount cannot be null");
        }
        if (amount.compareTo(BigDecimal.ZERO) <= 0) {
            throw new IllegalArgumentException("Amount must be positive");
        }
        
        synchronized (lock) { // Thread safety
            if (balance.compareTo(amount) < 0) {
                throw new InsufficientFundsException("Insufficient balance");
            }
            balance = balance.subtract(amount);
        }
    }
}
```

### Q23: Encapsulation design patterns?

**Answer:** Several patterns enhance encapsulation:

**Builder Pattern:**
```java
public class ComplexObject {
    private final String field1;
    private final int field2;
    
    private ComplexObject(Builder builder) {
        this.field1 = builder.field1;
        this.field2 = builder.field2;
    }
    
    public static class Builder {
        private String field1;
        private int field2;
        
        public Builder setField1(String field1) {
            this.field1 = field1;
            return this;
        }
        
        public ComplexObject build() {
            validate(); // Encapsulated validation
            return new ComplexObject(this);
        }
    }
}
```

**Factory Pattern:**
```java
public class ConnectionFactory {
    private static final Map<String, Connection> pool = new ConcurrentHashMap<>();
    
    // Encapsulates connection creation logic
    public static Connection getConnection(String type) {
        return pool.computeIfAbsent(type, ConnectionFactory::createConnection);
    }
    
    private static Connection createConnection(String type) {
        // Complex creation logic hidden
        switch (type) {
            case "mysql": return new MySQLConnection();
            case "oracle": return new OracleConnection();
            default: throw new IllegalArgumentException("Unknown type");
        }
    }
}
```

### Q24: Performance implications of encapsulation?

**Answer:**
- **Method call overhead**: Getters/setters add method call cost
- **JIT optimization**: HotSpot inlines simple getters/setters
- **Memory overhead**: Additional methods increase class size
- **Modern JVMs**: Performance difference is negligible for simple accessors

```java
// JIT will likely inline these simple methods
public String getName() { return name; }
public void setName(String name) { this.name = name; }
```

### Q25: Encapsulation with modern Java features?

**Answer:**

**Records (Java 14+):**
```java
// Automatic encapsulation with immutability
public record Person(String name, int age) {
    // Automatic getters: name(), age()
    // Automatic constructor with validation
    public Person {
        if (age < 0) throw new IllegalArgumentException("Age cannot be negative");
    }
}
```

**Module System (Java 9+):**
```java
// module-info.java
module com.example.banking {
    exports com.example.banking.api;      // Public API
    // com.example.banking.internal is encapsulated
}
```

---

## 🎭 Common Pitfalls & Best Practices

### 1. **Mutable Object Return:**
```java
// ❌ Bad - breaks encapsulation
public Date getCreatedDate() {
    return createdDate; // Client can modify internal date
}

// ✅ Good - defensive copy
public Date getCreatedDate() {
    return new Date(createdDate.getTime());
}

// ✅ Better - use immutable types
public LocalDateTime getCreatedDate() {
    return createdDate; // LocalDateTime is immutable
}
```

### 2. **Constructor Parameter Validation:**
```java
public class Rectangle {
    private final double width;
    private final double height;
    
    public Rectangle(double width, double height) {
        // ✅ Validate in constructor
        if (width <= 0 || height <= 0) {
            throw new IllegalArgumentException("Dimensions must be positive");
        }
        this.width = width;
        this.height = height;
    }
}
```

### 3. **Thread Safety Consideration:**
```java
public class ThreadSafeCounter {
    private volatile int count = 0; // volatile for visibility
    private final Object lock = new Object();
    
    public int getCount() {
        return count; // Safe to read volatile
    }
    
    public void increment() {
        synchronized (lock) { // Synchronization for compound operation
            count++;
        }
    }
}
```

### 4. **Inheritance and Protected Access:**
```java
public class BaseClass {
    protected String protectedField; // Accessible to subclasses
    
    // Template method pattern
    public final void templateMethod() {
        step1();
        step2(); // Protected method for subclass customization
        step3();
    }
    
    protected abstract void step2(); // Controlled extension point
}
```

---

## 🏗️ Real-world Examples

### 1. **JavaBeans Convention:**
```java
public class Customer implements Serializable {
    private String customerId;
    private String name;
    private List<Order> orders = new ArrayList<>();
    
    // Standard JavaBean pattern
    public String getCustomerId() { return customerId; }
    public void setCustomerId(String customerId) { this.customerId = customerId; }
    
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    
    // Defensive copy for collections
    public List<Order> getOrders() {
        return new ArrayList<>(orders);
    }
    
    public void addOrder(Order order) {
        if (order != null) {
            orders.add(order);
        }
    }
}
```

### 2. **Spring Framework Bean:**
```java
@Component
public class UserService {
    @Autowired
    private UserRepository userRepository; // Encapsulated dependency
    
    @Value("${user.max.age:100}")
    private int maxAge; // Encapsulated configuration
    
    public User createUser(UserRequest request) {
        validateRequest(request); // Encapsulated validation
        User user = mapToEntity(request); // Encapsulated mapping
        return userRepository.save(user);
    }
    
    private void validateRequest(UserRequest request) {
        // Internal validation logic
    }
}
```

### 3. **Database Connection Pool:**
```java
public class ConnectionPool {
    private final Queue<Connection> availableConnections;
    private final Set<Connection> usedConnections;
    private final int maxPoolSize;
    
    // Constructor and configuration methods...
    
    public synchronized Connection getConnection() {
        if (availableConnections.isEmpty() && usedConnections.size() < maxPoolSize) {
            availableConnections.offer(createNewConnection());
        }
        
        Connection connection = availableConnections.poll();
        if (connection != null) {
            usedConnections.add(connection);
        }
        return connection;
    }
    
    public synchronized void releaseConnection(Connection connection) {
        if (usedConnections.remove(connection)) {
            availableConnections.offer(connection);
        }
    }
    
    private Connection createNewConnection() {
        // Connection creation logic encapsulated
        return DriverManager.getConnection(url, username, password);
    }
}
```

---

## 📝 Quick Reference Card

```java
// Encapsulation Best Practices Checklist
public class EncapsulatedClass {
    // ✅ Data encapsulation
    private String sensitiveData;           // Private fields
    private final List<String> items = new ArrayList<>(); // Final collections
    
    // ✅ Controlled access
    public String getSensitiveData() {      // Public getter
        return sensitiveData;
    }
    
    public void setSensitiveData(String data) { // Validated setter
        if (data != null && data.length() > 0) {
            this.sensitiveData = data;
        }
    }
    
    // ✅ Defensive copying
    public List<String> getItems() {
        return new ArrayList<>(items);      // Return copy, not original
    }
    
    // ✅ Validation in constructor
    public EncapsulatedClass(String data) {
        if (data == null) throw new IllegalArgumentException("Data cannot be null");
        this.sensitiveData = data;
    }
    
    // ✅ Internal helper methods
    private void validateData(String data) { // Private helper
        // Validation logic
    }
}
```

---

## 🎯 Interview Success Strategy

### **Key Points to Emphasize:**

1. **Data protection** - Primary purpose of encapsulation
2. **Controlled access** - Getters/setters with validation
3. **Access modifiers** - Understanding visibility levels
4. **Defensive programming** - Protecting against misuse
5. **Real-world examples** - JavaBeans, frameworks, APIs

### **Common Interview Scenarios:**
- **Code review**: "What's wrong with this class design?"
- **Design questions**: "How would you design a thread-safe counter?"
- **Best practices**: "How do you handle mutable objects in getters?"
- **Breaking scenarios**: "How can encapsulation be broken?"

### **Buzzwords to Use:**
- Data Hiding
- Controlled Access
- Defensive Copying
- Information Hiding
- Access Modifiers
- JavaBeans Convention
- Immutable Objects

Remember: Encapsulation is about **protecting and controlling access** to data - demonstrate with practical examples and security considerations!
