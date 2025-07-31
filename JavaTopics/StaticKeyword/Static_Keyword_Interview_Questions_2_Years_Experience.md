# Static Keyword - Interview Questions (2 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is static keyword and why use it?
**Answer:** Static keyword creates class-level members that belong to class, not instances. Benefits: shared data, utility methods, memory efficiency, no object creation needed.

### Q2: Difference between static and instance variables?
**Answer:**
| Static Variables | Instance Variables |
|-----------------|-------------------|
| One copy per class | One copy per object |
| Shared among all objects | Unique to each object |
| Memory in Method Area | Memory in Heap |
| Accessed via class name | Accessed via object reference |

### Q3: Can static methods access non-static variables?
**Answer:** NO. Static methods cannot directly access non-static variables because static methods exist before any object is created.
```java
class Test {
    int instanceVar = 10;
    static void method() {
        // System.out.println(instanceVar); // ERROR!
        Test obj = new Test();
        System.out.println(obj.instanceVar); // OK - via object
    }
}
```

### Q4: What are static blocks and when do they execute?
**Answer:** Static blocks initialize static variables and execute once when class is first loaded, before main method.
```java
class Test {
    static int count;
    static {
        count = 100; // Executes during class loading
        System.out.println("Static block executed");
    }
}
```

### Q5: Can we override static methods?
**Answer:** NO. Static methods cannot be overridden, they can only be **hidden** (method hiding).
```java
class Parent {
    static void method() { System.out.println("Parent"); }
}
class Child extends Parent {
    static void method() { System.out.println("Child"); } // Hiding, not overriding
}
```

### Q6: Order of execution in static context?
**Answer:** 
1. Static variables (default values)
2. Static blocks (in order of appearance)
3. Static variable initialization
4. Main method
```java
class Test {
    static int a = 10;           // Step 1 & 3
    static { System.out.println("Block 1"); } // Step 2
    static int b = 20;           // Step 1 & 3
    static { System.out.println("Block 2"); } // Step 2
}
```

### Q7: Can we use this/super in static methods?
**Answer:** NO. `this` and `super` refer to current object instance, but static methods belong to class, not instances.
```java
class Test {
    static void method() {
        // System.out.println(this); // ERROR!
        // super.toString();          // ERROR!
    }
}
```

### Q8: What happens to static variables in inheritance?
**Answer:** Static variables are shared between parent and child classes. Child doesn't create new copy.
```java
class Parent {
    static int count = 0;
}
class Child extends Parent {
    // Child shares same 'count' variable with Parent
}
Parent.count++; // Changes count for both Parent and Child
```

### Q9: Can we access static members with null reference?
**Answer:** YES. Static members are resolved at compile-time based on reference type, not runtime object.
```java
Test obj = null;
obj.staticMethod(); // Works! No NullPointerException
```

### Q10: Can static variables be serialized?
**Answer:** NO. Static variables belong to class, not instances. During deserialization, static variables retain their current class values.

---

## 🔥 Tricky Interview Questions (High Probability)

### Q11: What's the output?
```java
class Parent {
    static { System.out.println("Parent static"); }
    { System.out.println("Parent instance"); }
    Parent() { System.out.println("Parent constructor"); }
}

class Child extends Parent {
    static { System.out.println("Child static"); }
    { System.out.println("Child instance"); }
    Child() { System.out.println("Child constructor"); }
}

public class Test {
    public static void main(String[] args) {
        new Child();
    }
}
```
**Answer:** 
```
Parent static
Child static
Parent instance
Parent constructor
Child instance
Child constructor
```

### Q12: Method hiding vs Overriding - What's called?
```java
class Parent {
    static void method() { System.out.println("Parent static"); }
    void instanceMethod() { System.out.println("Parent instance"); }
}

class Child extends Parent {
    static void method() { System.out.println("Child static"); }
    void instanceMethod() { System.out.println("Child instance"); }
}

Parent p = new Child();
p.method();         // ?
p.instanceMethod(); // ?
```
**Answer:** 
- `p.method()` → "Parent static" (method hiding - compile-time)
- `p.instanceMethod()` → "Child instance" (overriding - runtime)

### Q13: Static variable initialization order?
```java
class Test {
    static int a = getValue("a");
    static { System.out.println("Static block 1"); }
    static int b = getValue("b");
    static { System.out.println("Static block 2"); }
    
    static int getValue(String name) {
        System.out.println("Initializing " + name);
        return 10;
    }
    
    public static void main(String[] args) {
        System.out.println("Main method");
    }
}
```
**Answer:**
```
Initializing a
Static block 1
Initializing b
Static block 2
Main method
```

### Q14: Null reference static access?
```java
class Test {
    static void staticMethod() { System.out.println("Static method"); }
    void instanceMethod() { System.out.println("Instance method"); }
    
    public static void main(String[] args) {
        Test obj = null;
        obj.staticMethod();    // ?
        obj.instanceMethod();  // ?
    }
}
```
**Answer:**
- `obj.staticMethod()` → "Static method" (works fine)
- `obj.instanceMethod()` → NullPointerException

### Q15: Static block execution with inheritance?
```java
class A {
    static { System.out.println("A static"); }
}

class B extends A {
    static { System.out.println("B static"); }
}

class C extends B {
    static { System.out.println("C static"); }
}

public class Test {
    public static void main(String[] args) {
        C obj = new C();
    }
}
```
**Answer:**
```
A static
B static
C static
```

### Q16: Static import confusion?
```java
import static java.lang.System.out;
import static java.lang.Math.*;

class Test {
    static int out = 100;  // Name conflict!
    
    public static void main(String[] args) {
        out.println("Hello");  // ?
        System.out.println(PI); // ?
    }
}
```
**Answer:** 
- `out.println("Hello")` → Compilation error (ambiguous)
- `System.out.println(PI)` → Works, prints 3.141592653589793

### Q17: Final static variable initialization?
```java
class Test {
    static final int A = 10;        // OK
    static final int B;             // Must initialize in static block
    static final int C;             // ?
    
    static {
        B = 20;  // OK
        // C not initialized - what happens?
    }
}
```
**Answer:** Compilation error - final static variable C must be initialized either at declaration or in static block.

### Q18: Multiple static blocks execution?
```java
class Test {
    static int x = 10;
    static { x = 20; System.out.println("Block 1: x = " + x); }
    static int y = getValue();
    static { x = 40; System.out.println("Block 2: x = " + x); }
    
    static int getValue() {
        System.out.println("getValue called: x = " + x);
        return 30;
    }
    
    public static void main(String[] args) {
        System.out.println("Final x = " + x + ", y = " + y);
    }
}
```
**Answer:**
```
Block 1: x = 20
getValue called: x = 20
Block 2: x = 40
Final x = 40, y = 30
```

---

## 🎯 Advanced Questions (Bonus Points)

### Q19: Static nested class vs Inner class?
**Answer:**
```java
class Outer {
    static int staticVar = 10;
    int instanceVar = 20;
    
    static class StaticNested {
        void method() {
            System.out.println(staticVar);    // OK
            // System.out.println(instanceVar); // ERROR!
        }
    }
    
    class Inner {
        void method() {
            System.out.println(staticVar);    // OK
            System.out.println(instanceVar);  // OK
        }
    }
}
```

### Q20: Static context memory allocation?
**Answer:** Static variables are stored in **Method Area** (Metaspace in Java 8+), not in Heap. They're loaded when class is first referenced.

### Q21: Can static methods be synchronized?
**Answer:** YES. Static synchronized methods acquire class-level lock (Class object lock).
```java
class Test {
    static synchronized void method1() { } // Class level lock
    synchronized void method2() { }        // Instance level lock
}
```

---

## 💡 Key Points for Your Experience Level

### Common Interview Mistakes (Avoid These):
1. **Saying static methods can be overridden** - They can only be hidden
2. **Confusing static variable sharing** - Same variable shared in inheritance
3. **Not knowing execution order** - Static blocks → Static variables → Main
4. **this/super in static context** - Not allowed, belongs to instances
5. **Static import conflicts** - Name clashes with local static members

### What Interviewers Expect from 2-Year Experience:
- **Clear understanding** of static vs instance context
- **Know execution order** in inheritance hierarchy
- **Understand method hiding** vs overriding concept
- **Handle tricky scenarios** like null reference access
- **Memory allocation** knowledge (Method Area vs Heap)

### Code Scenarios to Practice:
```java
// Execution order
class Parent {
    static int a = print("Parent static var");
    static { print("Parent static block"); }
    { print("Parent instance block"); }
    Parent() { print("Parent constructor"); }
    
    static int print(String msg) {
        System.out.println(msg);
        return 1;
    }
}
```

---

## 🚀 Last Minute Quick Review

### One-Liner Answers:
1. **What is static?** → Class-level members, shared among all instances
2. **Can static access non-static?** → NO, need object reference
3. **Static method overriding?** → NO, only method hiding possible
4. **Static block purpose?** → Initialize static variables during class loading
5. **this/super in static?** → NOT allowed, no instance context
6. **Static variable inheritance?** → Same variable shared between parent-child
7. **Static execution order?** → Static vars → Static blocks → Main method
8. **Null reference static access?** → YES, resolved at compile-time
9. **Static serialization?** → NO, static variables not serialized
10. **Static memory location?** → Method Area (Metaspace), not Heap

### Memory Tips:
- **Static = Class level** (not instance level)
- **Method hiding ≠ Overriding** (compile-time vs runtime)
- **Execution order**: Parent static → Child static → Parent instance → Child instance
- **No this/super** in static context (no instance reference)

### Common Patterns:
```java
// Singleton pattern with static
class Singleton {
    private static Singleton instance;
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

// Utility class pattern
class MathUtils {
    private MathUtils() {} // Prevent instantiation
    
    public static int add(int a, int b) {
        return a + b;
    }
}
```

---

## 🎯 Final Tips

### During Interview:
- **Always provide examples** when explaining static concepts
- **Draw memory diagrams** for Method Area vs Heap allocation
- **Trace execution order** for inheritance scenarios
- **Mention practical use cases** (Singleton, Utility classes, Constants)

### Real-world Applications:
- **Database connections** - static connection pools
- **Configuration constants** - static final variables
- **Utility methods** - Math.max(), System.out.println()
- **Counters** - tracking total objects created

---

*🎯 Perfect for 2-year experience static keyword interviews!*
