# JUnit 5 Hands-on Practice Guide

## 1. JUnit 5 Basics

### Problem 1: Calculator Class Testing

**Task**: Create a Calculator class and test its basic operations using JUnit 5 annotations and assertions.

#### Production Code:
```java
public class Calculator {
    
    public int add(int a, int b) {
        return a + b;
    }
    
    public int subtract(int a, int b) {
        return a - b;
    }
    
    public double divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Cannot divide by zero");
        }
        return (double) a / b;
    }
    
    public int multiply(int a, int b) {
        return a * b;
    }
}
```

#### Test Code:
```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {
    
    private Calculator calculator;
    
    @BeforeAll
    static void initAll() {
        System.out.println("Starting Calculator tests...");
    }
    
    @BeforeEach
    void init() {
        calculator = new Calculator();
        System.out.println("Calculator instance created");
    }
    
    @AfterEach
    void tearDown() {
        calculator = null;
        System.out.println("Calculator instance destroyed");
    }
    
    @AfterAll
    static void tearDownAll() {
        System.out.println("All Calculator tests completed");
    }
    
    @Test
    @DisplayName("Addition should work correctly")
    void testAddition() {
        assertEquals(5, calculator.add(2, 3));
        assertEquals(0, calculator.add(-1, 1));
        assertEquals(-5, calculator.add(-2, -3));
    }
    
    @Test
    @DisplayName("Subtraction should work correctly")
    void testSubtraction() {
        assertEquals(1, calculator.subtract(3, 2));
        assertEquals(-2, calculator.subtract(-1, 1));
        assertEquals(1, calculator.subtract(-2, -3));
    }
    
    @Test
    @DisplayName("Division should work correctly")
    void testDivision() {
        assertEquals(2.0, calculator.divide(4, 2));
        assertEquals(2.5, calculator.divide(5, 2));
        assertTrue(calculator.divide(1, 3) > 0.33);
    }
    
    @Test
    @DisplayName("Multiplication should work correctly")
    void testMultiplication() {
        assertEquals(6, calculator.multiply(2, 3));
        assertEquals(0, calculator.multiply(0, 5));
        assertEquals(-10, calculator.multiply(-2, 5));
    }
}
```

### Problem 2: Bank Account Management

**Task**: Create a BankAccount class with deposit, withdraw, and balance operations. Test using various assertions.

#### Production Code:
```java
public class BankAccount {
    private double balance;
    private String accountHolder;
    private boolean isActive;
    
    public BankAccount(String accountHolder, double initialBalance) {
        if (accountHolder == null || accountHolder.trim().isEmpty()) {
            throw new IllegalArgumentException("Account holder name cannot be null or empty");
        }
        if (initialBalance < 0) {
            throw new IllegalArgumentException("Initial balance cannot be negative");
        }
        this.accountHolder = accountHolder;
        this.balance = initialBalance;
        this.isActive = true;
    }
    
    public void deposit(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Deposit amount must be positive");
        }
        if (!isActive) {
            throw new IllegalStateException("Account is not active");
        }
        balance += amount;
    }
    
    public void withdraw(double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Withdrawal amount must be positive");
        }
        if (!isActive) {
            throw new IllegalStateException("Account is not active");
        }
        if (amount > balance) {
            throw new IllegalArgumentException("Insufficient funds");
        }
        balance -= amount;
    }
    
    public double getBalance() {
        return balance;
    }
    
    public String getAccountHolder() {
        return accountHolder;
    }
    
    public boolean isActive() {
        return isActive;
    }
    
    public void deactivateAccount() {
        isActive = false;
    }
}
```

#### Test Code:
```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

class BankAccountTest {
    
    private BankAccount account;
    
    @BeforeEach
    void setUp() {
        account = new BankAccount("John Doe", 1000.0);
    }
    
    @Test
    @DisplayName("Account creation should set initial values correctly")
    void testAccountCreation() {
        assertEquals(1000.0, account.getBalance());
        assertEquals("John Doe", account.getAccountHolder());
        assertTrue(account.isActive());
        assertNotNull(account);
    }
    
    @Test
    @DisplayName("Deposit should increase balance")
    void testDeposit() {
        account.deposit(500.0);
        assertEquals(1500.0, account.getBalance());
        
        account.deposit(250.75);
        assertEquals(1750.75, account.getBalance());
    }
    
    @Test
    @DisplayName("Withdrawal should decrease balance")
    void testWithdrawal() {
        account.withdraw(300.0);
        assertEquals(700.0, account.getBalance());
        
        account.withdraw(700.0);
        assertEquals(0.0, account.getBalance());
    }
    
    @Test
    @DisplayName("Account deactivation should work")
    void testAccountDeactivation() {
        assertTrue(account.isActive());
        
        account.deactivateAccount();
        assertFalse(account.isActive());
    }
    
    @Test
    @DisplayName("Creating account with null name should fail")
    void testNullAccountHolder() {
        assertThrows(IllegalArgumentException.class, () -> {
            new BankAccount(null, 100.0);
        });
        
        assertThrows(IllegalArgumentException.class, () -> {
            new BankAccount("", 100.0);
        });
    }
}
```

---

## 2. Parameterized Tests

### Problem 1: Number Validation Utility

**Task**: Create a utility class to validate numbers and test with multiple inputs using parameterized tests.

#### Production Code:
```java
public class NumberValidator {
    
    public boolean isEven(int number) {
        return number % 2 == 0;
    }
    
    public boolean isPrime(int number) {
        if (number <= 1) return false;
        if (number <= 3) return true;
        if (number % 2 == 0 || number % 3 == 0) return false;
        
        for (int i = 5; i * i <= number; i += 6) {
            if (number % i == 0 || number % (i + 2) == 0) {
                return false;
            }
        }
        return true;
    }
    
    public boolean isPositive(int number) {
        return number > 0;
    }
    
    public String getNumberType(int number) {
        if (number > 0) return "POSITIVE";
        if (number < 0) return "NEGATIVE";
        return "ZERO";
    }
}
```

#### Test Code:
```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;
import org.junit.jupiter.params.provider.CsvSource;
import static org.junit.jupiter.api.Assertions.*;

class NumberValidatorTest {
    
    private NumberValidator validator;
    
    @BeforeEach
    void setUp() {
        validator = new NumberValidator();
    }
    
    @ParameterizedTest
    @ValueSource(ints = {2, 4, 6, 8, 10, 100, -2, -4, 0})
    @DisplayName("Even numbers should return true")
    void testIsEven(int number) {
        assertTrue(validator.isEven(number));
    }
    
    @ParameterizedTest
    @ValueSource(ints = {1, 3, 5, 7, 9, 99, -1, -3})
    @DisplayName("Odd numbers should return false for isEven")
    void testIsOdd(int number) {
        assertFalse(validator.isEven(number));
    }
    
    @ParameterizedTest
    @ValueSource(ints = {2, 3, 5, 7, 11, 13, 17, 19, 23})
    @DisplayName("Prime numbers should return true")
    void testIsPrime(int number) {
        assertTrue(validator.isPrime(number));
    }
    
    @ParameterizedTest
    @ValueSource(ints = {1, 4, 6, 8, 9, 10, 12, 15, 20})
    @DisplayName("Non-prime numbers should return false")
    void testIsNotPrime(int number) {
        assertFalse(validator.isPrime(number));
    }
    
    @ParameterizedTest
    @CsvSource({
        "5, POSITIVE",
        "10, POSITIVE", 
        "100, POSITIVE",
        "-5, NEGATIVE",
        "-10, NEGATIVE",
        "-100, NEGATIVE",
        "0, ZERO"
    })
    @DisplayName("Number type classification should work correctly")
    void testGetNumberType(int number, String expectedType) {
        assertEquals(expectedType, validator.getNumberType(number));
    }
    
    @ParameterizedTest
    @CsvSource({
        "1, false",
        "5, true",
        "10, true",
        "-1, false",
        "-5, false",
        "0, false"
    })
    @DisplayName("Positive number validation should work correctly")
    void testIsPositive(int number, boolean expected) {
        assertEquals(expected, validator.isPositive(number));
    }
}
```

### Problem 2: String Utility Functions

**Task**: Create string utility functions and test them with various string inputs using parameterized tests.

#### Production Code:
```java
public class StringUtils {
    
    public boolean isPalindrome(String str) {
        if (str == null) return false;
        str = str.toLowerCase().replaceAll("[^a-z0-9]", "");
        int left = 0, right = str.length() - 1;
        
        while (left < right) {
            if (str.charAt(left) != str.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }
        return true;
    }
    
    public int countVowels(String str) {
        if (str == null) return 0;
        int count = 0;
        String vowels = "aeiouAEIOU";
        
        for (char c : str.toCharArray()) {
            if (vowels.indexOf(c) != -1) {
                count++;
            }
        }
        return count;
    }
    
    public String reverseString(String str) {
        if (str == null) return null;
        return new StringBuilder(str).reverse().toString();
    }
    
    public boolean isValidEmail(String email) {
        if (email == null) return false;
        return email.matches("^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$");
    }
}
```

#### Test Code:
```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;
import org.junit.jupiter.params.provider.CsvSource;
import org.junit.jupiter.params.provider.NullAndEmptySource;
import static org.junit.jupiter.api.Assertions.*;

class StringUtilsTest {
    
    private StringUtils stringUtils;
    
    @BeforeEach
    void setUp() {
        stringUtils = new StringUtils();
    }
    
    @ParameterizedTest
    @ValueSource(strings = {"racecar", "madam", "A man a plan a canal Panama", "race a car", "Was it a car or a cat I saw"})
    @DisplayName("Valid palindromes should return true")
    void testValidPalindromes(String str) {
        assertTrue(stringUtils.isPalindrome(str));
    }
    
    @ParameterizedTest
    @ValueSource(strings = {"hello", "world", "java", "junit", "testing"})
    @DisplayName("Non-palindromes should return false")
    void testNonPalindromes(String str) {
        assertFalse(stringUtils.isPalindrome(str));
    }
    
    @ParameterizedTest
    @CsvSource({
        "hello, 2",
        "programming, 3",
        "aeiou, 5",
        "bcdfg, 0",
        "HELLO, 2",
        "AeIoU, 5",
        "'', 0"
    })
    @DisplayName("Vowel counting should work correctly")
    void testCountVowels(String input, int expectedCount) {
        assertEquals(expectedCount, stringUtils.countVowels(input));
    }
    
    @ParameterizedTest
    @CsvSource({
        "hello, olleh",
        "world, dlrow",
        "Java, avaJ",
        "12345, 54321",
        "a, a",
        "'', ''"
    })
    @DisplayName("String reversal should work correctly")
    void testReverseString(String input, String expected) {
        assertEquals(expected, stringUtils.reverseString(input));
    }
    
    @ParameterizedTest
    @ValueSource(strings = {
        "user@example.com",
        "test.email@domain.org",
        "valid_email@test-domain.co.uk",
        "user123@example123.com"
    })
    @DisplayName("Valid emails should return true")
    void testValidEmails(String email) {
        assertTrue(stringUtils.isValidEmail(email));
    }
    
    @ParameterizedTest
    @ValueSource(strings = {
        "invalid.email",
        "@domain.com",
        "user@",
        "user@domain",
        "user@.com",
        "user space@domain.com"
    })
    @DisplayName("Invalid emails should return false")
    void testInvalidEmails(String email) {
        assertFalse(stringUtils.isValidEmail(email));
    }
    
    @ParameterizedTest
    @NullAndEmptySource
    @DisplayName("Null and empty strings should be handled correctly")
    void testNullAndEmptyStrings(String input) {
        assertFalse(stringUtils.isPalindrome(input));
        assertEquals(0, stringUtils.countVowels(input));
        assertFalse(stringUtils.isValidEmail(input));
    }
}
```

---

## 3. Testing Exceptions

### Problem 1: User Registration Service

**Task**: Create a user registration service that validates user data and throws appropriate exceptions.

#### Production Code:
```java
public class UserRegistrationService {
    
    public void registerUser(String username, String email, int age) {
        validateUsername(username);
        validateEmail(email);
        validateAge(age);
        
        // Simulate registration logic
        System.out.println("User registered successfully: " + username);
    }
    
    private void validateUsername(String username) {
        if (username == null || username.trim().isEmpty()) {
            throw new IllegalArgumentException("Username cannot be null or empty");
        }
        if (username.length() < 3) {
            throw new IllegalArgumentException("Username must be at least 3 characters long");
        }
        if (username.length() > 20) {
            throw new IllegalArgumentException("Username cannot exceed 20 characters");
        }
        if (!username.matches("^[a-zA-Z0-9_]+$")) {
            throw new IllegalArgumentException("Username can only contain letters, numbers, and underscores");
        }
    }
    
    private void validateEmail(String email) {
        if (email == null || email.trim().isEmpty()) {
            throw new IllegalArgumentException("Email cannot be null or empty");
        }
        if (!email.matches("^[A-Za-z0-9+_.-]+@[A-Za-z0-9.-]+\\.[A-Za-z]{2,}$")) {
            throw new IllegalArgumentException("Invalid email format");
        }
    }
    
    private void validateAge(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
        if (age < 13) {
            throw new IllegalArgumentException("User must be at least 13 years old");
        }
        if (age > 120) {
            throw new IllegalArgumentException("Age cannot exceed 120 years");
        }
    }
    
    public void deleteUser(String username) {
        if (username == null || username.trim().isEmpty()) {
            throw new IllegalArgumentException("Username cannot be null or empty");
        }
        
        // Simulate user not found scenario
        if (username.equals("nonexistent")) {
            throw new RuntimeException("User not found: " + username);
        }
        
        System.out.println("User deleted successfully: " + username);
    }
}
```

#### Test Code:
```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class UserRegistrationServiceTest {
    
    private UserRegistrationService service;
    
    @BeforeEach
    void setUp() {
        service = new UserRegistrationService();
    }
    
    @Test
    @DisplayName("Valid user registration should succeed")
    void testValidUserRegistration() {
        assertDoesNotThrow(() -> {
            service.registerUser("john_doe", "john@example.com", 25);
        });
    }
    
    @Test
    @DisplayName("Null username should throw IllegalArgumentException")
    void testNullUsername() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.registerUser(null, "john@example.com", 25);
        });
        assertEquals("Username cannot be null or empty", exception.getMessage());
    }
    
    @Test
    @DisplayName("Empty username should throw IllegalArgumentException")
    void testEmptyUsername() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.registerUser("", "john@example.com", 25);
        });
        assertEquals("Username cannot be null or empty", exception.getMessage());
    }
    
    @Test
    @DisplayName("Short username should throw IllegalArgumentException")
    void testShortUsername() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.registerUser("ab", "john@example.com", 25);
        });
        assertEquals("Username must be at least 3 characters long", exception.getMessage());
    }
    
    @Test
    @DisplayName("Long username should throw IllegalArgumentException")
    void testLongUsername() {
        String longUsername = "this_is_a_very_long_username_that_exceeds_limit";
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.registerUser(longUsername, "john@example.com", 25);
        });
        assertEquals("Username cannot exceed 20 characters", exception.getMessage());
    }
    
    @Test
    @DisplayName("Invalid username characters should throw IllegalArgumentException")
    void testInvalidUsernameCharacters() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.registerUser("john-doe", "john@example.com", 25);
        });
        assertEquals("Username can only contain letters, numbers, and underscores", exception.getMessage());
    }
    
    @Test
    @DisplayName("Invalid email should throw IllegalArgumentException")
    void testInvalidEmail() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.registerUser("john_doe", "invalid-email", 25);
        });
        assertEquals("Invalid email format", exception.getMessage());
    }
    
    @Test
    @DisplayName("Negative age should throw IllegalArgumentException")
    void testNegativeAge() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.registerUser("john_doe", "john@example.com", -1);
        });
        assertEquals("Age cannot be negative", exception.getMessage());
    }
    
    @Test
    @DisplayName("Under-age user should throw IllegalArgumentException")
    void testUnderageUser() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.registerUser("john_doe", "john@example.com", 12);
        });
        assertEquals("User must be at least 13 years old", exception.getMessage());
    }
    
    @Test
    @DisplayName("Over-age user should throw IllegalArgumentException")
    void testOverageUser() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.registerUser("john_doe", "john@example.com", 121);
        });
        assertEquals("Age cannot exceed 120 years", exception.getMessage());
    }
    
    @Test
    @DisplayName("Deleting nonexistent user should throw RuntimeException")
    void testDeleteNonexistentUser() {
        RuntimeException exception = assertThrows(RuntimeException.class, () -> {
            service.deleteUser("nonexistent");
        });
        assertEquals("User not found: nonexistent", exception.getMessage());
    }
}
```

### Problem 2: File Processing Service

**Task**: Create a file processing service that handles various file operation exceptions.

#### Production Code:
```java
import java.io.File;
import java.io.IOException;

public class FileProcessingService {
    
    public void processFile(String filePath) throws IOException {
        validateFilePath(filePath);
        
        File file = new File(filePath);
        if (!file.exists()) {
            throw new IOException("File not found: " + filePath);
        }
        
        if (!file.canRead()) {
            throw new IOException("File is not readable: " + filePath);
        }
        
        // Simulate file processing
        System.out.println("Processing file: " + filePath);
    }
    
    public void createFile(String filePath, String content) {
        if (filePath == null || filePath.trim().isEmpty()) {
            throw new IllegalArgumentException("File path cannot be null or empty");
        }
        
        if (content == null) {
            throw new IllegalArgumentException("Content cannot be null");
        }
        
        // Simulate file creation with potential failure
        if (filePath.contains("readonly")) {
            throw new RuntimeException("Cannot create file in read-only directory");
        }
        
        System.out.println("File created: " + filePath);
    }
    
    public long getFileSize(String filePath) {
        validateFilePath(filePath);
        
        File file = new File(filePath);
        if (!file.exists()) {
            throw new RuntimeException("File does not exist: " + filePath);
        }
        
        return file.length();
    }
    
    public void deleteFile(String filePath) {
        validateFilePath(filePath);
        
        // Simulate system files that cannot be deleted
        if (filePath.contains("system")) {
            throw new SecurityException("Cannot delete system file: " + filePath);
        }
        
        // Simulate file in use
        if (filePath.contains("locked")) {
            throw new RuntimeException("File is currently in use and cannot be deleted: " + filePath);
        }
        
        System.out.println("File deleted: " + filePath);
    }
    
    private void validateFilePath(String filePath) {
        if (filePath == null || filePath.trim().isEmpty()) {
            throw new IllegalArgumentException("File path cannot be null or empty");
        }
        
        if (filePath.contains("..")) {
            throw new SecurityException("File path cannot contain '..' for security reasons");
        }
        
        if (filePath.length() > 260) {
            throw new IllegalArgumentException("File path is too long (max 260 characters)");
        }
    }
}
```

#### Test Code:
```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import java.io.IOException;
import static org.junit.jupiter.api.Assertions.*;

class FileProcessingServiceTest {
    
    private FileProcessingService service;
    
    @BeforeEach
    void setUp() {
        service = new FileProcessingService();
    }
    
    @Test
    @DisplayName("Null file path should throw IllegalArgumentException")
    void testNullFilePath() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.getFileSize(null);
        });
        assertEquals("File path cannot be null or empty", exception.getMessage());
    }
    
    @Test
    @DisplayName("Empty file path should throw IllegalArgumentException")
    void testEmptyFilePath() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.getFileSize("");
        });
        assertEquals("File path cannot be null or empty", exception.getMessage());
    }
    
    @Test
    @DisplayName("File path with '..' should throw SecurityException")
    void testInsecureFilePath() {
        SecurityException exception = assertThrows(SecurityException.class, () -> {
            service.getFileSize("../../../system/file.txt");
        });
        assertEquals("File path cannot contain '..' for security reasons", exception.getMessage());
    }
    
    @Test
    @DisplayName("Too long file path should throw IllegalArgumentException")
    void testTooLongFilePath() {
        String longPath = "a".repeat(261);
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.getFileSize(longPath);
        });
        assertEquals("File path is too long (max 260 characters)", exception.getMessage());
    }
    
    @Test
    @DisplayName("Nonexistent file should throw RuntimeException")
    void testNonexistentFile() {
        RuntimeException exception = assertThrows(RuntimeException.class, () -> {
            service.getFileSize("nonexistent/file.txt");
        });
        assertEquals("File does not exist: nonexistent/file.txt", exception.getMessage());
    }
    
    @Test
    @DisplayName("Creating file with null content should throw IllegalArgumentException")
    void testCreateFileWithNullContent() {
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            service.createFile("test.txt", null);
        });
        assertEquals("Content cannot be null", exception.getMessage());
    }
    
    @Test
    @DisplayName("Creating file in readonly directory should throw RuntimeException")
    void testCreateFileInReadonlyDirectory() {
        RuntimeException exception = assertThrows(RuntimeException.class, () -> {
            service.createFile("readonly/test.txt", "content");
        });
        assertEquals("Cannot create file in read-only directory", exception.getMessage());
    }
    
    @Test
    @DisplayName("Deleting system file should throw SecurityException")
    void testDeleteSystemFile() {
        SecurityException exception = assertThrows(SecurityException.class, () -> {
            service.deleteFile("system/important.dll");
        });
        assertEquals("Cannot delete system file: system/important.dll", exception.getMessage());
    }
    
    @Test
    @DisplayName("Deleting locked file should throw RuntimeException")
    void testDeleteLockedFile() {
        RuntimeException exception = assertThrows(RuntimeException.class, () -> {
            service.deleteFile("locked/file.txt");
        });
        assertEquals("File is currently in use and cannot be deleted: locked/file.txt", exception.getMessage());
    }
    
    @Test
    @DisplayName("Valid file operations should not throw exceptions")
    void testValidFileOperations() {
        assertDoesNotThrow(() -> {
            service.createFile("valid/file.txt", "Some content");
        });
        
        assertDoesNotThrow(() -> {
            service.deleteFile("valid/file.txt");
        });
    }
    
    @Test
    @DisplayName("Multiple exception types should be handled correctly")
    void testMultipleExceptionTypes() {
        // Test IllegalArgumentException
        assertThrows(IllegalArgumentException.class, () -> {
            service.createFile(null, "content");
        });
        
        // Test SecurityException
        assertThrows(SecurityException.class, () -> {
            service.deleteFile("system/file.txt");
        });
        
        // Test RuntimeException
        assertThrows(RuntimeException.class, () -> {
            service.getFileSize("nonexistent.txt");
        });
    }
}
```

---

# Advanced JUnit 5 Practice Guide

## 4. Mocking with Mockito (Basic)

### Problem 1: UserService with UserRepository Dependency

**Task**: Create a UserService that depends on UserRepository and test it using Mockito mocks.

#### Production Code:

```java
// User Entity
public class User {
    private Long id;
    private String username;
    private String email;
    private boolean active;
    
    public User(Long id, String username, String email) {
        this.id = id;
        this.username = username;
        this.email = email;
        this.active = true;
    }
    
    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
    
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    
    public boolean isActive() { return active; }
    public void setActive(boolean active) { this.active = active; }
}

// Repository Interface
public interface UserRepository {
    User findById(Long id);
    User findByUsername(String username);
    User save(User user);
    void deleteById(Long id);
    boolean existsByUsername(String username);
    boolean existsByEmail(String email);
}

// Service Class
public class UserService {
    private final UserRepository userRepository;
    
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
    
    public User createUser(String username, String email) {
        if (username == null || username.trim().isEmpty()) {
            throw new IllegalArgumentException("Username cannot be null or empty");
        }
        if (email == null || email.trim().isEmpty()) {
            throw new IllegalArgumentException("Email cannot be null or empty");
        }
        
        if (userRepository.existsByUsername(username)) {
            throw new IllegalArgumentException("Username already exists: " + username);
        }
        
        if (userRepository.existsByEmail(email)) {
            throw new IllegalArgumentException("Email already exists: " + email);
        }
        
        User user = new User(null, username, email);
        return userRepository.save(user);
    }
    
    public User getUserById(Long id) {
        if (id == null) {
            throw new IllegalArgumentException("User ID cannot be null");
        }
        
        User user = userRepository.findById(id);
        if (user == null) {
            throw new RuntimeException("User not found with ID: " + id);
        }
        
        return user;
    }
    
    public User updateUserEmail(Long userId, String newEmail) {
        if (userId == null) {
            throw new IllegalArgumentException("User ID cannot be null");
        }
        if (newEmail == null || newEmail.trim().isEmpty()) {
            throw new IllegalArgumentException("Email cannot be null or empty");
        }
        
        User user = getUserById(userId);
        
        if (userRepository.existsByEmail(newEmail)) {
            throw new IllegalArgumentException("Email already exists: " + newEmail);
        }
        
        user.setEmail(newEmail);
        return userRepository.save(user);
    }
    
    public void deactivateUser(Long userId) {
        User user = getUserById(userId);
        user.setActive(false);
        userRepository.save(user);
    }
    
    public void deleteUser(Long userId) {
        User user = getUserById(userId);
        userRepository.deleteById(userId);
    }
}
```

#### Test Code:

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private UserRepository userRepository;
    
    @InjectMocks
    private UserService userService;
    
    private User testUser;
    
    @BeforeEach
    void setUp() {
        testUser = new User(1L, "john_doe", "john@example.com");
    }
    
    @Test
    @DisplayName("Create user should work when username and email are unique")
    void testCreateUserSuccess() {
        // Arrange
        String username = "new_user";
        String email = "new@example.com";
        User savedUser = new User(2L, username, email);
        
        when(userRepository.existsByUsername(username)).thenReturn(false);
        when(userRepository.existsByEmail(email)).thenReturn(false);
        when(userRepository.save(any(User.class))).thenReturn(savedUser);
        
        // Act
        User result = userService.createUser(username, email);
        
        // Assert
        assertNotNull(result);
        assertEquals(2L, result.getId());
        assertEquals(username, result.getUsername());
        assertEquals(email, result.getEmail());
        assertTrue(result.isActive());
        
        // Verify interactions
        verify(userRepository).existsByUsername(username);
        verify(userRepository).existsByEmail(email);
        verify(userRepository).save(any(User.class));
    }
    
    @Test
    @DisplayName("Create user should throw exception when username exists")
    void testCreateUserUsernameExists() {
        // Arrange
        String username = "existing_user";
        String email = "new@example.com";
        
        when(userRepository.existsByUsername(username)).thenReturn(true);
        
        // Act & Assert
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            userService.createUser(username, email);
        });
        
        assertEquals("Username already exists: " + username, exception.getMessage());
        
        // Verify that we don't check email or save when username exists
        verify(userRepository).existsByUsername(username);
        verify(userRepository, never()).existsByEmail(anyString());
        verify(userRepository, never()).save(any(User.class));
    }
    
    @Test
    @DisplayName("Create user should throw exception when email exists")
    void testCreateUserEmailExists() {
        // Arrange
        String username = "new_user";
        String email = "existing@example.com";
        
        when(userRepository.existsByUsername(username)).thenReturn(false);
        when(userRepository.existsByEmail(email)).thenReturn(true);
        
        // Act & Assert
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            userService.createUser(username, email);
        });
        
        assertEquals("Email already exists: " + email, exception.getMessage());
        
        // Verify interactions
        verify(userRepository).existsByUsername(username);
        verify(userRepository).existsByEmail(email);
        verify(userRepository, never()).save(any(User.class));
    }
    
    @Test
    @DisplayName("Get user by ID should return user when found")
    void testGetUserByIdSuccess() {
        // Arrange
        Long userId = 1L;
        when(userRepository.findById(userId)).thenReturn(testUser);
        
        // Act
        User result = userService.getUserById(userId);
        
        // Assert
        assertNotNull(result);
        assertEquals(testUser.getId(), result.getId());
        assertEquals(testUser.getUsername(), result.getUsername());
        assertEquals(testUser.getEmail(), result.getEmail());
        
        verify(userRepository).findById(userId);
    }
    
    @Test
    @DisplayName("Get user by ID should throw exception when not found")
    void testGetUserByIdNotFound() {
        // Arrange
        Long userId = 999L;
        when(userRepository.findById(userId)).thenReturn(null);
        
        // Act & Assert
        RuntimeException exception = assertThrows(RuntimeException.class, () -> {
            userService.getUserById(userId);
        });
        
        assertEquals("User not found with ID: " + userId, exception.getMessage());
        verify(userRepository).findById(userId);
    }
    
    @Test
    @DisplayName("Update user email should work when new email is unique")
    void testUpdateUserEmailSuccess() {
        // Arrange
        Long userId = 1L;
        String newEmail = "updated@example.com";
        User updatedUser = new User(userId, testUser.getUsername(), newEmail);
        
        when(userRepository.findById(userId)).thenReturn(testUser);
        when(userRepository.existsByEmail(newEmail)).thenReturn(false);
        when(userRepository.save(testUser)).thenReturn(updatedUser);
        
        // Act
        User result = userService.updateUserEmail(userId, newEmail);
        
        // Assert
        assertNotNull(result);
        assertEquals(newEmail, result.getEmail());
        
        verify(userRepository).findById(userId);
        verify(userRepository).existsByEmail(newEmail);
        verify(userRepository).save(testUser);
    }
    
    @Test
    @DisplayName("Deactivate user should set active to false")
    void testDeactivateUser() {
        // Arrange
        Long userId = 1L;
        when(userRepository.findById(userId)).thenReturn(testUser);
        when(userRepository.save(testUser)).thenReturn(testUser);
        
        // Act
        userService.deactivateUser(userId);
        
        // Assert
        assertFalse(testUser.isActive());
        
        verify(userRepository).findById(userId);
        verify(userRepository).save(testUser);
    }
    
    @Test
    @DisplayName("Delete user should call repository delete")
    void testDeleteUser() {
        // Arrange
        Long userId = 1L;
        when(userRepository.findById(userId)).thenReturn(testUser);
        
        // Act
        userService.deleteUser(userId);
        
        // Assert
        verify(userRepository).findById(userId);
        verify(userRepository).deleteById(userId);
    }
    
    @Test
    @DisplayName("Create user with null username should throw exception")
    void testCreateUserNullUsername() {
        // Act & Assert
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            userService.createUser(null, "email@example.com");
        });
        
        assertEquals("Username cannot be null or empty", exception.getMessage());
        
        // Verify no repository interactions
        verifyNoInteractions(userRepository);
    }
}
```

### Problem 2: OrderService with Multiple Dependencies

**Task**: Create an OrderService that depends on multiple repositories and external services.

#### Production Code:

```java
// Order Entity
public class Order {
    private Long id;
    private Long customerId;
    private String productName;
    private int quantity;
    private double totalAmount;
    private String status;
    
    public Order(Long customerId, String productName, int quantity, double totalAmount) {
        this.customerId = customerId;
        this.productName = productName;
        this.quantity = quantity;
        this.totalAmount = totalAmount;
        this.status = "PENDING";
    }
    
    // Getters and Setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    
    public Long getCustomerId() { return customerId; }
    public void setCustomerId(Long customerId) { this.customerId = customerId; }
    
    public String getProductName() { return productName; }
    public void setProductName(String productName) { this.productName = productName; }
    
    public int getQuantity() { return quantity; }
    public void setQuantity(int quantity) { this.quantity = quantity; }
    
    public double getTotalAmount() { return totalAmount; }
    public void setTotalAmount(double totalAmount) { this.totalAmount = totalAmount; }
    
    public String getStatus() { return status; }
    public void setStatus(String status) { this.status = status; }
}

// Repository and Service Interfaces
public interface OrderRepository {
    Order save(Order order);
    Order findById(Long id);
}

public interface CustomerRepository {
    boolean existsById(Long customerId);
}

public interface PaymentService {
    boolean processPayment(Long customerId, double amount);
}

public interface EmailService {
    void sendOrderConfirmation(Long customerId, Order order);
}

// Order Service
public class OrderService {
    private final OrderRepository orderRepository;
    private final CustomerRepository customerRepository;
    private final PaymentService paymentService;
    private final EmailService emailService;
    
    public OrderService(OrderRepository orderRepository, 
                       CustomerRepository customerRepository,
                       PaymentService paymentService,
                       EmailService emailService) {
        this.orderRepository = orderRepository;
        this.customerRepository = customerRepository;
        this.paymentService = paymentService;
        this.emailService = emailService;
    }
    
    public Order createOrder(Long customerId, String productName, int quantity, double unitPrice) {
        // Validate inputs
        if (customerId == null) {
            throw new IllegalArgumentException("Customer ID cannot be null");
        }
        if (productName == null || productName.trim().isEmpty()) {
            throw new IllegalArgumentException("Product name cannot be null or empty");
        }
        if (quantity <= 0) {
            throw new IllegalArgumentException("Quantity must be positive");
        }
        if (unitPrice <= 0) {
            throw new IllegalArgumentException("Unit price must be positive");
        }
        
        // Check if customer exists
        if (!customerRepository.existsById(customerId)) {
            throw new IllegalArgumentException("Customer not found: " + customerId);
        }
        
        double totalAmount = quantity * unitPrice;
        Order order = new Order(customerId, productName, quantity, totalAmount);
        
        return orderRepository.save(order);
    }
    
    public Order processOrder(Long orderId) {
        Order order = orderRepository.findById(orderId);
        if (order == null) {
            throw new RuntimeException("Order not found: " + orderId);
        }
        
        if (!"PENDING".equals(order.getStatus())) {
            throw new IllegalStateException("Order is not in pending status");
        }
        
        // Process payment
        boolean paymentSuccess = paymentService.processPayment(order.getCustomerId(), order.getTotalAmount());
        
        if (paymentSuccess) {
            order.setStatus("CONFIRMED");
            order = orderRepository.save(order);
            
            // Send confirmation email
            emailService.sendOrderConfirmation(order.getCustomerId(), order);
        } else {
            order.setStatus("PAYMENT_FAILED");
            order = orderRepository.save(order);
        }
        
        return order;
    }
    
    public Order getOrder(Long orderId) {
        if (orderId == null) {
            throw new IllegalArgumentException("Order ID cannot be null");
        }
        
        Order order = orderRepository.findById(orderId);
        if (order == null) {
            throw new RuntimeException("Order not found: " + orderId);
        }
        
        return order;
    }
}
```

#### Test Code:

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.InjectMocks;
import org.mockito.Mock;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.junit.jupiter.api.Assertions.*;
import static org.mockito.Mockito.*;

@ExtendWith(MockitoExtension.class)
class OrderServiceTest {
    
    @Mock
    private OrderRepository orderRepository;
    
    @Mock
    private CustomerRepository customerRepository;
    
    @Mock
    private PaymentService paymentService;
    
    @Mock
    private EmailService emailService;
    
    @InjectMocks
    private OrderService orderService;
    
    private Order testOrder;
    
    @BeforeEach
    void setUp() {
        testOrder = new Order(1L, "Laptop", 2, 2000.0);
        testOrder.setId(1L);
    }
    
    @Test
    @DisplayName("Create order should work with valid inputs")
    void testCreateOrderSuccess() {
        // Arrange
        Long customerId = 1L;
        String productName = "Laptop";
        int quantity = 2;
        double unitPrice = 1000.0;
        
        when(customerRepository.existsById(customerId)).thenReturn(true);
        when(orderRepository.save(any(Order.class))).thenReturn(testOrder);
        
        // Act
        Order result = orderService.createOrder(customerId, productName, quantity, unitPrice);
        
        // Assert
        assertNotNull(result);
        assertEquals(customerId, result.getCustomerId());
        assertEquals(productName, result.getProductName());
        assertEquals(quantity, result.getQuantity());
        assertEquals(2000.0, result.getTotalAmount());
        assertEquals("PENDING", result.getStatus());
        
        verify(customerRepository).existsById(customerId);
        verify(orderRepository).save(any(Order.class));
    }
    
    @Test
    @DisplayName("Create order should throw exception when customer not found")
    void testCreateOrderCustomerNotFound() {
        // Arrange
        Long customerId = 999L;
        when(customerRepository.existsById(customerId)).thenReturn(false);
        
        // Act & Assert
        IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
            orderService.createOrder(customerId, "Laptop", 2, 1000.0);
        });
        
        assertEquals("Customer not found: " + customerId, exception.getMessage());
        
        verify(customerRepository).existsById(customerId);
        verify(orderRepository, never()).save(any(Order.class));
    }
    
    @Test
    @DisplayName("Process order should confirm order when payment succeeds")
    void testProcessOrderPaymentSuccess() {
        // Arrange
        Long orderId = 1L;
        when(orderRepository.findById(orderId)).thenReturn(testOrder);
        when(paymentService.processPayment(testOrder.getCustomerId(), testOrder.getTotalAmount())).thenReturn(true);
        when(orderRepository.save(testOrder)).thenReturn(testOrder);
        
        // Act
        Order result = orderService.processOrder(orderId);
        
        // Assert
        assertEquals("CONFIRMED", result.getStatus());
        
        verify(orderRepository).findById(orderId);
        verify(paymentService).processPayment(testOrder.getCustomerId(), testOrder.getTotalAmount());
        verify(orderRepository).save(testOrder);
        verify(emailService).sendOrderConfirmation(testOrder.getCustomerId(), testOrder);
    }
    
    @Test
    @DisplayName("Process order should fail when payment fails")
    void testProcessOrderPaymentFailure() {
        // Arrange
        Long orderId = 1L;
        when(orderRepository.findById(orderId)).thenReturn(testOrder);
        when(paymentService.processPayment(testOrder.getCustomerId(), testOrder.getTotalAmount())).thenReturn(false);
        when(orderRepository.save(testOrder)).thenReturn(testOrder);
        
        // Act
        Order result = orderService.processOrder(orderId);
        
        // Assert
        assertEquals("PAYMENT_FAILED", result.getStatus());
        
        verify(orderRepository).findById(orderId);
        verify(paymentService).processPayment(testOrder.getCustomerId(), testOrder.getTotalAmount());
        verify(orderRepository).save(testOrder);
        verify(emailService, never()).sendOrderConfirmation(any(), any());
    }
    
    @Test
    @DisplayName("Process order should throw exception when order not found")
    void testProcessOrderNotFound() {
        // Arrange
        Long orderId = 999L;
        when(orderRepository.findById(orderId)).thenReturn(null);
        
        // Act & Assert
        RuntimeException exception = assertThrows(RuntimeException.class, () -> {
            orderService.processOrder(orderId);
        });
        
        assertEquals("Order not found: " + orderId, exception.getMessage());
        
        verify(orderRepository).findById(orderId);
        verifyNoInteractions(paymentService);
        verifyNoInteractions(emailService);
    }
    
    @Test
    @DisplayName("Process order should throw exception when order is not pending")
    void testProcessOrderNotPending() {
        // Arrange
        Long orderId = 1L;
        testOrder.setStatus("CONFIRMED");
        when(orderRepository.findById(orderId)).thenReturn(testOrder);
        
        // Act & Assert
        IllegalStateException exception = assertThrows(IllegalStateException.class, () -> {
            orderService.processOrder(orderId);
        });
        
        assertEquals("Order is not in pending status", exception.getMessage());
        
        verify(orderRepository).findById(orderId);
        verifyNoInteractions(paymentService);
        verifyNoInteractions(emailService);
    }
}
```

---

## 5. Test Suites & Nested Tests

### Problem 1: Nested Tests for Calculator with Different Operations

**Task**: Organize calculator tests using nested test classes for better structure.

#### Production Code:

```java
public class AdvancedCalculator {
    
    public double add(double a, double b) {
        return a + b;
    }
    
    public double subtract(double a, double b) {
        return a - b;
    }
    
    public double multiply(double a, double b) {
        return a * b;
    }
    
    public double divide(double a, double b) {
        if (b == 0) {
            throw new ArithmeticException("Cannot divide by zero");
        }
        return a / b;
    }
    
    public double power(double base, double exponent) {
        return Math.pow(base, exponent);
    }
    
    public double sqrt(double number) {
        if (number < 0) {
            throw new IllegalArgumentException("Cannot calculate square root of negative number");
        }
        return Math.sqrt(number);
    }
    
    public double percentage(double value, double percentage) {
        return (value * percentage) / 100;
    }
    
    public boolean isEven(int number) {
        return number % 2 == 0;
    }
    
    public int factorial(int n) {
        if (n < 0) {
            throw new IllegalArgumentException("Factorial is not defined for negative numbers");
        }
        if (n == 0 || n == 1) {
            return 1;
        }
        return n * factorial(n - 1);
    }
}
```

#### Test Code:

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Nested;
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

@DisplayName("Advanced Calculator Tests")
class AdvancedCalculatorTest {
    
    private AdvancedCalculator calculator;
    
    @BeforeEach
    void setUp() {
        calculator = new AdvancedCalculator();
    }
    
    @Nested
    @DisplayName("Basic Arithmetic Operations")
    class BasicArithmeticTests {
        
        @Nested
        @DisplayName("Addition Tests")
        class AdditionTests {
            
            @Test
            @DisplayName("Should add positive numbers correctly")
            void testAddPositiveNumbers() {
                assertEquals(5.0, calculator.add(2.0, 3.0));
                assertEquals(10.5, calculator.add(4.5, 6.0));
            }
            
            @Test
            @DisplayName("Should add negative numbers correctly")
            void testAddNegativeNumbers() {
                assertEquals(-5.0, calculator.add(-2.0, -3.0));
                assertEquals(1.0, calculator.add(-2.0, 3.0));
            }
            
            @Test
            @DisplayName("Should handle zero correctly")
            void testAddWithZero() {
                assertEquals(5.0, calculator.add(5.0, 0.0));
                assertEquals(0.0, calculator.add(0.0, 0.0));
            }
        }
        
        @Nested
        @DisplayName("Subtraction Tests")
        class SubtractionTests {
            
            @Test
            @DisplayName("Should subtract positive numbers correctly")
            void testSubtractPositiveNumbers() {
                assertEquals(1.0, calculator.subtract(3.0, 2.0));
                assertEquals(-1.0, calculator.subtract(2.0, 3.0));
            }
            
            @Test
            @DisplayName("Should subtract negative numbers correctly")
            void testSubtractNegativeNumbers() {
                assertEquals(1.0, calculator.subtract(-2.0, -3.0));
                assertEquals(-5.0, calculator.subtract(-2.0, 3.0));
            }
        }
        
        @Nested
        @DisplayName("Multiplication Tests")
        class MultiplicationTests {
            
            @Test
            @DisplayName("Should multiply positive numbers correctly")
            void testMultiplyPositiveNumbers() {
                assertEquals(6.0, calculator.multiply(2.0, 3.0));
                assertEquals(12.0, calculator.multiply(3.0, 4.0));
            }
            
            @Test
            @DisplayName("Should handle multiplication by zero")
            void testMultiplyByZero() {
                assertEquals(0.0, calculator.multiply(5.0, 0.0));
                assertEquals(0.0, calculator.multiply(0.0, 5.0));
            }
        }
        
        @Nested
        @DisplayName("Division Tests")
        class DivisionTests {
            
            @Test
            @DisplayName("Should divide numbers correctly")
            void testDivideNumbers() {
                assertEquals(2.0, calculator.divide(6.0, 3.0));
                assertEquals(2.5, calculator.divide(5.0, 2.0));
            }
            
            @Test
            @DisplayName("Should throw exception when dividing by zero")
            void testDivideByZero() {
                ArithmeticException exception = assertThrows(ArithmeticException.class, () -> {
                    calculator.divide(5.0, 0.0);
                });
                assertEquals("Cannot divide by zero", exception.getMessage());
            }
        }
    }
    
    @Nested
    @DisplayName("Advanced Mathematical Operations")
    class AdvancedMathTests {
        
        @Nested
        @DisplayName("Power Operations")
        class PowerTests {
            
            @Test
            @DisplayName("Should calculate power correctly")
            void testPowerCalculation() {
                assertEquals(8.0, calculator.power(2.0, 3.0));
                assertEquals(1.0, calculator.power(5.0, 0.0));
                assertEquals(5.0, calculator.power(5.0, 1.0));
            }
            
            @Test
            @DisplayName("Should handle negative exponents")
            void testNegativeExponents() {
                assertEquals(0.25, calculator.power(2.0, -2.0), 0.001);
                assertEquals(0.2, calculator.power(5.0, -1.0), 0.001);
            }
        }
        
        @Nested
        @DisplayName("Square Root Operations")
        class SquareRootTests {
            
            @Test
            @DisplayName("Should calculate square root correctly")
            void testSquareRoot() {
                assertEquals(3.0, calculator.sqrt(9.0));
                assertEquals(4.0, calculator.sqrt(16.0));
                assertEquals(0.0, calculator.sqrt(0.0));
            }
            
            @Test
            @DisplayName("Should throw exception for negative numbers")
            void testSquareRootNegative() {
                IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
                    calculator.sqrt(-1.0);
                });
                assertEquals("Cannot calculate square root of negative number", exception.getMessage());
            }
        }
        
        @Nested
        @DisplayName("Factorial Operations")
        class FactorialTests {
            
            @Test
            @DisplayName("Should calculate factorial correctly")
            void testFactorial() {
                assertEquals(1, calculator.factorial(0));
                assertEquals(1, calculator.factorial(1));
                assertEquals(6, calculator.factorial(3));
                assertEquals(120, calculator.factorial(5));
            }
            
            @Test
            @DisplayName("Should throw exception for negative numbers")
            void testFactorialNegative() {
                IllegalArgumentException exception = assertThrows(IllegalArgumentException.class, () -> {
                    calculator.factorial(-1);
                });
                assertEquals("Factorial is not defined for negative numbers", exception.getMessage());
            }
        }
    }
    
    @Nested
    @DisplayName("Utility Operations")
    class UtilityTests {
        
        @Test
        @DisplayName("Should calculate percentage correctly")
        void testPercentage() {
            assertEquals(20.0, calculator.percentage(100.0, 20.0));
            assertEquals(7.5, calculator.percentage(50.0, 15.0));
        }
        
        @Test
        @DisplayName("Should check even numbers correctly")
        void testIsEven() {
            assertTrue(calculator.isEven(2));
            assertTrue(calculator.isEven(0));
            assertTrue(calculator.isEven(-4));
            assertFalse(calculator.isEven(1));
            assertFalse(calculator.isEven(-3));
        }
    }
}
```

### Problem 2: Test Suite for Multiple Service Classes

**Task**: Create a test suite that runs multiple test classes together.

#### Test Suite Configuration:

```java
import org.junit.platform.suite.api.SelectClasses;
import org.junit.platform.suite.api.Suite;

@Suite
@SelectClasses({
    UserServiceTest.class,
    OrderServiceTest.class,
    AdvancedCalculatorTest.class
})
class AllServicesTestSuite {
    // This class remains empty, it is used only as a holder for the above annotations
}
```

#### Alternative Test Suite by Package:

```java
import org.junit.platform.suite.api.SelectPackages;
import org.junit.platform.suite.api.Suite;

@Suite
@SelectPackages("com.example.services")
class ServicePackageTestSuite {
    // Runs all tests in the specified package
}
```

---

## 6. Advanced Assertions (Optional)

### Problem 1: User Profile Validation with Multiple Assertions

**Task**: Create a user profile class and test it using grouped assertions and timeout testing.

#### Production Code:

```java
import java.time.LocalDate;
import java.time.Period;

public class UserProfile {
    private String firstName;
    private String lastName;
    private String email;
    private LocalDate birthDate;
    private String phoneNumber;
    private String address;
    
    public UserProfile(String firstName, String lastName, String email, 
                      LocalDate birthDate, String phoneNumber, String address) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.email = email;
        this.birthDate = birthDate;
        this.phoneNumber = phoneNumber;
        this.address = address;
    }
    
    public String getFullName() {
        return firstName + " " + lastName;
    }
    
    public int getAge() {
        if (birthDate == null) {
            return 0;
        }
        return Period.between(birthDate, LocalDate.now()).getYears();
    }
    
    public boolean isAdult() {
        return getAge() >= 18;
    }
    
    public String getFormattedContact() {
        return String.format("Email: %s, Phone: %s", email, phoneNumber);
    }
    
    public UserProfile updateEmail(String newEmail) {
        // Simulate some processing time
        try {
            Thread.sleep(100);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        
        this.email = newEmail;
        return this;
    }
    
    public UserProfile performComplexCalculation() {
        // Simulate complex calculation that should complete quickly
        long startTime = System.currentTimeMillis();
        while (System.currentTimeMillis() - startTime < 50) {
            // Simulate work
            Math.sqrt(Math.random() * 1000);
        }
        return this;
    }
    
    // Getters and Setters
    public String getFirstName() { return firstName; }
    public void setFirstName(String firstName) { this.firstName = firstName; }
    
    public String getLastName() { return lastName; }
    public void setLastName(String lastName) { this.lastName = lastName; }
    
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    
    public LocalDate getBirthDate() { return birthDate; }
    public void setBirthDate(LocalDate birthDate) { this.birthDate = birthDate; }
    
    public String getPhoneNumber() { return phoneNumber; }
    public void setPhoneNumber(String phoneNumber) { this.phoneNumber = phoneNumber; }
    
    public String getAddress() { return address; }
    public void setAddress(String address) { this.address = address; }
}
```

#### Test Code:

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.Timeout;
import java.time.Duration;
import java.time.LocalDate;
import java.util.concurrent.TimeUnit;
import static org.junit.jupiter.api.Assertions.*;

class UserProfileTest {
    
    private UserProfile userProfile;
    private LocalDate birthDate;
    
    @BeforeEach
    void setUp() {
        birthDate = LocalDate.of(1990, 5, 15);
        userProfile = new UserProfile(
            "John", 
            "Doe", 
            "john.doe@example.com",
            birthDate,
            "+1-234-567-8900",
            "123 Main St, City, State"
        );
    }
    
    @Test
    @DisplayName("User profile should be created with all properties set correctly")
    void testUserProfileCreation() {
        // Using assertAll for grouped assertions
        assertAll("User Profile Creation",
            () -> assertEquals("John", userProfile.getFirstName(), "First name should match"),
            () -> assertEquals("Doe", userProfile.getLastName(), "Last name should match"),
            () -> assertEquals("john.doe@example.com", userProfile.getEmail(), "Email should match"),
            () -> assertEquals(birthDate, userProfile.getBirthDate(), "Birth date should match"),
            () -> assertEquals("+1-234-567-8900", userProfile.getPhoneNumber(), "Phone should match"),
            () -> assertEquals("123 Main St, City, State", userProfile.getAddress(), "Address should match")
        );
    }
    
    @Test
    @DisplayName("User profile computed properties should work correctly")
    void testComputedProperties() {
        assertAll("Computed Properties",
            () -> assertEquals("John Doe", userProfile.getFullName(), "Full name should be correct"),
            () -> assertTrue(userProfile.getAge() > 0, "Age should be positive"),
            () -> assertTrue(userProfile.isAdult(), "User should be adult"),
            () -> assertEquals("Email: john.doe@example.com, Phone: +1-234-567-8900", 
                              userProfile.getFormattedContact(), "Formatted contact should be correct")
        );
    }
    
    @Test
    @DisplayName("Age calculation should work for different birth dates")
    void testAgeCalculation() {
        // Test with different ages
        UserProfile child = new UserProfile("Jane", "Smith", "jane@example.com",
                                           LocalDate.now().minusYears(10), "123-456-7890", "Address");
        UserProfile adult = new UserProfile("Bob", "Wilson", "bob@example.com",
                                           LocalDate.now().minusYears(25), "123-456-7890", "Address");
        
        assertAll("Age Calculations",
            () -> assertEquals(10, child.getAge(), "Child age should be 10"),
            () -> assertFalse(child.isAdult(), "Child should not be adult"),
            () -> assertEquals(25, adult.getAge(), "Adult age should be 25"),
            () -> assertTrue(adult.isAdult(), "Adult should be adult")
        );
    }
    
    @Test
    @DisplayName("Email update should complete within reasonable time")
    @Timeout(value = 200, unit = TimeUnit.MILLISECONDS)
    void testEmailUpdateTimeout() {
        String newEmail = "newemail@example.com";
        UserProfile result = userProfile.updateEmail(newEmail);
        
        assertAll("Email Update",
            () -> assertEquals(newEmail, result.getEmail(), "Email should be updated"),
            () -> assertSame(userProfile, result, "Should return same instance")
        );
    }
    
    @Test
    @DisplayName("Complex calculation should complete quickly")
    void testComplexCalculationTimeout() {
        assertTimeout(Duration.ofMillis(100), () -> {
            UserProfile result = userProfile.performComplexCalculation();
            assertSame(userProfile, result);
        }, "Complex calculation should complete within 100ms");
    }
    
    @Test
    @DisplayName("Multiple operations should complete within time limit")
    void testMultipleOperationsTimeout() {
        assertTimeoutPreemptively(Duration.ofMillis(300), () -> {
            userProfile.updateEmail("temp1@example.com");
            userProfile.performComplexCalculation();
            userProfile.updateEmail("temp2@example.com");
            
            return userProfile.getEmail();
        }, "All operations should complete within 300ms");
    }
    
    @Test
    @DisplayName("All user data should be valid after multiple updates")
    void testMultipleUpdatesIntegrity() {
        // Perform multiple updates
        userProfile.setFirstName("Jane");
        userProfile.setLastName("Smith");
        userProfile.updateEmail("jane.smith@example.com");
        userProfile.setPhoneNumber("+1-987-654-3210");
        
        // Verify all properties are consistent
        assertAll("Data Integrity After Updates",
            () -> assertEquals("Jane", userProfile.getFirstName()),
            () -> assertEquals("Smith", userProfile.getLastName()),
            () -> assertEquals("jane.smith@example.com", userProfile.getEmail()),
            () -> assertEquals("+1-987-654-3210", userProfile.getPhoneNumber()),
            () -> assertEquals("Jane Smith", userProfile.getFullName()),
            () -> assertEquals("Email: jane.smith@example.com, Phone: +1-987-654-3210", 
                              userProfile.getFormattedContact()),
            () -> assertEquals(birthDate, userProfile.getBirthDate(), "Birth date should remain unchanged"),
            () -> assertTrue(userProfile.isAdult(), "Adult status should remain true")
        );
    }
    
    @Test
    @DisplayName("Edge cases should be handled correctly")
    void testEdgeCases() {
        // Test with null birth date
        UserProfile profileWithNullBirthDate = new UserProfile(
            "Test", "User", "test@example.com", null, "123-456-7890", "Address"
        );
        
        // Test with future birth date
        UserProfile profileWithFutureBirthDate = new UserProfile(
            "Future", "Baby", "future@example.com", 
            LocalDate.now().plusDays(1), "123-456-7890", "Address"
        );
        
        assertAll("Edge Cases",
            () -> assertEquals(0, profileWithNullBirthDate.getAge(), "Age should be 0 for null birth date"),
            () -> assertFalse(profileWithNullBirthDate.isAdult(), "Should not be adult with null birth date"),
            () -> assertTrue(profileWithFutureBirthDate.getAge() <= 0, "Age should be 0 or negative for future birth date"),
            () -> assertFalse(profileWithFutureBirthDate.isAdult(), "Should not be adult with future birth date")
        );
    }
}
```

---
