# Java Static Keyword - Complete Guide

## Table of Contents
1. [Introduction to Static Keyword](#introduction)
2. [Static Variables](#static-variables)
3. [Static Blocks](#static-blocks)
4. [Static Methods](#static-methods)
5. [Static Classes](#static-classes)
6. [Hierarchy Flow](#hierarchy-flow)
7. [Interview Questions](#interview-questions)
8. [Common Mistakes (Wrong vs Right Code)](#common-mistakes)

---

## Introduction to Static Keyword {#introduction}

The `static` keyword in Java is used to create class-level variables and methods that belong to the class rather than to any specific instance of the class. Static members are shared among all instances of the class and can be accessed without creating an object of the class.

**Key Points:**
- Static members belong to the class, not to instances
- Memory allocation happens only once when the class is first loaded
- Can be accessed using class name directly
- Loaded during class loading time

---

## 1. Static Variables {#static-variables}

Static variables are also called **class variables**. They are shared among all instances of the class.

### Characteristics:
- Only one copy exists regardless of how many objects are created
- Initialized when the class is first loaded
- Memory allocation in Method Area (Metaspace in Java 8+)

### Example Code:

```java
class Student {
    private String name;
    private int rollNo;
    
    // Static variable - shared among all instances
    private static String collegeName = "XYZ University";
    private static int totalStudents = 0;
    
    public Student(String name, int rollNo) {
        this.name = name;
        this.rollNo = rollNo;
        totalStudents++; // Incrementing static variable
    }
    
    public void displayInfo() {
        System.out.println("Name: " + name);
        System.out.println("Roll No: " + rollNo);
        System.out.println("College: " + collegeName);
        System.out.println("Total Students: " + totalStudents);
    }
    
    // Static method to access static variable
    public static int getTotalStudents() {
        return totalStudents;
    }
}

public class StaticVariableExample {
    public static void main(String[] args) {
        Student s1 = new Student("Alice", 101);
        Student s2 = new Student("Bob", 102);
        Student s3 = new Student("Charlie", 103);
        
        s1.displayInfo();
        System.out.println("Total students created: " + Student.getTotalStudents());
    }
}
```

**Output:**
```
Name: Alice
Roll No: 101
College: XYZ University
Total Students: 3
Total students created: 3
```

---

## 2. Static Blocks {#static-blocks}

Static blocks are used to initialize static variables and are executed when the class is first loaded into memory.

### Characteristics:
- Executed only once when class is loaded
- Executed before the main method
- Cannot access non-static variables directly
- Multiple static blocks are executed in the order they appear

### Example Code:

```java
class DatabaseConfig {
    private static String databaseUrl;
    private static String username;
    private static String password;
    
    // Static block 1
    static {
        System.out.println("Static block 1 executed");
        databaseUrl = "jdbc:mysql://localhost:3306/mydb";
    }
    
    // Static block 2
    static {
        System.out.println("Static block 2 executed");
        username = "admin";
        password = "password123";
        System.out.println("Database configuration initialized");
    }
    
    public static void displayConfig() {
        System.out.println("Database URL: " + databaseUrl);
        System.out.println("Username: " + username);
        System.out.println("Password: " + password);
    }
}

public class StaticBlockExample {
    static {
        System.out.println("Main class static block executed");
    }
    
    public static void main(String[] args) {
        System.out.println("Main method started");
        DatabaseConfig.displayConfig();
    }
}
```

**Output:**
```
Main class static block executed
Static block 1 executed
Static block 2 executed
Database configuration initialized
Main method started
Database URL: jdbc:mysql://localhost:3306/mydb
Username: admin
Password: password123
```

---

## 3. Static Methods {#static-methods}

Static methods belong to the class and can be called without creating an instance of the class.

### Characteristics:
- Cannot access non-static variables or methods directly
- Cannot use `this` or `super` keywords
- Cannot be overridden (but can be hidden)
- Can be accessed using class name

### Example Code:

```java
class MathUtils {
    private static final double PI = 3.14159;
    
    // Static method for calculating area of circle
    public static double calculateCircleArea(double radius) {
        return PI * radius * radius;
    }
    
    // Static method for calculating factorial
    public static long factorial(int n) {
        if (n <= 1) return 1;
        return n * factorial(n - 1);
    }
    
    // Static method to find maximum of two numbers
    public static int max(int a, int b) {
        return (a > b) ? a : b;
    }
}

class Calculator {
    private String brand;
    
    public Calculator(String brand) {
        this.brand = brand;
    }
    
    // Non-static method
    public void displayBrand() {
        System.out.println("Calculator brand: " + brand);
    }
    
    // Static method
    public static double add(double a, double b) {
        return a + b;
    }
    
    // Static method accessing static method
    public static double multiply(double a, double b) {
        return a * b;
    }
}

public class StaticMethodExample {
    public static void main(String[] args) {
        // Calling static methods without creating objects
        double area = MathUtils.calculateCircleArea(5.0);
        System.out.println("Circle area: " + area);
        
        long fact = MathUtils.factorial(5);
        System.out.println("Factorial of 5: " + fact);
        
        int maximum = MathUtils.max(10, 20);
        System.out.println("Maximum: " + maximum);
        
        // Calling static method of Calculator
        double sum = Calculator.add(10.5, 20.3);
        System.out.println("Sum: " + sum);
    }
}
```

---

## 4. Static Classes {#static-classes}

In Java, only nested classes can be static. Static nested classes don't need a reference to the outer class.

### Characteristics:
- Can only be applied to nested classes
- Cannot access non-static members of outer class
- Can be instantiated without creating outer class instance

### Example Code:

```java
class OuterClass {
    private static String staticOuterField = "Static Outer Field";
    private String nonStaticOuterField = "Non-Static Outer Field";
    
    // Static nested class
    static class StaticNestedClass {
        private String nestedField = "Nested Field";
        
        public void display() {
            // Can access static members of outer class
            System.out.println("Accessing: " + staticOuterField);
            
            // Cannot access non-static members directly
            // System.out.println(nonStaticOuterField); // Compilation error
        }
        
        public static void staticMethod() {
            System.out.println("Static method in static nested class");
        }
    }
    
    // Non-static nested class (Inner class)
    class InnerClass {
        public void display() {
            // Can access both static and non-static members
            System.out.println("Static field: " + staticOuterField);
            System.out.println("Non-static field: " + nonStaticOuterField);
        }
    }
    
    public void createInnerClass() {
        InnerClass inner = new InnerClass();
        inner.display();
    }
}

public class StaticClassExample {
    public static void main(String[] args) {
        // Creating static nested class instance
        // No need to create outer class instance
        OuterClass.StaticNestedClass staticNested = new OuterClass.StaticNestedClass();
        staticNested.display();
        
        // Calling static method of static nested class
        OuterClass.StaticNestedClass.staticMethod();
        
        // For inner class, we need outer class instance
        OuterClass outer = new OuterClass();
        outer.createInnerClass();
        
        // Alternative way to create inner class
        OuterClass.InnerClass inner = outer.new InnerClass();
        inner.display();
    }
}
```

---

## 5. Hierarchy Flow {#hierarchy-flow}

### Class Loading and Static Member Initialization Flow:

```
1. JVM loads the class file
2. Static variables are allocated memory in Method Area
3. Static blocks are executed in order of appearance
4. Static variables are initialized with default values first, then with specified values
5. Main method is called (if present)
6. Instance creation follows normal object lifecycle
```

### Example demonstrating the flow:

```java
class Parent {
    static {
        System.out.println("1. Parent static block");
    }
    
    private static String parentStaticField = initParentStaticField();
    
    private static String initParentStaticField() {
        System.out.println("2. Parent static field initialization");
        return "Parent Static Field";
    }
    
    {
        System.out.println("5. Parent instance block");
    }
    
    public Parent() {
        System.out.println("6. Parent constructor");
    }
}

class Child extends Parent {
    static {
        System.out.println("3. Child static block");
    }
    
    private static String childStaticField = initChildStaticField();
    
    private static String initChildStaticField() {
        System.out.println("4. Child static field initialization");
        return "Child Static Field";
    }
    
    {
        System.out.println("7. Child instance block");
    }
    
    public Child() {
        System.out.println("8. Child constructor");
    }
}

public class HierarchyFlowExample {
    public static void main(String[] args) {
        System.out.println("9. Main method started");
        Child child = new Child();
        System.out.println("10. Object creation completed");
    }
}
```

**Output:**
```
1. Parent static block
2. Parent static field initialization
3. Child static block
4. Child static field initialization
9. Main method started
5. Parent instance block
6. Parent constructor
7. Child instance block
8. Child constructor
10. Object creation completed
```

---

## 6. Interview Questions {#interview-questions}

### Basic Level Questions:

**Q1: What is the static keyword in Java?**
A: The static keyword is used to create class-level variables and methods that belong to the class rather than instances. Static members are shared among all instances and can be accessed without creating objects.

**Q2: Can we override static methods?**
A: No, static methods cannot be overridden. They can be hidden by declaring a method with the same signature in the subclass, but it's not overriding.

**Q3: Can we access non-static variables from static methods?**
A: No, static methods cannot directly access non-static variables because static methods belong to the class and are loaded before any instance is created.

### Intermediate Level Questions:

**Q4: What happens if we don't initialize a static variable?**
A: Static variables get default values based on their type (0 for int, null for objects, false for boolean, etc.).

**Q5: Can we have static constructors in Java?**
A: No, Java doesn't have static constructors. We use static blocks for static initialization.

**Q6: What is the difference between static and instance variables?**
```java
class Example {
    static int staticVar = 10;    // Class variable - one copy
    int instanceVar = 20;         // Instance variable - copy per object
}
```

### Advanced Level Questions:

**Q7: Can we serialize static variables?**
A: No, static variables are not serialized because they belong to the class, not to instances.

**Q8: What happens to static variables during inheritance?**
```java
class Parent {
    static int count = 0;
}

class Child extends Parent {
    // Child shares the same static variable 'count' with Parent
}
```

**Q9: Can we access static members using null reference?**
```java
public class Test {
    static void staticMethod() {
        System.out.println("Static method called");
    }
    
    public static void main(String[] args) {
        Test obj = null;
        obj.staticMethod(); // This works! No NullPointerException
    }
}
```

---

## 7. Common Mistakes (Wrong vs Right Code) {#common-mistakes}

### Mistake 1: Accessing non-static members from static context

**❌ Wrong Code:**
```java
class WrongExample {
    private int instanceVar = 10;
    
    public static void staticMethod() {
        System.out.println(instanceVar); // Compilation Error!
    }
}
```

**✅ Right Code:**
```java
class RightExample {
    private int instanceVar = 10;
    
    public static void staticMethod() {
        RightExample obj = new RightExample();
        System.out.println(obj.instanceVar); // Correct!
    }
}
```

### Mistake 2: Using this keyword in static method

**❌ Wrong Code:**
```java
class WrongExample {
    private static int staticVar = 10;
    
    public static void staticMethod() {
        System.out.println(this.staticVar); // Compilation Error!
    }
}
```

**✅ Right Code:**
```java
class RightExample {
    private static int staticVar = 10;
    
    public static void staticMethod() {
        System.out.println(staticVar); // Correct!
        // OR
        System.out.println(RightExample.staticVar); // Also correct!
    }
}
```

### Mistake 3: Trying to override static methods

**❌ Wrong Understanding:**
```java
class Parent {
    public static void staticMethod() {
        System.out.println("Parent static method");
    }
}

class Child extends Parent {
    // This is method hiding, not overriding!
    public static void staticMethod() {
        System.out.println("Child static method");
    }
}
```

**✅ Right Understanding:**
```java
public class StaticMethodTest {
    public static void main(String[] args) {
        Parent.staticMethod(); // Output: Parent static method
        Child.staticMethod();  // Output: Child static method
        
        Parent p = new Child();
        p.staticMethod(); // Output: Parent static method (not Child!)
    }
}
```

### Mistake 4: Static block execution order

**❌ Wrong Understanding:**
```java
class WrongOrder {
    public WrongOrder() {
        System.out.println("Constructor");
    }
    
    {
        System.out.println("Instance block");
    }
    
    static {
        System.out.println("Static block");
    }
}

// Output will be:
// Static block
// Instance block  
// Constructor
```

### Mistake 5: Static import confusion

**❌ Wrong Code:**
```java
import static java.lang.System.*; // Importing all static members

public class StaticImportWrong {
    public static void main(String[] args) {
        // This creates confusion
        out.println("Hello"); // Which 'out' is this?
    }
}
```

**✅ Right Code:**
```java
import static java.lang.System.out; // Import specific static member

public class StaticImportRight {
    public static void main(String[] args) {
        out.println("Hello World!"); // Clear and specific
    }
}
```

### Mistake 6: Memory leak with static variables

**❌ Potential Memory Leak:**
```java
class DataCache {
    private static List<String> cache = new ArrayList<>();
    
    public static void addData(String data) {
        cache.add(data); // Data never gets removed!
    }
}
```

**✅ Better Approach:**
```java
class DataCache {
    private static Map<String, String> cache = new LinkedHashMap<String, String>() {
        @Override
        protected boolean removeEldestEntry(Map.Entry<String, String> eldest) {
            return size() > 1000; // Limit cache size
        }
    };
    
    public static void addData(String key, String data) {
        cache.put(key, data);
    }
    
    public static void clearCache() {
        cache.clear(); // Provide way to clear cache
    }
}
```

---

## Summary

The static keyword is fundamental in Java programming. Key takeaways:

1. **Static Variables**: Shared among all instances, initialized once
2. **Static Blocks**: Execute during class loading, used for complex static initialization
3. **Static Methods**: Belong to class, cannot access instance members directly
4. **Static Classes**: Only nested classes can be static, don't need outer class reference
5. **Hierarchy Flow**: Static members are initialized during class loading in inheritance hierarchy
6. **Best Practices**: Use static judiciously, avoid memory leaks, understand method hiding vs overriding

Remember: Static members are powerful but should be used carefully to avoid tight coupling and memory issues.
