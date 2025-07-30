# SOLID Principles - Complete Guide

## Table of Contents
1. [Introduction to SOLID Principles](#introduction)
2. [S - Single Responsibility Principle (SRP)](#single-responsibility-principle)
3. [O - Open/Closed Principle (OCP)](#open-closed-principle)
4. [L - Liskov Substitution Principle (LSP)](#liskov-substitution-principle)
5. [I - Interface Segregation Principle (ISP)](#interface-segregation-principle)
6. [D - Dependency Inversion Principle (DIP)](#dependency-inversion-principle)
7. [Benefits of SOLID Principles](#benefits)
8. [Interview Questions](#interview-questions)
9. [Common Violations (Wrong vs Right Code)](#common-violations)
10. [Real-World Examples](#real-world-examples)

---

## Introduction to SOLID Principles {#introduction}

SOLID is an acronym for five design principles intended to make software designs more understandable, flexible, and maintainable. These principles were introduced by Robert C. Martin (Uncle Bob).

**SOLID Stands For:**
- **S** - Single Responsibility Principle (SRP)
- **O** - Open/Closed Principle (OCP)
- **L** - Liskov Substitution Principle (LSP)
- **I** - Interface Segregation Principle (ISP)
- **D** - Dependency Inversion Principle (DIP)

**Why SOLID Principles Matter:**
- Reduce code complexity
- Increase code readability and maintainability
- Make software more flexible and extensible
- Reduce coupling between components
- Improve testability

---

## 1. Single Responsibility Principle (SRP) {#single-responsibility-principle}

**Definition:** A class should have only one reason to change. It should have only one job or responsibility.

### Key Points:
- Each class should focus on a single functionality
- High cohesion within the class
- Easier to understand, test, and maintain
- Reduces the impact of changes

### ❌ Wrong Code (SRP Violation):

```java
// BAD: This Book class does TOO MANY things!
class Book {
    private String title;
    private String author;
    private int pages;
    
    public Book(String title, String author, int pages) {
        this.title = title;
        this.author = author;
        this.pages = pages;
    }
    
    // Job 1: Store book data
    public String getTitle() { return title; }
    public String getAuthor() { return author; }
    public int getPages() { return pages; }
    
    // Job 2: Print the book
    public void printBook() {
        System.out.println("Printing: " + title);
        // Printing logic here
    }
    
    // Job 3: Save to database
    public void saveToDatabase() {
        System.out.println("Saving " + title + " to database");
        // Database logic here
    }
    
    // Job 4: Send email notification
    public void sendEmailNotification() {
        System.out.println("Sending email about " + title);
        // Email logic here
    }
}

// Problem: If printing logic changes, we modify Book class
// If database changes, we modify Book class
// If email changes, we modify Book class
// Book class has too many reasons to change!
```

### ✅ Right Code (Following SRP):

```java
// GOOD: Each class has ONE job only!

// Job 1: Only store book data
class Book {
    private String title;
    private String author;
    private int pages;
    
    public Book(String title, String author, int pages) {
        this.title = title;
        this.author = author;
        this.pages = pages;
    }
    
    public String getTitle() { return title; }
    public String getAuthor() { return author; }
    public int getPages() { return pages; }
}

// Job 2: Only handle printing
class BookPrinter {
    public void print(Book book) {
        System.out.println("Printing: " + book.getTitle());
        System.out.println("Author: " + book.getAuthor());
        // Only printing logic here
    }
}

// Job 3: Only handle database operations
class BookDatabase {
    public void save(Book book) {
        System.out.println("Saving " + book.getTitle() + " to database");
        // Only database logic here
    }
    
    public Book findByTitle(String title) {
        System.out.println("Finding book: " + title);
        return null; // Database search logic here
    }
}

// Job 4: Only handle email notifications
class BookEmailNotifier {
    public void sendNotification(Book book) {
        System.out.println("Sending email about new book: " + book.getTitle());
        // Only email logic here
    }
}

// Usage Example
public class SRPExample {
    public static void main(String[] args) {
        Book book = new Book("Java Basics", "John Doe", 300);
        
        BookPrinter printer = new BookPrinter();
        BookDatabase database = new BookDatabase();
        BookEmailNotifier emailer = new BookEmailNotifier();
        
        printer.print(book);          // Print the book
        database.save(book);          // Save to database
        emailer.sendNotification(book); // Send email
    }
}

// Benefits:
// - If printing changes, only BookPrinter changes
// - If database changes, only BookDatabase changes
// - If email changes, only BookEmailNotifier changes
// - Easy to test each class separately
```

---

## 2. Open/Closed Principle (OCP) {#open-closed-principle}

**Definition:** Software entities should be open for extension but closed for modification. You should be able to add new functionality without changing existing code.

### Key Points:
- Open for extension: New behavior can be added
- Closed for modification: Existing code remains unchanged
- Use abstraction and polymorphism
- Prevents breaking existing functionality

### ❌ Wrong Code (OCP Violation):

```java
// BAD: Adding new animal requires changing existing code
class AnimalSound {
    public void makeSound(String animalType) {
        if (animalType.equals("Dog")) {
            System.out.println("Woof! Woof!");
        } else if (animalType.equals("Cat")) {
            System.out.println("Meow! Meow!");
        }
        // Problem: If I want to add "Cow", I must modify this method!
        // else if (animalType.equals("Cow")) {
        //     System.out.println("Moo! Moo!");
        // }
    }
}

// Usage
public class BadExample {
    public static void main(String[] args) {
        AnimalSound sound = new AnimalSound();
        sound.makeSound("Dog");  // Woof! Woof!
        sound.makeSound("Cat");  // Meow! Meow!
        // To add Cow, we must change AnimalSound class!
    }
}
```

### ✅ Right Code (Following OCP):

```java
// GOOD: We can add new animals without changing existing code

// Step 1: Create a common interface
interface Animal {
    void makeSound();
}

// Step 2: Each animal implements the interface
class Dog implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof! Woof!");
    }
}

class Cat implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow! Meow!");
    }
}

// Step 3: Adding new animal is easy - no existing code changes!
class Cow implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Moo! Moo!");
    }
}

// Step 4: Animal sound manager stays the same
class AnimalSoundManager {
    public void playSound(Animal animal) {
        animal.makeSound(); // Works for any animal!
    }
}

// Usage Example
public class GoodExample {
    public static void main(String[] args) {
        AnimalSoundManager manager = new AnimalSoundManager();
        
        Animal dog = new Dog();
        Animal cat = new Cat();
        Animal cow = new Cow(); // New animal added easily!
        
        manager.playSound(dog);  // Woof! Woof!
        manager.playSound(cat);  // Meow! Meow!
        manager.playSound(cow);  // Moo! Moo!
        
        // To add Horse, just create Horse class implementing Animal
        // No need to modify AnimalSoundManager!
    }
}

// Benefits:
// - Add new animals without changing existing code
// - Each animal is responsible for its own sound
// - Easy to test and maintain
```

---

## 3. Liskov Substitution Principle (LSP) {#liskov-substitution-principle}

**Definition:** Objects of a superclass should be replaceable with objects of a subclass without breaking the application. Derived classes must be substitutable for their base classes.

### Key Points:
- Subclasses should enhance, not replace the functionality of base class
- Preconditions cannot be strengthened in subclasses
- Postconditions cannot be weakened in subclasses
- Maintain behavioral compatibility

### ❌ Wrong Code (LSP Violation):

```java
// BAD: Bird class that doesn't work for all birds
class Bird {
    public void fly() {
        System.out.println("Bird is flying");
    }
}

class Sparrow extends Bird {
    @Override
    public void fly() {
        System.out.println("Sparrow flies fast!");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        // Problem: Penguins can't fly!
        throw new RuntimeException("Penguin can't fly!");
    }
}

// This method expects all birds to fly
class BirdHandler {
    public void makeBirdFly(Bird bird) {
        bird.fly(); // This will crash for Penguin!
    }
}

// Usage - this will crash!
public class BadExample {
    public static void main(String[] args) {
        BirdHandler handler = new BirdHandler();
        
        Bird sparrow = new Sparrow();
        Bird penguin = new Penguin();
        
        handler.makeBirdFly(sparrow); // Works fine
        handler.makeBirdFly(penguin); // CRASH! Exception thrown
    }
}
```

### ✅ Right Code (Following LSP):

```java
// GOOD: Separate interfaces for different capabilities

// Base interface for all birds
interface Bird {
    void eat();
    void sleep();
}

// Separate interface for birds that can fly
interface FlyingBird extends Bird {
    void fly();
}

// Separate interface for birds that can swim
interface SwimmingBird extends Bird {
    void swim();
}

// Flying birds implement FlyingBird
class Sparrow implements FlyingBird {
    @Override
    public void eat() {
        System.out.println("Sparrow is eating seeds");
    }
    
    @Override
    public void sleep() {
        System.out.println("Sparrow is sleeping in nest");
    }
    
    @Override
    public void fly() {
        System.out.println("Sparrow flies fast!");
    }
}

// Swimming birds implement SwimmingBird
class Penguin implements SwimmingBird {
    @Override
    public void eat() {
        System.out.println("Penguin is eating fish");
    }
    
    @Override
    public void sleep() {
        System.out.println("Penguin is sleeping on ice");
    }
    
    @Override
    public void swim() {
        System.out.println("Penguin swims gracefully!");
    }
}

// Handlers work with appropriate interfaces
class BirdHandler {
    public void feedBird(Bird bird) {
        bird.eat(); // Works for any bird
    }
    
    public void makeFlyingBirdFly(FlyingBird bird) {
        bird.fly(); // Only works for flying birds
    }
    
    public void makeSwimmingBirdSwim(SwimmingBird bird) {
        bird.swim(); // Only works for swimming birds
    }
}

// Usage - no crashes!
public class GoodExample {
    public static void main(String[] args) {
        BirdHandler handler = new BirdHandler();
        
        Sparrow sparrow = new Sparrow();
        Penguin penguin = new Penguin();
        
        // Both can eat (common behavior)
        handler.feedBird(sparrow);
        handler.feedBird(penguin);
        
        // Only sparrow can fly
        handler.makeFlyingBirdFly(sparrow);
        
        // Only penguin can swim
        handler.makeSwimmingBirdSwim(penguin);
        
        // No crashes! Each bird does what it can do.
    }
}
```

---

## 4. Interface Segregation Principle (ISP) {#interface-segregation-principle}

**Definition:** No client should be forced to depend on methods it does not use. Create specific interfaces rather than one general-purpose interface.

### Key Points:
- Split large interfaces into smaller, more specific ones
- Clients should only know about methods they use
- Reduces coupling and increases flexibility
- Easier to maintain and test

### ❌ Wrong Code (ISP Violation):

```java
// BAD: One big interface with too many methods
interface SmartPhone {
    // Phone features
    void makeCall();
    void sendSMS();
    
    // Camera features
    void takePhoto();
    void recordVideo();
    
    // Music features
    void playMusic();
    void pauseMusic();
    
    // Game features
    void playGame();
    void pauseGame();
}

// Basic phone forced to implement all features
class BasicPhone implements SmartPhone {
    @Override
    public void makeCall() {
        System.out.println("Making call...");
    }
    
    @Override
    public void sendSMS() {
        System.out.println("Sending SMS...");
    }
    
    @Override
    public void takePhoto() {
        // Basic phone has no camera!
        throw new UnsupportedOperationException("No camera!");
    }
    
    @Override
    public void recordVideo() {
        // Basic phone has no camera!
        throw new UnsupportedOperationException("No camera!");
    }
    
    @Override
    public void playMusic() {
        // Basic phone has no music player!
        throw new UnsupportedOperationException("No music player!");
    }
    
    @Override
    public void pauseMusic() {
        // Basic phone has no music player!
        throw new UnsupportedOperationException("No music player!");
    }
    
    @Override
    public void playGame() {
        // Basic phone can't play games!
        throw new UnsupportedOperationException("No games!");
    }
    
    @Override
    public void pauseGame() {
        // Basic phone can't play games!
        throw new UnsupportedOperationException("No games!");
    }
}
```

### ✅ Right Code (Following ISP):

```java
// GOOD: Small, focused interfaces

// Basic phone interface
interface Phone {
    void makeCall();
    void sendSMS();
}

// Camera interface
interface Camera {
    void takePhoto();
    void recordVideo();
}

// Music player interface
interface MusicPlayer {
    void playMusic();
    void pauseMusic();
}

// Gaming interface
interface GameDevice {
    void playGame();
    void pauseGame();
}

// Basic phone implements only what it can do
class BasicPhone implements Phone {
    @Override
    public void makeCall() {
        System.out.println("Making call...");
    }
    
    @Override
    public void sendSMS() {
        System.out.println("Sending SMS...");
    }
}

// Camera phone implements phone + camera
class CameraPhone implements Phone, Camera {
    @Override
    public void makeCall() {
        System.out.println("Making call...");
    }
    
    @Override
    public void sendSMS() {
        System.out.println("Sending SMS...");
    }
    
    @Override
    public void takePhoto() {
        System.out.println("Taking photo...");
    }
    
    @Override
    public void recordVideo() {
        System.out.println("Recording video...");
    }
}

// Smart phone implements all features
class SmartPhone implements Phone, Camera, MusicPlayer, GameDevice {
    @Override
    public void makeCall() {
        System.out.println("Making call...");
    }
    
    @Override
    public void sendSMS() {
        System.out.println("Sending SMS...");
    }
    
    @Override
    public void takePhoto() {
        System.out.println("Taking photo...");
    }
    
    @Override
    public void recordVideo() {
        System.out.println("Recording video...");
    }
    
    @Override
    public void playMusic() {
        System.out.println("Playing music...");
    }
    
    @Override
    public void pauseMusic() {
        System.out.println("Pausing music...");
    }
    
    @Override
    public void playGame() {
        System.out.println("Playing game...");
    }
    
    @Override
    public void pauseGame() {
        System.out.println("Pausing game...");
    }
}

// Usage Example
public class ISPExample {
    public static void main(String[] args) {
        BasicPhone basic = new BasicPhone();
        CameraPhone camera = new CameraPhone();
        SmartPhone smart = new SmartPhone();
        
        // Each phone does only what it can do
        basic.makeCall();    // Works
        camera.takePhoto();  // Works
        smart.playMusic();   // Works
        
        // No exceptions thrown!
    }
}

// Benefits:
// - Each phone implements only what it needs
// - No UnsupportedOperationException
// - Easy to add new features
// - Clear separation of concerns
```

---

## 5. Dependency Inversion Principle (DIP) {#dependency-inversion-principle}

**Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions.

### Key Points:
- Depend on abstractions, not concretions
- High-level policy should not depend on low-level details
- Use dependency injection
- Increases flexibility and testability

### ❌ Wrong Code (DIP Violation):

```java
// BAD: Car directly depends on specific engine (tight coupling)
class PetrolEngine {
    public void start() {
        System.out.println("Petrol engine started");
    }
    
    public void stop() {
        System.out.println("Petrol engine stopped");
    }
}

class Car {
    private PetrolEngine engine; // Directly dependent on specific engine!
    
    public Car() {
        this.engine = new PetrolEngine(); // Hard-coded dependency!
    }
    
    public void startCar() {
        engine.start();
        System.out.println("Car is running");
    }
    
    public void stopCar() {
        engine.stop();
        System.out.println("Car stopped");
    }
}

// Problem: What if we want Diesel or Electric engine?
// We would need to modify Car class!

public class BadExample {
    public static void main(String[] args) {
        Car car = new Car(); // Always gets petrol engine
        car.startCar();
        
        // No way to use different engine without modifying Car class!
    }
}
```

### ✅ Right Code (Following DIP):

```java
// GOOD: Use interface (abstraction) instead of concrete class

// Step 1: Create interface (abstraction)
interface Engine {
    void start();
    void stop();
}

// Step 2: Different engines implement the interface
class PetrolEngine implements Engine {
    @Override
    public void start() {
        System.out.println("Petrol engine started");
    }
    
    @Override
    public void stop() {
        System.out.println("Petrol engine stopped");
    }
}

class DieselEngine implements Engine {
    @Override
    public void start() {
        System.out.println("Diesel engine started");
    }
    
    @Override
    public void stop() {
        System.out.println("Diesel engine stopped");
    }
}

class ElectricEngine implements Engine {
    @Override
    public void start() {
        System.out.println("Electric engine started silently");
    }
    
    @Override
    public void stop() {
        System.out.println("Electric engine stopped");
    }
}

// Step 3: Car depends on interface, not specific engine
class Car {
    private Engine engine; // Depends on abstraction!
    
    // Dependency injection through constructor
    public Car(Engine engine) {
        this.engine = engine;
    }
    
    public void startCar() {
        engine.start();
        System.out.println("Car is running");
    }
    
    public void stopCar() {
        engine.stop();
        System.out.println("Car stopped");
    }
}

// Usage Example
public class GoodExample {
    public static void main(String[] args) {
        // Can use any engine without changing Car class!
        Car petrolCar = new Car(new PetrolEngine());
        Car dieselCar = new Car(new DieselEngine());
        Car electricCar = new Car(new ElectricEngine());
        
        petrolCar.startCar();   // Uses petrol engine
        dieselCar.startCar();   // Uses diesel engine
        electricCar.startCar(); // Uses electric engine
        
        // Easy to add new engine types without changing Car!
    }
}

// Benefits:
// - Car class never needs to change
// - Easy to add new engine types
// - Easy to test with mock engines
// - Flexible and maintainable
```

---

## 6. Benefits of SOLID Principles {#benefits}

### 1. **Maintainability**
- Code is easier to understand and modify
- Changes in one part don't affect other parts
- Reduced risk of introducing bugs

### 2. **Flexibility**
- Easy to add new features
- Easy to change implementations
- Supports different configurations

### 3. **Testability**
- Dependencies can be easily mocked
- Each class has a single responsibility
- Isolated testing is possible

### 4. **Reusability**
- Small, focused classes can be reused
- Interfaces promote code reuse
- Composition over inheritance

### 5. **Scalability**
- Code can grow without becoming unmaintainable
- New team members can understand code faster
- Parallel development is easier

---

## 7. Interview Questions {#interview-questions}

### Basic Level Questions:

**Q1: What does SOLID stand for?**
A: SOLID stands for:
- **S** - Single Responsibility Principle
- **O** - Open/Closed Principle
- **L** - Liskov Substitution Principle
- **I** - Interface Segregation Principle
- **D** - Dependency Inversion Principle

**Q2: Explain Single Responsibility Principle with an example.**
A: SRP states that a class should have only one reason to change. For example, a `User` class should only handle user data, not database operations, email sending, or report generation.

**Q3: What is the difference between Open/Closed Principle and modification?**
A: OCP means classes should be open for extension (adding new features) but closed for modification (changing existing code). Use inheritance, interfaces, and polymorphism instead of modifying existing classes.

### Intermediate Level Questions:

**Q4: How does Liskov Substitution Principle relate to inheritance?**
A: LSP ensures that subclasses can replace their parent classes without breaking functionality. The classic Rectangle-Square problem violates LSP because Square changes Rectangle's behavior unexpectedly.

**Q5: Give an example of Interface Segregation Principle violation.**
A: A fat interface like `Worker` with methods `work()`, `eat()`, `sleep()`, `code()` violates ISP because a `RobotWorker` doesn't need `eat()` or `sleep()` methods.

**Q6: How does Dependency Inversion Principle improve testability?**
A: DIP allows you to inject mock implementations of dependencies during testing, making unit tests isolated and reliable.

### Advanced Level Questions:

**Q7: How do SOLID principles work together?**
A: They complement each other:
- SRP makes classes focused
- OCP makes them extensible
- LSP ensures proper inheritance
- ISP keeps interfaces lean
- DIP reduces coupling

**Q8: What are the trade-offs of following SOLID principles?**
A: 
- **Benefits**: Better maintainability, testability, flexibility
- **Trade-offs**: More classes, initial complexity, potential over-engineering

**Q9: How do you refactor legacy code to follow SOLID principles?**
A: 
1. Identify responsibilities (SRP)
2. Extract interfaces (ISP, DIP)
3. Use composition over inheritance (LSP)
4. Apply strategy pattern (OCP)
5. Inject dependencies (DIP)

---

## 8. Common Violations (Wrong vs Right Code) {#common-violations}

### Violation 1: God Class (SRP Violation)

**❌ Wrong:**
```java
class OrderProcessor {
    // Too many responsibilities!
    public void validateOrder(Order order) { }
    public void calculateTax(Order order) { }
    public void saveToDatabase(Order order) { }
    public void sendEmail(Order order) { }
    public void generateInvoice(Order order) { }
    public void updateInventory(Order order) { }
    public void processPayment(Order order) { }
}
```

**✅ Right:**
```java
class OrderValidator { public void validate(Order order) { } }
class TaxCalculator { public double calculate(Order order) { } }
class OrderRepository { public void save(Order order) { } }
class EmailService { public void sendOrderConfirmation(Order order) { } }
class InvoiceGenerator { public Invoice generate(Order order) { } }
class InventoryService { public void update(Order order) { } }
class PaymentProcessor { public void process(Order order) { } }
```

### Violation 2: Conditional Logic (OCP Violation)

**❌ Wrong:**
```java
class DiscountCalculator {
    public double calculate(Customer customer, double amount) {
        if (customer.getType().equals("REGULAR")) {
            return amount * 0.05;
        } else if (customer.getType().equals("PREMIUM")) {
            return amount * 0.10;
        } else if (customer.getType().equals("VIP")) {
            return amount * 0.15;
        }
        return 0;
    }
}
```

**✅ Right:**
```java
interface DiscountStrategy {
    double calculate(double amount);
}

class RegularCustomerDiscount implements DiscountStrategy {
    public double calculate(double amount) { return amount * 0.05; }
}

class PremiumCustomerDiscount implements DiscountStrategy {
    public double calculate(double amount) { return amount * 0.10; }
}

class VIPCustomerDiscount implements DiscountStrategy {
    public double calculate(double amount) { return amount * 0.15; }
}
```

---

## 9. Real-World Examples {#real-world-examples}

### Example 1: E-commerce System

```java
// Product management following SOLID principles

// SRP: Each class has single responsibility
class Product {
    private String id;
    private String name;
    private double price;
    // Only product data, no business logic
}

// OCP: Open for extension with new product types
abstract class ProductPriceCalculator {
    public abstract double calculatePrice(Product product);
}

class RegularPriceCalculator extends ProductPriceCalculator {
    public double calculatePrice(Product product) {
        return product.getPrice();
    }
}

class DiscountedPriceCalculator extends ProductPriceCalculator {
    private double discountPercentage;
    
    public double calculatePrice(Product product) {
        return product.getPrice() * (1 - discountPercentage);
    }
}

// ISP: Specific interfaces for different operations
interface ProductRepository {
    Product findById(String id);
    void save(Product product);
}

interface ProductSearchService {
    List<Product> searchByName(String name);
    List<Product> searchByCategory(String category);
}

// DIP: High-level modules depend on abstractions
class ProductService {
    private final ProductRepository repository;
    private final ProductPriceCalculator priceCalculator;
    
    public ProductService(ProductRepository repository, 
                         ProductPriceCalculator priceCalculator) {
        this.repository = repository;
        this.priceCalculator = priceCalculator;
    }
    
    public double getProductPrice(String productId) {
        Product product = repository.findById(productId);
        return priceCalculator.calculatePrice(product);
    }
}
```

### Example 2: Logging System

```java
// Logging system demonstrating all SOLID principles

// SRP: Logger only handles logging
interface Logger {
    void log(String message);
}

// OCP: Easy to add new log destinations
class FileLogger implements Logger {
    public void log(String message) {
        // Write to file
    }
}

class DatabaseLogger implements Logger {
    public void log(String message) {
        // Write to database
    }
}

class CloudLogger implements Logger {
    public void log(String message) {
        // Send to cloud service
    }
}

// ISP: Separate interfaces for different log levels
interface ErrorLogger {
    void logError(String message);
}

interface InfoLogger {
    void logInfo(String message);
}

interface DebugLogger {
    void logDebug(String message);
}

// DIP: Application depends on abstraction
class Application {
    private final Logger logger;
    
    public Application(Logger logger) {
        this.logger = logger;
    }
    
    public void doSomething() {
        logger.log("Application is doing something");
    }
}
```

---

## Summary

SOLID principles are fundamental guidelines for writing maintainable, flexible, and robust object-oriented code:

1. **SRP**: One class, one responsibility
2. **OCP**: Extend behavior without modifying existing code
3. **LSP**: Subclasses should be substitutable for their base classes
4. **ISP**: Many specific interfaces are better than one general-purpose interface
5. **DIP**: Depend on abstractions, not concretions

**Key Takeaways:**
- Start with SOLID principles in mind, don't retrofit
- Balance between following principles and over-engineering
- Use design patterns that support SOLID principles
- Regular refactoring helps maintain SOLID compliance
- Team education is crucial for consistent application

Remember: SOLID principles are guidelines, not rigid rules. Apply them judiciously based on your specific context and requirements.
