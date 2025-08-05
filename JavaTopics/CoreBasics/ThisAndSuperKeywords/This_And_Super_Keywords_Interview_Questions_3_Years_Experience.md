# this() and super() Keywords - Interview Questions (3 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is the difference between this() and super()?
**Answer:** 
- **this()**: Calls constructor of same class (constructor chaining)
- **super()**: Calls constructor of parent class (inheritance)

Both must be first statement in constructor and cannot be used together.

### Q2: Rules for this() and super() usage?
**Answer:**
- Must be **first statement** in constructor
- Cannot use both in same constructor
- If no explicit call, compiler adds `super()` automatically
- Can only be used in constructors, not methods
- Cannot be used in static context

### Q3: What happens if parent class has no default constructor?
**Answer:** Compilation error if child doesn't explicitly call parent's parameterized constructor using `super(params)`.
```java
class Parent {
    Parent(String name) { } // No default constructor
}
class Child extends Parent {
    Child() { 
        super("test"); // MUST call parent's constructor
    }
}
```

### Q4: Constructor chaining execution order?
**Answer:** Parent constructors execute **before** child constructors in inheritance hierarchy.
```java
class A { A() { System.out.println("A"); } }
class B extends A { B() { System.out.println("B"); } }
class C extends B { C() { System.out.println("C"); } }
new C(); // Output: A B C
```

### Q5: Can this() and super() be used in methods?
**Answer:** NO. They can only be used in constructors, not in regular methods.
```java
void method() {
    // this();    // ❌ Compilation error
    // super();   // ❌ Compilation error
}
```

### Q6: Difference between this/super keywords vs this()/super() calls?
**Answer:**
| Feature | this/super | this()/super() |
|---------|------------|----------------|
| Usage | Reference to object | Constructor calls |
| Context | Methods, constructors | Only constructors |
| Position | Anywhere | Must be first statement |
| Purpose | Access members | Constructor chaining |

### Q7: What if we don't call super() explicitly?
**Answer:** Compiler automatically adds `super()` as first statement if no explicit constructor call is made.
```java
class Child extends Parent {
    Child() {
        // Compiler adds: super(); automatically
        System.out.println("Child constructor");
    }
}
```

### Q8: Can we access parent's overridden method using super?
**Answer:** YES. Use `super.methodName()` to call parent's version of overridden method.
```java
class Parent {
    void display() { System.out.println("Parent"); }
}
class Child extends Parent {
    void display() { 
        super.display(); // Calls parent's method
        System.out.println("Child"); 
    }
}
```

### Q9: this() with method overloading?
**Answer:** this() can call any overloaded constructor based on parameters provided.
```java
class Student {
    Student() { this("Unknown"); }           // Calls constructor below
    Student(String name) { this(name, 0); } // Calls constructor below  
    Student(String name, int age) { /* implementation */ }
}
```

### Q10: Can super() call grandparent constructor directly?
**Answer:** NO. super() only calls immediate parent class constructor. For grandparent, parent must use super() in its constructor.
```java
class GrandParent { GrandParent(int x) {} }
class Parent extends GrandParent { 
    Parent() { super(10); } // Calls GrandParent constructor
}
class Child extends Parent {
    Child() { 
        super(); // Calls Parent constructor only
        // Cannot directly call GrandParent constructor
    }
}
```

---

## 🔥 Tricky Questions (High Probability)

### Q11: What's the output?
```java
class A {
    A() { this(10); System.out.println("A()"); }
    A(int x) { System.out.println("A(int)"); }
}
class B extends A {
    B() { System.out.println("B()"); }
}
new B(); // Output?
```
**Answer:** A(int), A(), B() 
(B() calls super(), which calls A(), which calls A(int))

### Q12: Will this compile?
```java
class Parent {
    Parent(String s) { }
}
class Child extends Parent {
    Child() { 
        System.out.println("Before super");
        super("test");
    }
}
```
**Answer:** NO. Compilation error - super() must be first statement.

### Q13: What's the issue here?
```java
class Test {
    Test() {
        this(getValue()); // Problem?
    }
    Test(int x) { }
    static int getValue() { return 42; }
}
```
**Answer:** NO issue. Static method calls are allowed in this()/super() calls because they don't depend on object state.

### Q14: Tricky constructor chaining output?
```java
class A {
    A() { System.out.println("A1"); }
    A(int x) { this(); System.out.println("A2"); }
}
class B extends A {
    B() { this(5); System.out.println("B1"); }
    B(int x) { super(x); System.out.println("B2"); }
}
new B(); // Output?
```
**Answer:** A1, A2, B2, B1
(B() → B(5) → A(5) → A() → prints A1, A2, B2, B1)

### Q15: Will this work?
```java
class Parent {
    int value = 10;
    Parent() { System.out.println(value); }
}
class Child extends Parent {
    int value = 20;
    Child() { super(); }
}
new Child(); // Output?
```
**Answer:** Prints 10. Parent constructor executes first, accessing parent's value (instance variables are not overridden).

### Q16: Complex this() chaining?
```java
class Test {
    Test() { this(1); System.out.println("1"); }
    Test(int a) { this(a, a); System.out.println("2"); }
    Test(int a, int b) { System.out.println("3"); }
}
new Test(); // Execution order?
```
**Answer:** 3, 2, 1 (chained constructors execute in reverse order of calling)

### Q17: What happens here?
```java
class A {
    A() { method(); }
    void method() { System.out.println("A"); }
}
class B extends A {
    int value = 5;
    void method() { System.out.println("B: " + value); }
}
new B(); // Output?
```
**Answer:** "B: 0" - Overridden method is called but instance variables are not initialized yet (default value 0).

### Q18: Can this be used in super() call?
```java
class Parent {
    Parent(Object obj) { }
}
class Child extends Parent {
    Child() {
        super(this); // Valid?
    }
}
```
**Answer:** NO. Compilation error - cannot use 'this' in super() call because object is not fully constructed yet.

### Q19: Multiple inheritance simulation?
```java
class A {
    A() { System.out.println("A"); }
}
class B {
    B() { System.out.println("B"); }
}
class C extends A {
    B b = new B();
    C() { System.out.println("C"); }
}
new C(); // Output order?
```
**Answer:** A, B, C (Parent constructor → Instance variable initialization → Current constructor)

### Q20: Final tricky scenario?
```java
class Test {
    static { System.out.println("Static"); }
    { System.out.println("Instance"); }
    Test() { 
        this(42);
        System.out.println("Default"); 
    }
    Test(int x) { 
        System.out.println("Parameterized"); 
    }
}
new Test(); // Complete output?
```
**Answer:** Static, Instance, Parameterized, Default
(Static block → Instance block → Chained constructor → Current constructor)

---

## 💡 Expert Level Concepts (3+ Years)

### Q21: Constructor reference with this()/super()?
**Answer:** Constructor references (ClassName::new) cannot use this() or super() explicitly - handled by JVM internally.

### Q22: this()/super() with lambda expressions?
**Answer:** Lambdas cannot contain constructor calls (this/super) as they're not constructors themselves.

### Q23: this()/super() in enum constructors?
**Answer:** Enum constructors can use this() for chaining but super() is implicitly called for java.lang.Enum.

### Q24: Performance implications?
**Answer:** 
- Constructor chaining adds slight overhead
- JVM optimizes simple chains
- Deep chaining can impact object creation performance
- Consider factory methods for complex initialization

### Q25: Best practices for this()/super()?
**Answer:**
- Use this() to avoid code duplication in constructors
- Always call super() explicitly when parent has no default constructor
- Avoid deep constructor chaining (max 3-4 levels)
- Use builder pattern for complex object construction
- Document constructor dependencies clearly

---

## 🎭 Common Pitfalls & Debugging Tips

### 1. **Compilation Errors:**
- super()/this() not first statement
- Using both in same constructor
- Missing parent constructor call

### 2. **Runtime Issues:**
- NPE when accessing uninitialized variables in constructor
- Infinite constructor loops (rare but possible)

### 3. **Design Issues:**
- Over-complicated constructor chains
- Breaking encapsulation with protected constructors

### 4. **Debugging Strategy:**
- Use debugger to trace constructor execution order
- Add logging to understand initialization sequence
- Check parent class constructor signatures

---

## 📝 Quick Reference Card

```java
// Constructor Chaining Rules
class Demo {
    Demo() { this(0); }           // ✅ Valid
    Demo(int x) { super(); }      // ✅ Valid (explicit)
    Demo(String s) {              // ✅ Valid (implicit super())
        // super() added by compiler
    }
    
    // ❌ Invalid patterns
    Demo(double d) {
        System.out.println("Hi");
        super(); // Error: not first statement
    }
    
    Demo(float f) {
        this();
        super(); // Error: cannot use both
    }
}
```

---

## 🏆 Interview Success Tips

1. **Always mention** the "first statement" rule
2. **Explain** automatic super() insertion by compiler
3. **Trace through** execution order in complex scenarios
4. **Differentiate** between this/super (references) vs this()/super() (calls)
5. **Give practical examples** of when to use constructor chaining

Remember: Interviewers love testing edge cases and execution order with this()/super()!
