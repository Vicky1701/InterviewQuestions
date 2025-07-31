# SOLID Principles - Interview Questions (2 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is SOLID and why is it important?
**Answer:** SOLID is 5 design principles for maintainable OOP code:
- **S**ingle Responsibility Principle
- **O**pen/Closed Principle  
- **L**iskov Substitution Principle
- **I**nterface Segregation Principle
- **D**ependency Inversion Principle

Benefits: Better maintainability, testability, flexibility, reduced coupling.

### Q2: Explain Single Responsibility Principle with example?
**Answer:** A class should have only ONE reason to change (one job).

**❌ Bad:**
```java
class User {
    void saveToDatabase() { }     // Job 1: Database
    void sendEmail() { }          // Job 2: Email  
    void validateData() { }       // Job 3: Validation
}
```

**✅ Good:**
```java
class User { /* only user data */ }
class UserRepository { void save(User user) { } }
class EmailService { void send(User user) { } }
class UserValidator { boolean validate(User user) { } }
```

### Q3: What is Open/Closed Principle?
**Answer:** Classes should be **open for extension** but **closed for modification**. Add new features without changing existing code.

**❌ Bad:**
```java
class DiscountCalculator {
    double calculate(String customerType, double amount) {
        if (customerType.equals("REGULAR")) return amount * 0.05;
        if (customerType.equals("PREMIUM")) return amount * 0.10;
        // Adding VIP requires modifying this method!
    }
}
```

**✅ Good:**
```java
interface DiscountStrategy {
    double calculate(double amount);
}

class RegularDiscount implements DiscountStrategy {
    public double calculate(double amount) { return amount * 0.05; }
}

class PremiumDiscount implements DiscountStrategy {
    public double calculate(double amount) { return amount * 0.10; }
}

// Add VIP without modifying existing code!
class VIPDiscount implements DiscountStrategy {
    public double calculate(double amount) { return amount * 0.15; }
}
```

### Q4: Explain Liskov Substitution Principle?
**Answer:** Subclasses should be **substitutable for their parent class** without breaking functionality.

**❌ Bad:**
```java
class Bird {
    void fly() { System.out.println("Flying"); }
}

class Penguin extends Bird {
    void fly() { throw new UnsupportedOperationException(); } // Breaks LSP!
}

void makeBirdFly(Bird bird) {
    bird.fly(); // Crashes for Penguin!
}
```

**✅ Good:**
```java
interface Bird { void eat(); }
interface FlyingBird extends Bird { void fly(); }

class Sparrow implements FlyingBird {
    public void eat() { }
    public void fly() { }
}

class Penguin implements Bird {
    public void eat() { } // Only implements what it can do
}
```

### Q5: What is Interface Segregation Principle?
**Answer:** **Don't force clients to depend on unused methods**. Create specific interfaces instead of fat interfaces.

**❌ Bad:**
```java
interface Worker {
    void work();
    void eat();
    void sleep();
}

class Robot implements Worker {
    public void work() { }
    public void eat() { throw new UnsupportedOperationException(); } // Robot doesn't eat!
    public void sleep() { throw new UnsupportedOperationException(); } // Robot doesn't sleep!
}
```

**✅ Good:**
```java
interface Workable { void work(); }
interface Eatable { void eat(); }
interface Sleepable { void sleep(); }

class Human implements Workable, Eatable, Sleepable {
    public void work() { }
    public void eat() { }
    public void sleep() { }
}

class Robot implements Workable {
    public void work() { } // Only implements what it needs
}
```

### Q6: Explain Dependency Inversion Principle?
**Answer:** **Depend on abstractions, not concretions**. High-level modules shouldn't depend on low-level modules.

**❌ Bad:**
```java
class Car {
    private PetrolEngine engine = new PetrolEngine(); // Tight coupling!
    
    void start() { engine.start(); }
}
```

**✅ Good:**
```java
interface Engine { void start(); }

class Car {
    private Engine engine; // Depends on abstraction
    
    public Car(Engine engine) { this.engine = engine; } // Dependency injection
    
    void start() { engine.start(); }
}

// Can use any engine type!
Car petrolCar = new Car(new PetrolEngine());
Car electricCar = new Car(new ElectricEngine());
```

### Q7: How do SOLID principles work together?
**Answer:**
- **SRP** makes classes focused and cohesive
- **OCP** makes them extensible without modification  
- **LSP** ensures proper inheritance relationships
- **ISP** keeps interfaces lean and specific
- **DIP** reduces coupling through abstraction

### Q8: What are benefits of following SOLID?
**Answer:**
- **Maintainability** - Easy to understand and modify
- **Testability** - Easy to mock dependencies
- **Flexibility** - Easy to add new features
- **Reusability** - Small, focused components
- **Scalability** - Code grows without becoming messy

### Q9: When might you violate SOLID principles?
**Answer:** 
- **Simple applications** - SOLID might be overkill
- **Performance critical** - Abstractions add overhead
- **Tight deadlines** - Quick fixes over clean code
- **Legacy systems** - Gradual refactoring needed

### Q10: How to refactor code to follow SOLID?
**Answer:**
1. **Extract classes** for different responsibilities (SRP)
2. **Create interfaces** for extension points (OCP)
3. **Use composition** over inheritance (LSP)
4. **Split fat interfaces** into specific ones (ISP)
5. **Inject dependencies** instead of creating them (DIP)

---

## 🔥 Tricky Interview Questions (High Probability)

### Q11: Is this SRP violation? Why?
```java
class EmailService {
    void sendEmail(String to, String subject, String body) {
        validateEmail(to);           // ?
        formatMessage(subject, body); // ?
        sendToSMTPServer(to, subject, body); // ?
        logEmailSent(to);           // ?
    }
    
    private void validateEmail(String email) { }
    private void formatMessage(String subject, String body) { }
    private void sendToSMTPServer(String to, String subject, String body) { }
    private void logEmailSent(String to) { }
}
```
**Answer:** YES, SRP violation. EmailService has 4 responsibilities: validation, formatting, sending, logging. Should be split into separate classes.

### Q12: Which SOLID principle does this violate?
```java
class PaymentProcessor {
    void processPayment(String type, double amount) {
        if (type.equals("CREDIT_CARD")) {
            // Credit card logic
        } else if (type.equals("PAYPAL")) {
            // PayPal logic  
        } else if (type.equals("BANK_TRANSFER")) {
            // Bank transfer logic
        }
        // Adding new payment method requires modifying this class!
    }
}
```
**Answer:** **Open/Closed Principle** violation. Adding new payment methods requires modifying existing code instead of extending through interfaces.

### Q13: Rectangle-Square LSP Problem - What's wrong?
```java
class Rectangle {
    protected int width, height;
    
    void setWidth(int width) { this.width = width; }
    void setHeight(int height) { this.height = height; }
    int getArea() { return width * height; }
}

class Square extends Rectangle {
    void setWidth(int width) { 
        this.width = width; 
        this.height = width; // Forces height = width
    }
    
    void setHeight(int height) { 
        this.height = height; 
        this.width = height; // Forces width = height
    }
}

// Test code
void testArea(Rectangle r) {
    r.setWidth(5);
    r.setHeight(4);
    assert r.getArea() == 20; // Fails for Square!
}
```
**Answer:** **LSP violation**. Square changes Rectangle's behavior. When Square is passed as Rectangle, the assertion fails because Square forces width=height.

### Q14: Fat Interface Problem - How to fix?
```java
interface Printer {
    void printDocument();
    void scanDocument();
    void faxDocument();
    void emailDocument();
}

class SimplePrinter implements Printer {
    public void printDocument() { /* print */ }
    public void scanDocument() { throw new UnsupportedOperationException(); }
    public void faxDocument() { throw new UnsupportedOperationException(); }
    public void emailDocument() { throw new UnsupportedOperationException(); }
}
```
**Answer:** **ISP violation**. Fix by splitting into specific interfaces:
```java
interface Printable { void printDocument(); }
interface Scannable { void scanDocument(); }
interface Faxable { void faxDocument(); }
interface Emailable { void emailDocument(); }

class SimplePrinter implements Printable {
    public void printDocument() { /* print */ }
}

class AllInOnePrinter implements Printable, Scannable, Faxable, Emailable {
    // Implements all features
}
```

### Q15: Dependency Injection Types - Which is better?
```java
// Constructor Injection
class UserService {
    private final UserRepository repository;
    
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}

// Setter Injection  
class UserService {
    private UserRepository repository;
    
    public void setRepository(UserRepository repository) {
        this.repository = repository;
    }
}

// Field Injection (with @Autowired)
class UserService {
    @Autowired
    private UserRepository repository;
}
```
**Answer:** **Constructor Injection** is best because:
- Makes dependencies explicit and required
- Enables immutable objects (final fields)
- Fails fast if dependencies missing
- Easy to test without framework

### Q16: Can you spot all SOLID violations?
```java
class OrderProcessor {
    public void processOrder(Order order) {
        // Validate order
        if (order.getItems().isEmpty()) {
            throw new IllegalArgumentException("No items");
        }
        
        // Calculate total
        double total = 0;
        for (Item item : order.getItems()) {
            if (item.getType().equals("BOOK")) {
                total += item.getPrice() * 0.9; // 10% discount for books
            } else if (item.getType().equals("ELECTRONICS")) {
                total += item.getPrice() * 0.95; // 5% discount for electronics
            } else {
                total += item.getPrice();
            }
        }
        
        // Save to database
        Connection conn = DriverManager.getConnection("jdbc:mysql://...");
        PreparedStatement stmt = conn.prepareStatement("INSERT INTO orders...");
        stmt.execute();
        
        // Send email
        SMTPClient client = new SMTPClient();
        client.sendEmail(order.getCustomerEmail(), "Order Confirmed", "Thank you");
    }
}
```
**Answer:** **ALL SOLID principles violated**:
- **SRP**: Does validation, calculation, database, email (4 responsibilities)
- **OCP**: Adding new item types requires code modification
- **LSP**: N/A (no inheritance)
- **ISP**: N/A (no interfaces used)
- **DIP**: Directly depends on concrete classes (Connection, SMTPClient)

### Q17: Strategy Pattern and SOLID - How does it help?
```java
interface PaymentStrategy {
    void pay(double amount);
}

class CreditCardPayment implements PaymentStrategy {
    public void pay(double amount) { /* credit card logic */ }
}

class PayPalPayment implements PaymentStrategy {
    public void pay(double amount) { /* PayPal logic */ }
}

class PaymentProcessor {
    private PaymentStrategy strategy;
    
    public PaymentProcessor(PaymentStrategy strategy) {
        this.strategy = strategy;
    }
    
    public void processPayment(double amount) {
        strategy.pay(amount);
    }
}
```
**Answer:** Strategy pattern supports **multiple SOLID principles**:
- **SRP**: Each payment method has single responsibility
- **OCP**: Add new payment methods without modifying existing code
- **DIP**: PaymentProcessor depends on abstraction (PaymentStrategy)

### Q18: Template Method and LSP - Is this correct?
```java
abstract class DataProcessor {
    public final void process() {
        loadData();
        processData();
        saveData();
    }
    
    protected abstract void loadData();
    protected abstract void processData();
    protected abstract void saveData();
}

class XMLProcessor extends DataProcessor {
    protected void loadData() { /* load XML */ }
    protected void processData() { /* process XML */ }
    protected void saveData() { /* save XML */ }
}

class CSVProcessor extends DataProcessor {
    protected void loadData() { /* load CSV */ }
    protected void processData() { /* process CSV */ }
    protected void saveData() { 
        throw new UnsupportedOperationException("CSV is read-only"); 
    }
}
```
**Answer:** **LSP violation**. CSVProcessor breaks the contract by throwing exception in saveData(). All subclasses should be able to perform all template steps.

---

## 🎯 Advanced Questions (Bonus Points)

### Q19: SOLID and Design Patterns relationship?
**Answer:**
- **Strategy Pattern** → OCP, DIP (extension through interfaces)
- **Observer Pattern** → OCP, DIP (loose coupling)  
- **Factory Pattern** → SRP, DIP (creation responsibility)
- **Decorator Pattern** → OCP, SRP (extend behavior)
- **Adapter Pattern** → LSP, ISP (interface compatibility)

### Q20: How does SOLID apply to microservices?
**Answer:**
- **SRP**: Each service has single business responsibility
- **OCP**: Add new services without modifying existing ones
- **LSP**: Services should honor API contracts
- **ISP**: Service interfaces should be specific to client needs
- **DIP**: Services depend on abstractions (APIs), not implementations

### Q21: SOLID in functional programming?
**Answer:**
- **SRP**: Functions have single purpose
- **OCP**: Extend through composition and higher-order functions
- **LSP**: Function contracts must be preserved
- **ISP**: Small, focused function interfaces
- **DIP**: Depend on function abstractions, inject functions as parameters

---

## 💡 Key Points for Your Experience Level

### Common Interview Mistakes (Avoid These):
1. **Confusing principles** - Know each principle's specific purpose
2. **Over-engineering** - SOLID doesn't mean complex for simple problems
3. **Academic examples only** - Provide real-world scenarios
4. **Missing trade-offs** - Understand when NOT to apply SOLID
5. **Pattern confusion** - Know which patterns support which principles

### What Interviewers Expect from 2-Year Experience:
- **Understand all 5 principles** with clear examples
- **Identify violations** in code snippets
- **Suggest refactoring** approaches
- **Real-world application** experience
- **Trade-offs awareness** - when to apply vs when to skip

### Practical Application Examples:
```java
// E-commerce system following SOLID

// SRP - Single responsibility
class Product { /* only product data */ }
class ProductRepository { /* only database operations */ }
class ProductService { /* only business logic */ }

// OCP - Strategy pattern for pricing
interface PricingStrategy { double calculatePrice(Product product); }
class RegularPricing implements PricingStrategy { }
class DiscountPricing implements PricingStrategy { }

// ISP - Specific interfaces
interface Readable { Product read(String id); }
interface Writable { void write(Product product); }

// DIP - Dependency injection
class ProductController {
    private final ProductService service;
    
    public ProductController(ProductService service) {
        this.service = service;
    }
}
```

---

## 🚀 Last Minute Quick Review

### One-Liner Answers:
1. **What is SOLID?** → 5 OOP design principles for maintainable code
2. **SRP purpose?** → One class, one responsibility, one reason to change
3. **OCP meaning?** → Open for extension, closed for modification
4. **LSP rule?** → Subclasses must be substitutable for parent classes
5. **ISP principle?** → Many specific interfaces > one fat interface
6. **DIP concept?** → Depend on abstractions, not concrete implementations
7. **Main benefits?** → Maintainability, testability, flexibility, reusability
8. **When to avoid?** → Simple apps, performance-critical code, tight deadlines
9. **Best DI type?** → Constructor injection (explicit, immutable, testable)
10. **Common violations?** → God classes, if-else chains, fat interfaces, tight coupling

### Memory Tricks:
- **S**ingle job per class
- **O**pen for extension, **C**losed for modification
- **L**iskov substitution (parent-child relationship)
- **I**nterface segregation (split fat interfaces)
- **D**ependency inversion (abstractions over concretions)

### Quick Violation Checklist:
```java
// SRP Violation Signs:
class User {
    void saveToDatabase() { }  // Database responsibility
    void sendEmail() { }       // Email responsibility  
    void validateData() { }    // Validation responsibility
}

// OCP Violation Signs:
if (type.equals("A")) { }
else if (type.equals("B")) { }  // Adding C requires modification

// LSP Violation Signs:
class Square extends Rectangle {
    void setWidth(int w) { width = height = w; }  // Changes behavior
}

// ISP Violation Signs:
interface Worker {
    void work();
    void eat();    // Robot doesn't need this
    void sleep();  // Robot doesn't need this
}

// DIP Violation Signs:
class Car {
    PetrolEngine engine = new PetrolEngine();  // Tight coupling
}
```

### Design Patterns Supporting SOLID:
- **Strategy** → OCP, DIP
- **Factory** → SRP, DIP
- **Observer** → OCP, DIP
- **Decorator** → OCP, SRP
- **Template Method** → OCP, LSP

---

## 🎯 Final Tips

### During Interview:
- **Start with definitions** then give examples
- **Use real-world scenarios** from your experience
- **Show code before/after** refactoring
- **Mention trade-offs** and when not to apply
- **Connect to design patterns** you've used

### Red Flags to Avoid:
- Saying SOLID must always be followed religiously
- Not knowing specific principle definitions
- Only giving academic examples (Rectangle-Square)
- Confusing principles with each other
- Not understanding dependency injection types

### Sample Answer Structure:
1. **Define** the principle clearly
2. **Explain why** it's important  
3. **Show bad example** (violation)
4. **Show good example** (following principle)
5. **Mention real-world** application
6. **Discuss trade-offs** if asked

---

*🎯 Perfect for 2-year experience SOLID principles interviews!*
