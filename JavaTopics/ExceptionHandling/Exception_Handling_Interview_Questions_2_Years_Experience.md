# Exception Handling Interview Questions - 2 Years Java Experience

## Must Know Questions (Core Concepts)

### 1. What is Exception Handling in Java and why is it important?

**Answer:**
Exception handling is a mechanism to handle runtime errors gracefully without terminating the program.

**Key Points:**
- Prevents program crashes
- Provides meaningful error messages
- Allows graceful recovery from errors
- Improves user experience

**Code Example:**
```java
// Without exception handling - program crashes
public void badExample() {
    int result = 10 / 0; // Program terminates here
    System.out.println("This line never executes");
}

// With exception handling - program continues
public void goodExample() {
    try {
        int result = 10 / 0;
        System.out.println("Result: " + result);
    } catch (ArithmeticException e) {
        System.out.println("Cannot divide by zero!");
    }
    System.out.println("Program continues..."); // This executes
}
```

### 2. What's the difference between Checked and Unchecked exceptions?

**Answer:**

**Checked Exceptions:**
- Must be handled at compile time
- Extend Exception class (but not RuntimeException)
- Examples: IOException, SQLException, ClassNotFoundException

**Unchecked Exceptions:**
- Optional to handle
- Extend RuntimeException class
- Examples: NullPointerException, ArithmeticException, ArrayIndexOutOfBoundsException

**Code Example:**
```java
public class CheckedVsUnchecked {
    
    // Checked - Must handle or declare throws
    public void readFile() throws IOException {
        FileReader file = new FileReader("test.txt"); // Checked exception
    }
    
    // Unchecked - Optional to handle
    public void divide() {
        int result = 10 / 0; // Unchecked exception - optional to handle
    }
}
```

### 3. Explain the difference between throw and throws keywords.

**Answer:**

**throw:**
- Used to actually throw an exception
- Used inside method body
- Can throw only one exception at a time

**throws:**
- Used to declare that method might throw exceptions
- Used in method signature
- Can declare multiple exceptions

**Code Example:**
```java
public class ThrowVsThrows {
    
    // throws - declares exceptions method might throw
    public void validateAge(int age) throws IllegalArgumentException {
        
        // throw - actually throws the exception
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
    }
    
    // Multiple exceptions with throws
    public void processFile() throws IOException, SQLException {
        // Method implementation
    }
}
```

### 4. What is the purpose of finally block? When is it executed?

**Answer:**
Finally block contains code that always executes, regardless of whether an exception occurs or not.

**When Finally Executes:**
- After try block (if no exception)
- After catch block (if exception occurs)
- Even if return statement is in try/catch
- Does NOT execute only if JVM exits (System.exit())

**Code Example:**
```java
public void demonstrateFinally() {
    try {
        System.out.println("Try block");
        int result = 10 / 0; // Exception occurs
        return; // This return won't prevent finally
    } catch (ArithmeticException e) {
        System.out.println("Catch block");
        return; // This return won't prevent finally
    } finally {
        System.out.println("Finally block - ALWAYS executes");
    }
}

// Output:
// Try block
// Catch block  
// Finally block - ALWAYS executes
```

### 5. What is try-with-resources? When should you use it?

**Answer:**
Try-with-resources automatically closes resources that implement AutoCloseable interface.

**Benefits:**
- Automatic resource management
- No need for finally block
- Exception suppression handling
- Cleaner code

**Code Example:**
```java
// Old way - manual resource management
public void oldWay() {
    FileReader reader = null;
    try {
        reader = new FileReader("file.txt");
        // Use reader
    } catch (IOException e) {
        System.out.println("Error: " + e.getMessage());
    } finally {
        if (reader != null) {
            try {
                reader.close(); // Manual closing
            } catch (IOException e) {
                System.out.println("Error closing file");
            }
        }
    }
}

// New way - try-with-resources
public void newWay() {
    try (FileReader reader = new FileReader("file.txt")) {
        // Use reader
    } catch (IOException e) {
        System.out.println("Error: " + e.getMessage());
    }
    // Reader automatically closed
}
```

### 6. Can you have multiple catch blocks? What's the correct order?

**Answer:**
Yes, you can have multiple catch blocks. The order must be from most specific to most general.

**Rule:** Child exceptions must be caught before parent exceptions.

**Code Example:**
```java
public void multipleCatchExample() {
    try {
        String str = args[0];
        int num = Integer.parseInt(str);
        int result = 100 / num;
        
    } catch (ArrayIndexOutOfBoundsException e) {
        // Most specific
        System.out.println("No arguments provided");
        
    } catch (NumberFormatException e) {
        // Specific
        System.out.println("Invalid number format");
        
    } catch (ArithmeticException e) {
        // Specific
        System.out.println("Division by zero");
        
    } catch (RuntimeException e) {
        // More general
        System.out.println("Runtime error");
        
    } catch (Exception e) {
        // Most general - must be last
        System.out.println("Unexpected error");
    }
}
```

### 7. How do you create custom exceptions?

**Answer:**
Custom exceptions are created by extending Exception class (checked) or RuntimeException class (unchecked).

**Code Example:**
```java
// Custom checked exception
class InsufficientBalanceException extends Exception {
    private double requestedAmount;
    private double availableBalance;
    
    public InsufficientBalanceException(double requested, double available) {
        super("Insufficient balance. Requested: " + requested + 
              ", Available: " + available);
        this.requestedAmount = requested;
        this.availableBalance = available;
    }
    
    // Getters
    public double getRequestedAmount() { return requestedAmount; }
    public double getAvailableBalance() { return availableBalance; }
}

// Custom unchecked exception
class InvalidAccountException extends RuntimeException {
    public InvalidAccountException(String message) {
        super(message);
    }
}

// Usage
public class BankAccount {
    private double balance;
    
    public void withdraw(double amount) throws InsufficientBalanceException {
        if (amount > balance) {
            throw new InsufficientBalanceException(amount, balance);
        }
        balance -= amount;
    }
}
```

### 8. What happens if an exception is thrown in finally block?

**Answer:**
If an exception is thrown in finally block, it will mask/suppress the original exception from try or catch block.

**Code Example:**
```java
public void problematicFinally() {
    try {
        throw new RuntimeException("Original exception");
    } catch (RuntimeException e) {
        System.out.println("Caught: " + e.getMessage());
        throw new RuntimeException("Exception from catch");
    } finally {
        throw new RuntimeException("Exception from finally");
    }
    // Only "Exception from finally" will be thrown!
    // Original exceptions are lost
}

// Better approach
public void betterFinally() {
    try {
        throw new RuntimeException("Original exception");
    } catch (RuntimeException e) {
        System.out.println("Caught: " + e.getMessage());
        throw e; // Re-throw original
    } finally {
        // Don't throw exceptions here
        System.out.println("Cleanup code only");
    }
}
```

### 9. What is exception propagation?

**Answer:**
Exception propagation is the process where an uncaught exception is passed up the call stack until it's handled or reaches the main method.

**Code Example:**
```java
public class ExceptionPropagation {
    
    public void method1() {
        method2(); // Exception propagates here
    }
    
    public void method2() {
        method3(); // Exception propagates here
    }
    
    public void method3() {
        int result = 10 / 0; // ArithmeticException occurs here
    }
    
    public static void main(String[] args) {
        try {
            new ExceptionPropagation().method1();
        } catch (ArithmeticException e) {
            // Exception caught here after propagating through call stack
            System.out.println("Caught propagated exception: " + e.getMessage());
        }
    }
}
```

### 10. Can a method override and change the exception specification?

**Answer:**
When overriding methods, you can only throw same exceptions, subclass exceptions, or no exceptions. You cannot throw broader exceptions.

**Code Example:**
```java
class Parent {
    public void method1() throws IOException {
        // Parent method throws IOException
    }
    
    public void method2() throws Exception {
        // Parent method throws Exception
    }
}

class Child extends Parent {
    
    // ✅ Valid - same exception
    @Override
    public void method1() throws IOException {
    }
    
    // ✅ Valid - subclass exception
    @Override 
    public void method1() throws FileNotFoundException { // FileNotFoundException extends IOException
    }
    
    // ✅ Valid - no exception
    @Override
    public void method1() {
    }
    
    // ❌ Invalid - broader exception
    // @Override
    // public void method1() throws Exception { // Compilation error!
    // }
    
    // ✅ Valid - more specific exceptions
    @Override
    public void method2() throws IOException, SQLException {
    }
}
```

## Tricky Questions (Scenario-Based)

### 11. What will be the output of this code?

```java
public class TrickyException {
    public static void main(String[] args) {
        try {
            System.out.println("1");
            return;
        } catch (Exception e) {
            System.out.println("2");
        } finally {
            System.out.println("3");
        }
        System.out.println("4");
    }
}
```

**Answer:** Output will be: `1` and `3`

**Explanation:**
- "1" prints from try block
- return statement doesn't prevent finally block
- "3" prints from finally block
- "4" never prints because method returns before reaching it

### 12. What's wrong with this exception handling?

```java
try {
    FileInputStream file = new FileInputStream("test.txt");
} catch (Exception e) {
    System.out.println("File error");
} catch (FileNotFoundException e) {
    System.out.println("File not found");
}
```

**Answer:** **Compilation Error!**

**Problem:** FileNotFoundException is a subclass of Exception. Since Exception is caught first, the FileNotFoundException catch block is unreachable.

**Solution:**
```java
try {
    FileInputStream file = new FileInputStream("test.txt");
} catch (FileNotFoundException e) {
    System.out.println("File not found");
} catch (IOException e) {
    System.out.println("IO error");
} catch (Exception e) {
    System.out.println("General error");
}
```

### 13. What happens in this code with nested try-catch?

```java
public void nestedTryCatch() {
    try {
        try {
            int result = 10 / 0;
        } catch (NullPointerException e) {
            System.out.println("Inner catch");
        }
    } catch (ArithmeticException e) {
        System.out.println("Outer catch");
    }
}
```

**Answer:** Output will be: `Outer catch`

**Explanation:**
- Inner try throws ArithmeticException
- Inner catch only handles NullPointerException
- Exception propagates to outer try-catch
- Outer catch handles ArithmeticException

### 14. What's the output when finally has return statement?

```java
public int finallyReturn() {
    try {
        return 1;
    } catch (Exception e) {
        return 2;
    } finally {
        return 3;
    }
}
```

**Answer:** Returns `3`

**Explanation:**
- Finally block's return statement overrides try block's return
- This is generally bad practice as it can mask exceptions
- Finally return takes precedence over try/catch returns

### 15. What happens with exception in constructor?

```java
class TestClass {
    public TestClass() throws Exception {
        throw new Exception("Constructor failed");
    }
}

public void createObject() {
    TestClass obj = null;
    try {
        obj = new TestClass();
    } catch (Exception e) {
        System.out.println("Exception caught: " + e.getMessage());
    }
    System.out.println("Object is: " + obj);
}
```

**Answer:** 
```
Exception caught: Constructor failed
Object is: null
```

**Explanation:**
- Constructor throws exception before object creation completes
- Object reference remains null
- Exception is caught and handled

### 16. What's the issue with this resource management?

```java
public void problematicResource() {
    FileInputStream input = null;
    FileOutputStream output = null;
    
    try {
        input = new FileInputStream("source.txt");
        output = new FileOutputStream("dest.txt"); // If this fails...
        
        // Copy data
        
    } catch (IOException e) {
        System.out.println("Error: " + e.getMessage());
    } finally {
        try {
            input.close();  // NullPointerException if input creation failed!
            output.close(); // NullPointerException if output creation failed!
        } catch (IOException e) {
            System.out.println("Error closing resources");
        }
    }
}
```

**Answer:** **NullPointerException Risk**

**Problem:** If FileOutputStream constructor fails, input is created but output is null. Finally block tries to close null references.

**Solution:**
```java
public void betterResource() {
    try (FileInputStream input = new FileInputStream("source.txt");
         FileOutputStream output = new FileOutputStream("dest.txt")) {
        
        // Copy data
        
    } catch (IOException e) {
        System.out.println("Error: " + e.getMessage());
    }
    // Resources automatically closed safely
}
```

### 17. What happens with multiple exceptions in try-with-resources?

```java
class ProblematicResource implements AutoCloseable {
    private String name;
    
    public ProblematicResource(String name) throws Exception {
        this.name = name;
        if (name.equals("bad")) {
            throw new Exception("Bad resource");
        }
    }
    
    public void doWork() throws Exception {
        throw new Exception("Work failed for " + name);
    }
    
    @Override
    public void close() throws Exception {
        throw new Exception("Close failed for " + name);
    }
}

public void multipleExceptions() {
    try (ProblematicResource resource = new ProblematicResource("good")) {
        resource.doWork(); // Throws exception
    } catch (Exception e) {
        System.out.println("Caught: " + e.getMessage());
        
        // Check suppressed exceptions
        Throwable[] suppressed = e.getSuppressed();
        for (Throwable t : suppressed) {
            System.out.println("Suppressed: " + t.getMessage());
        }
    }
}
```

**Answer:**
```
Caught: Work failed for good
Suppressed: Close failed for good
```

**Explanation:**
- doWork() throws primary exception
- close() throws suppressed exception
- Suppressed exceptions are attached to primary exception

### 18. What's wrong with this exception chaining?

```java
public void badChaining() throws ServiceException {
    try {
        // Some operation
        throw new IOException("File operation failed");
    } catch (IOException e) {
        // Losing original exception information
        throw new ServiceException("Service failed");
    }
}

public void goodChaining() throws ServiceException {
    try {
        // Some operation  
        throw new IOException("File operation failed");
    } catch (IOException e) {
        // Preserving original exception
        throw new ServiceException("Service failed", e);
    }
}
```

**Answer:** **Bad chaining loses original exception information**

**Problem:** Original IOException details are lost in bad chaining.

**Solution:** Always pass original exception as cause to preserve stack trace and debugging information.

## Advanced Questions (Architecture & Design)

### 19. How would you design a robust exception handling strategy for a multi-layered application?

**Answer:**

**Layer-wise Exception Handling:**

```java
// 1. Data Access Layer - Convert technical exceptions
@Repository
public class UserDAO {
    public User findById(Long id) throws DataAccessException {
        try {
            // Database operation
            return database.findUser(id);
        } catch (SQLException e) {
            throw new DataAccessException("Failed to retrieve user: " + id, e);
        }
    }
}

// 2. Service Layer - Business logic exceptions
@Service  
public class UserService {
    public User getUser(Long id) throws UserNotFoundException {
        try {
            User user = userDAO.findById(id);
            if (user == null) {
                throw new UserNotFoundException("User not found: " + id);
            }
            return user;
        } catch (DataAccessException e) {
            // Log technical error, throw business exception
            logger.error("Data access error", e);
            throw new ServiceException("Unable to retrieve user", e);
        }
    }
}

// 3. Controller Layer - Handle and respond appropriately
@RestController
public class UserController {
    
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        try {
            User user = userService.getUser(id);
            return ResponseEntity.ok(user);
        } catch (UserNotFoundException e) {
            return ResponseEntity.notFound().build();
        } catch (ServiceException e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("Service temporarily unavailable");
        }
    }
}
```

### 20. How do you handle exceptions in concurrent/multi-threaded environments?

**Answer:**

**Thread Exception Handling:**

```java
public class ThreadExceptionHandling {
    
    // 1. Handle exceptions in thread execution
    public void handleThreadExceptions() {
        Thread thread = new Thread(() -> {
            try {
                // Risky operation
                int result = 10 / 0;
            } catch (Exception e) {
                // Handle exception within thread
                System.err.println("Thread exception: " + e.getMessage());
                // Could notify main thread or log error
            }
        });
        
        thread.start();
    }
    
    // 2. Uncaught exception handler
    public void setUncaughtExceptionHandler() {
        Thread thread = new Thread(() -> {
            throw new RuntimeException("Uncaught exception");
        });
        
        thread.setUncaughtExceptionHandler((t, e) -> {
            System.err.println("Uncaught exception in thread " + 
                             t.getName() + ": " + e.getMessage());
            // Log error, cleanup resources, notify monitoring system
        });
        
        thread.start();
    }
    
    // 3. ExecutorService exception handling
    public void handleExecutorExceptions() {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        
        // Submit callable that can throw checked exceptions
        Future<String> future = executor.submit(() -> {
            if (Math.random() > 0.5) {
                throw new Exception("Task failed");
            }
            return "Success";
        });
        
        try {
            String result = future.get(); // This throws ExecutionException
            System.out.println("Result: " + result);
        } catch (ExecutionException e) {
            Throwable cause = e.getCause();
            System.err.println("Task failed: " + cause.getMessage());
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
            System.err.println("Task interrupted");
        } finally {
            executor.shutdown();
        }
    }
}
```

### 21. Design an exception handling framework for a REST API with proper HTTP status codes.

**Answer:**

**REST API Exception Framework:**

```java
// 1. Custom Exception Classes
@ResponseStatus(HttpStatus.NOT_FOUND)
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}

@ResponseStatus(HttpStatus.BAD_REQUEST)
public class InvalidRequestException extends RuntimeException {
    private Map<String, String> validationErrors;
    
    public InvalidRequestException(String message, Map<String, String> errors) {
        super(message);
        this.validationErrors = errors;
    }
    
    public Map<String, String> getValidationErrors() {
        return validationErrors;
    }
}

// 2. Global Exception Handler
@ControllerAdvice
public class GlobalExceptionHandler {
    
    private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFound(ResourceNotFoundException e) {
        ErrorResponse error = new ErrorResponse(
            "RESOURCE_NOT_FOUND",
            e.getMessage(),
            HttpStatus.NOT_FOUND.value()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    @ExceptionHandler(InvalidRequestException.class)
    public ResponseEntity<ErrorResponse> handleInvalidRequest(InvalidRequestException e) {
        ErrorResponse error = new ErrorResponse(
            "INVALID_REQUEST",
            e.getMessage(),
            HttpStatus.BAD_REQUEST.value(),
            e.getValidationErrors()
        );
        return ResponseEntity.badRequest().body(error);
    }
    
    @ExceptionHandler(DataAccessException.class)
    public ResponseEntity<ErrorResponse> handleDataAccess(DataAccessException e) {
        logger.error("Database error", e);
        ErrorResponse error = new ErrorResponse(
            "INTERNAL_ERROR",
            "Internal server error occurred",
            HttpStatus.INTERNAL_SERVER_ERROR.value()
        );
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
    
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneral(Exception e) {
        logger.error("Unexpected error", e);
        ErrorResponse error = new ErrorResponse(
            "INTERNAL_ERROR",
            "An unexpected error occurred",
            HttpStatus.INTERNAL_SERVER_ERROR.value()
        );
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}

// 3. Error Response DTO
public class ErrorResponse {
    private String errorCode;
    private String message;
    private int status;
    private LocalDateTime timestamp;
    private Map<String, String> validationErrors;
    
    public ErrorResponse(String errorCode, String message, int status) {
        this.errorCode = errorCode;
        this.message = message;
        this.status = status;
        this.timestamp = LocalDateTime.now();
    }
    
    public ErrorResponse(String errorCode, String message, int status, 
                        Map<String, String> validationErrors) {
        this(errorCode, message, status);
        this.validationErrors = validationErrors;
    }
    
    // Getters and setters
}
```

## Quick Review & Memory Tips

### Exception Hierarchy (Remember This):
```
Throwable
├── Error (Don't catch these)
│   ├── OutOfMemoryError
│   └── StackOverflowError
└── Exception
    ├── IOException (Checked)
    ├── SQLException (Checked)
    └── RuntimeException (Unchecked)
        ├── NullPointerException
        ├── ArithmeticException
        └── ArrayIndexOutOfBoundsException
```

### Key Rules to Remember:
1. **Specific before General**: Catch child exceptions before parent
2. **Try-with-resources**: Always use for AutoCloseable resources
3. **Don't ignore exceptions**: Always handle or log them
4. **Finally always runs**: Except when JVM exits
5. **Checked exceptions**: Must handle or declare throws
6. **Custom exceptions**: Extend Exception (checked) or RuntimeException (unchecked)

### Common Pitfalls:
- Catching generic Exception unnecessarily
- Wrong order of catch blocks
- Throwing exceptions in finally block
- Not using try-with-resources
- Losing original exception information
- Ignoring exceptions silently

### Interview Success Tips:
1. Always provide code examples
2. Explain the "why" behind exception handling
3. Discuss real-world scenarios
4. Show knowledge of best practices
5. Demonstrate understanding of exception hierarchy
6. Know when to use checked vs unchecked exceptions

---

**Remember:** Exception handling is not just about catching errors—it's about building robust, maintainable applications that gracefully handle unexpected situations!
