# Java Exception Handling - Complete Guide

## Table of Contents
1. [Introduction to Exception Handling](#introduction)
2. [Types of Exceptions](#types-of-exceptions)
3. [Try-Catch-Finally Blocks](#try-catch-finally)
4. [Checked vs Unchecked Exceptions](#checked-vs-unchecked)
5. [Throw and Throws Keywords](#throw-and-throws)
6. [Custom Exceptions](#custom-exceptions)
7. [Exception Hierarchy](#exception-hierarchy)
8. [Best Practices](#best-practices)
9. [Interview Questions](#interview-questions)
10. [Common Mistakes (Wrong vs Right Code)](#common-mistakes)

---

## Introduction to Exception Handling {#introduction}

Exception handling is a powerful mechanism in Java that allows you to handle runtime errors gracefully, maintaining the normal flow of your application.

**What is an Exception?**
- An unexpected event that occurs during program execution
- Disrupts the normal flow of the program
- Can be handled to prevent program termination

**Why Exception Handling?**
- Prevents program crashes
- Provides meaningful error messages
- Allows graceful recovery from errors
- Improves user experience
- Makes debugging easier

**Key Components:**
- **Exception**: An object representing an error condition
- **Throwing**: Creating and signaling an exception
- **Catching**: Handling the exception
- **Finally**: Code that always executes

---

## 1. Types of Exceptions {#types-of-exceptions}

### Exception Categories

```java
// 1. CHECKED EXCEPTIONS (Compile-time exceptions)
// Must be handled or declared

public class CheckedExceptionExample {
    
    // FileNotFoundException - must be handled
    public void readFile() throws IOException {
        FileReader file = new FileReader("nonexistent.txt"); // Throws IOException
    }
    
    // SQLException - must be handled
    public void connectDatabase() throws SQLException {
        Connection conn = DriverManager.getConnection("invalid_url");
    }
    
    // ClassNotFoundException - must be handled
    public void loadClass() throws ClassNotFoundException {
        Class.forName("NonExistentClass");
    }
}

// 2. UNCHECKED EXCEPTIONS (Runtime exceptions)
// Optional to handle

public class UncheckedExceptionExample {
    
    public void demonstrateRuntimeExceptions() {
        // NullPointerException
        String str = null;
        // System.out.println(str.length()); // Throws NullPointerException
        
        // ArrayIndexOutOfBoundsException
        int[] array = {1, 2, 3};
        // System.out.println(array[5]); // Throws ArrayIndexOutOfBoundsException
        
        // ArithmeticException
        int a = 10;
        int b = 0;
        // System.out.println(a / b); // Throws ArithmeticException
        
        // NumberFormatException
        // int num = Integer.parseInt("abc"); // Throws NumberFormatException
        
        // IllegalArgumentException
        // Thread.sleep(-1); // Throws IllegalArgumentException
    }
}

// 3. ERRORS (System-level problems)
// Should not be caught

public class ErrorExample {
    public void demonstrateErrors() {
        // OutOfMemoryError - JVM runs out of memory
        // StackOverflowError - infinite recursion
        // VirtualMachineError - JVM problems
        
        // Example of StackOverflowError
        // recursiveMethod(); // Don't actually run this!
    }
    
    public void recursiveMethod() {
        recursiveMethod(); // Infinite recursion causes StackOverflowError
    }
}
```

---

## 2. Try-Catch-Finally Blocks {#try-catch-finally}

### Basic Syntax and Examples

### Simple Try-Catch Example

```java
public class BasicTryCatchExample {
    
    public void divideNumbers() {
        try {
            int a = 10;
            int b = 0;
            int result = a / b; // This will throw ArithmeticException
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Error: Cannot divide by zero!");
            System.out.println("Exception message: " + e.getMessage());
        }
        
        System.out.println("Program continues after exception handling");
    }
    
    public static void main(String[] args) {
        BasicTryCatchExample example = new BasicTryCatchExample();
        example.divideNumbers();
    }
}

// Output:
// Error: Cannot divide by zero!
// Exception message: / by zero
// Program continues after exception handling
```

### Multiple Catch Blocks

```java
public class MultipleCatchExample {
    
    public void handleDifferentExceptions(String[] args) {
        try {
            // Multiple operations that can throw different exceptions
            int[] numbers = {1, 2, 3};
            
            if (args.length > 0) {
                int index = Integer.parseInt(args[0]); // NumberFormatException
                int value = numbers[index]; // ArrayIndexOutOfBoundsException
                int result = 100 / value; // ArithmeticException
                System.out.println("Result: " + result);
            }
            
        } catch (NumberFormatException e) {
            System.out.println("Invalid number format: " + e.getMessage());
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Array index out of bounds: " + e.getMessage());
        } catch (ArithmeticException e) {
            System.out.println("Arithmetic error: " + e.getMessage());
        } catch (Exception e) {
            // Generic catch block - should be last
            System.out.println("Unexpected error: " + e.getMessage());
        }
    }
    
    public static void main(String[] args) {
        MultipleCatchExample example = new MultipleCatchExample();
        
        // Test different scenarios
        example.handleDifferentExceptions(new String[]{"abc"}); // NumberFormatException
        example.handleDifferentExceptions(new String[]{"5"});   // ArrayIndexOutOfBoundsException
        example.handleDifferentExceptions(new String[]{"0"});   // ArithmeticException
    }
}
```

### Try-Catch-Finally Example

```java
public class TryCatchFinallyExample {
    
    public void demonstrateFinally() {
        FileReader file = null;
        
        try {
            System.out.println("Opening file...");
            file = new FileReader("test.txt");
            
            // Simulate reading file
            System.out.println("Reading file...");
            
            // Simulate an exception
            int result = 10 / 0; // ArithmeticException
            
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + e.getMessage());
        } catch (ArithmeticException e) {
            System.out.println("Math error: " + e.getMessage());
        } finally {
            // This block ALWAYS executes
            System.out.println("Cleaning up resources...");
            if (file != null) {
                try {
                    file.close();
                    System.out.println("File closed successfully");
                } catch (IOException e) {
                    System.out.println("Error closing file: " + e.getMessage());
                }
            }
        }
        
        System.out.println("Method completed");
    }
    
    public static void main(String[] args) {
        TryCatchFinallyExample example = new TryCatchFinallyExample();
        example.demonstrateFinally();
    }
}

// Output:
// Opening file...
// Math error: / by zero
// Cleaning up resources...
// Error closing file: Stream closed
// Method completed
```

### Try-with-Resources (Java 7+)

```java
public class TryWithResourcesExample {
    
    // Automatic resource management - no need for finally block
    public void readFileWithAutoClose() {
        try (FileReader file = new FileReader("test.txt");
             BufferedReader reader = new BufferedReader(file)) {
            
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
            
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + e.getMessage());
        } catch (IOException e) {
            System.out.println("IO error: " + e.getMessage());
        }
        // Resources are automatically closed here!
    }
    
    // Multiple resources
    public void copyFile(String source, String destination) {
        try (FileInputStream input = new FileInputStream(source);
             FileOutputStream output = new FileOutputStream(destination)) {
            
            byte[] buffer = new byte[1024];
            int bytesRead;
            
            while ((bytesRead = input.read(buffer)) != -1) {
                output.write(buffer, 0, bytesRead);
            }
            
            System.out.println("File copied successfully!");
            
        } catch (IOException e) {
            System.out.println("Error copying file: " + e.getMessage());
        }
    }
}
```

---

## 3. Checked vs Unchecked Exceptions {#checked-vs-unchecked}

### Understanding the Difference

```java
public class CheckedVsUncheckedExample {
    
    // CHECKED EXCEPTIONS - Must be handled at compile time
    
    // Method must declare throws or handle the exception
    public void checkedExceptionExample() throws IOException, SQLException {
        
        // 1. File operations - IOException (checked)
        try {
            FileReader file = new FileReader("data.txt");
            file.read();
            file.close();
        } catch (IOException e) {
            System.out.println("File error: " + e.getMessage());
            throw e; // Re-throwing the exception
        }
        
        // 2. Database operations - SQLException (checked)
        try {
            Class.forName("com.mysql.jdbc.Driver");
            Connection conn = DriverManager.getConnection("jdbc:mysql://localhost/test");
        } catch (ClassNotFoundException | SQLException e) {
            System.out.println("Database error: " + e.getMessage());
            throw new SQLException("Database connection failed", e);
        }
    }
    
    // UNCHECKED EXCEPTIONS - Optional to handle
    
    public void uncheckedExceptionExample() {
        
        // 1. Null Pointer Exception
        handleNullPointer();
        
        // 2. Array Index Out of Bounds
        handleArrayBounds();
        
        // 3. Number Format Exception
        handleNumberFormat();
        
        // 4. Arithmetic Exception
        handleArithmetic();
    }
    
    private void handleNullPointer() {
        try {
            String text = null;
            int length = text.length(); // NullPointerException
        } catch (NullPointerException e) {
            System.out.println("Null pointer error: Object is null");
        }
    }
    
    private void handleArrayBounds() {
        try {
            int[] array = {1, 2, 3};
            int value = array[5]; // ArrayIndexOutOfBoundsException
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Array error: Index out of bounds");
        }
    }
    
    private void handleNumberFormat() {
        try {
            int number = Integer.parseInt("abc"); // NumberFormatException
        } catch (NumberFormatException e) {
            System.out.println("Number format error: Invalid number");
        }
    }
    
    private void handleArithmetic() {
        try {
            int result = 10 / 0; // ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Math error: Division by zero");
        }
    }
    
    public static void main(String[] args) {
        CheckedVsUncheckedExample example = new CheckedVsUncheckedExample();
        
        try {
            example.checkedExceptionExample();
        } catch (IOException | SQLException e) {
            System.out.println("Handled checked exception: " + e.getMessage());
        }
        
        example.uncheckedExceptionExample();
    }
}
```

---

## 4. Throw and Throws Keywords {#throw-and-throws}

### Understanding Throw vs Throws

```java
public class ThrowAndThrowsExample {
    
    // THROWS - declares that method might throw exceptions
    public void methodWithThrows() throws IOException, IllegalArgumentException {
        System.out.println("This method might throw exceptions");
        
        // Conditions that might throw exceptions
        boolean fileExists = false;
        if (!fileExists) {
            throw new IOException("File not found");
        }
    }
    
    // THROW - actually throws an exception
    public void validateAge(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative: " + age);
        }
        if (age > 150) {
            throw new IllegalArgumentException("Age cannot be greater than 150: " + age);
        }
        System.out.println("Valid age: " + age);
    }
    
    // Example: Bank Account Operations
    public class BankAccount {
        private double balance;
        private String accountNumber;
        
        public BankAccount(String accountNumber, double initialBalance) {
            if (accountNumber == null || accountNumber.trim().isEmpty()) {
                throw new IllegalArgumentException("Account number cannot be null or empty");
            }
            if (initialBalance < 0) {
                throw new IllegalArgumentException("Initial balance cannot be negative");
            }
            
            this.accountNumber = accountNumber;
            this.balance = initialBalance;
        }
        
        public void withdraw(double amount) throws InsufficientFundsException {
            if (amount <= 0) {
                throw new IllegalArgumentException("Withdrawal amount must be positive");
            }
            
            if (amount > balance) {
                throw new InsufficientFundsException(
                    "Insufficient funds. Balance: " + balance + ", Requested: " + amount);
            }
            
            balance -= amount;
            System.out.println("Withdrawal successful. New balance: " + balance);
        }
        
        public void deposit(double amount) {
            if (amount <= 0) {
                throw new IllegalArgumentException("Deposit amount must be positive");
            }
            
            balance += amount;
            System.out.println("Deposit successful. New balance: " + balance);
        }
        
        public double getBalance() {
            return balance;
        }
    }
    
    // Custom Exception Class
    public class InsufficientFundsException extends Exception {
        public InsufficientFundsException(String message) {
            super(message);
        }
        
        public InsufficientFundsException(String message, Throwable cause) {
            super(message, cause);
        }
    }
    
    // Demonstration method
    public void demonstrateThrowAndThrows() {
        
        // 1. Testing throw with validation
        try {
            validateAge(25);  // Valid
            validateAge(-5);  // Throws IllegalArgumentException
        } catch (IllegalArgumentException e) {
            System.out.println("Age validation error: " + e.getMessage());
        }
        
        // 2. Testing throws with bank account
        try {
            BankAccount account = new BankAccount("ACC123", 1000.0);
            
            account.deposit(500.0);   // Works fine
            account.withdraw(200.0);  // Works fine
            account.withdraw(2000.0); // Throws InsufficientFundsException
            
        } catch (IllegalArgumentException e) {
            System.out.println("Account error: " + e.getMessage());
        } catch (InsufficientFundsException e) {
            System.out.println("Transaction error: " + e.getMessage());
        }
        
        // 3. Testing method that declares throws
        try {
            methodWithThrows();
        } catch (IOException e) {
            System.out.println("IO Error: " + e.getMessage());
        } catch (IllegalArgumentException e) {
            System.out.println("Argument Error: " + e.getMessage());
        }
    }
    
    public static void main(String[] args) {
        ThrowAndThrowsExample example = new ThrowAndThrowsExample();
        example.demonstrateThrowAndThrows();
    }
}
```

---

## 5. Custom Exceptions {#custom-exceptions}

### Creating and Using Custom Exceptions

```java
// Custom Checked Exception
class InvalidEmailException extends Exception {
    private String email;
    
    public InvalidEmailException(String email) {
        super("Invalid email format: " + email);
        this.email = email;
    }
    
    public InvalidEmailException(String email, String message) {
        super(message);
        this.email = email;
    }
    
    public String getEmail() {
        return email;
    }
}

// Custom Unchecked Exception
class InsufficientBalanceException extends RuntimeException {
    private double requestedAmount;
    private double availableBalance;
    
    public InsufficientBalanceException(double requestedAmount, double availableBalance) {
        super(String.format("Insufficient balance. Requested: %.2f, Available: %.2f", 
                           requestedAmount, availableBalance));
        this.requestedAmount = requestedAmount;
        this.availableBalance = availableBalance;
    }
    
    public double getRequestedAmount() {
        return requestedAmount;
    }
    
    public double getAvailableBalance() {
        return availableBalance;
    }
}

// Application-specific Exception
class UserRegistrationException extends Exception {
    public enum ErrorType {
        INVALID_EMAIL,
        DUPLICATE_USERNAME,
        WEAK_PASSWORD,
        INVALID_AGE
    }
    
    private ErrorType errorType;
    
    public UserRegistrationException(ErrorType errorType, String message) {
        super(message);
        this.errorType = errorType;
    }
    
    public ErrorType getErrorType() {
        return errorType;
    }
}

// Using Custom Exceptions
public class CustomExceptionExample {
    
    // User Registration System
    public class UserService {
        private Set<String> existingUsernames = new HashSet<>();
        
        public void registerUser(String username, String email, String password, int age) 
                throws UserRegistrationException, InvalidEmailException {
            
            // Validate email
            validateEmail(email);
            
            // Check for duplicate username
            if (existingUsernames.contains(username)) {
                throw new UserRegistrationException(
                    UserRegistrationException.ErrorType.DUPLICATE_USERNAME,
                    "Username '" + username + "' is already taken"
                );
            }
            
            // Validate password
            if (password.length() < 8) {
                throw new UserRegistrationException(
                    UserRegistrationException.ErrorType.WEAK_PASSWORD,
                    "Password must be at least 8 characters long"
                );
            }
            
            // Validate age
            if (age < 18) {
                throw new UserRegistrationException(
                    UserRegistrationException.ErrorType.INVALID_AGE,
                    "User must be at least 18 years old"
                );
            }
            
            // Registration successful
            existingUsernames.add(username);
            System.out.println("User registered successfully: " + username);
        }
        
        private void validateEmail(String email) throws InvalidEmailException {
            if (email == null || !email.contains("@") || !email.contains(".")) {
                throw new InvalidEmailException(email);
            }
        }
    }
    
    // E-commerce Shopping Cart
    public class ShoppingCart {
        private double balance;
        private List<String> items;
        
        public ShoppingCart(double initialBalance) {
            this.balance = initialBalance;
            this.items = new ArrayList<>();
        }
        
        public void purchaseItem(String item, double price) {
            if (price > balance) {
                throw new InsufficientBalanceException(price, balance);
            }
            
            balance -= price;
            items.add(item);
            System.out.println("Purchased: " + item + " for $" + price);
            System.out.println("Remaining balance: $" + balance);
        }
        
        public double getBalance() {
            return balance;
        }
        
        public List<String> getItems() {
            return new ArrayList<>(items);
        }
    }
    
    // Demonstration
    public void demonstrateCustomExceptions() {
        
        // 1. User Registration Examples
        UserService userService = new UserService();
        
        // Valid registration
        try {
            userService.registerUser("john_doe", "john@email.com", "password123", 25);
        } catch (UserRegistrationException | InvalidEmailException e) {
            System.out.println("Registration failed: " + e.getMessage());
        }
        
        // Invalid email
        try {
            userService.registerUser("jane_doe", "invalid-email", "password123", 25);
        } catch (InvalidEmailException e) {
            System.out.println("Email error: " + e.getMessage());
            System.out.println("Invalid email was: " + e.getEmail());
        } catch (UserRegistrationException e) {
            System.out.println("Registration error: " + e.getMessage());
        }
        
        // Duplicate username
        try {
            userService.registerUser("john_doe", "john2@email.com", "password123", 25);
        } catch (UserRegistrationException e) {
            System.out.println("Registration error: " + e.getMessage());
            System.out.println("Error type: " + e.getErrorType());
        } catch (InvalidEmailException e) {
            System.out.println("Email error: " + e.getMessage());
        }
        
        // 2. Shopping Cart Examples
        ShoppingCart cart = new ShoppingCart(100.0);
        
        try {
            cart.purchaseItem("Book", 25.0);     // Success
            cart.purchaseItem("Laptop", 80.0);   // Should fail - insufficient balance
        } catch (InsufficientBalanceException e) {
            System.out.println("Purchase failed: " + e.getMessage());
            System.out.println("You tried to spend: $" + e.getRequestedAmount());
            System.out.println("You only have: $" + e.getAvailableBalance());
        }
    }
    
    public static void main(String[] args) {
        CustomExceptionExample example = new CustomExceptionExample();
        example.demonstrateCustomExceptions();
    }
}
```

---

## 6. Exception Hierarchy {#exception-hierarchy}

### Java Exception Class Hierarchy

```java
/*
Exception Hierarchy:

java.lang.Object
    └── java.lang.Throwable
        ├── java.lang.Error (Unchecked)
        │   ├── OutOfMemoryError
        │   ├── StackOverflowError
        │   └── VirtualMachineError
        └── java.lang.Exception (Checked)
            ├── IOException
            │   ├── FileNotFoundException
            │   └── SocketException
            ├── SQLException
            ├── ClassNotFoundException
            └── java.lang.RuntimeException (Unchecked)
                ├── NullPointerException
                ├── ArrayIndexOutOfBoundsException
                ├── ArithmeticException
                ├── NumberFormatException
                ├── IllegalArgumentException
                └── ClassCastException
*/

public class ExceptionHierarchyExample {
    
    public void demonstrateHierarchy() {
        
        // 1. Catching parent exception catches all child exceptions
        try {
            // This could throw any RuntimeException
            riskyOperation();
        } catch (RuntimeException e) {
            // This catches NullPointerException, ArithmeticException, etc.
            System.out.println("Caught runtime exception: " + e.getClass().getSimpleName());
            System.out.println("Message: " + e.getMessage());
        }
        
        // 2. Specific exceptions should be caught before general ones
        try {
            anotherRiskyOperation();
        } catch (FileNotFoundException e) {
            // Most specific exception first
            System.out.println("File not found: " + e.getMessage());
        } catch (IOException e) {
            // More general IOException
            System.out.println("IO error: " + e.getMessage());
        } catch (Exception e) {
            // Most general exception last
            System.out.println("Unexpected error: " + e.getMessage());
        }
        
        // 3. Multi-catch for exceptions at same level
        try {
            String operation = "divide";
            if (operation.equals("divide")) {
                int result = 10 / 0; // ArithmeticException
            } else if (operation.equals("parse")) {
                int number = Integer.parseInt("abc"); // NumberFormatException
            }
        } catch (ArithmeticException | NumberFormatException e) {
            // Catching multiple exceptions of same hierarchy level
            System.out.println("Math or parsing error: " + e.getMessage());
        }
    }
    
    private void riskyOperation() {
        // Simulate different runtime exceptions
        Random random = new Random();
        int choice = random.nextInt(3);
        
        switch (choice) {
            case 0:
                String str = null;
                str.length(); // NullPointerException
                break;
            case 1:
                int result = 10 / 0; // ArithmeticException
                break;
            case 2:
                int[] array = {1, 2, 3};
                int value = array[5]; // ArrayIndexOutOfBoundsException
                break;
        }
    }
    
    private void anotherRiskyOperation() throws IOException {
        // Simulate different checked exceptions
        Random random = new Random();
        boolean fileExists = random.nextBoolean();
        
        if (!fileExists) {
            throw new FileNotFoundException("Required file is missing");
        } else {
            throw new IOException("General IO error occurred");
        }
    }
    
    // Exception information methods
    public void printExceptionInfo(Exception e) {
        System.out.println("=== Exception Information ===");
        System.out.println("Exception Type: " + e.getClass().getName());
        System.out.println("Message: " + e.getMessage());
        System.out.println("Stack Trace:");
        e.printStackTrace();
        
        // Get cause if exists
        Throwable cause = e.getCause();
        if (cause != null) {
            System.out.println("Caused by: " + cause.getClass().getName());
            System.out.println("Cause message: " + cause.getMessage());
        }
    }
    
    public static void main(String[] args) {
        ExceptionHierarchyExample example = new ExceptionHierarchyExample();
        example.demonstrateHierarchy();
    }
}
```

---

## 7. Best Practices {#best-practices}

### Exception Handling Best Practices

```java
public class ExceptionBestPractices {
    
    // ✅ GOOD PRACTICES
    
    // 1. Be specific with exception types
    public void goodSpecificException() {
        try {
            int age = Integer.parseInt("25");
            validateAge(age);
        } catch (NumberFormatException e) {
            System.out.println("Invalid number format: " + e.getMessage());
        } catch (IllegalArgumentException e) {
            System.out.println("Invalid age value: " + e.getMessage());
        }
    }
    
    // 2. Provide meaningful error messages
    public void validateAge(int age) {
        if (age < 0) {
            throw new IllegalArgumentException(
                "Age cannot be negative. Provided age: " + age);
        }
        if (age > 150) {
            throw new IllegalArgumentException(
                "Age cannot be greater than 150. Provided age: " + age);
        }
    }
    
    // 3. Use try-with-resources for automatic resource management
    public void goodResourceManagement() {
        try (FileReader reader = new FileReader("data.txt");
             BufferedReader bufferedReader = new BufferedReader(reader)) {
            
            String line = bufferedReader.readLine();
            System.out.println("First line: " + line);
            
        } catch (IOException e) {
            System.out.println("Error reading file: " + e.getMessage());
        }
        // Resources automatically closed
    }
    
    // 4. Log exceptions properly
    public void goodLogging() {
        try {
            processData();
        } catch (Exception e) {
            // Log the full exception with stack trace
            System.err.println("Error processing data: " + e.getMessage());
            e.printStackTrace();
            
            // You could also use a logging framework like Log4j
            // logger.error("Error processing data", e);
        }
    }
    
    // 5. Don't ignore exceptions
    public void goodExceptionHandling() {
        try {
            riskyOperation();
        } catch (Exception e) {
            // Handle appropriately - don't just ignore
            System.out.println("Operation failed: " + e.getMessage());
            
            // Take corrective action
            performFallbackOperation();
        }
    }
    
    // 6. Validate input parameters
    public double calculateInterest(double principal, double rate, int years) {
        if (principal <= 0) {
            throw new IllegalArgumentException("Principal must be positive: " + principal);
        }
        if (rate < 0) {
            throw new IllegalArgumentException("Rate cannot be negative: " + rate);
        }
        if (years <= 0) {
            throw new IllegalArgumentException("Years must be positive: " + years);
        }
        
        return principal * rate * years / 100;
    }
    
    // ❌ BAD PRACTICES (What NOT to do)
    
    // 1. Don't catch generic Exception unless necessary
    public void badGenericCatch() {
        try {
            someOperation();
        } catch (Exception e) {
            // Too generic - masks specific problems
            System.out.println("Something went wrong");
        }
    }
    
    // 2. Don't ignore exceptions
    public void badIgnoreException() {
        try {
            riskyOperation();
        } catch (Exception e) {
            // NEVER do this - silently ignoring exceptions
            // This makes debugging very difficult
        }
    }
    
    // 3. Don't use exceptions for control flow
    public void badControlFlow() {
        try {
            for (int i = 0; ; i++) {
                int value = getArrayValue(i);
                System.out.println(value);
            }
        } catch (ArrayIndexOutOfBoundsException e) {
            // Bad - using exception to exit loop
            System.out.println("End of array");
        }
    }
    
    // 4. Don't throw exceptions in finally block
    public void badFinallyException() {
        try {
            riskyOperation();
        } catch (Exception e) {
            System.out.println("Caught: " + e.getMessage());
        } finally {
            // Bad - this exception will mask the original exception
            // throw new RuntimeException("Finally block exception");
        }
    }
    
    // 5. Don't lose original exception information
    public void badExceptionWrapping() throws Exception {
        try {
            riskyOperation();
        } catch (Exception e) {
            // Bad - losing original exception information
            throw new Exception("Operation failed");
            
            // Good way:
            // throw new Exception("Operation failed", e);
        }
    }
    
    // Helper methods
    private void processData() throws Exception {
        throw new Exception("Data processing error");
    }
    
    private void riskyOperation() throws Exception {
        throw new Exception("Risky operation failed");
    }
    
    private void performFallbackOperation() {
        System.out.println("Performing fallback operation");
    }
    
    private void someOperation() throws IOException {
        throw new IOException("Some operation failed");
    }
    
    private int getArrayValue(int index) {
        int[] array = {1, 2, 3, 4, 5};
        return array[index]; // Will throw exception when index >= 5
    }
    
    public static void main(String[] args) {
        ExceptionBestPractices example = new ExceptionBestPractices();
        
        // Demonstrate good practices
        example.goodSpecificException();
        example.goodResourceManagement();
        example.goodLogging();
        example.goodExceptionHandling();
        
        try {
            double interest = example.calculateInterest(1000, 5, 2);
            System.out.println("Interest: " + interest);
        } catch (IllegalArgumentException e) {
            System.out.println("Invalid input: " + e.getMessage());
        }
    }
}
```

---

## 8. Interview Questions {#interview-questions}

### Basic Level Questions

**Q1: What is exception handling in Java?**
A: Exception handling is a mechanism to handle runtime errors gracefully, preventing program termination and providing meaningful error messages to users.

**Q2: What's the difference between checked and unchecked exceptions?**
A: 
- **Checked exceptions** must be handled at compile time (IOException, SQLException)
- **Unchecked exceptions** are optional to handle (NullPointerException, ArithmeticException)

**Q3: What's the difference between throw and throws?**
A:
- **throw**: Used to actually throw an exception
- **throws**: Used to declare that a method might throw exceptions

### Intermediate Level Questions

**Q4: What happens if an exception is thrown in a finally block?**
A: If an exception is thrown in a finally block, it will mask any exception that was thrown in the try or catch block. The original exception will be lost.

**Q5: Can we have multiple catch blocks for a single try block?**
A: Yes, but specific exceptions must be caught before general ones. You cannot catch a parent exception before its child exceptions.

**Q6: What is try-with-resources and when should you use it?**
A: Try-with-resources automatically closes resources that implement AutoCloseable. Use it for file operations, database connections, etc.

```java
// Example
try (FileReader reader = new FileReader("file.txt")) {
    // Use reader
} catch (IOException e) {
    // Handle exception
}
// Reader automatically closed
```

### Advanced Level Questions

**Q7: How do you create a custom exception?**
A: Extend Exception class for checked exceptions or RuntimeException for unchecked exceptions:

```java
public class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}
```

**Q8: What is exception chaining?**
A: Exception chaining allows you to associate one exception with another, preserving the original exception information:

```java
try {
    // Some operation
} catch (IOException e) {
    throw new ServiceException("Service failed", e);
}
```

**Q9: Can a finally block prevent a method from returning?**
A: Yes, if the finally block throws an exception or has a return statement, it can prevent the method from returning the original value.

---

## 9. Common Mistakes (Wrong vs Right Code) {#common-mistakes}

### Mistake 1: Ignoring Exceptions

**❌ Wrong Code:**
```java
public void badExample() {
    try {
        int result = Integer.parseInt("abc");
        System.out.println(result);
    } catch (NumberFormatException e) {
        // Silently ignoring - BAD!
    }
}
```

**✅ Right Code:**
```java
public void goodExample() {
    try {
        int result = Integer.parseInt("abc");
        System.out.println(result);
    } catch (NumberFormatException e) {
        System.out.println("Invalid number format: " + e.getMessage());
        // Or log the error, show user message, etc.
    }
}
```

### Mistake 2: Catching Generic Exception

**❌ Wrong Code:**
```java
public void badGenericCatch() {
    try {
        readFile();
        parseData();
        saveToDatabase();
    } catch (Exception e) {
        // Too generic - can't handle different problems appropriately
        System.out.println("Something went wrong");
    }
}
```

**✅ Right Code:**
```java
public void goodSpecificCatch() {
    try {
        readFile();
        parseData();
        saveToDatabase();
    } catch (IOException e) {
        System.out.println("File error: " + e.getMessage());
    } catch (NumberFormatException e) {
        System.out.println("Data parsing error: " + e.getMessage());
    } catch (SQLException e) {
        System.out.println("Database error: " + e.getMessage());
    }
}
```

### Mistake 3: Not Using Try-with-Resources

**❌ Wrong Code:**
```java
public void badResourceManagement() {
    FileReader reader = null;
    try {
        reader = new FileReader("file.txt");
        // Use reader
    } catch (IOException e) {
        System.out.println("Error: " + e.getMessage());
    } finally {
        if (reader != null) {
            try {
                reader.close(); // Might throw exception too!
            } catch (IOException e) {
                System.out.println("Error closing file");
            }
        }
    }
}
```

**✅ Right Code:**
```java
public void goodResourceManagement() {
    try (FileReader reader = new FileReader("file.txt")) {
        // Use reader
    } catch (IOException e) {
        System.out.println("Error: " + e.getMessage());
    }
    // Reader automatically closed, even if exception occurs
}
```

### Mistake 4: Wrong Exception Order

**❌ Wrong Code:**
```java
public void badExceptionOrder() {
    try {
        riskyOperation();
    } catch (Exception e) {
        // This catches everything
        System.out.println("General error");
    } catch (IOException e) {
        // This will never be reached - compilation error!
        System.out.println("IO error");
    }
}
```

**✅ Right Code:**
```java
public void goodExceptionOrder() {
    try {
        riskyOperation();
    } catch (FileNotFoundException e) {
        // Most specific first
        System.out.println("File not found");
    } catch (IOException e) {
        // More general
        System.out.println("IO error");
    } catch (Exception e) {
        // Most general last
        System.out.println("General error");
    }
}
```

### Mistake 5: Losing Exception Information

**❌ Wrong Code:**
```java
public void processData() throws DataProcessingException {
    try {
        // Some operation that throws IOException
        readAndParseData();
    } catch (IOException e) {
        // Lost original exception information
        throw new DataProcessingException("Processing failed");
    }
}
```

**✅ Right Code:**
```java
public void processData() throws DataProcessingException {
    try {
        readAndParseData();
    } catch (IOException e) {
        // Preserve original exception as cause
        throw new DataProcessingException("Processing failed", e);
    }
}
```

---

## Summary

Exception handling is crucial for writing robust Java applications. Key takeaways:

### **Core Concepts:**
1. **Try-Catch-Finally**: Handle exceptions gracefully
2. **Checked vs Unchecked**: Understand when handling is required
3. **Throw vs Throws**: Know when to use each keyword
4. **Custom Exceptions**: Create meaningful, specific exceptions
5. **Exception Hierarchy**: Use appropriate exception types

### **Best Practices:**
- Be specific with exception types
- Provide meaningful error messages
- Use try-with-resources for automatic cleanup
- Don't ignore exceptions
- Log exceptions properly
- Validate input parameters

### **Common Pitfalls to Avoid:**
- Catching generic Exception unnecessarily
- Silently ignoring exceptions
- Wrong exception catching order
- Not using try-with-resources
- Losing original exception information

Remember: Good exception handling makes your code more reliable, maintainable, and user-friendly!
