# Java Core Interview Questions - 200 Questions

## 🏗️ **OBJECT-ORIENTED PROGRAMMING (OOP) - 60 Questions**

### **Classes & Objects (15 Questions)**
1. Create a class with private fields and public getter/setter methods
2. Implement constructor overloading with different parameter combinations
3. Demonstrate constructor chaining using this() and super()
4. Create static vs non-static methods and explain memory allocation
5. Implement a singleton class with thread-safe lazy initialization
6. Design a class with static blocks and instance blocks execution order
7. Create immutable class with all best practices
8. Implement builder pattern for complex object creation
9. Demonstrate object cloning (shallow vs deep copy)
10. Create factory pattern for object creation
11. Implement class with finalize() method and garbage collection
12. Design class with static nested class and inner class
13. Create anonymous class and lambda expression comparison
14. Implement class with multiple constructors and default values
15. Design class hierarchy with proper encapsulation

### **Inheritance (15 Questions)**
16. Create multi-level inheritance with method overriding
17. Implement method hiding vs method overriding
18. Demonstrate super keyword usage in inheritance
19. Create abstract class with abstract and concrete methods
20. Implement template method pattern using inheritance
21. Design class hierarchy with protected access modifier
22. Create diamond problem scenario and solution
23. Implement inheritance with constructor chaining
24. Design is-a vs has-a relationship examples
25. Create covariant return types in method overriding
26. Implement inheritance with static methods
27. Design class hierarchy with final classes and methods
28. Create inheritance with interface implementation
29. Implement method overriding with different access modifiers
30. Design inheritance tree with proper abstraction levels

### **Polymorphism (15 Questions)**
31. Implement runtime polymorphism with method overriding
32. Create compile-time polymorphism with method overloading
33. Demonstrate dynamic method dispatch
34. Implement polymorphism with interfaces
35. Create operator overloading simulation in Java
36. Design polymorphic behavior with abstract classes
37. Implement visitor pattern using polymorphism
38. Create strategy pattern with polymorphic behavior
39. Demonstrate upcasting and downcasting with instanceof
40. Implement polymorphism with generic types
41. Create command pattern using polymorphism
42. Design state pattern with polymorphic state changes
43. Implement observer pattern with polymorphic notifications
44. Create factory method pattern with polymorphic products
45. Design adapter pattern using polymorphism

### **Encapsulation & Abstraction (15 Questions)**
46. Create class with proper data hiding and access control
47. Implement package-private access modifier scenarios
48. Design abstract class vs interface comparison
49. Create encapsulation with validation in setters
50. Implement abstraction using interfaces and abstract classes
51. Design data transfer object (DTO) with encapsulation
52. Create class with read-only and write-only properties
53. Implement encapsulation with inner classes
54. Design abstraction layers for database operations
55. Create encapsulation with enum types
56. Implement abstraction with functional interfaces
57. Design encapsulation with immutable collections
58. Create abstraction with generic bounded types
59. Implement encapsulation with proxy pattern
60. Design abstraction with dependency injection

---

## 🔧 **JAVA LANGUAGE CONSTRUCTS - 80 Questions**

### **Variables & Data Types (20 Questions)**
1. Demonstrate primitive vs reference data types
2. Create examples of autoboxing and unboxing
3. Implement variable scope (local, instance, static)
4. Show integer overflow and underflow scenarios
5. Create floating-point precision issues and solutions
6. Demonstrate string immutability with examples
7. Implement string pool vs heap memory allocation
8. Create examples of type casting (implicit vs explicit)
9. Show final variables with different contexts
10. Implement static variables and their lifecycle
11. Create examples of variable shadowing
12. Demonstrate primitive wrapper classes usage
13. Show default values for different data types
14. Create examples of constant declaration best practices
15. Implement variable initialization order
16. Show differences between String, StringBuilder, StringBuffer
17. Create examples of array declaration and initialization
18. Demonstrate multidimensional array operations
19. Show var keyword usage in local variable type inference
20. Create examples of numeric literal formats

### **Control Flow Statements (20 Questions)**
21. Implement nested if-else statements with complex conditions
22. Create switch statement with enum and string cases
23. Demonstrate enhanced switch expressions (Java 12+)
24. Implement for loop variations (traditional, enhanced, infinite)
25. Create while vs do-while loop scenarios
26. Show break and continue with labeled statements
27. Implement nested loops with proper control flow
28. Create early return patterns vs nested if statements
29. Demonstrate ternary operator vs if-else performance
30. Implement loop optimization techniques
31. Create examples of unreachable code scenarios
32. Show goto simulation using break labels
33. Implement state machine using switch statements
34. Create complex boolean expressions with short-circuiting
35. Demonstrate switch fall-through behavior
36. Implement loop-and-a-half patterns
37. Create examples of iterator pattern with loops
38. Show for-each loop with different collection types
39. Implement recursive vs iterative solutions
40. Create examples of loop invariants

### **Methods & Parameters (20 Questions)**
41. Implement method overloading with different parameter types
42. Create varargs methods with proper usage
43. Demonstrate pass-by-value vs pass-by-reference simulation
44. Implement recursive methods with base cases
45. Create method chaining (fluent interface) pattern
46. Show method hiding vs method overriding
47. Implement static methods and their restrictions
48. Create generic methods with bounded type parameters
49. Demonstrate method parameter validation
50. Implement default methods in interfaces
51. Create functional methods with lambda expressions
52. Show method references (static, instance, constructor)
53. Implement tail recursion optimization
54. Create methods with multiple return points
55. Demonstrate method signature rules and conflicts
56. Implement callback methods using functional interfaces
57. Create utility methods with proper design
58. Show method overloading with autoboxing conflicts
59. Implement method parameter object pattern
60. Create methods with optional parameters simulation

### **Packages & Access Modifiers (20 Questions)**
61. Create package structure with proper naming conventions
62. Implement public, private, protected, package-private access
63. Demonstrate import statements (static, wildcard, specific)
64. Create package-private classes and interfaces
65. Show classpath and package resolution
66. Implement access modifier inheritance rules
67. Create sealed classes and permitted subclasses
68. Demonstrate module system basics (Java 9+)
69. Show package versioning and conflicts
70. Implement access control with inner classes
71. Create package documentation with javadoc
72. Show dependency management between packages
73. Implement package-private constructors
74. Create access modifier best practices
75. Demonstrate protected access across packages
76. Implement package structure for layered architecture
77. Create package naming conventions for organizations
78. Show package circular dependency issues
79. Implement access control with reflection
80. Create package-private utility classes

---

## ⚠️ **EXCEPTION HANDLING - 60 Questions**

### **Basic Exception Handling (15 Questions)**
1. Create try-catch-finally blocks with proper resource cleanup
2. Implement multiple catch blocks with exception hierarchy
3. Demonstrate checked vs unchecked exceptions
4. Create custom exception classes with proper inheritance
5. Show exception propagation through method calls
6. Implement exception handling in constructor
7. Create exception handling with return statements
8. Demonstrate exception handling in static blocks
9. Show exception handling with inheritance overriding
10. Implement exception handling with threads
11. Create exception chaining with cause
12. Show exception handling with generics
13. Implement exception handling with lambda expressions
14. Create exception handling with streams
15. Demonstrate exception handling best practices

### **Try-with-Resources (15 Questions)**
16. Implement try-with-resources for file operations
17. Create custom resource classes implementing AutoCloseable
18. Show multiple resources in try-with-resources
19. Demonstrate exception suppression in try-with-resources
20. Implement try-with-resources with inheritance
21. Create try-with-resources with lambda expressions
22. Show try-with-resources vs traditional try-finally
23. Implement try-with-resources with custom exceptions
24. Create nested try-with-resources scenarios
25. Show try-with-resources with database connections
26. Implement try-with-resources with network operations
27. Create try-with-resources with concurrent operations
28. Show try-with-resources with generic types
29. Implement try-with-resources with factory methods
30. Create try-with-resources performance considerations

### **Custom Exceptions (15 Questions)**
31. Create business logic specific exception classes
32. Implement exception class hierarchy for application layers
33. Create exception classes with additional fields and methods
34. Show exception translation between layers
35. Implement exception classes with internationalization
36. Create exception classes with error codes
37. Show exception classes with logging integration
38. Implement exception classes with stack trace customization
39. Create exception classes with retry mechanisms
40. Show exception classes with configuration
41. Implement exception classes with metrics collection
42. Create exception classes with context information
43. Show exception classes with validation
44. Implement exception classes with recovery strategies
45. Create exception classes with notification systems

### **Exception Handling Patterns (15 Questions)**
46. Implement exception handling with template method pattern
47. Create exception handling with chain of responsibility
48. Show exception handling with observer pattern
49. Implement exception handling with strategy pattern
50. Create exception handling with decorator pattern
51. Show exception handling with proxy pattern
52. Implement exception handling with factory pattern
53. Create exception handling with builder pattern
54. Show exception handling with singleton pattern
55. Implement exception handling with adapter pattern
56. Create exception handling with facade pattern
57. Show exception handling with command pattern
58. Implement exception handling with state pattern
59. Create exception handling with visitor pattern
60. Show exception handling with mediator pattern

---

## 🎯 **DIFFICULTY LEVELS**

### **⭐ BEGINNER (Questions 1-33% in each category)**
- Basic syntax and concepts
- Simple implementations
- Understanding fundamentals
- **Time**: 10-15 minutes per question

### **⭐⭐ INTERMEDIATE (Questions 34-66% in each category)**
- Combining multiple concepts
- Design patterns application
- Performance considerations
- **Time**: 15-25 minutes per question

### **⭐⭐⭐ ADVANCED (Questions 67-100% in each category)**
- Complex design scenarios
- Enterprise patterns
- Optimization techniques
- **Time**: 25-40 minutes per question

---

## 📚 **STUDY PLAN - 6 WEEKS**

### **Week 1: Classes, Objects & Inheritance**
- **Day 1-2**: Classes & Objects (Questions 1-15)
- **Day 3-4**: Inheritance (Questions 16-30)
- **Day 5-6**: Practice and Review
- **Day 7**: Mock Interview on OOP basics

### **Week 2: Polymorphism & Encapsulation**
- **D
