# Polymorphism - Interview Questions (3 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is Polymorphism and its types in Java?

**Answer:** Polymorphism means "one interface, many implementations." Java supports two types:

- **Compile-time Polymorphism**: Method overloading, operator overloading
- **Runtime Polymorphism**: Method overriding with inheritance and interfaces

Benefits: Code flexibility, maintainability, extensibility, loose coupling.

### Q2: Difference between Compile-time vs Runtime Polymorphism?

**Answer:**

| Compile-time Polymorphism | Runtime Polymorphism |
|---------------------------|----------------------|
| Method Overloading | Method Overriding |
| Resolved at compile time | Resolved at runtime |
| Static binding | Dynamic binding |
| No inheritance required | Requires inheritance |
| Example: method(int), method(String) | Example: parent.method() calls child version |

### Q3: What is Dynamic Method Dispatch?

**Answer:** Dynamic Method Dispatch is the mechanism by which JVM determines which overridden method to call at runtime based on actual object type, not reference type.

```java
Animal animal = new Dog();
animal.sound(); // Calls Dog's sound() method, not Animal's
```

JVM uses **Virtual Method Table (vtable)** for efficient method lookup.

### Q4: Rules for Method Overriding in Polymorphism?

**Answer:**

- Same method signature (name, parameters, return type)
- Cannot reduce visibility (private → public ✅, public → private ❌)
- Cannot throw broader checked exceptions
- Return type must be same or covariant
- final, static, private methods cannot be overridden

### Q5: What is the difference between Overloading and Overriding?

**Answer:**

| Method Overloading | Method Overriding |
|-------------------|-------------------|
| Same class | Different classes (inheritance) |
| Different parameters | Same signature |
| Compile-time | Runtime |
| IS-A relationship not required | IS-A relationship required |
| Return type can vary | Return type must be same/covariant |

### Q6: Can we override static methods?

**Answer:** NO. Static methods cannot be overridden, they can only be **hidden** (method hiding).

```java
class Parent {
    static void method() { System.out.println("Parent"); }
}
class Child extends Parent {
    static void method() { System.out.println("Child"); } // Hiding, not overriding
}
Parent p = new Child();
p.method(); // Prints "Parent" - no polymorphism
```

### Q7: What is Covariant Return Type?

**Answer:** Overriding method can return a subtype of the original return type (since Java 5).

```java
class Animal { 
    Animal reproduce() { return new Animal(); }
}
class Dog extends Animal {
    Dog reproduce() { return new Dog(); } // Covariant return type
}
```

### Q8: How does polymorphism work with interfaces?

**Answer:** Interface references can point to any implementing class object, enabling polymorphism.

```java
interface Shape {
    void draw();
}
class Circle implements Shape {
    public void draw() { System.out.println("Drawing Circle"); }
}
Shape shape = new Circle(); // Interface polymorphism
shape.draw(); // Calls Circle's implementation
```

### Q9: What is instanceof operator and when to use it?

**Answer:** instanceof checks if an object is an instance of a specific class/interface. Use for type checking before casting.

```java
if (animal instanceof Dog) {
    Dog dog = (Dog) animal; // Safe casting
    dog.bark();
}
```

### Q10: Polymorphism with Abstract Classes?

**Answer:** Abstract classes enable polymorphism by providing common interface while allowing concrete classes to implement specific behavior.

```java
abstract class Vehicle {
    abstract void start();
    void stop() { System.out.println("Vehicle stopped"); }
}
class Car extends Vehicle {
    void start() { System.out.println("Car started"); }
}
Vehicle v = new Car(); // Polymorphic reference
```

---

## 🔥 Tricky Questions (High Probability)

### Q11: What's the output?

```java
class A {
    void method() { System.out.println("A"); }
}
class B extends A {
    void method() { System.out.println("B"); }
}
class C extends B {
    void method() { System.out.println("C"); }
}
A obj = new C();
obj.method(); // Output?
```

**Answer:** "C" - Dynamic method dispatch calls the most overridden version.

### Q12: Constructor and Polymorphism?

```java
class Parent {
    Parent() { method(); }
    void method() { System.out.println("Parent"); }
}
class Child extends Parent {
    int value = 10;
    void method() { System.out.println("Child: " + value); }
}
new Child(); // Output?
```

**Answer:** "Child: 0" - Constructor calls overridden method but instance variables aren't initialized yet.

### Q13: Polymorphism with method parameters?

```java
class Test {
    void method(Object obj) { System.out.println("Object"); }
    void method(String str) { System.out.println("String"); }
}
Test t = new Test();
Object obj = "Hello";
t.method(obj); // Output?
```

**Answer:** "Object" - Method resolution based on reference type, not actual object type (compile-time).

### Q14: Static method hiding vs overriding?

```java
class Parent {
    static void staticMethod() { System.out.println("Parent Static"); }
    void instanceMethod() { System.out.println("Parent Instance"); }
}
class Child extends Parent {
    static void staticMethod() { System.out.println("Child Static"); }
    void instanceMethod() { System.out.println("Child Instance"); }
}
Parent p = new Child();
p.staticMethod();    // Output?
p.instanceMethod();  // Output?
```

**Answer:** 
- "Parent Static" (static method hiding - no polymorphism)
- "Child Instance" (method overriding - polymorphism)

### Q15: Polymorphism with private methods?

```java
class Parent {
    private void method() { System.out.println("Parent Private"); }
    void callMethod() { method(); }
}
class Child extends Parent {
    private void method() { System.out.println("Child Private"); }
}
Parent p = new Child();
p.callMethod(); // Output?
```

**Answer:** "Parent Private" - Private methods cannot be overridden, no polymorphism.

### Q16: Multiple inheritance simulation with interfaces?

```java
interface A { default void method() { System.out.println("A"); } }
interface B { default void method() { System.out.println("B"); } }
class C implements A, B {
    // What's required here?
}
```

**Answer:** Must override method() to resolve conflict:

```java
public void method() {
    A.super.method(); // or B.super.method() or custom implementation
}
```

### Q17: Polymorphism with generics?

```java
List<Animal> animals = new ArrayList<>();
animals.add(new Dog());
animals.add(new Cat());
for(Animal animal : animals) {
    animal.makeSound(); // Polymorphism works?
}
```

**Answer:** YES - Polymorphism works with generics. Each animal calls its overridden method.

### Q18: Virtual method table confusion?

```java
class A {
    void method1() { method2(); }
    void method2() { System.out.println("A2"); }
}
class B extends A {
    void method2() { System.out.println("B2"); }
}
A obj = new B();
obj.method1(); // Output?
```

**Answer:** "B2" - method1() calls method2(), which is dynamically dispatched to B's version.

### Q19: Polymorphism with final methods?

```java
class Parent {
    final void finalMethod() { System.out.println("Parent Final"); }
    void normalMethod() { System.out.println("Parent Normal"); }
}
class Child extends Parent {
    // void finalMethod() { } // Compilation error
    void normalMethod() { System.out.println("Child Normal"); }
}
Parent p = new Child();
p.finalMethod();  // Output?
p.normalMethod(); // Output?
```

**Answer:**
- "Parent Final" (final methods cannot be overridden)
- "Child Normal" (normal method overriding works)

### Q20: Complex polymorphism scenario?

```java
class Vehicle {
    void start() { System.out.println("Vehicle start"); }
}
class Car extends Vehicle {
    void start() { System.out.println("Car start"); }
    void honk() { System.out.println("Car honk"); }
}
Vehicle v = new Car();
v.start(); // Output?
// v.honk(); // Compilation error - why?
((Car) v).honk(); // Output?
```

**Answer:**
- "Car start" (polymorphism)
- Compilation error: Vehicle reference cannot access Car-specific methods
- "Car honk" (after casting)

---

## 💡 Expert Level Concepts (3+ Years)

### Q21: Polymorphism and Performance?

**Answer:**
- **Virtual method calls**: Slight overhead due to vtable lookup
- **JIT optimization**: HotSpot optimizes frequent polymorphic calls
- **Monomorphic sites**: Single implementation calls are inlined
- **Megamorphic sites**: Multiple implementations cause performance degradation

### Q22: Double Dispatch Pattern?

**Answer:** Technique to achieve method selection based on runtime types of two objects:

```java
interface Visitor {
    void visit(Circle c);
    void visit(Rectangle r);
}
abstract class Shape {
    abstract void accept(Visitor v);
}
class Circle extends Shape {
    void accept(Visitor v) { v.visit(this); }
}
```

### Q23: Polymorphism with Lambda Expressions?

**Answer:** Functional interfaces enable polymorphism with lambdas:

```java
Function<String, Integer> func = s -> s.length(); // or String::length
// Different implementations can be assigned to same reference
func = s -> s.hashCode();
```

### Q24: Method Resolution in Complex Hierarchies?

**Answer:** JVM uses **C3 linearization** algorithm for method resolution order (MRO) in complex inheritance:

1. Current class methods
2. Parent class methods (depth-first)
3. Interface default methods (breadth-first)
4. Most specific implementation wins

### Q25: Polymorphism Best Practices?

**Answer:**
- **Prefer composition over inheritance** for flexibility
- **Program to interfaces**, not implementations
- **Use abstract classes** for common behavior + polymorphism
- **Avoid deep inheritance hierarchies** (max 3-4 levels)
- **Consider Strategy pattern** for algorithm polymorphism

---

## 🎭 Common Pitfalls & Advanced Scenarios

### 1. **Constructor Polymorphism Trap:**
```java
class Parent {
    Parent() { overriddenMethod(); } // Dangerous!
    void overriddenMethod() { }
}
```
**Problem:** Child's overridden method called before child initialization.

### 2. **Static Method Hiding Confusion:**
```java
Parent p = new Child();
p.staticMethod(); // Calls Parent's version, not Child's
```

### 3. **Covariant Return Types:**
```java
class Parent {
    Number getValue() { return 42; }
}
class Child extends Parent {
    Integer getValue() { return 100; } // Valid covariant return
}
```

### 4. **Interface Default Method Conflicts:**
```java
interface A { default void method() {} }
interface B { default void method() {} }
class C implements A, B {
    public void method() { A.super.method(); } // Must resolve
}
```

### 5. **Generic Type Erasure Impact:**
```java
List<String> strings = new ArrayList<>();
List<Object> objects = strings; // Compilation error
// Generics don't support true covariance
```

---

## 🏗️ Design Patterns Using Polymorphism

### 1. **Strategy Pattern:**
```java
interface PaymentStrategy {
    void pay(double amount);
}
class CreditCard implements PaymentStrategy {
    public void pay(double amount) { /* implementation */ }
}
class PaymentContext {
    private PaymentStrategy strategy;
    void executePayment(double amount) {
        strategy.pay(amount); // Polymorphic call
    }
}
```

### 2. **Template Method Pattern:**
```java
abstract class DataProcessor {
    final void process() {
        readData();    // Concrete method
        processData(); // Abstract method (polymorphic)
        saveData();    // Concrete method
    }
    abstract void processData();
}
```

### 3. **Factory Pattern:**
```java
interface Shape { void draw(); }
class ShapeFactory {
    static Shape createShape(String type) {
        switch(type) {
            case "circle": return new Circle();
            case "square": return new Square();
        }
        return null;
    }
}
```

---

## 📝 Quick Reference Card

```java
// Polymorphism Checklist
class PolymorphismDemo {
    // ✅ Runtime Polymorphism Requirements
    // 1. Inheritance (IS-A relationship)
    // 2. Method overriding
    // 3. Parent reference to child object
    
    Animal animal = new Dog(); // Polymorphic reference
    animal.makeSound(); // Calls Dog's implementation
    
    // ✅ Interface Polymorphism
    List<String> list = new ArrayList<>(); // or LinkedList
    
    // ✅ Covariant Return Types
    Animal reproduce() { return new Animal(); }
    Dog reproduce() { return new Dog(); } // Valid override
    
    // ❌ Common Mistakes
    // Static methods - no polymorphism
    // Private methods - no inheritance
    // Final methods - cannot override
    // Constructor calls - be careful with overridden methods
}
```

---

## 🎯 Interview Success Strategy

### **Key Points to Remember:**

1. **Always explain Dynamic Method Dispatch** - show you understand the mechanism
2. **Differentiate static vs instance methods** - common interview trap
3. **Mention Virtual Method Table (vtable)** - shows deeper understanding
4. **Give real-world examples** - Strategy pattern, Collections framework
5. **Discuss performance implications** - virtual method call overhead

### **Common Interview Patterns:**

- **Code tracing**: "What's the output?" questions
- **Design scenarios**: "How would you implement...?" using polymorphism
- **Troubleshooting**: "Why doesn't this work?" with casting/method calls
- **Best practices**: "When to use inheritance vs composition?"

### **Buzzwords to Use:**
- Dynamic Method Dispatch
- Virtual Method Table
- Late Binding
- Covariant Return Types
- Method Hiding vs Overriding
- Liskov Substitution Principle

Remember: Polymorphism is the foundation of OOP design patterns - connect it to real-world scenarios!
