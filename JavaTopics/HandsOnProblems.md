# Java Interview Hands-On Coding Problems

## 1. Object-Oriented Programming (OOP)

### Classes & Objects Problems

1. **Bank Account System**
   - Create a BankAccount class with balance, accountNumber, accountHolderName
   - Implement deposit(), withdraw(), getBalance(), displayAccountInfo() methods
   - Create multiple account objects and perform operations

2. **Car Management System**
   - Create Car class with brand, model, year, price
   - Implement methods: startEngine(), stopEngine(), displayDetails()
   - Create array of cars and find most expensive car

3. **Student Grade Calculator**
   - Create Student class with name, rollNumber, marks array
   - Implement calculateGrade(), displayStudentInfo() methods
   - Create multiple students and find topper

### Constructor Problems

4. **Employee Management with Different Constructors**
   - Create Employee class with name, id, salary, department
   - Implement: default constructor, parameterized constructor, copy constructor
   - Demonstrate constructor overloading with 4 different constructors

5. **Rectangle Constructor Challenge**
   - Create Rectangle class with length, width
   - Default constructor (sets 1,1), parameterized constructor, square constructor (one parameter)
   - Implement area(), perimeter(), isSquare() methods

6. **Book Library System**
   - Create Book class with title, author, ISBN, price
   - Multiple constructors for different initialization scenarios
   - Implement displayBook(), comparePrice() methods

### this Keyword Problems

7. **Chain Constructor Calls**
   - Create Person class demonstrating constructor chaining using this()
   - Implement method chaining pattern using this keyword
   - Show this as method parameter passing

8. **Builder Pattern Implementation**
   - Create Computer class with CPU, RAM, storage, GPU
   - Implement builder pattern using this keyword
   - Chain multiple method calls

### Inheritance Problems

9. **Vehicle Hierarchy**
   - Create Vehicle base class with brand, year, price
   - Create Car, Motorcycle, Truck subclasses
   - Implement specific methods for each vehicle type
   - Create array of vehicles and calculate total value

10. **Shape Inheritance System**
    - Create Shape base class with color, filled properties
    - Create Circle, Rectangle, Triangle subclasses
    - Implement area(), perimeter() methods in each
    - Create shape array and find largest area

11. **Employee Inheritance**
    - Create Employee base class
    - Create Manager, Developer, Tester subclasses
    - Implement salary calculation for each type
    - Calculate total company payroll

### Method Overloading vs Overriding

12. **Calculator Overloading**
    - Create Calculator class with overloaded add() methods
    - Support: int, double, String, array parameters
    - Implement multiply(), divide() with different parameter types

13. **Animal Overriding System**
    - Create Animal base class with makeSound() method
    - Create Dog, Cat, Bird subclasses overriding makeSound()
    - Implement feeding behavior differently in each

14. **Math Operations**
    - Create MathUtils class with overloaded max() methods
    - Support finding max of 2, 3, 4 numbers and arrays
    - Override toString() in custom classes

### super Keyword Problems

15. **University System**
    - Create Person class with name, age
    - Create Student extending Person with studentId, course
    - Create GraduateStudent extending Student with thesis topic
    - Use super in constructors and methods

16. **Vehicle Registration**
    - Create Vehicle with registrationNumber, owner
    - Create Car extending Vehicle with numberOfDoors
    - Use super to access parent methods and constructors

### Access Modifiers Problems

17. **Bank Security System**
    - Create BankAccount with private balance
    - Implement public methods for operations
    - Create package-private helper methods
    - Demonstrate protected inheritance access

18. **Library Management**
    - Create Book class with different access levels
    - Implement borrowing system with proper encapsulation
    - Create different packages to test access levels

### Encapsulation Problems

19. **Student Information System**
    - Create Student class with private fields
    - Implement getter/setter methods with validation
    - Age should be between 16-60, marks between 0-100

20. **Product Inventory**
    - Create Product class with private price, quantity
    - Implement proper getter/setters with business logic
    - Prevent negative values, implement discount logic

### Abstraction Problems

21. **Payment Processing System**
    - Create abstract Payment class with processPayment() method
    - Implement CreditCard, DebitCard, PayPal classes
    - Create payment processor that handles different payment types

22. **Database Connection**
    - Create abstract Database class with connect(), disconnect() methods
    - Implement MySQL, Oracle, MongoDB concrete classes
    - Create connection manager

### Polymorphism Problems

23. **Animal Farm Simulation**
    - Create Animal base class with eat(), sleep(), makeSound()
    - Create multiple animal types
    - Create farm with animal array demonstrating runtime polymorphism

24. **Media Player**
    - Create Media abstract class with play(), stop(), pause()
    - Implement Audio, Video, Podcast classes
    - Create playlist that can handle different media types

## 2. Java Language Constructs

### Static Keyword Problems

25. **Student Counter**
    - Create Student class with static counter for total students
    - Implement static methods to get student count
    - Create utility methods using static

26. **Math Utility Class**
    - Create MathUtils with static methods for common calculations
    - Implement static variables for mathematical constants
    - Create static block for initialization

27. **Singleton Pattern**
    - Implement Database connection using singleton pattern
    - Use static keyword appropriately
    - Handle thread safety

### Final Keyword Problems

28. **Constants Class**
    - Create Constants class with final variables
    - Implement final methods that cannot be overridden
    - Create final class that cannot be extended

29. **Configuration Manager**
    - Create final configuration values
    - Implement final methods for security
    - Demonstrate final with collections

### instanceof Operator Problems

30. **Type Checking System**
    - Create vehicle hierarchy
    - Implement method that processes different vehicle types using instanceof
    - Handle type-safe operations

31. **Object Processor**
    - Create method that accepts Object parameter
    - Use instanceof to process String, Integer, Double differently
    - Implement safe type casting

### Nested & Inner Classes Problems

32. **Outer-Inner Communication**
    - Create outer class with private members
    - Create inner class that accesses outer class members
    - Implement static nested class

33. **Data Structure Implementation**
    - Create LinkedList class with Node as inner class
    - Implement add, remove, display methods
    - Use inner class for encapsulation

### Anonymous Classes Problems

34. **Event Handling Simulation**
    - Create interface with event handling methods
    - Implement using anonymous classes
    - Compare with lambda expressions

35. **Comparator Implementation**
    - Create list of custom objects
    - Sort using anonymous Comparator classes
    - Implement multiple sorting criteria

### Enum Problems

36. **Day of Week Operations**
    - Create Day enum with weekday/weekend identification
    - Implement methods in enum
    - Create schedule manager using enum

37. **Priority System**
    - Create Priority enum (LOW, MEDIUM, HIGH, CRITICAL)
    - Implement task management system
    - Use enum in switch statements

## 3. Packages and Imports

### Package Problems

38. **Multi-Package Project**
    - Create com.company.employee package with Employee class
    - Create com.company.payroll package with PayrollCalculator
    - Implement cross-package communication

39. **Utility Package System**
    - Create com.utils.math with mathematical utilities
    - Create com.utils.string with string utilities
    - Create main application using both packages

## 4. Exception Handling

### Basic Exception Problems

40. **Division Calculator**
    - Create calculator that handles ArithmeticException
    - Implement try-catch-finally blocks
    - Handle multiple exception types

41. **File Reader System**
    - Create file reading application
    - Handle FileNotFoundException, IOException
    - Implement proper resource cleanup

42. **Array Operations**
    - Create array utility class
    - Handle ArrayIndexOutOfBoundsException
    - Implement safe array operations

### Custom Exception Problems

43. **Banking Exception System**
    - Create InsufficientFundsException
    - Create InvalidAccountException
    - Implement exception hierarchy

44. **Age Validation System**
    - Create InvalidAgeException
    - Implement age validator with custom exceptions
    - Create exception with error codes

### Exception Hierarchy Problems

45. **Exception Chain Demonstration**
    - Create method that catches one exception and throws another
    - Implement exception wrapping
    - Demonstrate exception cause tracking

46. **Multi-Level Exception Handling**
    - Create nested method calls with different exception types
    - Implement proper exception propagation
    - Handle both checked and unchecked exceptions

## 5. Advanced Practice Problems

### Comprehensive System Design

47. **Library Management System**
    - Implement complete library system using all OOP concepts
    - Include Book, Member, Librarian classes
    - Handle book borrowing, returning, fine calculation

48. **Online Shopping Cart**
    - Create Product, Customer, Order, ShoppingCart classes
    - Implement inheritance for different product types
    - Handle exceptions for inventory, payment processing

49. **Hotel Reservation System**
    - Create Room, Guest, Reservation classes
    - Implement polymorphism for different room types
    - Handle booking exceptions and validation

50. **Employee Management System**
    - Create comprehensive employee hierarchy
    - Implement salary calculation, performance evaluation
    - Use all OOP principles and exception handling

## Challenge Problems for Advanced Interview

51. **Design Pattern Implementation**
    - Implement Factory Pattern for creating different objects
    - Create Observer Pattern for event notification
    - Use Strategy Pattern for different algorithms

52. **Collection Framework Integration**
    - Create custom classes that work with Java Collections
    - Implement Comparable and Comparator interfaces
    - Handle generic types properly

53. **Thread-Safe Implementations**
    - Create thread-safe singleton
    - Implement synchronized methods in banking system
    - Handle concurrent access to shared resources

54. **Reflection-Based Problems**
    - Create class that can instantiate objects dynamically
    - Implement method invocation using reflection
    - Handle reflection exceptions properly

55. **Annotation Processing**
    - Create custom annotations
    - Implement annotation processing logic
    - Use annotations for validation

## Tips for Practice:
- Start with basic problems and gradually increase complexity
- Focus on clean, readable code
- Handle edge cases and exceptions
- Practice explaining your code logic
- Time yourself while solving problems
- Create test cases for your solutions
