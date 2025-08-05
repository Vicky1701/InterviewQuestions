# Method Overloading - Interview Questions (2 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is method overloading and why use it?
**Answer:** Method overloading allows multiple methods with same name but different parameters. Benefits: code reusabsility, better API design, compile-time polymorphism.

### Q2: Method overloading rules?
**Answer:** 
- Same method name
- Different parameters (number/type/sequence)
- Return type alone CANNOT differentiate
- Access modifiers can be different

### Q3: Method resolution priority order?
**Answer:**
1. **Exact match**
2. **Widening primitive** (int → long)
3. **Autoboxing** (int → Integer)
4. **Widening reference** (Child → Parent)
5. **Varargs** (lowest priority)

### Q4: What happens with null arguments?
**Answer:** Compiler chooses most specific type. If ambiguous → compilation error.
```java
method(String s) vs method(Object o)
obj.method(null); // Compilation error - ambiguous
obj.method((String) null); // OK - explicit cast
```

### Q5: Autoboxing vs Widening priority?
**Answer:** Widening has higher priority than autoboxing.
```java
method(long l) vs method(Integer i)
method(42); // Calls long method (widening preferred)
```

### Q6: Can return type alone differentiate methods?
**Answer:** NO. Method signature = name + parameters (NOT return type)
```java
// ❌ COMPILATION ERROR
int getValue() { return 1; }
String getValue() { return "Hello"; } // Same signature
```

### Q7: Constructor overloading?
**Answer:** Same rules as method overloading. Use `this()` for chaining (must be first statement).
```java
public Student() { this("Unknown", 0); }
public Student(String name) { this(name, 0); }
public Student(String name, int age) { /* implementation */ }
```

### Q8: Varargs overloading priority?
**Answer:** Varargs has LOWEST priority. Fixed-argument methods always preferred.
```java
method(int a) vs method(int... args)
method(42); // Calls fixed method, not varargs
```

### Q9: Can we overload main method?
**Answer:** YES, but JVM only calls `public static void main(String[] args)`
```java
public static void main(String[] args) { } // JVM calls this
public static void main(int args) { }      // Valid overload
```

### Q10: Static vs Instance method with same signature?
**Answer:** NOT allowed. Cannot have both static and instance methods with identical signature.
```java
// ❌ COMPILATION ERROR
public static void method(int a) { }
public void method(int a) { } // Error: same signature
```

---

## 🔥 Tricky Questions (High Probability)

### Q11: What's the output?
```java
class Test {
    void method(Object o) { System.out.println("Object"); }
    void method(String s) { System.out.println("String"); }
    void method(Integer i) { System.out.println("Integer"); }
}
new Test().method(null); // ?
```
**Answer:** Compilation error - ambiguous call (need explicit cast)

### Q12: Constructor chaining output?
```java
class Test {
    Test() { this(10); System.out.println("A"); }
    Test(int x) { this(x, x); System.out.println("B"); }
    Test(int x, int y) { System.out.println("C"); }
}
new Test(); // Output order?
```
**Answer:** C, B, A (chaining executes called constructor first)

### Q13: Varargs vs Array ambiguity?
```java
void method(int[] array) { }
void method(int... varargs) { }
method(new int[]{1,2,3}); // ?
```
**Answer:** Compilation error - ambiguous (int[] and int... are equivalent)

### Q14: Primitive wrapper priority?
```java
void method(long l) { }
void method(Integer i) { }
method(42); // Which method?
```
**Answer:** long method (widening > autoboxing)

### Q15: Ambiguous method calls?
```java
void method(int a, double b) { }
void method(double a, int b) { }
method(10, 20); // ?
```
**Answer:** Compilation error - both need conversion, equally specific

---

## 🎯 Advanced Questions (Bonus Points)

### Q16: Generic type overloading issues?
**Answer:** Type erasure makes `List<String>` and `List<Integer>` same signature → compilation error.
```java
// ❌ COMPILATION ERROR
void process(List<String> list) { }
void process(List<Integer> list) { } // Same erasure
```

### Q17: Inheritance + Overloading behavior?
**Answer:** Child inherits parent's overloaded methods + can add more. Method selection based on compile-time type.
```java
class Parent { void method(Object o) { } }
class Child extends Parent { void method(String s) { } }
// Child has both methods available
```

### Q18: Method hiding vs Overloading?
**Answer:** Static methods can be hidden (not overridden) but can be overloaded.
```java
class Parent { static void method(int a) { } }
class Child extends Parent { 
    static void method(int a) { }    // Hiding
    static void method(String a) { } // Overloading
}
```

---

## 💡 Key Points for Your Experience Level

### Common Interview Mistakes (Avoid These):
1. **Saying return type matters** - It doesn't for overloading
2. **Confusing with overriding** - Know the differences clearly
3. **Not knowing priority order** - Memorize: Exact → Widening → Autoboxing → Reference → Varargs
4. **Null handling confusion** - Always mention explicit casting solution
5. **Varargs placement** - Must be last parameter, only one allowed

### What Interviewers Expect from 2-Year Experience:
- **Solid understanding** of overloading rules
- **Know priority order** and can explain with examples
- **Handle tricky scenarios** like null arguments, varargs
- **Distinguish** between overloading and overriding clearly
- **Understand** constructor chaining with this()

### Code Examples to Practice:
```java
// Priority order example
class Test {
    void method(int i) { System.out.println("int"); }
    void method(long l) { System.out.println("long"); }
    void method(Integer i) { System.out.println("Integer"); }
    void method(Object o) { System.out.println("Object"); }
    void method(int... args) { System.out.println("varargs"); }
}
new Test().method(42); // Calls int method (exact match)
```

---

## 🚀 Last Minute Quick Review

### One-Liner Answers:
1. **Overloading definition?** → Same name, different parameters, compile-time
2. **Can return type overload?** → NO, only parameters matter
3. **Priority order?** → Exact → Widening → Autoboxing → Reference → Varargs
4. **Null handling?** → Most specific type or explicit cast needed
5. **Varargs priority?** → Lowest, fixed methods preferred
6. **Constructor chaining?** → Use this(), must be first statement
7. **Static overloading?** → YES, same rules apply
8. **Main method overloading?** → YES, but JVM calls specific signature
9. **Ambiguous calls?** → Compilation error when equal specificity
10. **Type erasure impact?** → Generic types cause overloading issues

### Final Tip:
**Always provide code examples** when explaining concepts. Interviewers love candidates who can demonstrate understanding with practical examples!

---

*🎯 Perfect for 2-year experience level interviews!*
