# Interface - Interview Questions (3 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is an Interface and why use it?

**Answer:** Interface is a contract that defines what a class can do without specifying how it does it. It provides **100% abstraction** (before Java 8).

**Benefits:**
- Multiple inheritance support
- Loose coupling between classes
- Contract-based programming
- Polymorphism support
- Testability (easy mocking)

### Q2: Interface evolution from Java 8 onwards?

**Answer:**

| Java Version | Features |
|--------------|----------|
| Before Java 8 | Only abstract methods and constants |
| Java 8 | Default methods, static methods |
| Java 9 | Private methods |
| Java 17 | Sealed interfaces |

```java
interface ModernInterface {
    // Constants (implicitly public static final)
    int CONSTANT = 10;
    
    // Abstract method (implicitly public abstract)
    void abstractMethod();
    
    // Default method (Java 8+)
    default void defaultMethod() { System.out.println("Default"); }
    
    // Static method (Java 8+)
    static void staticMethod() { System.out.println("Static"); }
    
    // Private method (Java 9+)
    private void privateHelper() { System.out.println("Helper"); }
}
```

### Q3: Rules for Interface implementation?

**Answer:**

- Class must implement ALL abstract methods
- Can override default methods (optional)
- Cannot override static methods
- Interface methods are implicitly **public**
- Interface variables are implicitly **public static final**
- Interface cannot have constructors
- Interface cannot have instance variables

### Q4: Difference between Interface and Abstract Class?

**Answer:**

| Interface | Abstract Class |
|-----------|----------------|
| 100% abstraction (pre-Java 8) | 0-100% abstraction |
| Multiple inheritance | Single inheritance |
| No constructors | Can have constructors |
| No instance variables | Can have instance variables |
| All methods public | Any access modifier |
| implements keyword | extends keyword |
| Cannot be instantiated | Cannot be instantiated |

### Q5: What are Default Methods and why were they introduced?

**Answer:** Default methods allow adding new methods to interfaces without breaking existing implementations.

```java
interface List<E> {
    // Existing methods
    boolean add(E e);
    boolean remove(Object o);
    
    // Java 8 addition - doesn't break existing implementations
    default void sort(Comparator<? super E> c) {
        Collections.sort(this, c);
    }
}
```

**Why introduced:** 
- Backward compatibility
- Interface evolution
- Functional programming support (Stream API)

### Q6: Multiple inheritance of behavior with interfaces?

**Answer:** Java supports multiple inheritance through interfaces, allowing a class to inherit behavior from multiple sources.

```java
interface Flyable {
    default void fly() { System.out.println("Flying"); }
}

interface Swimmable {
    default void swim() { System.out.println("Swimming"); }
}

class Duck implements Flyable, Swimmable {
    // Inherits both fly() and swim() methods
}

Duck duck = new Duck();
duck.fly();   // Flying
duck.swim();  // Swimming
```

### Q7: Diamond Problem with default methods?

**Answer:** When multiple interfaces have same default method signature, implementing class must resolve the conflict.

```java
interface A {
    default void method() { System.out.println("A"); }
}

interface B {
    default void method() { System.out.println("B"); }
}

class C implements A, B {
    // Must override to resolve conflict
    public void method() {
        A.super.method(); // Call specific interface method
        // or B.super.method();
        // or provide custom implementation
    }
}
```

### Q8: Static methods in interfaces?

**Answer:** Interface static methods belong to interface, not implementing class. Cannot be overridden.

```java
interface Utility {
    static void helper() { System.out.println("Helper method"); }
}

class Implementation implements Utility {
    // Cannot override static method
    // static void helper() { } // This is hiding, not overriding
}

// Usage
Utility.helper(); // Call via interface name
// Implementation.helper(); // Compilation error
```

### Q9: Functional Interfaces and Lambda expressions?

**Answer:** Functional Interface has exactly ONE abstract method. Enables lambda expressions.

```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
    
    // Can have default and static methods
    default void display() { System.out.println("Calculator"); }
    static void info() { System.out.println("Performs calculations"); }
}

// Lambda usage
Calculator add = (a, b) -> a + b;
Calculator multiply = (a, b) -> a * b;

System.out.println(add.calculate(5, 3)); // 8
```

### Q10: Interface segregation principle?

**Answer:** Classes should not be forced to implement methods they don't use. Create smaller, focused interfaces.

```java
// ❌ Fat interface
interface Worker {
    void work();
    void eat();
    void sleep();
}

// ✅ Segregated interfaces
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

class Human implements Workable, Eatable, Sleepable {
    // Implements all behaviors
}

class Robot implements Workable {
    // Only implements work - doesn't need eat/sleep
}
```

---

## 🔥 Tricky Questions (High Probability)

### Q11: What happens here?

```java
interface A {
    int value = 10; // What type of variable?
}

class B implements A {
    void method() {
        value = 20; // Compilation error?
    }
}
```

**Answer:** COMPILATION ERROR. Interface variables are implicitly **public static final** (constants), cannot be modified.

### Q12: Interface inheritance hierarchy?

```java
interface A {
    default void method() { System.out.println("A"); }
}

interface B extends A {
    default void method() { System.out.println("B"); }
}

class C implements B {
    // Which method() is inherited?
}

new C().method(); // Output?
```

**Answer:** "B" - More specific interface (B) overrides parent interface (A) default method.

### Q13: Abstract class implementing interface?

```java
interface Drawable {
    void draw();
    void color();
}

abstract class Shape implements Drawable {
    void draw() { System.out.println("Drawing"); }
    // color() not implemented
}

class Circle extends Shape {
    // What must be implemented?
}
```

**Answer:** Circle must implement `color()` method. Abstract classes can partially implement interfaces.

### Q14: Interface with private methods?

```java
interface Java9Interface {
    default void publicMethod() {
        commonLogic();
    }
    
    private void commonLogic() { // Java 9+
        System.out.println("Common logic");
    }
    
    static void staticMethod() {
        staticHelper();
    }
    
    private static void staticHelper() { // Java 9+
        System.out.println("Static helper");
    }
}
```

**Answer:** VALID (Java 9+). Private methods help reduce code duplication in default and static methods.

### Q15: Marker interfaces?

```java
interface Serializable {
    // No methods - marker interface
}

class Student implements Serializable {
    // Now Student objects can be serialized
}
```

**Answer:** Marker interfaces have no methods but provide metadata to JVM/frameworks. Examples: Serializable, Cloneable, Remote.

### Q16: Interface method resolution?

```java
interface A {
    default void method() { System.out.println("A"); }
}

interface B {
    default void method() { System.out.println("B"); }
}

interface C extends A, B {
    // What happens here?
}
```

**Answer:** COMPILATION ERROR. Interface C must override method() to resolve the diamond problem between A and B.

### Q17: Functional interface with default methods?

```java
@FunctionalInterface
interface MyInterface {
    void abstractMethod(); // One abstract method
    
    default void defaultMethod1() { }
    default void defaultMethod2() { }
    
    static void staticMethod() { }
    
    boolean equals(Object obj); // Object class method
}
```

**Answer:** VALID functional interface. Can have multiple default/static methods and Object class method overrides.

### Q18: Interface variables and inheritance?

```java
interface Parent {
    int value = 10;
}

interface Child extends Parent {
    int value = 20; // Hiding parent's value
}

class Test implements Child {
    void method() {
        System.out.println(value);        // Output?
        System.out.println(Parent.value); // Output?
        System.out.println(Child.value);  // Output?
    }
}
```

**Answer:** 
- `value`: 20 (Child's value)
- `Parent.value`: 10 
- `Child.value`: 20

### Q19: Lambda with method references?

```java
interface Processor {
    void process(String s);
}

class StringProcessor {
    static void staticProcess(String s) { System.out.println("Static: " + s); }
    void instanceProcess(String s) { System.out.println("Instance: " + s); }
}

// Which method references are valid?
Processor p1 = StringProcessor::staticProcess;    // Valid?
Processor p2 = new StringProcessor()::instanceProcess; // Valid?
Processor p3 = System.out::println;              // Valid?
```

**Answer:** All are VALID method references:
- Static method reference
- Instance method reference
- Instance method reference to System.out.println

### Q20: Sealed interfaces (Java 17+)?

```java
public sealed interface Shape
    permits Circle, Rectangle, Triangle {
    double area();
}

final class Circle implements Shape {
    public double area() { return Math.PI * radius * radius; }
}

// class Square implements Shape { } // Compilation error
```

**Answer:** Sealed interfaces restrict which classes can implement them. Only permitted classes can implement the interface.

---


### Q21: Interface design patterns?

**Answer:** Interfaces are foundation for many design patterns:

**Strategy Pattern:**
```java
interface PaymentStrategy {
    void pay(double amount);
}

class CreditCard implements PaymentStrategy {
    public void pay(double amount) { /* implementation */ }
}

class PayPal implements PaymentStrategy {
    public void pay(double amount) { /* implementation */ }
}
```

**Observer Pattern:**
```java
interface Observer {
    void update(String message);
}

interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers();
}
```

### Q22: Interface evolution strategies?

**Answer:** 

**Adding new methods:**
1. **Default methods** - Non-breaking change
2. **Abstract methods** - Breaking change
3. **New interface** - Versioning approach

```java
// Evolution strategy
interface PaymentProcessor {
    void processPayment(Payment payment);
    
    // Java 8+ - non-breaking addition
    default void validatePayment(Payment payment) {
        // Default implementation
    }
    
    // Java 9+ - helper methods
    private boolean isValid(Payment payment) {
        return payment != null && payment.getAmount() > 0;
    }
}
```

### Q23: Performance implications of interfaces?

**Answer:**
- **Virtual method calls**: Slight overhead for interface method calls
- **Method resolution**: JVM uses invokeinterface bytecode
- **JIT optimization**: HotSpot optimizes frequent interface calls
- **Compared to classes**: Modern JVMs have minimal performance difference

### Q24: Interface best practices?

**Answer:**

```java
// ✅ Good interface design
interface UserService {
    // Focused, cohesive methods
    User findById(Long id);
    void save(User user);
    void delete(Long id);
    
    // Default method for convenience
    default boolean exists(Long id) {
        return findById(id) != null;
    }
}

// ❌ Poor interface design
interface UserManager {
    // Too many responsibilities
    User findUser(Long id);
    void saveUser(User user);
    void sendEmail(User user);
    void generateReport();
    void validatePayment();
}
```

### Q25: Modern Java features with interfaces?

**Answer:**
- **Pattern matching** (Preview): Works with sealed interfaces
- **Records**: Can implement interfaces
- **Switch expressions**: Enhanced pattern matching with sealed interfaces

```java
// Pattern matching with sealed interfaces
public sealed interface Result permits Success, Error {
    // Interface methods
}

// Switch expression with pattern matching
String handleResult(Result result) {
    return switch (result) {
        case Success s -> "Success: " + s.getMessage();
        case Error e -> "Error: " + e.getCode();
    };
}
```

---

## 🎭 Common Pitfalls & Best Practices

### 1. **Diamond Problem Resolution:**
```java
interface A { default void method() { } }
interface B { default void method() { } }

class C implements A, B {
    public void method() {
        A.super.method(); // Explicit resolution required
    }
}
```

### 2. **Interface Pollution:**
```java
// ❌ Avoid - too many methods
interface SuperInterface {
    void method1(); void method2(); /* ... */ void method20();
}

// ✅ Better - focused interfaces
interface Readable { void read(); }
interface Writable { void write(); }
```

### 3. **Static Method Confusion:**
```java
interface Utility {
    static void helper() { }
}

class Implementation implements Utility {
    // Cannot call helper() directly
    void method() {
        // helper(); // Compilation error
        Utility.helper(); // Correct way
    }
}
```

### 4. **Functional Interface Best Practices:**
```java
@FunctionalInterface // Always use annotation
interface Calculator {
    int calculate(int a, int b);
    
    // Don't add more abstract methods
    // void anotherMethod(); // Breaks functional interface
}
```

---

## 🏗️ Real-world Examples

### 1. **Collections Framework:**
```java
interface List<E> extends Collection<E> {
    boolean add(E e);
    E get(int index);
    
    // Java 8 additions with default methods
    default void sort(Comparator<? super E> c) {
        Collections.sort(this, c);
    }
    
    default Spliterator<E> spliterator() {
        return Spliterators.spliterator(this, Spliterator.ORDERED);
    }
}
```

### 2. **Stream API:**
```java
// Functional interfaces enabling fluent API
Stream<String> result = names.stream()
    .filter(name -> name.length() > 3)    // Predicate interface
    .map(String::toUpperCase)             // Function interface
    .sorted()
    .collect(Collectors.toList());
```

### 3. **Spring Framework:**
```java
// Repository pattern with interfaces
interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByLastName(String lastName);
    
    @Query("SELECT u FROM User u WHERE u.email = ?1")
    User findByEmail(String email);
}
```

---

## 📝 Quick Reference Card

```java
// Interface Checklist
interface InterfaceDemo {
    // ✅ Allowed in interface
    int CONSTANT = 10;                    // public static final (implicit)
    void abstractMethod();                // public abstract (implicit)
    default void defaultMethod() { }      // Java 8+
    static void staticMethod() { }        // Java 8+
    private void privateHelper() { }      // Java 9+
    private static void staticHelper() { } // Java 9+
    
    // ❌ Not allowed
    // InterfaceDemo() { }                // No constructors
    // int instanceVar;                   // No instance variables
    // protected void method();           // All methods are public
    // final void method();               // No final methods (redundant)
}
```

---

## 🎯 Interview Success Strategy

### **Key Points to Emphasize:**

1. **Evolution story** - Java 8 default methods, Java 9 private methods
2. **Multiple inheritance** - Key advantage over classes
3. **Diamond problem resolution** - Show you understand conflicts
4. **Functional interfaces** - Connection to lambda expressions
5. **Contract-based design** - OOP principle understanding

### **Common Interview Scenarios:**
- **API design**: "Design a payment processing system using interfaces"
- **Code analysis**: "What's wrong with this interface implementation?"
- **Evolution**: "How would you add a new method to an existing interface?"
- **Best practices**: "Interface vs abstract class - when to use which?"

### **Buzzwords to Use:**
- Contract-based Programming
- Multiple Inheritance of Behavior
- Diamond Problem Resolution
- Default Method Evolution
- Functional Interface
- Interface Segregation Principle
- Polymorphism

Remember: Interfaces are about **contracts and behavior**, not implementation - focus on design and flexibility!
