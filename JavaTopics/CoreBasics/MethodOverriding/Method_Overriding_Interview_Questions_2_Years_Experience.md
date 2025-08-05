# Method Overriding Interview Questions - 2 Years Java Experience

## Must Know Questions (Core Concepts)

### 1. What is Method Overriding in Java?

**Answer:**
Method overriding is a feature that allows a subclass to provide a specific implementation of a method that is already defined in its parent class.

**Key Points:**
- Runtime polymorphism (dynamic method dispatch)
- Method signature must be same as parent class
- Enables specific behavior in child classes
- Resolved at runtime, not compile time

**Code Example:**
```java
class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks: Woof! Woof!");
    }
}

class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows: Meow! Meow!");
    }
}

public class OverridingExample {
    public static void main(String[] args) {
        Animal animal1 = new Dog();    // Upcasting
        Animal animal2 = new Cat();    // Upcasting
        
        animal1.makeSound(); // Output: Dog barks: Woof! Woof!
        animal2.makeSound(); // Output: Cat meows: Meow! Meow!
        
        // Runtime decides which method to call based on actual object type
    }
}
```

### 2. What's the difference between Method Overriding and Method Overloading?

**Answer:**

**Method Overriding:**
- Same method signature in parent and child class
- Runtime polymorphism
- IS-A relationship (inheritance)
- Different implementations

**Method Overloading:**
- Same method name, different parameters
- Compile-time polymorphism
- Within same class or inherited classes
- Different method signatures

**Code Example:**
```java
class Calculator {
    // Method Overloading - same class, different parameters
    public int add(int a, int b) {
        return a + b;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
}

class Parent {
    public void display() {
        System.out.println("Parent display");
    }
}

class Child extends Parent {
    // Method Overriding - same signature, different implementation
    @Override
    public void display() {
        System.out.println("Child display");
    }
}
```

### 3. What are the rules for Method Overriding?

**Answer:**

**Method Signature Rules:**
- Method name must be same
- Parameter list must be identical
- Return type must be same or covariant (subtype)

**Access Modifier Rules:**
- Cannot reduce access level
- Can increase access level

**Exception Rules:**
- Cannot throw broader checked exceptions
- Can throw same, narrower, or no exceptions
- Can throw any unchecked exceptions

**Code Example:**
```java
class Parent {
    protected String getData() throws IOException {
        return "Parent data";
    }
    
    public void process() throws Exception {
        System.out.println("Parent process");
    }
}

class Child extends Parent {
    // ✅ Valid - same return type
    @Override
    protected String getData() throws IOException {
        return "Child data";
    }
    
    // ✅ Valid - increased access level (protected -> public)
    @Override
    public void process() throws IOException { // Narrower exception
        System.out.println("Child process");
    }
    
    // ❌ Invalid examples (compilation errors):
    // private String getData() { } // Cannot reduce access level
    // public void process() throws Exception, SQLException { } // Cannot throw broader exceptions
}
```

### 4. What is the @Override annotation and why should you use it?

**Answer:**
@Override is an annotation that explicitly marks a method as overriding a parent class method.

**Benefits:**
- Compile-time checking
- Prevents accidental overloading instead of overriding
- Improves code readability
- IDE support for refactoring

**Code Example:**
```java
class Vehicle {
    public void startEngine() {
        System.out.println("Vehicle engine started");
    }
}

class Car extends Vehicle {
    // Without @Override - risky
    public void startEngine() { // What if parent method signature changes?
        System.out.println("Car engine started");
    }
    
    // With @Override - safe
    @Override
    public void startEngine() {
        System.out.println("Car engine started with key");
    }
    
    // This will cause compilation error - method doesn't exist in parent
    // @Override
    // public void startEngineWithButton() { }
}

// Real-world example - accidental overloading instead of overriding
class Parent {
    public void process(String data) {
        System.out.println("Parent processing: " + data);
    }
}

class Child extends Parent {
    // Accidentally created overload, not override (parameter type different)
    // Without @Override, this compiles but doesn't override
    public void process(Object data) {
        System.out.println("Child processing: " + data);
    }
    
    // With @Override, this would cause compilation error
    // @Override
    // public void process(Object data) { } // Compilation error!
}
```

### 5. Can you override static methods? Why or why not?

**Answer:**
**No, you cannot override static methods.** Static methods belong to the class, not to instances, so they are resolved at compile time.

**What happens instead:**
- Static methods can be "hidden" (method hiding)
- The method called depends on the reference type, not object type
- No runtime polymorphism for static methods

**Code Example:**
```java
class Parent {
    public static void staticMethod() {
        System.out.println("Parent static method");
    }
    
    public void instanceMethod() {
        System.out.println("Parent instance method");
    }
}

class Child extends Parent {
    // This is method hiding, NOT overriding
    public static void staticMethod() {
        System.out.println("Child static method");
    }
    
    @Override
    public void instanceMethod() {
        System.out.println("Child instance method");
    }
}

public class StaticMethodExample {
    public static void main(String[] args) {
        Parent parent = new Child();
        
        // Static method - resolved at compile time based on reference type
        parent.staticMethod(); // Output: Parent static method
        
        // Instance method - resolved at runtime based on object type
        parent.instanceMethod(); // Output: Child instance method
        
        // Direct class calls
        Parent.staticMethod(); // Output: Parent static method
        Child.staticMethod();  // Output: Child static method
    }
}
```

### 6. Can you override private methods?

**Answer:**
**No, you cannot override private methods** because private methods are not inherited by subclasses.

**Explanation:**
- Private methods are not visible to subclasses
- If you create a method with same signature in child class, it's a new method
- No polymorphic behavior occurs

**Code Example:**
```java
class Parent {
    private void privateMethod() {
        System.out.println("Parent private method");
    }
    
    public void callPrivateMethod() {
        privateMethod(); // Calls parent's private method
    }
}

class Child extends Parent {
    // This is NOT overriding - it's a completely new method
    private void privateMethod() {
        System.out.println("Child private method");
    }
    
    public void testPrivateMethods() {
        privateMethod(); // Calls child's private method
        callPrivateMethod(); // Calls parent's public method, which calls parent's private method
    }
}

public class PrivateMethodExample {
    public static void main(String[] args) {
        Child child = new Child();
        child.testPrivateMethods();
        
        // Output:
        // Child private method
        // Parent private method
    }
}
```

### 7. Can you override final methods?

**Answer:**
**No, you cannot override final methods.** The `final` keyword prevents method overriding.

**Purpose of final methods:**
- Ensures method behavior cannot be changed
- Security and design constraints
- Performance optimization (can be inlined)

**Code Example:**
```java
class Parent {
    public final void finalMethod() {
        System.out.println("This method cannot be overridden");
    }
    
    public void normalMethod() {
        System.out.println("This method can be overridden");
    }
}

class Child extends Parent {
    // ❌ Compilation error - cannot override final method
    // public void finalMethod() {
    //     System.out.println("Trying to override final method");
    // }
    
    // ✅ Valid - can override normal method
    @Override
    public void normalMethod() {
        System.out.println("Child implementation");
    }
}

// Real-world example - String class methods
public class FinalMethodExample {
    public void demonstrateFinalMethods() {
        // String class has many final methods like hashCode(), equals()
        // You cannot create a subclass of String and override these methods
        
        // This is why String is immutable and secure
        String text = "Hello";
        int hash = text.hashCode(); // Final method - behavior guaranteed
    }
}
```

### 8. What is covariant return type in method overriding?

**Answer:**
Covariant return type allows an overriding method to return a subtype of the return type declared in the parent class method.

**Benefits:**
- More specific return types in subclasses
- Better type safety
- Avoids unnecessary casting

**Code Example:**
```java
class Animal {
    public Animal reproduce() {
        System.out.println("Animal reproducing");
        return new Animal();
    }
}

class Dog extends Animal {
    // Covariant return type - Dog is subtype of Animal
    @Override
    public Dog reproduce() {
        System.out.println("Dog reproducing");
        return new Dog();
    }
}

class Cat extends Animal {
    // Covariant return type - Cat is subtype of Animal  
    @Override
    public Cat reproduce() {
        System.out.println("Cat reproducing");
        return new Cat();
    }
}

public class CovariantExample {
    public static void main(String[] args) {
        Animal animal = new Dog();
        Animal offspring = animal.reproduce(); // Returns Dog, but type is Animal
        
        Dog dog = new Dog();
        Dog puppy = dog.reproduce(); // Returns Dog, no casting needed
        
        // Before covariant return types (Java 1.4 and earlier):
        // Animal offspring = animal.reproduce();
        // Dog puppy = (Dog) offspring; // Casting was required
    }
}

// Real-world example - clone() method
class Person implements Cloneable {
    private String name;
    
    public Person(String name) {
        this.name = name;
    }
    
    @Override
    public Person clone() { // Covariant return type (was Object in Object class)
        try {
            return (Person) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new RuntimeException(e);
        }
    }
}
```

### 9. How does method overriding work with constructors?

**Answer:**
**Constructors cannot be overridden** because:
- Constructors are not inherited
- Constructor names must match the class name
- Constructors are called in inheritance chain during object creation

**Constructor Chaining in Inheritance:**

**Code Example:**
```java
class Parent {
    private String parentField;
    
    public Parent() {
        System.out.println("Parent default constructor");
        parentField = "Parent";
    }
    
    public Parent(String value) {
        System.out.println("Parent parameterized constructor: " + value);
        parentField = value;
    }
    
    public void display() {
        System.out.println("Parent field: " + parentField);
    }
}

class Child extends Parent {
    private String childField;
    
    public Child() {
        super(); // Explicit call to parent constructor (optional here)
        System.out.println("Child default constructor");
        childField = "Child";
    }
    
    public Child(String parentValue, String childValue) {
        super(parentValue); // Must be first statement if used
        System.out.println("Child parameterized constructor: " + childValue);
        childField = childValue;
    }
    
    @Override
    public void display() {
        super.display(); // Call parent method
        System.out.println("Child field: " + childField);
    }
}

public class ConstructorExample {
    public static void main(String[] args) {
        System.out.println("Creating Child object:");
        Child child = new Child("ParentValue", "ChildValue");
        child.display();
        
        // Output:
        // Creating Child object:
        // Parent parameterized constructor: ParentValue
        // Child parameterized constructor: ChildValue
        // Parent field: ParentValue
        // Child field: ChildValue
    }
}
```

### 10. What happens when you override a method that calls other methods?

**Answer:**
When an overridden method calls other methods, it follows normal method resolution rules - checking the actual object type for method implementations.

**Important Concepts:**
- Method calls within overridden methods use dynamic dispatch
- `this` refers to the current object (runtime type)
- `super` explicitly calls parent class methods

**Code Example:**
```java
class Parent {
    public void mainMethod() {
        System.out.println("Parent mainMethod start");
        helperMethod(); // This will call child's version if overridden
        finalHelperMethod(); // This always calls parent's version (final)
        System.out.println("Parent mainMethod end");
    }
    
    public void helperMethod() {
        System.out.println("Parent helperMethod");
    }
    
    public final void finalHelperMethod() {
        System.out.println("Parent finalHelperMethod");
    }
}

class Child extends Parent {
    @Override
    public void mainMethod() {
        System.out.println("Child mainMethod start");
        super.mainMethod(); // Explicitly call parent method
        System.out.println("Child mainMethod end");
    }
    
    @Override
    public void helperMethod() {
        System.out.println("Child helperMethod");
    }
    
    // Cannot override finalHelperMethod - it's final
}

public class MethodCallExample {
    public static void main(String[] args) {
        Parent parent = new Child();
        parent.mainMethod();
        
        // Output:
        // Child mainMethod start
        // Parent mainMethod start
        // Child helperMethod          <- Dynamic dispatch
        // Parent finalHelperMethod    <- Final method
        // Parent mainMethod end
        // Child mainMethod end
    }
}
```

## Tricky Questions (Scenario-Based)

### 11. What will be the output of this code?

```java
class A {
    public void method1() {
        System.out.print("A1 ");
        method2();
    }
    
    public void method2() {
        System.out.print("A2 ");
    }
}

class B extends A {
    public void method1() {
        System.out.print("B1 ");
        super.method1();
    }
    
    public void method2() {
        System.out.print("B2 ");
    }
}

public class Test {
    public static void main(String[] args) {
        A obj = new B();
        obj.method1();
    }
}
```

**Answer:** Output will be: `B1 A1 B2`

**Explanation:**
1. `obj.method1()` calls B's method1() → prints "B1"
2. `super.method1()` calls A's method1() → prints "A1"
3. A's method1() calls `method2()` → but this resolves to B's method2() (dynamic dispatch) → prints "B2"

### 12. What's wrong with this overriding attempt?

```java
class Parent {
    public Number getValue() {
        return 42;
    }
}

class Child extends Parent {
    @Override
    public Integer getValue() { // Is this valid?
        return 100;
    }
}
```

**Answer:** **This is VALID!**

**Explanation:**
- Integer is a subclass of Number
- This is covariant return type (allowed since Java 5)
- Overriding method can return a more specific type

**Invalid Example:**
```java
class Parent {
    public Integer getValue() {
        return 42;
    }
}

class Child extends Parent {
    // @Override
    // public Number getValue() { // ❌ INVALID - cannot return broader type
    //     return 100.5;
    // }
}
```

### 13. What happens with method overriding and exception handling?

```java
class Parent {
    public void process() throws IOException {
        System.out.println("Parent process");
    }
}

class Child extends Parent {
    // Which of these are valid overrides?
    
    // Option 1:
    public void process() throws FileNotFoundException {
        System.out.println("Child process 1");
    }
    
    // Option 2:
    public void process() throws IOException, SQLException {
        System.out.println("Child process 2");
    }
    
    // Option 3:
    public void process() {
        System.out.println("Child process 3");
    }
    
    // Option 4:
    public void process() throws Exception {
        System.out.println("Child process 4");
    }
}
```

**Answer:**

**Valid Overrides:**
- **Option 1:** ✅ Valid - FileNotFoundException is subclass of IOException
- **Option 3:** ✅ Valid - No exceptions is allowed

**Invalid Overrides:**
- **Option 2:** ❌ Invalid - SQLException is not subclass of IOException
- **Option 4:** ❌ Invalid - Exception is broader than IOException

**Rule:** Overriding method cannot throw broader checked exceptions than parent method.

### 14. What's the issue with this inheritance hierarchy?

```java
class Animal {
    public void eat(Food food) {
        System.out.println("Animal eating");
    }
}

class Dog extends Animal {
    public void eat(DogFood food) { // Is this overriding?
        System.out.println("Dog eating dog food");
    }
}
```

**Answer:** **This is NOT overriding - it's overloading!**

**Problem:**
- Parameter types are different (Food vs DogFood)
- This creates method overloading, not overriding
- No polymorphic behavior

**Correct Override:**
```java
class Dog extends Animal {
    @Override
    public void eat(Food food) { // Same parameter type
        if (food instanceof DogFood) {
            System.out.println("Dog eating dog food");
        } else {
            System.out.println("Dog eating regular food");
        }
    }
}
```

### 15. What happens with private method and same signature in child class?

```java
class Parent {
    private void secretMethod() {
        System.out.println("Parent secret");
    }
    
    public void publicMethod() {
        secretMethod();
    }
}

class Child extends Parent {
    private void secretMethod() { // Same signature
        System.out.println("Child secret");
    }
    
    public void testMethods() {
        secretMethod(); // Which method is called?
        publicMethod(); // Which secretMethod does this call?
    }
}
```

**Answer:**

```java
Child child = new Child();
child.testMethods();

// Output:
// Child secret       <- Child's private method
// Parent secret      <- Parent's private method (called from Parent's publicMethod)
```

**Explanation:**
- Private methods are not inherited
- Each class has its own private method
- No overriding relationship exists

### 16. What's wrong with this method signature override?

```java
class Parent {
    public void process(String... args) {
        System.out.println("Parent varargs");
    }
}

class Child extends Parent {
    @Override
    public void process(String[] args) { // Valid override?
        System.out.println("Child array");
    }
}
```

**Answer:** **This is VALID override!**

**Explanation:**
- Varargs (String...) and array (String[]) are equivalent for method signature
- JVM treats them the same way
- This is proper method overriding

**Test Code:**
```java
Parent parent = new Child();
parent.process("a", "b", "c");     // Calls Child's method
parent.process(new String[]{"x"}); // Calls Child's method
```

### 17. What happens with overriding and synchronized methods?

```java
class Parent {
    public synchronized void syncMethod() {
        System.out.println("Parent synchronized");
    }
}

class Child extends Parent {
    @Override
    public void syncMethod() { // Not synchronized
        System.out.println("Child not synchronized");
    }
}
```

**Answer:** **This is VALID!**

**Key Points:**
- `synchronized` is not part of method signature
- Child method is not synchronized (different behavior)
- Each method has its own synchronization behavior
- This can lead to thread safety issues

**Best Practice:**
```java
class Child extends Parent {
    @Override
    public synchronized void syncMethod() { // Keep synchronized
        System.out.println("Child synchronized");
    }
}
```

### 18. What's the output of this tricky inheritance scenario?

```java
class A {
    public A() {
        method();
    }
    
    public void method() {
        System.out.println("A method");
    }
}

class B extends A {
    private String value = "B value";
    
    public B() {
        System.out.println("B constructor");
    }
    
    @Override
    public void method() {
        System.out.println("B method: " + value);
    }
}

public class Test {
    public static void main(String[] args) {
        B b = new B();
    }
}
```

**Answer:** Output will be:
```
B method: null
B constructor
```

**Explanation:**
1. A's constructor calls `method()`
2. Due to overriding, B's `method()` is called
3. But B's instance variables are not initialized yet
4. `value` is null at this point
5. Then B's constructor completes

**Lesson:** Avoid calling overridable methods from constructors!

## Advanced Questions (Architecture & Design)

### 19. How would you design a plugin architecture using method overriding?

**Answer:**

**Plugin Architecture Design:**

```java
// 1. Base Plugin Interface/Abstract Class
public abstract class Plugin {
    protected String name;
    protected String version;
    
    public Plugin(String name, String version) {
        this.name = name;
        this.version = version;
    }
    
    // Template method pattern
    public final void execute() {
        if (initialize()) {
            processData();
            cleanup();
        }
    }
    
    // Abstract methods that plugins must implement
    protected abstract boolean initialize();
    protected abstract void processData();
    protected abstract void cleanup();
    
    // Common functionality
    public String getInfo() {
        return name + " v" + version;
    }
}

// 2. Specific Plugin Implementations
public class DatabasePlugin extends Plugin {
    private Connection connection;
    
    public DatabasePlugin() {
        super("Database Plugin", "1.0");
    }
    
    @Override
    protected boolean initialize() {
        try {
            connection = DriverManager.getConnection("jdbc:mysql://localhost/test");
            System.out.println("Database connection established");
            return true;
        } catch (SQLException e) {
            System.err.println("Failed to connect to database: " + e.getMessage());
            return false;
        }
    }
    
    @Override
    protected void processData() {
        System.out.println("Processing database operations...");
        // Database-specific processing logic
    }
    
    @Override
    protected void cleanup() {
        try {
            if (connection != null) {
                connection.close();
                System.out.println("Database connection closed");
            }
        } catch (SQLException e) {
            System.err.println("Error closing connection: " + e.getMessage());
        }
    }
}

public class FilePlugin extends Plugin {
    private File workingDirectory;
    
    public FilePlugin() {
        super("File Plugin", "2.0");
    }
    
    @Override
    protected boolean initialize() {
        workingDirectory = new File("./data");
        if (!workingDirectory.exists()) {
            boolean created = workingDirectory.mkdirs();
            System.out.println("Working directory created: " + created);
            return created;
        }
        return true;
    }
    
    @Override
    protected void processData() {
        System.out.println("Processing files in: " + workingDirectory.getAbsolutePath());
        // File-specific processing logic
    }
    
    @Override
    protected void cleanup() {
        System.out.println("File processing completed");
        // File cleanup logic
    }
}

// 3. Plugin Manager
public class PluginManager {
    private List<Plugin> plugins = new ArrayList<>();
    
    public void registerPlugin(Plugin plugin) {
        plugins.add(plugin);
        System.out.println("Registered: " + plugin.getInfo());
    }
    
    public void executeAllPlugins() {
        for (Plugin plugin : plugins) {
            System.out.println("\nExecuting: " + plugin.getInfo());
            plugin.execute();
        }
    }
}

// 4. Usage Example
public class PluginSystemDemo {
    public static void main(String[] args) {
        PluginManager manager = new PluginManager();
        
        // Register plugins
        manager.registerPlugin(new DatabasePlugin());
        manager.registerPlugin(new FilePlugin());
        
        // Execute all plugins
        manager.executeAllPlugins();
    }
}
```

### 20. How do you handle method overriding in a framework where behavior needs to be customizable?

**Answer:**

**Framework Design with Customizable Behavior:**

```java
// 1. Base Framework Class with Hook Methods
public abstract class DataProcessor {
    
    // Template method - defines the algorithm structure
    public final ProcessingResult process(DataSet dataSet) {
        try {
            // Pre-processing hooks
            if (!validateInput(dataSet)) {
                return ProcessingResult.failure("Input validation failed");
            }
            
            onProcessingStart(dataSet);
            
            // Core processing (customizable)
            ProcessingResult result = performProcessing(dataSet);
            
            // Post-processing hooks
            onProcessingComplete(result);
            
            return result;
            
        } catch (Exception e) {
            ProcessingResult errorResult = handleProcessingError(e, dataSet);
            onProcessingError(e, dataSet);
            return errorResult;
        }
    }
    
    // Abstract methods - must be implemented by subclasses
    protected abstract ProcessingResult performProcessing(DataSet dataSet);
    
    // Hook methods - optional to override
    protected boolean validateInput(DataSet dataSet) {
        return dataSet != null && !dataSet.isEmpty();
    }
    
    protected void onProcessingStart(DataSet dataSet) {
        System.out.println("Processing started for dataset: " + dataSet.getName());
    }
    
    protected void onProcessingComplete(ProcessingResult result) {
        System.out.println("Processing completed with status: " + result.getStatus());
    }
    
    protected void onProcessingError(Exception e, DataSet dataSet) {
        System.err.println("Processing error: " + e.getMessage());
    }
    
    protected ProcessingResult handleProcessingError(Exception e, DataSet dataSet) {
        return ProcessingResult.failure("Processing failed: " + e.getMessage());
    }
}

// 2. Specific Framework Implementations
public class MLDataProcessor extends DataProcessor {
    
    @Override
    protected ProcessingResult performProcessing(DataSet dataSet) {
        System.out.println("Performing ML processing...");
        
        // ML-specific processing
        normalizeData(dataSet);
        trainModel(dataSet);
        
        return ProcessingResult.success("ML processing completed");
    }
    
    @Override
    protected boolean validateInput(DataSet dataSet) {
        // Enhanced validation for ML
        if (!super.validateInput(dataSet)) {
            return false;
        }
        
        return dataSet.hasNumericFeatures() && dataSet.size() > 100;
    }
    
    @Override
    protected void onProcessingStart(DataSet dataSet) {
        super.onProcessingStart(dataSet);
        System.out.println("ML Processing - Features: " + dataSet.getFeatureCount());
    }
    
    private void normalizeData(DataSet dataSet) {
        System.out.println("Normalizing data...");
    }
    
    private void trainModel(DataSet dataSet) {
        System.out.println("Training ML model...");
    }
}

public class ETLDataProcessor extends DataProcessor {
    
    @Override
    protected ProcessingResult performProcessing(DataSet dataSet) {
        System.out.println("Performing ETL processing...");
        
        // ETL-specific processing
        extractData(dataSet);
        transformData(dataSet);
        loadData(dataSet);
        
        return ProcessingResult.success("ETL processing completed");
    }
    
    @Override
    protected ProcessingResult handleProcessingError(Exception e, DataSet dataSet) {
        // Custom error handling for ETL
        if (e instanceof DataTransformationException) {
            // Try to recover
            System.out.println("Attempting ETL recovery...");
            return ProcessingResult.partialSuccess("ETL completed with warnings");
        }
        
        return super.handleProcessingError(e, dataSet);
    }
    
    private void extractData(DataSet dataSet) {
        System.out.println("Extracting data...");
    }
    
    private void transformData(DataSet dataSet) {
        System.out.println("Transforming data...");
    }
    
    private void loadData(DataSet dataSet) {
        System.out.println("Loading data...");
    }
}

// 3. Support Classes
public class DataSet {
    private String name;
    private List<DataRecord> records;
    
    public DataSet(String name) {
        this.name = name;
        this.records = new ArrayList<>();
    }
    
    // Getters and utility methods
    public String getName() { return name; }
    public boolean isEmpty() { return records.isEmpty(); }
    public int size() { return records.size(); }
    public boolean hasNumericFeatures() { return true; } // Simplified
    public int getFeatureCount() { return 10; } // Simplified
}

public class ProcessingResult {
    private String status;
    private String message;
    
    private ProcessingResult(String status, String message) {
        this.status = status;
        this.message = message;
    }
    
    public static ProcessingResult success(String message) {
        return new ProcessingResult("SUCCESS", message);
    }
    
    public static ProcessingResult failure(String message) {
        return new ProcessingResult("FAILURE", message);
    }
    
    public static ProcessingResult partialSuccess(String message) {
        return new ProcessingResult("PARTIAL_SUCCESS", message);
    }
    
    public String getStatus() { return status; }
    public String getMessage() { return message; }
}
```

### 21. Design a callback system using method overriding for event handling.

**Answer:**

**Event-Driven Callback System:**

```java
// 1. Base Event System
public abstract class EventHandler<T extends Event> {
    
    // Template method for event handling
    public final void handleEvent(T event) {
        try {
            if (canHandle(event)) {
                beforeHandle(event);
                processEvent(event);
                afterHandle(event);
            }
        } catch (Exception e) {
            onError(event, e);
        }
    }
    
    // Abstract method - must be implemented
    protected abstract void processEvent(T event);
    
    // Hook methods - optional to override
    protected boolean canHandle(T event) {
        return true;
    }
    
    protected void beforeHandle(T event) {
        System.out.println("Before handling: " + event.getType());
    }
    
    protected void afterHandle(T event) {
        System.out.println("After handling: " + event.getType());
    }
    
    protected void onError(T event, Exception e) {
        System.err.println("Error handling event " + event.getType() + ": " + e.getMessage());
    }
}

// 2. Event Types
public abstract class Event {
    private final String type;
    private final long timestamp;
    
    public Event(String type) {
        this.type = type;
        this.timestamp = System.currentTimeMillis();
    }
    
    public String getType() { return type; }
    public long getTimestamp() { return timestamp; }
}

public class UserEvent extends Event {
    private String userId;
    private String action;
    
    public UserEvent(String userId, String action) {
        super("USER_EVENT");
        this.userId = userId;
        this.action = action;
    }
    
    public String getUserId() { return userId; }
    public String getAction() { return action; }
}

public class SystemEvent extends Event {
    private String component;
    private String message;
    
    public SystemEvent(String component, String message) {
        super("SYSTEM_EVENT");
        this.component = component;
        this.message = message;
    }
    
    public String getComponent() { return component; }
    public String getMessage() { return message; }
}

// 3. Specific Event Handlers
public class UserEventHandler extends EventHandler<UserEvent> {
    
    @Override
    protected void processEvent(UserEvent event) {
        System.out.println("Processing user event:");
        System.out.println("  User: " + event.getUserId());
        System.out.println("  Action: " + event.getAction());
        
        // Specific user event processing
        switch (event.getAction()) {
            case "LOGIN":
                handleLogin(event);
                break;
            case "LOGOUT":
                handleLogout(event);
                break;
            default:
                handleGenericUserAction(event);
        }
    }
    
    @Override
    protected boolean canHandle(UserEvent event) {
        // Only handle events for active users
        return isActiveUser(event.getUserId());
    }
    
    @Override
    protected void beforeHandle(UserEvent event) {
        super.beforeHandle(event);
        System.out.println("User event validation completed");
    }
    
    private void handleLogin(UserEvent event) {
        System.out.println("User " + event.getUserId() + " logged in");
    }
    
    private void handleLogout(UserEvent event) {
        System.out.println("User " + event.getUserId() + " logged out");
    }
    
    private void handleGenericUserAction(UserEvent event) {
        System.out.println("Generic user action: " + event.getAction());
    }
    
    private boolean isActiveUser(String userId) {
        return userId != null && !userId.isEmpty();
    }
}

public class SystemEventHandler extends EventHandler<SystemEvent> {
    
    @Override
    protected void processEvent(SystemEvent event) {
        System.out.println("Processing system event:");
        System.out.println("  Component: " + event.getComponent());
        System.out.println("  Message: " + event.getMessage());
        
        // Log to monitoring system
        logToMonitoring(event);
        
        // Check if critical event
        if (isCriticalEvent(event)) {
            alertAdministrators(event);
        }
    }
    
    @Override
    protected void onError(SystemEvent event, Exception e) {
        super.onError(event, e);
        // Critical: system event handler failed
        alertAdministrators(event);
    }
    
    private void logToMonitoring(SystemEvent event) {
        System.out.println("Logged to monitoring system");
    }
    
    private boolean isCriticalEvent(SystemEvent event) {
        return event.getMessage().toLowerCase().contains("error") ||
               event.getMessage().toLowerCase().contains("failure");
    }
    
    private void alertAdministrators(SystemEvent event) {
        System.out.println("ALERT: Critical system event - " + event.getMessage());
    }
}

// 4. Event Manager with Observer Pattern
public class EventManager {
    private Map<Class<? extends Event>, List<EventHandler<? extends Event>>> handlers;
    
    public EventManager() {
        this.handlers = new HashMap<>();
    }
    
    @SuppressWarnings("unchecked")
    public <T extends Event> void registerHandler(Class<T> eventType, EventHandler<T> handler) {
        handlers.computeIfAbsent(eventType, k -> new ArrayList<>()).add(handler);
    }
    
    @SuppressWarnings("unchecked")
    public void fireEvent(Event event) {
        List<EventHandler<? extends Event>> eventHandlers = handlers.get(event.getClass());
        
        if (eventHandlers != null) {
            for (EventHandler handler : eventHandlers) {
                handler.handleEvent(event);
            }
        }
    }
}

// 5. Usage Example
public class EventSystemDemo {
    public static void main(String[] args) {
        EventManager eventManager = new EventManager();
        
        // Register handlers
        eventManager.registerHandler(UserEvent.class, new UserEventHandler());
        eventManager.registerHandler(SystemEvent.class, new SystemEventHandler());
        
        // Fire events
        eventManager.fireEvent(new UserEvent("user123", "LOGIN"));
        eventManager.fireEvent(new SystemEvent("Database", "Connection timeout error"));
        eventManager.fireEvent(new UserEvent("user456", "LOGOUT"));
    }
}
```

## Quick Review & Memory Tips

### Key Overriding Rules:
```
Method Signature: MUST be identical (name + parameters)
Return Type: Same or covariant (subtype)
Access Modifier: Cannot reduce (private < protected < public)
Exceptions: Cannot throw broader checked exceptions
Static Methods: Cannot be overridden (method hiding only)
Private Methods: Cannot be overridden (not inherited)
Final Methods: Cannot be overridden
Constructors: Cannot be overridden (not inherited)
```

### Override vs Overload Quick Check:
```
Override:
- Same signature ✓
- Different implementation ✓
- Runtime polymorphism ✓
- Inheritance required ✓

Overload:
- Different parameters ✓
- Same class or inheritance ✓  
- Compile-time resolution ✓
- Different signature ✓
```

### Common Interview Gotchas:
1. **Constructor calls in inheritance** - Overridden methods called from parent constructor
2. **Static method hiding** - Not overriding, reference type matters
3. **Private method "overriding"** - Creates new method, no polymorphism
4. **Exception narrowing** - Can throw more specific, not broader exceptions
5. **Covariant return types** - Return subtype is allowed
6. **@Override annotation** - Always use for compile-time safety

### Memory Tricks:
- **"SAME signature, DIFFERENT behavior"** - Overriding motto
- **"More SPECIFIC is OK"** - Return types, exceptions, access modifiers
- **"Static = Class level = No inheritance"** - Static methods can't be overridden
- **"Private = Hidden = No overriding"** - Private methods aren't inherited

### Real-World Applications:
1. **Framework design** - Template method pattern
2. **Plugin architectures** - Customizable behavior
3. **Event handling systems** - Callback mechanisms
4. **ORM frameworks** - Entity behavior customization
5. **Game development** - Character behavior specialization

---

**Interview Success Tip:** Always provide code examples and explain the runtime vs compile-time behavior difference!
