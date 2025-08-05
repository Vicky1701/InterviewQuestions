# Abstraction - Interview Questions (3 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is Abstraction and why is it important?

**Answer:** Abstraction is the process of hiding implementation details while showing only essential features to the user. It focuses on **WHAT** an object does rather than **HOW** it does it.

**Benefits:**
- Reduces complexity
- Improves code maintainability
- Enables loose coupling
- Provides clear interfaces
- Supports code reusability

### Q2: Difference between Abstraction and Encapsulation?

**Answer:**

| Abstraction | Encapsulation |
|-------------|---------------|
| Hides complexity/implementation | Hides data and methods |
| Focus on design level | Focus on implementation level |
| "What" perspective | "How" perspective |
| Achieved via abstract classes/interfaces | Achieved via access modifiers |
| Example: Remote control interface | Example: Private variables with getters/setters |

### Q3: How is Abstraction achieved in Java?

**Answer:** Abstraction in Java is achieved through:

1. **Abstract Classes** (0-100% abstraction)
2. **Interfaces** (100% abstraction until Java 8, now partial with default methods)

```java
// Abstract class example
abstract class Vehicle {
    abstract void start(); // Abstract method
    void stop() { System.out.println("Vehicle stopped"); } // Concrete method
}

// Interface example
interface Drawable {
    void draw(); // Implicitly abstract
}
```

### Q4: What are Abstract Classes and their rules?

**Answer:** Abstract class is a class that cannot be instantiated and may contain abstract methods.

**Rules:**
- Cannot be instantiated directly
- Can have both abstract and concrete methods
- Can have constructors, instance variables, static methods
- Child class must implement all abstract methods
- Can extend only one abstract class (single inheritance)

```java
abstract class Animal {
    String name; // Instance variable allowed
    Animal(String name) { this.name = name; } // Constructor allowed
    abstract void makeSound(); // Abstract method
    void sleep() { System.out.println("Sleeping"); } // Concrete method
}
```

### Q5: Can abstract class have constructors?

**Answer:** YES. Abstract classes can have constructors which are called when child class object is created.

```java
abstract class Parent {
    Parent() { System.out.println("Parent constructor"); }
    abstract void method();
}
class Child extends Parent {
    Child() { super(); } // Calls parent constructor
    void method() { System.out.println("Child method"); }
}
new Child(); // Output: "Parent constructor"
```

### Q6: Difference between Abstract Class and Interface?

**Answer:**

| Abstract Class | Interface |
|----------------|-----------|
| Partial abstraction | Complete abstraction (pre-Java 8) |
| Can have concrete methods | Only abstract methods (pre-Java 8) |
| Can have instance variables | Only public static final variables |
| Single inheritance | Multiple inheritance |
| Can have constructors | Cannot have constructors |
| Any access modifier | Public methods only (pre-Java 8) |

### Q7: Can we have abstract methods in concrete class?

**Answer:** NO. If a class has even one abstract method, the class must be declared abstract.

```java
// ❌ Compilation Error
class ConcreteClass {
    abstract void method(); // Error: abstract method in concrete class
}

// ✅ Correct
abstract class AbstractClass {
    abstract void method(); // OK
}
```

### Q8: What happens if child class doesn't implement abstract method?

**Answer:** Child class must also be declared abstract, or it will be a compilation error.

```java
abstract class Parent {
    abstract void method1();
    abstract void method2();
}

// ❌ Compilation Error
class Child extends Parent {
    void method1() { } // Implements only one method
    // Missing method2() implementation
}

// ✅ Option 1: Implement all methods
class Child extends Parent {
    void method1() { }
    void method2() { }
}

// ✅ Option 2: Make child abstract
abstract class Child extends Parent {
    void method1() { } // Partial implementation
}
```

### Q9: Can abstract class implement interface?

**Answer:** YES. Abstract class can implement interface and may choose not to implement all interface methods.

```java
interface Drawable {
    void draw();
    void color();
}

abstract class Shape implements Drawable {
    void draw() { System.out.println("Drawing shape"); } // Can implement
    // color() method not implemented - left for concrete subclass
}

class Circle extends Shape {
    void color() { System.out.println("Coloring circle"); } // Must implement
}
```

### Q10: Real-world example of Abstraction?

**Answer:** **Template Method Pattern** - Abstract class defines algorithm structure, concrete classes implement specific steps.

```java
abstract class DataProcessor {
    // Template method - defines algorithm
    final void process() {
        readData();
        validateData();
        processData(); // Abstract - varies by implementation
        saveData();
    }
    
    abstract void processData(); // Abstract method
    
    void readData() { System.out.println("Reading data"); }
    void validateData() { System.out.println("Validating data"); }
    void saveData() { System.out.println("Saving data"); }
}

class XMLProcessor extends DataProcessor {
    void processData() { System.out.println("Processing XML"); }
}

class JSONProcessor extends DataProcessor {
    void processData() { System.out.println("Processing JSON"); }
}
```

---

## 🔥 Tricky Questions (High Probability)

### Q11: What's the output?

```java
abstract class Parent {
    Parent() { method(); }
    abstract void method();
}
class Child extends Parent {
    int value = 10;
    void method() { System.out.println("Value: " + value); }
}
new Child(); // Output?
```

**Answer:** "Value: 0" - Constructor calls abstract method before instance variable initialization.

### Q12: Can abstract class have final methods?

```java
abstract class Test {
    final void finalMethod() { System.out.println("Final method"); }
    abstract void abstractMethod();
}
class Child extends Test {
    // Can override finalMethod()? 
    // Must implement abstractMethod()?
}
```

**Answer:** 
- NO - Cannot override final methods
- YES - Must implement abstract methods
- Final and abstract are compatible in same class

### Q13: Abstract class with all concrete methods?

```java
abstract class AllConcrete {
    void method1() { System.out.println("Method 1"); }
    void method2() { System.out.println("Method 2"); }
    // No abstract methods - valid?
}
```

**Answer:** VALID. Abstract class can have zero abstract methods. Purpose: prevent direct instantiation while allowing inheritance.

### Q14: Static abstract methods?

```java
abstract class Test {
    static abstract void method(); // Compilation error?
}
```

**Answer:** COMPILATION ERROR. Abstract methods cannot be static because:
- Abstract methods need to be overridden
- Static methods cannot be overridden
- Contradiction in concept

### Q15: Abstract class implementing multiple interfaces?

```java
interface A { void methodA(); }
interface B { void methodB(); }
abstract class AbstractClass implements A, B {
    void methodA() { System.out.println("A"); }
    // methodB() not implemented
}
class ConcreteClass extends AbstractClass {
    // What must be implemented?
}
```

**Answer:** ConcreteClass must implement `methodB()` from interface B. Abstract class can partially implement interfaces.

### Q16: Constructor chaining in abstract classes?

```java
abstract class GrandParent {
    GrandParent(String name) { System.out.println("GP: " + name); }
}
abstract class Parent extends GrandParent {
    Parent() { super("Parent"); System.out.println("Parent"); }
}
class Child extends Parent {
    Child() { System.out.println("Child"); }
}
new Child(); // Output order?
```

**Answer:** "GP: Parent", "Parent", "Child" - Constructor chaining works normally in abstract class hierarchy.

### Q17: Anonymous class extending abstract class?

```java
abstract class AbstractClass {
    abstract void method();
}
AbstractClass obj = new AbstractClass() {
    void method() { System.out.println("Anonymous implementation"); }
};
obj.method(); // Valid?
```

**Answer:** VALID. Anonymous inner classes can extend abstract classes and implement abstract methods.

### Q18: Abstract class with private abstract method?

```java
abstract class Test {
    private abstract void method(); // Compilation error?
}
```

**Answer:** COMPILATION ERROR. Abstract methods cannot be private because:
- Abstract methods must be overridden
- Private methods cannot be accessed by subclass
- Cannot override private methods

### Q19: Abstract class variables?

```java
abstract class Test {
    static int staticVar = 10;
    int instanceVar = 20;
    final int finalVar = 30;
    abstract int abstractVar; // Valid?
}
```

**Answer:** 
- Static, instance, final variables: VALID
- Abstract variables: INVALID - no such concept exists
- Only methods can be abstract, not variables

### Q20: Reflection with abstract classes?

```java
abstract class AbstractClass {
    abstract void method();
}
Class<?> clazz = AbstractClass.class;
AbstractClass obj = (AbstractClass) clazz.newInstance(); // What happens?
```

**Answer:** **InstantiationException** at runtime. Reflection cannot instantiate abstract classes directly.

---


### Q21: Abstract classes and design patterns?

**Answer:** Abstract classes are foundation for several design patterns:

**Template Method Pattern:**
```java
abstract class Algorithm {
    final void execute() { // Template method
        step1(); step2(); step3();
    }
    abstract void step2(); // Varying step
    void step1() { } // Common step
    void step3() { } // Common step
}
```

**Factory Method Pattern:**
```java
abstract class Creator {
    abstract Product createProduct();
    void someOperation() {
        Product p = createProduct(); // Uses factory method
    }
}
```

### Q22: Abstract classes vs Composition?

**Answer:** 
- **Use Abstract Classes**: When you have shared code and IS-A relationship
- **Use Composition**: When you need HAS-A relationship and more flexibility

```java
// Abstract class approach
abstract class Vehicle { /* shared code */ }

// Composition approach  
class Car {
    Engine engine; // HAS-A relationship
    Transmission transmission;
}
```

### Q23: Abstract classes and generics?

**Answer:** Abstract classes can be generic and provide type safety:

```java
abstract class AbstractRepository<T> {
    abstract void save(T entity);
    abstract T findById(Long id);
    
    void commonMethod() { /* shared logic */ }
}

class UserRepository extends AbstractRepository<User> {
    void save(User user) { /* implementation */ }
    User findById(Long id) { /* implementation */ }
}
```

### Q24: Performance implications of abstract classes?

**Answer:**
- **Virtual method calls**: Slight overhead for abstract method calls
- **Memory**: Abstract classes consume memory like regular classes
- **JIT Optimization**: HotSpot optimizes frequently called abstract methods
- **Compared to interfaces**: Similar performance in modern JVMs

### Q25: Modern Java features with abstract classes?

**Answer:**
- **Sealed Classes (Java 17)**: Restrict which classes can extend abstract class
- **Pattern Matching**: Works with abstract class hierarchies
- **Records**: Cannot extend abstract classes (records are final)

```java
// Sealed abstract class
public sealed abstract class Shape 
    permits Circle, Rectangle, Triangle {
    abstract double area();
}
```

---

## 🎭 Common Pitfalls & Best Practices

### 1. **Constructor Call Pitfall:**
```java
abstract class Parent {
    Parent() { overriddenMethod(); } // Dangerous!
    abstract void overriddenMethod();
}
```
**Problem:** Abstract method called before child initialization.

### 2. **Over-abstraction:**
```java
// ❌ Too many levels
abstract class Level1 { }
abstract class Level2 extends Level1 { }
abstract class Level3 extends Level2 { }
// Difficult to maintain
```

### 3. **Abstract vs Interface Choice:**
- **Choose Abstract Class**: When you have common implementation code
- **Choose Interface**: When you need multiple inheritance or pure contracts

### 4. **Template Method Best Practices:**
```java
abstract class Template {
    final void templateMethod() { // Make template method final
        step1();
        step2();
    }
    abstract void step2();
    private void step1() { } // Hide internal steps
}
```

---

## 🏗️ Real-world Examples

### 1. **Collections Framework:**
```java
abstract class AbstractList<E> implements List<E> {
    // Common implementation for all lists
    public boolean add(E e) { /* default implementation */ }
    
    // Abstract methods for specific implementations
    abstract public E get(int index);
    abstract public int size();
}
```

### 2. **Servlet API:**
```java
abstract class HttpServlet {
    void service() { /* common logic */ }
    abstract void doGet(HttpServletRequest req, HttpServletResponse resp);
    abstract void doPost(HttpServletRequest req, HttpServletResponse resp);
}
```

### 3. **Custom Framework Example:**
```java
abstract class BaseController {
    void handleRequest() {
        authenticate();
        authorize();
        processRequest(); // Abstract
        logActivity();
    }
    abstract void processRequest();
    private void authenticate() { /* common logic */ }
    private void authorize() { /* common logic */ }
    private void logActivity() { /* common logic */ }
}
```

---

## 📝 Quick Reference Card

```java
// Abstraction Checklist
abstract class AbstractDemo {
    // ✅ Allowed in abstract class
    int instanceVar;           // Instance variables
    static int staticVar;      // Static variables  
    final int finalVar = 10;   // Final variables
    
    AbstractDemo() { }         // Constructors
    static void staticMethod() { } // Static methods
    final void finalMethod() { }   // Final methods
    void concreteMethod() { }      // Concrete methods
    abstract void abstractMethod(); // Abstract methods
    
    // ❌ Not allowed
    // private abstract void method(); // Private abstract
    // static abstract void method();  // Static abstract
    // final abstract void method();   // Final abstract
    // abstract int variable;          // Abstract variables
}
```

---

## 🎯 Interview Success Strategy

### **Key Points to Emphasize:**

1. **Template Method Pattern** - Most important real-world usage
2. **Constructor behavior** - Common interview trap
3. **Abstract vs Interface** - Classic comparison question
4. **Partial abstraction** - Key difference from interfaces
5. **Cannot instantiate** - Fundamental characteristic

### **Common Interview Scenarios:**
- **Design questions**: "Design a payment processing system" (abstract classes for common logic)
- **Code analysis**: "What's wrong with this code?" (constructor calling abstract method)
- **Comparison**: "When would you use abstract class over interface?"
- **Best practices**: "How do you avoid over-abstraction?"

### **Buzzwords to Use:**
- Template Method Pattern
- Partial Abstraction
- IS-A Relationship
- Constructor Chaining
- Virtual Method Dispatch
- Code Reusability

Remember: Abstraction is about **hiding complexity** and providing **clean interfaces** - focus on real-world design scenarios!
