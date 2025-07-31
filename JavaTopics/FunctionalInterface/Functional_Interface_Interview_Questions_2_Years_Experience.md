# Functional Interface - Interview Questions (2 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is a Functional Interface and why use it?
**Answer:** A Functional Interface has exactly one abstract method (SAM - Single Abstract Method). Benefits: enables lambda expressions, functional programming, cleaner code, better Stream API integration.

### Q2: What are the main built-in functional interfaces?
**Answer:**
| Interface | Method | Purpose | Example |
|-----------|---------|---------|---------|
| Predicate<T> | `boolean test(T t)` | Test condition | `x -> x > 0` |
| Function<T,R> | `R apply(T t)` | Transform input | `s -> s.length()` |
| Consumer<T> | `void accept(T t)` | Consume input | `s -> System.out.println(s)` |
| Supplier<T> | `T get()` | Supply/Generate | `() -> new Date()` |

### Q3: Can Functional Interface have multiple methods?
**Answer:** YES, but only ONE abstract method. Can have multiple default and static methods.
```java
@FunctionalInterface
interface MyInterface {
    void abstractMethod();              // Only one abstract method
    default void defaultMethod() {}     // Multiple default methods OK
    static void staticMethod() {}       // Multiple static methods OK
}
```

### Q4: What's the difference between Function and BiFunction?
**Answer:**
- **Function<T,R>:** Takes 1 parameter → `R apply(T t)`
- **BiFunction<T,U,R>:** Takes 2 parameters → `R apply(T t, U u)`
```java
Function<String, Integer> length = s -> s.length();
BiFunction<String, String, String> concat = (s1, s2) -> s1 + s2;
```

### Q5: What are method references and their types?
**Answer:** Shorthand for lambda expressions. 4 types:
1. **Static method:** `ClassName::methodName` → `Integer::parseInt`
2. **Instance method:** `object::methodName` → `str::toUpperCase`
3. **Instance method of arbitrary object:** `ClassName::methodName` → `String::length`
4. **Constructor:** `ClassName::new` → `ArrayList::new`

### Q6: Purpose of @FunctionalInterface annotation?
**Answer:** 
- **Optional** but recommended
- **Compile-time safety** - prevents adding multiple abstract methods
- **Documentation** - clearly indicates intent
- **IDE support** - better code assistance

### Q7: What are primitive functional interfaces?
**Answer:** Specialized versions to avoid boxing/unboxing overhead:
```java
// Instead of Function<Integer, Integer>
IntUnaryOperator square = x -> x * x;

// Instead of Predicate<Integer>
IntPredicate isEven = x -> x % 2 == 0;

// Instead of Consumer<Integer>
IntConsumer printer = x -> System.out.println(x);
```

### Q8: How to handle exceptions in lambda expressions?
**Answer:** 3 approaches:
```java
// 1. Try-catch inside lambda
Function<String, String> reader = file -> {
    try {
        return Files.readString(Paths.get(file));
    } catch (IOException e) {
        return "Error: " + e.getMessage();
    }
};

// 2. Wrapper method
public static <T,R> Function<T,R> wrapper(FunctionWithException<T,R> func) {
    return t -> {
        try { return func.apply(t); }
        catch (Exception e) { throw new RuntimeException(e); }
    };
}
```

### Q9: Difference between Consumer and Supplier?
**Answer:**
- **Consumer<T>:** Takes input, returns nothing → `void accept(T t)`
- **Supplier<T>:** Takes nothing, returns output → `T get()`
```java
Consumer<String> printer = s -> System.out.println(s);  // Input → void
Supplier<String> generator = () -> UUID.randomUUID().toString();  // void → Output
```

### Q10: Can functional interfaces be used in inheritance?
**Answer:** YES, but carefully:
```java
@FunctionalInterface
interface Parent { void method(); }

@FunctionalInterface  
interface Child extends Parent { 
    // Inherits method() - still functional interface
}

// But this breaks it:
interface Broken extends Parent {
    void anotherMethod(); // Now has 2 abstract methods - NOT functional
}
```

---

## 🔥 Tricky Interview Questions (High Probability)

### Q11: What's the output?
```java
@FunctionalInterface
interface Test {
    void method1();
    default void method2() { System.out.println("Default"); }
}

public class Main {
    public static void main(String[] args) {
        Test t = () -> System.out.println("Lambda");
        t.method1();  // ?
        t.method2();  // ?
    }
}
```
**Answer:**
```
Lambda
Default
```

### Q12: Which lambda expressions are valid?
```java
Predicate<String> p1 = s -> s.length() > 5;           // ?
Predicate<String> p2 = (s) -> s.length() > 5;         // ?
Predicate<String> p3 = (String s) -> s.length() > 5;  // ?
Predicate<String> p4 = s -> { return s.length() > 5; }; // ?
```
**Answer:** ALL are valid. Different lambda syntax variations.

### Q13: Method reference compilation issue?
```java
class Test {
    public void instanceMethod(String s) { System.out.println(s); }
    public static void staticMethod(String s) { System.out.println(s); }
    
    public void test() {
        Consumer<String> c1 = this::instanceMethod;  // ?
        Consumer<String> c2 = Test::staticMethod;    // ?
        Consumer<String> c3 = Test::instanceMethod;  // ?
    }
}
```
**Answer:**
- `this::instanceMethod` ✅ Valid
- `Test::staticMethod` ✅ Valid  
- `Test::instanceMethod` ❌ Compilation error (instance method needs object)

### Q14: Functional interface inheritance puzzle?
```java
interface A { void methodA(); }
interface B { void methodB(); }

@FunctionalInterface
interface C extends A, B { } // ?
```
**Answer:** ❌ Compilation error - C has 2 abstract methods (methodA + methodB), so not a functional interface.

### Q15: Lambda variable capture?
```java
public void test() {
    int x = 10;
    Supplier<Integer> supplier = () -> {
        // x++; // ?
        return x;
    };
    // x = 20; // ?
}
```
**Answer:** Both lines cause compilation errors. Lambda can only capture **effectively final** variables.

### Q16: Method reference with overloaded methods?
```java
class Calculator {
    public static int add(int a, int b) { return a + b; }
    public static double add(double a, double b) { return a + b; }
}

BinaryOperator<Integer> intAdd = Calculator::add;  // ?
BinaryOperator<Double> doubleAdd = Calculator::add; // ?
```
**Answer:** Both work! Compiler resolves correct overloaded method based on target type.

### Q17: Default method conflict?
```java
interface A { default void method() { System.out.println("A"); } }
interface B { default void method() { System.out.println("B"); } }

@FunctionalInterface
interface C extends A, B {
    void abstractMethod();
    // What about method() conflict?
}
```
**Answer:** ❌ Compilation error - must override method() to resolve conflict:
```java
@FunctionalInterface
interface C extends A, B {
    void abstractMethod();
    default void method() { A.super.method(); } // Must resolve conflict
}
```

### Q18: Generic functional interface?
```java
@FunctionalInterface
interface GenericFunction<T, R> {
    R apply(T input);
}

GenericFunction<String, Integer> f1 = s -> s.length();
GenericFunction<Integer, String> f2 = i -> String.valueOf(i);

// Can we do this?
GenericFunction<String, String> f3 = GenericFunction.identity(); // ?
```
**Answer:** ❌ No static `identity()` method. Must use `Function.identity()` or `s -> s`.

---

## 🎯 Advanced Questions (Bonus Points)

### Q19: Custom functional interface with generics?
**Answer:**
```java
@FunctionalInterface
interface TriFunction<T, U, V, R> {
    R apply(T t, U u, V v);
    
    default <W> TriFunction<T, U, V, W> andThen(Function<R, W> after) {
        return (t, u, v) -> after.apply(apply(t, u, v));
    }
}

// Usage
TriFunction<Integer, Integer, Integer, Integer> sum = (a, b, c) -> a + b + c;
```

### Q20: Functional interface with bounded generics?
**Answer:**
```java
@FunctionalInterface
interface NumberProcessor<T extends Number> {
    T process(T input);
}

NumberProcessor<Integer> intProcessor = i -> i * 2;
NumberProcessor<Double> doubleProcessor = d -> d * 1.5;
// NumberProcessor<String> stringProcessor = s -> s; // ❌ Compilation error
```

### Q21: Primitive functional interfaces performance?
**Answer:** 
- **Generic:** `Function<Integer, Integer>` → Boxing/Unboxing overhead
- **Primitive:** `IntUnaryOperator` → No boxing, better performance
```java
// Slower - boxing overhead
Function<Integer, Integer> slow = i -> i * 2;

// Faster - no boxing
IntUnaryOperator fast = i -> i * 2;
```

---

## 💡 Key Points for Your Experience Level

### Common Interview Mistakes (Avoid These):
1. **Multiple abstract methods** - Only ONE abstract method allowed
2. **Confusing lambda syntax** - Know all valid syntax variations
3. **Method reference errors** - Understand static vs instance references
4. **Variable capture rules** - Only effectively final variables
5. **Exception handling** - Must handle checked exceptions in lambdas

### What Interviewers Expect from 2-Year Experience:
- **Know all 4 main functional interfaces** (Predicate, Function, Consumer, Supplier)
- **Understand method references** and when to use them
- **Lambda syntax variations** and valid/invalid forms
- **Exception handling** in functional contexts
- **Primitive specializations** for performance

### Practical Usage Patterns:
```java
// Filtering and processing collections
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.stream()
     .filter(name -> name.length() > 3)        // Predicate
     .map(String::toUpperCase)                 // Function
     .forEach(System.out::println);            // Consumer

// Factory pattern with Supplier
Supplier<List<String>> listFactory = ArrayList::new;

// Validation with Predicate chaining
Predicate<String> notNull = Objects::nonNull;
Predicate<String> notEmpty = s -> !s.isEmpty();
Predicate<String> valid = notNull.and(notEmpty);
```

---

## 🚀 Last Minute Quick Review

### One-Liner Answers:
1. **What is functional interface?** → Interface with exactly one abstract method
2. **@FunctionalInterface purpose?** → Compile-time safety and documentation
3. **Can have multiple methods?** → Yes, but only one abstract method
4. **Main 4 interfaces?** → Predicate, Function, Consumer, Supplier
5. **Method reference types?** → Static, Instance, Arbitrary object, Constructor
6. **Lambda variable capture?** → Only effectively final variables
7. **Exception in lambda?** → Must handle checked exceptions explicitly
8. **Primitive interfaces purpose?** → Avoid boxing/unboxing overhead
9. **BiFunction vs Function?** → BiFunction takes 2 parameters, Function takes 1
10. **Default method conflicts?** → Must override to resolve ambiguity

### Memory Tips:
- **P**redicate → **P**redicate (test/boolean)
- **F**unction → **F**orm transformation (input → output)
- **C**onsumer → **C**onsume input (void operation)
- **S**upplier → **S**upply output (factory method)

### Lambda Syntax Variations:
```java
// All valid forms:
Predicate<String> p1 = s -> s.isEmpty();           // Single parameter, no parentheses
Predicate<String> p2 = (s) -> s.isEmpty();         // Single parameter, with parentheses  
Predicate<String> p3 = (String s) -> s.isEmpty();  // Explicit type
Function<String, Integer> f1 = s -> s.length();    // Single expression
Function<String, Integer> f2 = s -> { return s.length(); }; // Block body
BiPredicate<String, String> bp = (s1, s2) -> s1.equals(s2); // Multiple parameters
```

### Method Reference Examples:
```java
// Static method reference
Function<String, Integer> parseInt = Integer::parseInt;

// Instance method reference  
String str = "hello";
Supplier<String> upper = str::toUpperCase;

// Instance method of arbitrary object
Function<String, String> toUpper = String::toUpperCase;

// Constructor reference
Supplier<List<String>> listMaker = ArrayList::new;
```

---

## 🎯 Final Tips

### During Interview:
- **Provide examples** for each functional interface type
- **Show lambda and method reference equivalents**
- **Explain performance benefits** of primitive specializations
- **Demonstrate exception handling** techniques
- **Mention real-world usage** with Stream API

### Common Use Cases:
- **Data processing** with Stream API
- **Event handling** in GUI applications
- **Callback mechanisms** in asynchronous programming
- **Strategy pattern** implementation
- **Factory methods** with Supplier

### Red Flags to Avoid:
- Saying functional interfaces can have multiple abstract methods
- Not knowing the difference between method types (static vs instance)
- Forgetting about primitive specializations
- Not understanding lambda variable capture rules
- Confusion between Consumer and Supplier

---

*🎯 Perfect for 2-year experience functional interface interviews!*
