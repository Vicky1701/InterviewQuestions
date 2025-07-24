**Better approach:**
- **Focus on critical business logic**
- **Test edge cases and error conditions**
- **Aim for 70-80% coverage with high-quality tests**
- **Use coverage as a guide, not a goal**

---

## 🎯 SCENARIO-BASED Questions (Medium Priority)

### **Q23: How would you test a REST controller?**

**Testing approaches for REST controllers:**

**Scenario:** UserController with POST endpoint
```java
@RestController
public class UserController {
    @Autowired
    private UserService userService;
    
    @PostMapping("/users")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User savedUser = userService.saveUser(user);
        return ResponseEntity.ok(savedUser);
    }
}
```

**Method 1: Unit Test (Mock the service)**
```java
@ExtendWith(MockitoExtension.class)
class UserControllerTest {
    @Mock
    private UserService userService;
    
    @InjectMocks
    private UserController userController;
    
    @Test
    void shouldCreateUser() {
        // Arrange
        User inputUser = new User("John", "john@email.com");
        User savedUser = new User(1L, "John", "john@email.com");
        when(userService.saveUser(inputUser)).thenReturn(savedUser);
        
        // Act
        ResponseEntity<User> response = userController.createUser(inputUser);
        
        // Assert
        assertEquals(HttpStatus.OK, response.getStatusCode());
        assertEquals(savedUser, response.getBody());
        verify(userService).saveUser(inputUser);
    }
}
```

**Method 2: Integration Test with @WebMvcTest**
```java
@WebMvcTest(UserController.class)
class UserControllerIntegrationTest {
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Test
    void shouldCreateUserViaHttp() throws Exception {
        // Arrange
        User savedUser = new User(1L, "John", "john@email.com");
        when(userService.saveUser(any(User.class))).thenReturn(savedUser);
        
        // Act & Assert
        mockMvc.perform(post("/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\":\"John\",\"email\":\"john@email.com\"}"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.id").value(1))
                .andExpect(jsonPath("$.name").value("John"))
                .andExpected(jsonPath("$.email").value("john@email.com"));
        
        verify(userService).saveUser(any(User.class));
    }
}
```

**Key points:**
- **Unit tests** - Test controller logic, mock dependencies
- **Integration tests** - Test HTTP layer, use @WebMvcTest
- **Mock external dependencies** - Focus on controller behavior

---

### **Q24: How do you test database operations without hitting the actual database?**

**Several approaches to avoid real database:**

**1. Mock the Repository (Most Common)**
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    @Mock
    private UserRepository userRepository;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    void shouldFindUserById() {
        // Arrange - Mock database response
        User expectedUser = new User("John");
        when(userRepository.findById(1L)).thenReturn(Optional.of(expectedUser));
        
        // Act
        User actualUser = userService.getUserById(1L);
        
        // Assert
        assertEquals("John", actualUser.getName());
        verify(userRepository).findById(1L);
    }
}
```

**2. In-Memory Database (H2)**
```java
@SpringBootTest
@TestPropertySource(properties = {
    "spring.datasource.url=jdbc:h2:mem:testdb",
    "spring.jpa.hibernate.ddl-auto=create-drop"
})
class UserRepositoryTest {
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldSaveAndFindUser() {
        // Arrange
        User user = new User("John", "john@email.com");
        
        // Act
        User savedUser = userRepository.save(user);
        Optional<User> foundUser = userRepository.findById(savedUser.getId());
        
        // Assert
        assertTrue(foundUser.isPresent());
        assertEquals("John", foundUser.get().getName());
    }
}
```

**3. @DataJpaTest for Repository Layer**
```java
@DataJpaTest
class UserRepositoryTest {
    @Autowired
    private TestEntityManager entityManager;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldFindUsersByEmail() {
        // Arrange - Insert test data
        User user = new User("John", "john@email.com");
        entityManager.persistAndFlush(user);
        
        // Act
        List<User> users = userRepository.findByEmail("john@email.com");
        
        // Assert
        assertEquals(1, users.size());
        assertEquals("John", users.get(0).getName());
    }
}
```

**4. Testcontainers (Real Database in Container)**
```java
@SpringBootTest
@Testcontainers
class UserRepositoryIntegrationTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:13")
            .withDatabaseName("testdb")
            .withUsername("test")
            .withPassword("test");
    
    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }
    
    @Test
    void shouldWorkWithRealDatabase() {
        // Test with actual PostgreSQL database in container
    }
}
```

**When to use each:**
- **Mocks** - Fast unit tests, test business logic
- **H2** - Quick integration tests
- **@DataJpaTest** - Test JPA queries and repositories
- **Testcontainers** - Full integration tests with real DB

---

### **Q25: What would you do if you need to test a method that depends on current time?**

**Problem:** Methods using `System.currentTimeMillis()` or `LocalDateTime.now()` are hard to test

**Solutions:**

**1. Dependency Injection (Recommended)**
```java
// Original problematic code
public class OrderService {
    public Order createOrder(String item) {
        Order order = new Order(item);
        order.setCreatedAt(LocalDateTime.now()); // Hard to test!
        return order;
    }
}

// Better - inject Clock dependency
public class OrderService {
    private final Clock clock;
    
    public OrderService(Clock clock) {
        this.clock = clock;
    }
    
    public Order createOrder(String item) {
        Order order = new Order(item);
        order.setCreatedAt(LocalDateTime.now(clock)); // Testable!
        return order;
    }
}
```

**Test with fixed time:**
```java
@Test
void shouldCreateOrderWithCurrentTime() {
    // Arrange - Fix the time
    Clock fixedClock = Clock.fixed(
        Instant.parse("2023-01-01T10:00:00Z"), 
        ZoneOffset.UTC
    );
    OrderService orderService = new OrderService(fixedClock);
    
    // Act
    Order order = orderService.createOrder("Laptop");
    
    // Assert
    assertEquals(LocalDateTime.of(2023, 1, 1, 10, 0), order.getCreatedAt());
}
```

**2. Wrapper Class for Time**
```java
// Time wrapper
public class TimeProvider {
    public LocalDateTime now() {
        return LocalDateTime.now();
    }
}

// Service using wrapper
public class OrderService {
    private final TimeProvider timeProvider;
    
    public OrderService(TimeProvider timeProvider) {
        this.timeProvider = timeProvider;
    }
    
    public Order createOrder(String item) {
        Order order = new Order(item);
        order.setCreatedAt(timeProvider.now());
        return order;
    }
}
```

**Test with mocked time:**
```java
@Test
void shouldCreateOrderWithMockedTime() {
    // Arrange
    TimeProvider mockTimeProvider = mock(TimeProvider.class);
    LocalDateTime fixedTime = LocalDateTime.of(2023, 1, 1, 10, 0);
    when(mockTimeProvider.now()).thenReturn(fixedTime);
    
    OrderService orderService = new OrderService(mockTimeProvider);
    
    // Act
    Order order = orderService.createOrder("Laptop");
    
    // Assert
    assertEquals(fixedTime, order.getCreatedAt());
}
```

**3. Static Method Wrapper (Last Resort)**
```java
public class DateUtils {
    public static LocalDateTime now() {
        return LocalDateTime.now();
    }
}

// Test with PowerMock (not recommended for new projects)
@PrepareForTest(DateUtils.class)
@Test
void testWithStaticMock() {
    // PowerMock can mock static methods, but it's complex
    mockStatic(DateUtils.class);
    when(DateUtils.now()).thenReturn(fixedDateTime);
    // ... test code
}
```

**Best practice:** Always inject time dependencies rather than using static methods.

---

### **Q26: How do you test private methods?**

**Short answer:** You generally don't test private methods directly.

**The philosophy:**
- Private methods are implementation details
- Test public methods that use the private methods
- If you need to test a private method, it might need to be public or in a separate class

**Approaches:**

**1. Test through public methods (Recommended)**
```java
public class Calculator {
    public double calculateDiscount(double price, String customerType) {
        double rate = getDiscountRate(customerType); // Private method
        return applyDiscount(price, rate); // Private method
    }
    
    private double getDiscountRate(String customerType) {
        return "PREMIUM".equals(customerType) ? 0.1 : 0.05;
    }
    
    private double applyDiscount(double price, double rate) {
        return price * (1 - rate);
    }
}
```

**Test the public method:**
```java
@Test
void shouldCalculateDiscountForPremiumCustomer() {
    Calculator calculator = new Calculator();
    
    // This tests both private methods indirectly
    double result = calculator.calculateDiscount(100.0, "PREMIUM");
    
    assertEquals(90.0, result); // Tests getDiscountRate() and applyDiscount()
}

@Test
void shouldCalculateDiscountForRegularCustomer() {
    Calculator calculator = new Calculator();
    
    double result = calculator.calculateDiscount(100.0, "REGULAR");
    
    assertEquals(95.0, result);
}
```

**2. Extract to separate class (When private method is complex)**
```java
// Extract complex private logic to its own class
public class DiscountCalculator {
    public double getDiscountRate(String customerType) { // Now public
        return "PREMIUM".equals(customerType) ? 0.1 : 0.05;
    }
    
    public double applyDiscount(double price, double rate) { // Now public
        return price * (1 - rate);
    }
}

// Original class becomes simpler
public class Calculator {
    private final DiscountCalculator discountCalculator;
    
    public double calculateDiscount(double price, String customerType) {
        double rate = discountCalculator.getDiscountRate(customerType);
        return discountCalculator.applyDiscount(price, rate);
    }
}
```

**Now you can test the extracted class directly:**
```java
@Test
void shouldGetPremiumDiscountRate() {
    DiscountCalculator calculator = new DiscountCalculator();
    assertEquals(0.1, calculator.getDiscountRate("PREMIUM"));
}
```

**3. Package-private methods (If you must)**
```java
public class Calculator {
    // Remove 'private', make it package-private
    double getDiscountRate(String customerType) {
        return "PREMIUM".equals(customerType) ? 0.1 : 0.05;
    }
}

// Test from same package
@Test
void shouldGetDiscountRate() {
    Calculator calculator = new Calculator();
    assertEquals(0.1, calculator.getDiscountRate("PREMIUM"));
}
```

**4. Reflection (Not recommended)**
```java
@Test
void testPrivateMethodWithReflection() throws Exception {
    Calculator calculator = new Calculator();
    
    // Get private method using reflection
    Method method = Calculator.class.getDeclaredMethod("getDiscountRate", String.class);
    method.setAccessible(true);
    
    // Invoke private method
    double result = (double) method.invoke(calculator, "PREMIUM");
    
    assertEquals(0.1, result);
}
```

**Best practices:**
- **Test through public API** - Most cases
- **Extract complex logic** - When private method is substantial
- **Avoid reflection** - Makes tests brittle and hard to maintain

---

### **Q27: Explain how you would write tests for legacy code**

**Legacy code challenges:**
- No existing tests
- Tightly coupled dependencies
- Hard to isolate components
- Large, complex methods

**Strategy - The "Golden Master" Approach:**

**Step 1: Characterization Tests (Capture current behavior)**
```java
// Legacy method you're afraid to change
public class LegacyOrderProcessor {
    public String processOrder(String orderData) {
        // 200 lines of complex logic
        // Multiple dependencies
        // No clear structure
        return someComplexResult;
    }
}
```

**Create tests that document current behavior:**
```java
@Test
void shouldProcessOrderLikeItCurrentlyDoes() {
    LegacyOrderProcessor processor = new LegacyOrderProcessor();
    
    // Test with various inputs and capture current outputs
    String result1 = processor.processOrder("input1");
    assertEquals("current_output1", result1); // Whatever it currently returns
    
    String result2 = processor.processOrder("input2");  
    assertEquals("current_output2", result2);
    
    // Even if the behavior seems wrong, capture it first
}
```

**Step 2: Add Seams (Dependency Injection)**
```java
// Before - hard to test
public class LegacyOrderProcessor {
    public String processOrder(String orderData) {
        Database database = new Database(); // Hard-coded dependency
        EmailService email = new EmailService(); // Hard-coded dependency
        // ... complex logic
    }
}

// After - testable
public class LegacyOrderProcessor {
    private Database database;
    private EmailService emailService;
    
    // Add constructor for testing
    public LegacyOrderProcessor(Database database, EmailService emailService) {
        this.database = database;
        this.emailService = emailService;
    }
    
    // Keep old constructor for existing code
    public LegacyOrderProcessor() {
        this(new Database(), new EmailService());
    }
}
```

**Step 3: Test with Mocks**
```java
@Test
void shouldProcessOrderWithMockedDependencies() {
    // Arrange
    Database mockDatabase = mock(Database.class);
    EmailService mockEmail = mock(EmailService.class);
    when(mockDatabase.getOrderInfo("123")).thenReturn("order_data");
    
    LegacyOrderProcessor processor = new LegacyOrderProcessor(mockDatabase, mockEmail);
    
    // Act
    String result = processor.processOrder("123");
    
    // Assert
    assertNotNull(result);
    verify(mockDatabase).getOrderInfo("123");
    verify(mockEmail).sendConfirmation(anyString());
}
```

**Step 4: Gradual Refactoring**
```java
// Extract small methods from large legacy method
public class LegacyOrderProcessor {
    public String processOrder(String orderData) {
        OrderInfo info = parseOrderData(orderData); // Extracted method
        ValidationResult validation = validateOrder(info); // Extracted method
        if (!validation.isValid()) {
            return handleInvalidOrder(validation); // Extracted method
        }
        return completeOrder(info); // Extracted method
    }
    
    // Now you can test these smaller methods individually
    OrderInfo parseOrderData(String orderData) {
        // Extracted logic
    }
}
```

**Step 5: Test Extracted Methods**
```java
@Test
void shouldParseOrderDataCorrectly() {
    LegacyOrderProcessor processor = new LegacyOrderProcessor();
    
    OrderInfo result = processor.parseOrderData("valid_order_string");
    
    assertEquals("expected_id", result.getId());
    assertEquals("expected_amount", result.getAmount());
}
```

**Practical tips:**
- **Start small** - Don't try to refactor everything at once
- **Keep characterization tests** - They prevent regressions
- **Add tests before changing** - Safety net for refactoring
- **Use IDE refactoring tools** - Extract method, introduce parameter
- **Work in small steps** - Each step should be testable

**Common patterns for legacy code:**
- **Strangler Fig** - Gradually replace old code with new
- **Branch by Abstraction** - Create interface, implement old and new
- **Parallel Run** - Run old and new code simultaneously to compare

---

## 📝 CODING Questions (Be Ready to Write)

### **Q28: Write a simple test for a method that adds two numbers**

**Method to test:**
```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

**Simple test:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {
    
    @Test
    void shouldAddTwoPositiveNumbers() {
        // Arrange
        Calculator calculator = new Calculator();
        
        // Act
        int result = calculator.add(2, 3);
        
        // Assert
        assertEquals(5, result);
    }
    
    @Test
    void shouldAddNegativeNumbers() {
        Calculator calculator = new Calculator();
        
        int result = calculator.add(-2, -3);
        
        assertEquals(-5, result);
    }
    
    @Test
    void shouldAddPositiveAndNegativeNumber() {
        Calculator calculator = new Calculator();
        
        int result = calculator.add(5, -3);
        
        assertEquals(2, result);
    }
    
    @Test
    void shouldAddZero() {
        Calculator calculator = new Calculator();
        
        assertEquals(5, calculator.add(5, 0));
        assertEquals(3, calculator.add(0, 3));
        assertEquals(0, calculator.add(0, 0));
    }
}
```

**Key points for interview:**
- Use AAA pattern (Arrange-Act-Assert)
- Test different scenarios (positive, negative, zero)
- Clear test method names
- One assertion per test (preferably)

---

### **Q29: Write a test that verifies an exception is thrown with a specific message**

**Method that throws exception:**
```java
public class BankAccount {
    private double balance;
    
    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }
    
    public void withdraw(double amount) {
        if (amount > balance) {
            throw new IllegalArgumentException("Insufficient funds. Balance: " + balance);
        }
        balance -= amount;
    }
}
```

**Test for exception:**
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class BankAccountTest {
    
    @Test
    void shouldThrowExceptionWhenWithdrawingMoreThanBalance() {
        // Arrange
        BankAccount account = new BankAccount(100.0);
        
        // Act & Assert
        IllegalArgumentException exception = assertThrows(
            IllegalArgumentException.class,
            () -> account.withdraw(150.0)
        );
        
        // Verify the exception message
        assertEquals("Insufficient funds. Balance: 100.0", exception.getMessage());
    }
    
    @Test
    void shouldThrowExceptionForNegativeWithdrawal() {
        BankAccount account = new BankAccount(100.0);
        
        // You can also test just that the exception is thrown
        assertThrows(IllegalArgumentException.class, () -> account.withdraw(-50.0));
    }
    
    @Test
    void shouldNotThrowExceptionForValidWithdrawal() {
        BankAccount account = new BankAccount(100.0);
        
        // This should not throw any exception
        assertDoesNotThrow(() -> account.withdraw(50.0));
    }
}
```

**Alternative older style (try-catch):**
```java
@Test
void shouldThrowExceptionOldStyle() {
    BankAccount account = new BankAccount(100.0);
    
    try {
        account.withdraw(150.0);
        fail("Expected IllegalArgumentException to be thrown");
    } catch (IllegalArgumentException e) {
        assertEquals("Insufficient funds. Balance: 100.0", e.getMessage());
    }
}
```

**Key points:**
- Use `assertThrows()` - cleaner than try-catch
- Capture the exception to check its message
- Test both exception and non-exception cases
- Use lambda expressions for the code that should throw

---

### **Q30: Show how to mock a dependency and verify it was called**

**Classes to test:**
```java
// Dependency to be mocked
public interface EmailService {
    void sendEmail(String recipient, String message);
    boolean isEmailValid(String email);
}

// Class under test
public class UserService {
    private EmailService emailService;
    
    public UserService(EmailService emailService) {
        this.emailService = emailService;
    }
    
    public void registerUser(String email, String name) {
        if (!emailService.isEmailValid(email)) {
            throw new IllegalArgumentException("Invalid email format");
        }
        
        // Registration logic would go here
        
        emailService.sendEmail(email, "Welcome " + name + "!");
    }
}
```

**Test with mocking:**
```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.Mock;
import org.mockito.InjectMocks;
import org.mockito.junit.jupiter.MockitoExtension;
import static org.mockito.Mockito.*;
import static org.junit.jupiter.api.Assertions.*;

@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private EmailService emailService; // Create mock
    
    @InjectMocks
    private UserService userService; // Inject mock into this
    
    @Test
    void shouldRegisterUserAndSendWelcomeEmail() {
        // Arrange - Stub the mock behavior
        when(emailService.isEmailValid("john@example.com")).thenReturn(true);
        
        // Act
        userService.registerUser("john@example.com", "John");
        
        // Assert - Verify the mock was called
        verify(emailService).isEmailValid("john@example.com");
        verify(emailService).sendEmail("john@example.com", "Welcome John!");
    }
    
    @Test
    void shouldThrowExceptionForInvalidEmail() {
        // Arrange
        when(emailService.isEmailValid("invalid-email")).thenReturn(false);
        
        // Act & Assert
        IllegalArgumentException exception = assertThrows(
            IllegalArgumentException.class,
            () -> userService.registerUser("invalid-email", "John")
        );
        
        assertEquals("Invalid email format", exception.getMessage());
        
        // Verify email validation was called, but welcome email was NOT sent
        verify(emailService).isEmailValid("invalid-email");
        verify(emailService, never()).sendEmail(anyString(), anyString());
    }
    
    @Test
    void shouldVerifyExactNumberOfCalls() {
        when(emailService.isEmailValid(anyString())).thenReturn(true);
        
        userService.registerUser("john@example.com", "John");
        userService.registerUser("jane@example.com", "Jane");
        
        // Verify method was called exactly 2 times
        verify(emailService, times(2)).isEmailValid(anyString());
        verify(emailService, times(2)).sendEmail(anyString(), anyString());
    }
    
    @Test
    void shouldVerifyNoUnexpectedInteractions() {
        when(emailService.isEmailValid("john@example.com")).thenReturn(true);
        
        userService.registerUser("john@example.com", "John");
        
        // Verify only expected methods were called
        verify(emailService).isEmailValid("john@example.com");
        verify(emailService).sendEmail("john@example.com", "Welcome John!");
        verifyNoMoreInteractions(emailService);
    }
}
```

**Alternative setup without annotations:**
```java
class UserServiceTest {
    private EmailService emailService;
    private UserService userService;
    
    @BeforeEach
    void setUp() {
        emailService = mock(EmailService.class);
        userService = new UserService(emailService);
    }
    
    @Test
    void shouldRegisterUser() {
        when(emailService.isEmailValid("john@example.com")).thenReturn(true);
        
        userService.registerUser("john@example.com", "John");
        
        verify(emailService).sendEmail("john@example.com", "Welcome John!");
    }
}
```

**Key verification methods:**
- `verify(mock).method()` - Called once
- `verify(mock, times(n)).method()` - Called exactly n times
- `verify(mock, never()).method()` - Never called
- `verify(mock, atLeast(n)).method()` - Called at least n times
- `verifyNoInteractions(mock)` - No methods called
- `verifyNoMoreInteractions(mock)` - No unexpected calls

---

### **Q31: Write a parameterized test for testing multiple input combinations**

**Method to test:**
```java
public class MathUtils {
    public static boolean isPrime(int number) {
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
}
```

**Parameterized test examples:**

**1. Simple parameterized test with @ValueSource:**
```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;
import static org.junit.jupiter.api.Assertions.*;

class MathUtilsTest {
    
    @ParameterizedTest
    @ValueSource(ints = {2, 3, 5, 7, 11, 13, 17, 19, 23})
    void shouldReturnTrueForPrimeNumbers(int number) {
        assertTrue(MathUtils.isPrime(number));
    }
    
    @ParameterizedTest
    @ValueSource(ints = {1, 4, 6, 8, 9, 10, 12, 14, 15, 16})
    void shouldReturnFalseForNonPrimeNumbers(int number) {
        assertFalse(MathUtils.isPrime(number));
    }
}
```

**2. Using @CsvSource for multiple parameters:**
```java
@ParameterizedTest
@CsvSource({
    "2, true",
    "3, true", 
    "4, false",
    "5, true",
    "6, false",
    "7, true",
    "8, false",
    "9, false",
    "10, false",
    "11, true"
})
void shouldCheckPrimeNumbers(int number, boolean expectedResult) {
    assertEquals(expectedResult, MathUtils.isPrime(number));
}
```

**3. Using @MethodSource for complex data:**
```java
import java.util.stream.Stream;
import org.junit.jupiter.params.provider.Arguments;
import org.junit.jupiter.params.provider.MethodSource;

class MathUtilsTest {
    
    @ParameterizedTest
    @MethodSource("providePrimeTestData")
    void shouldCheckPrimeNumbersWithMethodSource(int number, boolean expected, String description) {
        assertEquals(expected, MathUtils.isPrime(number), description);
    }
    
    static Stream<Arguments> providePrimeTestData() {
        return Stream.of(
            Arguments.of(2, true, "2 is the smallest prime"),
            Arguments.of(3, true, "3 is prime"),
            Arguments.of(4, false, "4 is divisible by 2"),
            Arguments.of(5, true, "5 is prime"),
            Arguments.of(1, false, "1 is not considered prime"),
            Arguments.of(0, false, "0 is not prime"),
            Arguments.of(-5, false, "Negative numbers are not prime")
        );
    }
}
```

**4. Testing calculator with multiple operations:**
```java
public class Calculator {
    public double calculate(double a, double b, String operation) {
        switch (operation) {
            case "+": return a + b;
            case "-": return a - b;
            case "*": return a * b;
            case "/": 
                if (b == 0) throw new IllegalArgumentException("Division by zero");
                return a / b;
            default: throw new IllegalArgumentException("Unknown operation");
        }
    }
}
```

**Parameterized test for calculator:**
```java
@ParameterizedTest
@CsvSource({
    "2.0, 3.0, '+', 5.0",
    "5.0, 3.0, '-', 2.0",
    "4.0, 3.0, '*', 12.0",
    "8.0, 2.0, '/', 4.0",
    "7.5, 2.5, '+', 10.0",
    "10.0, 3.0, '-', 7.0"
})
void shouldCalculateCorrectly(double a, double b, String operation, double expected) {
    Calculator calculator = new Calculator();
    
    double result = calculator.calculate(a, b, operation);
    
    assertEquals(expected, result, 0.001); // Delta for floating point comparison
}

@ParameterizedTest
@ValueSource(strings = {"%", "^", "invalid", ""})
void shouldThrowExceptionForInvalidOperations(String operation) {
    Calculator calculator = new Calculator();
    
    assertThrows(IllegalArgumentException.class, 
        () -> calculator.calculate(5.0, 3.0, operation));
}
```

**5. Custom display names:**
```java
@ParameterizedTest(name = "Test {index}: isPrime({0}) should return {1}")
@CsvSource({
    "2, true",
    "4, false", 
    "17, true",
    "25, false"
})
void shouldCheckPrimeNumbersWithCustomNames(int number, boolean expected) {
    assertEquals(expected, MathUtils.isPrime(number));
}
```

**Key benefits of parameterized tests:**
- **Reduce code duplication** - One test method, multiple data sets
- **Better coverage** - Easy to test edge cases and multiple scenarios
- **Maintainable** - Add new test cases by just adding data
- **Clear reporting** - See exactly which inputs failed

---

## 🔧 TOOL-SPECIFIC Questions (Lower Priority)

### **Q32: What is JaCoCo and how is it used?**

**What is JaCoCo:**
JaCoCo (Java Code Coverage) is a tool that measures how much of your code is executed during tests. It generates reports showing which lines, branches, and methods are covered.

**How it works:**
- Instruments your bytecode during test execution
- Tracks which lines are executed
- Generates HTML, XML, and CSV reports

**Maven configuration:**
```xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.8</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

**Commands:**
```bash
# Run tests with coverage
mvn clean test

# Generate coverage report
mvn jacoco:report

# View report at: target/site/jacoco/index.html
```

**Coverage metrics JaCoCo provides:**
- **Line coverage** - Percentage of executable lines covered
- **Branch coverage** - Percentage of if/else branches covered  
- **Instruction coverage** - Percentage of bytecode instructions covered
- **Method coverage** - Percentage of methods called
- **Class coverage** - Percentage of classes with at least one method called

**Typical usage:**
- **CI/CD pipelines** - Fail build if coverage drops below threshold
- **Code reviews** - See what new code isn't tested
- **Refactoring** - Ensure tests still cover the same code

---

### **Q33: How do you configure JUnit in Maven/Gradle?**

**Maven Configuration:**

**pom.xml dependencies:**
```xml
<dependencies>
    <!-- JUnit 5 -->
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.9.2</version>
        <scope>test</scope>
    </dependency>
    
    <!-- Mockito -->
    <dependency>
        <groupId>org.mockito</groupId>
        <artifactId>mockito-core</artifactId>
        <version>4.11.0</version>
        <scope>test</scope>
    </dependency>
    
    <!-- Mockito JUnit 5 extension -->
    <dependency>
        <groupId>org.mockito</groupId>
        <artifactId>mockito-junit-jupiter</artifactId>
        <version>4.11.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!-- Surefire plugin for running tests -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0-M9</version>
        </plugin>
    </plugins>
</build>
```

**Gradle Configuration:**

**build.gradle:**
```gradle
dependencies {
    // JUnit 5
    testImplementation 'org.junit.jupiter:junit-jupiter:5.9.2'
    
    // Mockito
    testImplementation 'org.mockito:mockito-core:4.11.0'
    testImplementation 'org.mockito:mockito-junit-jupiter:4.11.0'
}

test {
    useJUnitPlatform() // Enable JUnit 5
    
    // Optional: Configure test execution
    testLogging {
        events "passed", "skipped", "failed"
    }
}
```

**Common Maven commands:**
```bash
mvn test                    # Run all tests
mvn test -Dtest=Calculator* # Run specific test classes
mvn clean test              # Clean and run tests
```

**Common Gradle commands:**
```bash
./gradlew test              # Run all tests
./gradlew test --tests Calculator* # Run specific tests
./gradlew clean test        # Clean and run tests
```

**Directory structure:**
```
src/
├── main/
│   └── java/
│       └── com/example/Calculator.java
└── test/
    └── java/
        └── com/example/CalculatorTest.java
```

---

### **Q34: What's the difference between @RunWith and @ExtendWith?**

**@RunWith (JUnit 4) vs @ExtendWith (JUnit 5):**

| Aspect | @RunWith (JUnit 4) | @ExtendWith (JUnit 5) |
|--------|-------------------|---------------------|
| **Purpose** | Specify test runner | Specify test extension |
| **Usage** | Class level only | Class and method level |
| **Framework** | JUnit 4 | JUnit 5 |
| **Flexibility** | One runner per class | Multiple extensions |

**JUnit 4 with @RunWith:**
```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.mockito.junit.MockitoJUnitRunner;

@RunWith(MockitoJUnitRunner.class)
public class UserServiceTest {
    @Mock
    private UserRepository userRepository;
    
    @Test
    public void shouldCreateUser() {
        // Test code
    }
}
```

**Common JUnit 4 runners:**
- `MockitoJUnitRunner.class` - For Mockito support
- `SpringJUnit4ClassRunner.class` - For Spring integration
- `Parameterized.class` - For parameterized tests

**JUnit 5 with @ExtendWith:**
```java
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;
import org.mockito.junit.jupiter.MockitoExtension;

@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    @Mock
    private UserRepository userRepository;
    
    @Test
    void shouldCreateUser() {
        // Test code
    }
}
```

**Multiple extensions in JUnit 5:**
```java
@ExtendWith({MockitoExtension.class, SpringExtension.class})
class IntegrationTest {
    // Can use both Mockito and Spring features
}
```

**Method-level extensions (JUnit 5 only):**
```java
class DatabaseTest {
    @Test
    @ExtendWith(DatabaseExtension.class) // Only for this test
    void shouldAccessDatabase() {
        // Test with database extension
    }
}
```

**Migration from JUnit 4 to 5:**
```java
// JUnit 4
@RunWith(MockitoJUnitRunner.class)
public class OldTest {
    @Test
    public void test() { }
}

// JUnit 5 equivalent
@ExtendWith(MockitoExtension.class)
class NewTest {
    @Test
    void test() { }
}
```

**Key advantage of @ExtendWith:** More flexible, supports multiple extensions, works at method level.

---

### **Q35: Explain @WebMvcTest and @DataJpaTest annotations**

**These are Spring Boot test annotations for testing specific layers:**

### **@WebMvcTest - For Testing Web Layer**

**Purpose:** Test only the web layer (controllers) without loading the full application context.

**What it does:**
- Loads only web-related components (controllers, filters, etc.)
- Auto-configures MockMvc
- Doesn't load services, repositories, or database components
- Much faster than @SpringBootTest

**Example:**
```java
@WebMvcTest(UserController.class) // Test only this controller
class UserControllerTest {
    
    @Autowired
    private MockMvc mockMvc; // Auto-configured
    
    @MockBean // Mock the service dependency
    private UserService userService;
    
    @Test
    void shouldCreateUser() throws Exception {
        // Arrange
        User user = new User("John", "john@email.com");
        when(userService.createUser(any(User.class))).thenReturn(user);
        
        // Act & Assert
        mockMvc.perform(post("/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{\"name\":\"John\",\"email\":\"john@email.com\"}"))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.name").value("John"))
                .andExpect(jsonPath("$.email").value("john@email.com"));
        
        verify(userService).createUser(any(User.class));
    }
    
    @Test
    void shouldReturnBadRequestForInvalidInput() throws Exception {
        mockMvc.perform(post("/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{}")) // Empty JSON
                .andExpected(status().isBadRequest());
    }
}
```

**Controller being tested:**
```java
@RestController
public class UserController {
    @Autowired
    private UserService userService;
    
    @PostMapping("/users")
    public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
        User savedUser = userService.createUser(user);
        return ResponseEntity.ok(savedUser);
    }
}
```

### **@DataJpaTest - For Testing Repository Layer**

**Purpose:** Test JPA repositories without loading the full application context.

**What it does:**
- Configures in-memory database (H2 by default)
- Loads only JPA-related components
- Provides TestEntityManager for test data setup
- Transactions are rolled back after each test
- Much faster than full integration tests

**Example:**
```java
@DataJpaTest
class UserRepositoryTest {
    
    @Autowired
    private TestEntityManager entityManager; // For test data setup
    
    @Autowired
    private UserRepository userRepository; // Repository under test
    
    @Test
    void shouldFindUserByEmail() {
        // Arrange - Insert test data
        User user = new User("John", "john@email.com");
        entityManager.persistAndFlush(user);
        
        // Act
        Optional<User> found = userRepository.findByEmail("john@email.com");
        
        // Assert
        assertTrue(found.isPresent());
        assertEquals("John", found.get().getName());
    }
    
    @Test
    void shouldReturnEmptyWhenUserNotFound() {
        // Act
        Optional<User> found = userRepository.findByEmail("nonexistent@email.com");
        
        // Assert
        assertFalse(found.isPresent());
    }
    
    @Test
    void shouldSaveAndRetrieveUser() {
        // Arrange
        User user = new User("Jane", "jane@email.com");
        
        // Act
        User savedUser = userRepository.save(user);
        
        // Assert
        assertNotNull(savedUser.getId());
        assertEquals("Jane", savedUser.getName());
        
        // Verify it can be retrieved
        Optional<User> retrieved = userRepository.findById(savedUser.getId());
        assertTrue(retrieved.isPresent());
        assertEquals("Jane", retrieved.get().getName());
    }
}
```

**Repository being tested:**
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    Optional<User> findByEmail(String email);
    List<User> findByNameContaining(String name);
}
```

**Custom database configuration for @DataJpaTest:**
```java
@DataJpaTest
@TestPropertySource(properties = {
    "spring.jpa.show-sql=true",
    "spring.jpa.hibernate.ddl-auto=create-drop"
})
class UserRepositoryTest {
    // Tests with custom JPA settings
}
```

**Comparison:**

| Feature | @WebMvcTest | @DataJpaTest |
|---------|-------------|--------------|
| **Tests** | Controllers, web layer | Repositories, JPA layer |
| **Loads** | Web components only | JPA components only |
| **Database** | Not included | In-memory H2 database |
| **MockMvc** | Auto-configured | Not available |
| **TestEntityManager** | Not available | Auto-configured |
| **Speed** | Very fast | Fast |
| **Use case** | API endpoints, validation | Database queries, JPA mappings |

**When to use:**
- **@WebMvcTest** - Testing REST controllers, request/response handling, validation
- **@DataJpaTest** - Testing custom queries, repository methods, JPA mappings
- **@SpringBootTest** - Full integration tests (slower but more comprehensive)

---

## ⚡ QUICK-FIRE Questions (Know the Answers)

### **Q36: Can you run tests in parallel in JUnit?**

**Yes, JUnit 5 supports parallel test execution.**

**Enable parallel execution:**

**Method 1: junit-platform.properties file**
```properties
# Create src/test/resources/junit-platform.properties
junit.jupiter.execution.parallel.enabled=true
junit.jupiter.execution.parallel.mode.default=concurrent
```

**Method 2: System properties**
```bash
mvn test -Djunit.jupiter.execution.parallel.enabled=true
```

**Control parallel execution with annotations:**
```java
@Execution(ExecutionMode.CONCURRENT) // Run in parallel
class ParallelTest {
    
    @Test
    @Execution(ExecutionMode.SAME_THREAD) // Override: run sequentially
    void sequentialTest() {
        // This test runs alone
    }
    
    @Test
    void parallelTest1() {
        // Can run parallel with parallelTest2
    }
    
    @Test 
    void parallelTest2() {
        // Can run parallel with parallelTest1
    }
}
```

**Benefits:**
- **Faster test execution** - Especially with many tests
- **Better resource utilization** - Use multiple CPU cores

**Considerations:**
- **Thread safety** - Tests must not interfere with each other
- **Shared resources** - Database, files, static variables can cause issues
- **Test isolation** - Each test should be independent

---

### **Q37: What is @Nested annotation used for?**

**@Nested creates hierarchical test structures** - groups related tests together.

**Example:**
```java
class CalculatorTest {
    private Calculator calculator = new Calculator();
    
    @Nested
    class AdditionTests {
        @Test
        void shouldAddPositiveNumbers() {
            assertEquals(5, calculator.add(2, 3));
        }
        
        @Test
        void shouldAddNegativeNumbers() {
            assertEquals(-5, calculator.add(-2, -3));
        }
    }
    
    @Nested
    class DivisionTests {
        @Test
        void shouldDividePositiveNumbers() {
            assertEquals(2.0, calculator.divide(6, 3));
        }
        
        @Test
        void shouldThrowExceptionWhenDividingByZero() {
            assertThrows(ArithmeticException.class, 
                () -> calculator.divide(5, 0));
        }
    }
    
    @Nested
    class EdgeCaseTests {
        @Test
        void shouldHandleZero() {
            assertEquals(0, calculator.add(0, 0));
            assertEquals(0, calculator.multiply(5, 0));
        }
    }
}
```

**Benefits:**
- **Organization** - Group related tests logically
- **Readability** - Clear test structure
- **Setup sharing** - Nested classes can use outer class setup
- **Better reporting** - Test results show hierarchy

**With setup methods:**
```java
class UserServiceTest {
    private UserService userService;
    
    @BeforeEach
    void setUp() {
        userService = new UserService();
    }
    
    @Nested
    class WhenUserExists {
        private User existingUser;
        
        @BeforeEach
        void setUpExistingUser() {
            existingUser = new User("John");
            userService.save(existingUser);
        }
        
        @Test
        void shouldFindUser() {
            User found = userService.findById(existingUser.getId());
            assertEquals("John", found.getName());
        }
        
        @Test
        void shouldUpdateUser() {
            existingUser.setName("Jane");
            userService.update(existingUser);
            assertEquals("Jane", userService.findById(existingUser.getId()).getName());
        }
    }
    
    @Nested
    class WhenUserDoesNotExist {
        @Test
        void shouldReturnNull() {
            User found = userService.findById(999L);
            assertNull(found);
        }
        
        @Test
        void shouldThrowExceptionWhenUpdating() {
            User nonExistentUser = new User(999L, "Ghost");
            assertThrows(UserNotFoundException.class, 
                () -> userService.update(nonExistentUser));
        }
    }
}
```

---

### **Q38: How do you skip tests conditionally?**

**Several ways to skip tests based on conditions:**

**1. @EnabledOnOs / @DisabledOnOs**
```java
@Test
@EnabledOnOs(OS.WINDOWS)
void shouldRunOnWindowsOnly() {
    // Only runs on Windows
}

@Test
@DisabledOnOs({OS.LINUX, OS.MAC})
void shouldNotRunOnLinuxOrMac() {
    // Skipped on Linux and Mac
}
```

**2. @EnabledOnJre / @DisabledOnJre**
```java
@Test
@EnabledOnJre(JRE.JAVA_8)
void shouldRunOnJava8Only() {
    // Only runs on Java 8
}

@Test
@DisabledOnJre({JRE.JAVA_11, JRE.JAVA_17})
void shouldNotRunOnJava11Or17() {
    // Skipped on Java 11 and 17
}
```

**3. @EnabledIf / @DisabledIf (Custom conditions)**
```java
@Test
@EnabledIf("java.time.LocalDate.now().getDayOfWeek() == java.time.DayOfWeek.FRIDAY")
void shouldRunOnFridayOnly() {
    // Only runs on Fridays
}

@Test
@EnabledIf("systemProperty.get('test.environment') == 'integration'")
void shouldRunInIntegrationEnvironment() {
    // Only runs when system property is set
}
```

**4. @EnabledIfSystemProperty / @DisabledIfSystemProperty**
```java
@Test
@EnabledIfSystemProperty(named = "test.environment", matches = "integration")
void shouldRunInIntegrationTests() {
    // Only runs when -Dtest.environment=integration
}

@Test
@DisabledIfSystemProperty(named = "skip.slow.tests", matches = "true")
void slowTest() {
    // Skipped when -Dskip.slow.tests=true
}
```

**5. @EnabledIfEnvironmentVariable**
```java
@Test
@EnabledIfEnvironmentVariable(named = "CI", matches = "true")
void shouldRunInCIOnly() {
    // Only runs when CI=true environment variable is set
}
```

**6. Programmatic skipping with Assumptions**
```java
@Test
void shouldRunOnlyInDevelopment() {
    assumeTrue("development".equals(System.getProperty("environment")));
    
    // Test code only runs if assumption is true
    // Otherwise test is skipped
}

@Test
void shouldSkipOnWeekends() {
    LocalDate today = LocalDate.now();
    assumeFalse(today.getDayOfWeek() == DayOfWeek.SATURDAY || 
                today.getDayOfWeek() == DayOfWeek.SUNDAY);
    
    // Skipped on weekends
}
```

**Common use cases:**
- **Environment-specific tests** - Integration vs unit tests
- **OS-specific functionality** - File system operations
- **Resource availability** - Database, external services
- **Performance tests** - Skip slow tests in quick builds

---

### **Q39: What's the difference between unit and integration testing?**

**Quick comparison:**

| Aspect | Unit Testing | Integration Testing |
|--------|-------------|-------------------|
| **Scope** | Single component | Multiple components |
| **Dependencies** | Mocked/stubbed | Real dependencies |
| **Speed** | Very fast (ms) | Slower (seconds) |
| **Isolation** | Complete isolation | Tests interactions |
| **Setup** | Minimal | Complex setup |
| **Maintenance** | Easy | More complex |

**Unit Testing:**
```java
// Tests Calculator class in isolation
@ExtendWith(MockitoExtension.class)
class UserServiceUnitTest {
    @Mock
    private UserRepository userRepository; // Fake database
    
    @Mock 
    private EmailService emailService; // Fake email service
    
    @InjectMocks
    private UserService userService; // Real service, fake dependencies
    
    @Test
    void shouldCreateUser() {
        // Arrange - Control exactly what dependencies return
        User user = new User("John");
        when(userRepository.save(any(User.class))).thenReturn(user);
        
        // Act - Test only UserService logic
        User result = userService.createUser(user);
        
        // Assert - Verify UserService behavior
        assertEquals("John", result.getName());
        verify(emailService).sendWelcomeEmail(user.getEmail());
    }
}
```

**Integration Testing:**
```java
// Tests multiple components working together
@SpringBootTest
@TestPropertySource(properties = "spring.datasource.url=jdbc:h2:mem:testdb")
class UserServiceIntegrationTest {
    
    @Autowired
    private UserService userService; // Real service
    
    @Autowired
    private UserRepository userRepository; // Real repository
    
    // EmailService might be mocked or real depending on test scope
    
    @Test
    void shouldCreateUserEndToEnd() {
        // Act - Test real interaction between service and database
        User user = new User("John", "john@email.com");
        User savedUser = userService.createUser(user);
        
        // Assert - Verify data was actually saved to database
        assertNotNull(savedUser.getId());
        
        User foundUser = userRepository.findById(savedUser.getId()).orElse(null);
        assertNotNull(foundUser);
        assertEquals("John", foundUser.getName());
    }
}
```

**When to use:**

**Unit Tests:**
- Test business logic
- Fast feedback during development
- Easy to write and maintain
- Good for TDD (Test-Driven Development)
- Should make up 70-80% of your tests

**Integration Tests:**
- Test component interactions
- Verify system works end-to-end
- Test configuration and wiring
- Catch issues unit tests miss
- Should make up 15-25% of your tests

**Test Pyramid:**
```
        /\
       /  \    E2E Tests (5-10%)
      /____\   
     /      \  Integration Tests (15-25%)
    /________\ 
   /          \ Unit Tests (70-80%)
  /__________\
```

**Real-world example:**
```java
// Unit test - fast, isolated
@Test
void shouldCalculateOrderTotal() {
    when(taxService.getTaxRate()).thenReturn(0.1);
    assertEquals(11.0, orderService.calculateTotal(10.0));
}

// Integration test - real components
@Test
void shouldProcessOrderCompleteFlow() {
    Order order = orderService.createOrder("item123");
    paymentService.processPayment(order);
    assertTrue(order.isPaid());
    assertEquals(OrderStatus.CONFIRMED, order.getStatus());
}
```

---

### **Q40: What is TestNG? How does it compare to JUnit?**

**TestNG** is another Java testing framework, similar to JUnit but with additional features.

**Key differences:**

| Feature | JUnit 5 | TestNG |
|---------|---------|--------|
| **Annotations** | @Test, @BeforeEach | @Test, @BeforeMethod |
| **Grouping** | @Nested, @Tag | @Test(groups="...") |
| **Dependencies** | Limited | @Test(dependsOnMethods="...") |
| **Parameterization** | @ParameterizedTest | @DataProvider |
| **Parallel execution** | Built-in | Built-in |
| **Soft assertions** | No | Yes |

**TestNG example:**
```java
public class CalculatorTestNG {
    
    @BeforeClass
    public void setUp() {
        // Runs once before all tests in class
    }
    
    @Test(groups = "arithmetic")
    public void testAddition() {
        assertEquals(calculator.add(2, 3), 5);
    }
    
    @Test(groups = "arithmetic", dependsOnMethods = "testAddition")
    public void testMultiplication() {
        // Only runs if testAddition passes
        assertEquals(calculator.multiply(2, 3), 6);
    }
    
    @Test(dataProvider = "numbers")
    public void testWithData(int a, int b, int expected) {
        assertEquals(calculator.add(a, b), expected);
    }
    
    @DataProvider(name = "numbers")
    public Object[][] provideNumbers() {
        return new Object[][] {
            {1, 2, 3},
            {5, 5, 10},
            {-1, 1, 0}
        };
    }
}
```

**TestNG advantages:**
- **Test dependencies** - Run tests in specific order
- **Flexible grouping** - Run subsets of tests easily
- **Built-in data providers** - Simpler parameterized tests
- **Soft assertions** - Continue after assertion failure
- **Better reporting** - Rich HTML reports

**JUnit advantages:**
- **Simpler** - Easier to learn and use
- **Better Spring integration** - @SpringBootTest works seamlessly
- **More popular** - Larger community, more resources
- **Faster evolution** - JUnit 5 has modern features

**When to choose:**
- **JUnit** - Most Java projects, Spring applications, simpler testing needs
- **TestNG** - Complex test suites, need test dependencies, advanced reporting

**Migration considerations:**
```java
// TestNG
@Test(groups = "integration", dependsOnMethods = "setUp")
public void testUserService() { }

// JUnit 5 equivalent
@Test
@Tag("integration")
void testUserService() {
    // Use @BeforeEach instead of dependsOnMethods
}
```

Most teams choose **JUnit** because it's simpler and integrates better with the Java ecosystem.

---

## 🎪 BONUS Advanced Questions (If Time Permits)

### **Q41: What is Test-Driven Development (TDD)?**

**TDD is a development approach:** Write tests first, then write code to make them pass.

**The TDD Cycle (Red-Green-Refactor):**

**1. Red** - Write a failing test
**2. Green** - Write minimal code to make test pass  
**3. Refactor** - Improve code while keeping tests green

**Example TDD flow:**

**Step 1: Red - Write failing test**
```java
@Test
void shouldCalculateAreaOfRectangle() {
    Rectangle rectangle = new Rectangle(5, 3);
    assertEquals(15.0, rectangle.getArea());
}
```
*Compile error - Rectangle class doesn't exist*

**Step 2: Green - Minimal code to pass**
```java
public class Rectangle {
    private double width, height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    public double getArea() {
        return width * height; // Simplest implementation
    }
}
```
*Test passes!*

**Step 3: Refactor - Improve code**
```java
public class Rectangle {
    private final double width;
    private final double height;
    
    public Rectangle(double width, double height) {
        if (width <= 0 || height <= 0) {
            throw new IllegalArgumentException("Dimensions must be positive");
        }
        this.width = width;
        this.height = height;
    }
    
    public double getArea() {
        return width * height;
    }
}
```

**Add test for edge case:**
```java
@Test
void shouldThrowExceptionForNegativeDimensions() {
    assertThrows(IllegalArgumentException.class, 
        () -> new Rectangle(-5, 3));
}
```

**TDD Benefits:**
- **Better design** - Think about usage before implementation
- **100% test coverage** - Every line has a test
- **Fewer bugs** - Catch issues early
- **Confidence** - Safe to refactor
- **Documentation** - Tests show how code should be used

**TDD Challenges:**
- **Learning curve** - Requires practice
- **Slower initially** - More upfront thinking
- **Discipline required** - Easy to skip tests when pressured

**Common TDD patterns:**
- **Fake it 'til you make it** - Return hardcoded values first
- **Triangulation** - Use multiple examples to drive generalization
- **Obvious implementation** - Write real code when logic is clear

---

### **Q42: How do you test multithreaded code?**

**Testing concurrent code is challenging** - threads can execute in any order.

**Strategies:**

**1. Test with controlled concurrency:**
```java
@Test
void shouldHandleConcurrentAccess() throws InterruptedException {
    CounterService counter = new CounterService();
    int threadCount = 10;
    int incrementsPerThread = 100;
    
    CountDownLatch startSignal = new CountDownLatch(1);
    CountDownLatch doneSignal = new CountDownLatch(threadCount);
    
    // Create threads
    for (int i = 0; i < threadCount; i++) {
        new Thread(() -> {
            try {
                startSignal.await(); // Wait for start signal
                for (int j = 0; j < incrementsPerThread; j++) {
                    counter.increment();
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            } finally {
                doneSignal.countDown();
            }
        }).start();
    }
    
    startSignal.countDown(); // Start all threads
    doneSignal.await(); // Wait for all to complete
    
    assertEquals(threadCount * incrementsPerThread, counter.getValue());
}
```

**2. Test thread safety with repeated execution:**
```java
@RepeatedTest(100) // Run 100 times to catch race conditions
void shouldBeThreadSafe() {
    SharedResource resource = new SharedResource();
    
    CompletableFuture<Void> task1 = CompletableFuture.runAsync(() -> 
        resource.updateValue("Thread1"));
    CompletableFuture<Void> task2 = CompletableFuture.runAsync(() -> 
        resource.updateValue("Thread2"));
    
    CompletableFuture.allOf(task1, task2).join();
    
    // Verify final state is consistent
    assertNotNull(resource.getValue());
    assertTrue(resource.getValue().startsWith("Thread"));
}
```

**3. Mock time-sensitive operations:**
```java
@Test
void shouldTimeoutAfterDelay() {
    Clock mockClock = mock(Clock.class);
    when(mockClock.millis())
        .thenReturn(0L)      // Start time
        .thenReturn(5000L);  // After 5 seconds
    
    # JUnit Interview Questions - Easy-to-Understand Answers

## 🔥 MUST-KNOW Questions (High Priority)

### **Q1: What is unit testing and why is it important?**

**Simple Answer:**
Unit testing means testing individual pieces (units) of your code in isolation to make sure they work correctly.

**Why it's important:**
- **Catches bugs early** - Find problems before they reach production
- **Makes code reliable** - You know each piece works as expected
- **Saves time** - Automated tests run quickly vs manual testing
- **Documentation** - Tests show how your code should behave
- **Safe refactoring** - Change code confidently knowing tests will catch issues

**Example:** Testing a calculator's `add(2, 3)` method to ensure it returns `5`.

---

### **Q2: What is JUnit? What's the difference between JUnit 4 and JUnit 5?**

**What is JUnit:**
JUnit is a popular Java framework for writing and running unit tests. It provides annotations, assertions, and tools to test your code.

**JUnit 4 vs JUnit 5:**

| Feature | JUnit 4 | JUnit 5 |
|---------|---------|---------|
| **Annotations** | `@Before`, `@After`, `@BeforeClass` | `@BeforeEach`, `@AfterEach`, `@BeforeAll` |
| **Architecture** | Single JAR | Modular (Platform + Jupiter + Vintage) |
| **Java Version** | Java 5+ | Java 8+ |
| **Assertions** | Basic assertions | More powerful assertions + custom messages |
| **Parameterized Tests** | Complex setup | Simple `@ParameterizedTest` |

**Key Point:** JUnit 5 is more modern, flexible, and easier to use.

---

### **Q3: Explain @Test, @BeforeEach, @AfterEach, @BeforeAll, @AfterAll annotations**

**@Test** - Marks a method as a test case
```java
@Test
void shouldAddTwoNumbers() {
    // Test logic here
}
```

**@BeforeEach** - Runs before EACH test method
```java
@BeforeEach
void setUp() {
    calculator = new Calculator(); // Create fresh instance for each test
}
```

**@AfterEach** - Runs after EACH test method
```java
@AfterEach
void tearDown() {
    calculator = null; // Clean up after each test
}
```

**@BeforeAll** - Runs ONCE before all tests in the class
```java
@BeforeAll
static void initializeDatabase() {
    // Setup that's expensive and can be shared
}
```

**@AfterAll** - Runs ONCE after all tests in the class
```java
@AfterAll
static void cleanupDatabase() {
    // Final cleanup
}
```

---

### **Q4: What are assertions in JUnit? Name 5 common assertion methods**

**What are assertions:**
Assertions are statements that check if your code produces the expected result. If the assertion fails, the test fails.

**5 Common Assertion Methods:**

1. **assertEquals(expected, actual)** - Checks if two values are equal
```java
assertEquals(5, calculator.add(2, 3));
```

2. **assertTrue(condition)** - Checks if condition is true
```java
assertTrue(user.isActive());
```

3. **assertFalse(condition)** - Checks if condition is false
```java
assertFalse(user.isDeleted());
```

4. **assertNull(object)** - Checks if object is null
```java
assertNull(user.getDeletedDate());
```

5. **assertNotNull(object)** - Checks if object is not null
```java
assertNotNull(user.getId());
```

---

### **Q5: How do you test that a method throws an exception?**

**Two ways to test exceptions:**

**Method 1: Using assertThrows() (Recommended)**
```java
@Test
void shouldThrowExceptionWhenDividingByZero() {
    Calculator calculator = new Calculator();
    
    ArithmeticException exception = assertThrows(
        ArithmeticException.class,
        () -> calculator.divide(10, 0)
    );
    
    assertEquals("Cannot divide by zero", exception.getMessage());
}
```

**Method 2: Using try-catch (Old way)**
```java
@Test
void shouldThrowExceptionWhenDividingByZero() {
    Calculator calculator = new Calculator();
    
    try {
        calculator.divide(10, 0);
        fail("Expected ArithmeticException");
    } catch (ArithmeticException e) {
        assertEquals("Cannot divide by zero", e.getMessage());
    }
}
```

**Key Point:** Use `assertThrows()` - it's cleaner and more readable.

---

## Mocking & Dependencies

### **Q6: What is mocking and why do we need it in unit testing?**

**What is mocking:**
Mocking means creating fake versions of dependencies (like databases, web services, other classes) so you can test your code in isolation.

**Why we need it:**
- **Isolation** - Test only YOUR code, not external dependencies
- **Speed** - Fake objects are faster than real databases/web calls
- **Control** - Make the fake object return exactly what you want for testing
- **Reliability** - Tests won't fail because external services are down

**Real-world example:**
```java
// Instead of calling real database (slow, unreliable)
UserService userService = new UserService(realDatabase);

// Use a mock database (fast, predictable)
UserRepository mockRepository = mock(UserRepository.class);
UserService userService = new UserService(mockRepository);
```

---

### **Q7: Explain @Mock, @InjectMocks annotations in Mockito**

**@Mock** - Creates a fake version of a class
```java
@Mock
private UserRepository userRepository; // Fake repository
```

**@InjectMocks** - Injects the mocks into the class you're testing
```java
@InjectMocks
private UserService userService; // Real service with fake dependencies injected
```

**Complete example:**
```java
class UserServiceTest {
    @Mock
    private UserRepository userRepository; // Fake dependency
    
    @InjectMocks
    private UserService userService; // Real class being tested
    
    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this); // Initialize mocks
    }
}
```

**Think of it like:** @Mock creates fake actors, @InjectMocks puts them on the real stage.

---

### **Q8: How do you stub a method using Mockito? (when().thenReturn())**

**Stubbing** means telling a mock object what to return when a method is called.

**Basic syntax:**
```java
when(mockObject.methodCall()).thenReturn(expectedResult);
```

**Examples:**
```java
@Test
void shouldFindUserById() {
    // Arrange - Tell mock what to return
    User expectedUser = new User("John", "john@email.com");
    when(userRepository.findById(1L)).thenReturn(expectedUser);
    
    // Act - Call the real method
    User actualUser = userService.getUserById(1L);
    
    // Assert - Check the result
    assertEquals("John", actualUser.getName());
}
```

**Different stubbing options:**
```java
// Return a value
when(mock.method()).thenReturn(value);

// Throw an exception
when(mock.method()).thenThrow(new RuntimeException("Error"));

// Return different values on multiple calls
when(mock.method()).thenReturn(value1).thenReturn(value2);
```

---

### **Q9: How do you verify that a mock method was called?**

**Use verify() to check if a mock method was called:**

**Basic verification:**
```java
@Test
void shouldSaveUser() {
    User user = new User("John");
    
    // Act
    userService.createUser(user);
    
    // Verify the repository's save method was called with the user
    verify(userRepository).save(user);
}
```

**Advanced verification:**
```java
// Verify method was called exactly once
verify(userRepository, times(1)).save(any(User.class));

// Verify method was called 3 times
verify(userRepository, times(3)).findById(anyLong());

// Verify method was never called
verify(userRepository, never()).delete(any(User.class));

// Verify no interactions with mock
verifyNoInteractions(userRepository);
```

---

## Best Practices

### **Q10: What are the FIRST principles of unit testing?**

**FIRST is an acronym for good unit test principles:**

**F - Fast**
- Tests should run quickly (milliseconds, not seconds)
- Use mocks instead of real databases/web calls

**I - Independent**
- Tests shouldn't depend on each other
- Each test should work alone and in any order

**R - Repeatable**
- Same test should give same result every time
- Don't depend on random data or current time

**S - Self-Validating**
- Test should clearly pass or fail
- No manual checking of log files

**T - Timely**
- Write tests at the right time (ideally before or with the code)
- Don't leave testing until the end

**Memory trick:** "Fast Independent Repeatable Self-validating Timely tests"

---

### **Q11: How should you name your test methods?**

**Good naming patterns:**

**Pattern 1: should_ExpectedBehavior_When_StateUnderTest**
```java
@Test
void should_ReturnSum_When_AddingTwoPositiveNumbers() { }

@Test
void should_ThrowException_When_DividingByZero() { }
```

**Pattern 2: methodName_Condition_ExpectedResult**
```java
@Test
void add_TwoPositiveNumbers_ReturnsSum() { }

@Test
void divide_ByZero_ThrowsException() { }
```

**Pattern 3: Simple descriptive names**
```java
@Test
void shouldAddTwoNumbers() { }

@Test
void shouldThrowExceptionForInvalidInput() { }
```

**Key principles:**
- Be descriptive and clear
- Include what you're testing
- Include expected behavior
- Use consistent naming in your team

---

### **Q12: What is code coverage? What's a good coverage percentage?**

**What is code coverage:**
Code coverage measures how much of your code is executed when tests run. It's usually shown as a percentage.

**Types of coverage:**
- **Line coverage** - What percentage of code lines are executed
- **Branch coverage** - What percentage of if/else branches are tested
- **Method coverage** - What percentage of methods are called

**Good coverage percentage:**
- **70-80%** is generally considered good
- **80-90%** is very good
- **90%+** might be overkill for some projects

**Important points:**
- **Quality over quantity** - 60% well-written tests > 95% poor tests
- **100% coverage doesn't mean bug-free code**
- **Focus on critical business logic first**
- **Don't test getters/setters or simple constructors**

**Example:**
```java
// This method has 3 lines
public String processUser(User user) {
    if (user == null) return "Invalid";     // Line 1
    if (user.isActive()) return "Active";   // Line 2
    return "Inactive";                      // Line 3
}

// Testing all paths gives 100% line coverage
// Testing only valid active users gives 66% coverage
```

---

## 💡 LIKELY Questions (Medium Priority)

### Technical Details

### **Q13: What's the difference between assertEquals and assertSame?**

**assertEquals** - Compares VALUES (uses .equals() method)
**assertSame** - Compares OBJECT REFERENCES (uses == operator)

**Simple explanation:**
- `assertEquals` asks: "Do these have the same content?"
- `assertSame` asks: "Are these the exact same object in memory?"

**Examples:**
```java
@Test
void demonstrateEqualsVsSame() {
    String str1 = new String("Hello");
    String str2 = new String("Hello");
    String str3 = str1;
    
    // assertEquals - Compares content (PASSES)
    assertEquals(str1, str2); // Both contain "Hello"
    
    // assertSame - Compares references (FAILS)
    assertSame(str1, str2); // Different objects in memory
    
    // assertSame - Same reference (PASSES)
    assertSame(str1, str3); // str3 points to same object as str1
}
```

**When to use:**
- **assertEquals** - 99% of the time (comparing values)
- **assertSame** - When you need to ensure it's the exact same object (rare)

---

### **Q14: When would you use @Disabled annotation?**

**@Disabled** temporarily turns off a test without deleting it.

**Common scenarios:**

**1. Broken test that needs fixing later:**
```java
@Test
@Disabled("Bug #1234 - fix database connection issue")
void shouldConnectToDatabase() {
    // Test that's currently failing due to known issue
}
```

**2. Slow test you don't want to run regularly:**
```java
@Test
@Disabled("Performance test - run manually only")
void shouldHandleMillionRecords() {
    // Very slow test
}
```

**3. Feature not yet implemented:**
```java
@Test
@Disabled("Feature coming in Sprint 5")
void shouldSendEmailNotification() {
    // Test for future feature
}
```

**4. Environment-specific tests:**
```java
@Test
@Disabled("Only works in production environment")
void shouldAccessProductionAPI() {
    // Test that can't run locally
}
```

**Key point:** Always include a reason WHY the test is disabled!

---

### **Q15: Explain the AAA pattern in unit testing**

**AAA stands for Arrange, Act, Assert** - a standard structure for writing clear tests.

**The pattern:**

**Arrange** - Set up test data and mocks
**Act** - Call the method you're testing
**Assert** - Check the results

**Example:**
```java
@Test
void shouldCalculateDiscountedPrice() {
    // ARRANGE - Set up test data
    Product product = new Product("Laptop", 1000.0);
    DiscountService discountService = new DiscountService();
    double discountRate = 0.1; // 10% discount
    
    // ACT - Call the method being tested
    double discountedPrice = discountService.applyDiscount(product, discountRate);
    
    // ASSERT - Check the result
    assertEquals(900.0, discountedPrice);
}
```

**Why AAA is good:**
- **Readable** - Anyone can understand the test structure
- **Consistent** - All tests follow same pattern
- **Maintainable** - Easy to modify each section
- **Clear separation** - Setup vs testing vs verification

**Pro tip:** Use comments or blank lines to separate the three sections!

---

### **Q16: What's the difference between @Mock and @Spy in Mockito?**

**Simple difference:**

**@Mock** - Creates a completely FAKE object (all methods return null/empty by default)
**@Spy** - Creates a PARTIAL fake that calls real methods unless you override them

**Visual comparison:**

| @Mock | @Spy |
|-------|------|
| 100% fake | Real object + selective faking |
| All methods stubbed | Real methods work + you can override |
| Must stub everything | Stub only what you need |

**Examples:**

**@Mock example:**
```java
@Mock
private List<String> mockList;

@Test
void mockExample() {
    // Mock returns null by default
    assertNull(mockList.get(0)); // No exception, returns null
    
    // Must stub everything you want to work
    when(mockList.size()).thenReturn(5);
    assertEquals(5, mockList.size());
}
```

**@Spy example:**
```java
@Spy
private List<String> spyList = new ArrayList<>();

@Test
void spyExample() {
    // Real methods work
    spyList.add("Hello");
    assertEquals(1, spyList.size()); // Real behavior
    
    // But you can override specific methods
    when(spyList.size()).thenReturn(100);
    assertEquals(100, spyList.size()); // Overridden behavior
}
```

**When to use:**
- **@Mock** - When you want complete control (most common)
- **@Spy** - When you want mostly real behavior with some overrides

---

### **Q17: How do you test void methods?**

**Void methods don't return values, so you test their SIDE EFFECTS:**

**1. Verify method calls on dependencies:**
```java
@Test
void shouldSaveUserWhenCreatingAccount() {
    User user = new User("John");
    
    // Act - call void method
    userService.createAccount(user);
    
    // Assert - verify the side effect
    verify(userRepository).save(user);
}
```

**2. Check state changes:**
```java
@Test
void shouldActivateUser() {
    User user = new User("John");
    assertFalse(user.isActive()); // Initial state
    
    // Act - call void method
    userService.activateUser(user);
    
    // Assert - check state changed
    assertTrue(user.isActive());
}
```

**3. Verify exceptions are thrown:**
```java
@Test
void shouldThrowExceptionForInvalidUser() {
    User invalidUser = null;
    
    // Assert exception is thrown
    assertThrows(IllegalArgumentException.class, 
        () -> userService.validateUser(invalidUser));
}
```

**4. Check external interactions:**
```java
@Test
void shouldSendEmailNotification() {
    User user = new User("john@email.com");
    
    userService.sendWelcomeEmail(user);
    
    // Verify email service was called
    verify(emailService).sendEmail(eq("john@email.com"), anyString());
}
```

---

### **Q18: What makes a good unit test?**

**A good unit test is:**

**1. Fast** - Runs in milliseconds
```java
// Good - uses mocks, no database
@Test
void shouldCalculateTotal() {
    when(priceService.getPrice("item")).thenReturn(10.0);
    assertEquals(10.0, orderService.calculateTotal("item"));
}
```

**2. Independent** - Doesn't depend on other tests
```java
// Good - each test sets up its own data
@BeforeEach
void setUp() {
    calculator = new Calculator(); // Fresh instance each time
}
```

**3. Clear and readable**
```java
// Good - clear naming and structure
@Test
void shouldReturnZeroWhenCalculatingEmptyOrder() {
    Order emptyOrder = new Order();
    assertEquals(0.0, orderService.calculateTotal(emptyOrder));
}
```

**4. Tests one thing** - Single responsibility
```java
// Good - tests only addition
@Test 
void shouldAddTwoNumbers() {
    assertEquals(5, calculator.add(2, 3));
}

// Bad - tests multiple operations
@Test
void shouldDoMath() {
    assertEquals(5, calculator.add(2, 3));
    assertEquals(6, calculator.multiply(2, 3)); // Different concern
}
```

**5. Predictable** - Same result every time
```java
// Good - controlled input
@Test
void shouldFormatDate() {
    LocalDate date = LocalDate.of(2023, 1, 15);
    assertEquals("15-Jan-2023", formatter.format(date));
}

// Bad - depends on current time
@Test
void shouldFormatCurrentDate() {
    assertEquals("Today", formatter.format(LocalDate.now())); // Unpredictable
}
```

---

### Practical Scenarios

### **Q19: How would you test a service layer method that calls a repository?**

**Scenario:** UserService that calls UserRepository

**Service class:**
```java
public class UserService {
    private UserRepository userRepository;
    
    public User getUserById(Long id) {
        if (id == null) {
            throw new IllegalArgumentException("ID cannot be null");
        }
        return userRepository.findById(id);
    }
}
```

**Test approach:**
```java
class UserServiceTest {
    @Mock
    private UserRepository userRepository; // Mock the dependency
    
    @InjectMocks
    private UserService userService; // Inject mock into service
    
    @Test
    void shouldReturnUserWhenValidIdProvided() {
        // Arrange
        Long userId = 1L;
        User expectedUser = new User("John", "john@email.com");
        when(userRepository.findById(userId)).thenReturn(expectedUser);
        
        // Act
        User actualUser = userService.getUserById(userId);
        
        // Assert
        assertEquals("John", actualUser.getName());
        assertEquals("john@email.com", actualUser.getEmail());
        verify(userRepository).findById(userId); // Verify repository was called
    }
    
    @Test
    void shouldThrowExceptionWhenIdIsNull() {
        // Act & Assert
        assertThrows(IllegalArgumentException.class, 
            () -> userService.getUserById(null));
        
        // Verify repository was never called
        verifyNoInteractions(userRepository);
    }
}
```

**Key points:**
1. **Mock the repository** - Don't hit real database
2. **Stub repository methods** - Control what they return
3. **Test business logic** - Validation, error handling
4. **Verify interactions** - Ensure repository is called correctly

---

### **Q20: What test cases would you write for a calculator's divide method?**

**Think about different scenarios:**

```java
class CalculatorTest {
    private Calculator calculator = new Calculator();
    
    @Test
    void shouldDivideTwoPositiveNumbers() {
        assertEquals(2.0, calculator.divide(6, 3));
    }
    
    @Test
    void shouldDivideNegativeNumbers() {
        assertEquals(-2.0, calculator.divide(-6, 3));
        assertEquals(2.0, calculator.divide(-6, -3));
    }
    
    @Test
    void shouldHandleDecimalResults() {
        assertEquals(1.5, calculator.divide(3, 2), 0.001); // Delta for floating point
    }
    
    @Test
    void shouldThrowExceptionWhenDividingByZero() {
        ArithmeticException exception = assertThrows(
            ArithmeticException.class,
            () -> calculator.divide(5, 0)
        );
        assertEquals("Cannot divide by zero", exception.getMessage());
    }
    
    @Test
    void shouldReturnZeroWhenDividingZeroByNumber() {
        assertEquals(0.0, calculator.divide(0, 5));
    }
    
    @Test
    void shouldHandleVeryLargeNumbers() {
        assertEquals(2.0, calculator.divide(Double.MAX_VALUE, Double.MAX_VALUE/2), 0.001);
    }
    
    @Test
    void shouldHandleVerySmallNumbers() {
        double result = calculator.divide(0.000001, 0.000002);
        assertEquals(0.5, result, 0.001);
    }
}
```

**Test case categories:**
- **Happy path** - Normal positive cases
- **Edge cases** - Zero, negative numbers, decimals
- **Error cases** - Division by zero
- **Boundary cases** - Very large/small numbers

---

### **Q21: How do you handle flaky tests?**

**Flaky tests** pass sometimes and fail sometimes without code changes.

**Common causes and solutions:**

**1. Time-dependent tests:**
```java
// Bad - depends on current time
@Test
void shouldCreateRecentTimestamp() {
    long timestamp = service.getCurrentTimestamp();
    assertTrue(timestamp > System.currentTimeMillis() - 1000);
}

// Good - inject time dependency
@Test
void shouldCreateTimestampWithInjectedTime() {
    Clock fixedClock = Clock.fixed(Instant.parse("2023-01-01T10:00:00Z"), ZoneOffset.UTC);
    TimeService service = new TimeService(fixedClock);
    assertEquals(1672574400000L, service.getCurrentTimestamp());
}
```

**2. Random data:**
```java
// Bad - random values
@Test
void shouldProcessRandomNumber() {
    int random = new Random().nextInt(100);
    assertTrue(service.process(random) > 0); // Might fail for some values
}

// Good - fixed test data
@Test
void shouldProcessSpecificNumbers() {
    assertEquals(10, service.process(5));
    assertEquals(20, service.process(10));
}
```

**3. Async operations:**
```java
// Bad - timing issues
@Test
void shouldCompleteAsyncOperation() {
    service.startAsyncTask();
    Thread.sleep(100); // Unreliable timing
    assertTrue(service.isCompleted());
}

// Good - proper waiting
@Test
void shouldCompleteAsyncOperation() {
    service.startAsyncTask();
    await().atMost(5, SECONDS).until(() -> service.isCompleted());
}
```

**General strategies:**
- **Make tests deterministic** - Remove randomness
- **Use test doubles** - Mock external dependencies
- **Fix timing issues** - Use proper waits instead of sleeps
- **Isolate tests** - Ensure tests don't affect each other

---

### **Q22: Why is 100% code coverage not always desirable?**

**100% coverage problems:**

**1. Quality vs Quantity**
```java
// Bad test - 100% coverage but no real testing
@Test
void shouldGetName() {
    User user = new User("John");
    assertEquals("John", user.getName()); // Just testing getter
}

// Good test - Lower coverage but tests real logic
@Test
void shouldValidateEmailFormat() {
    assertThrows(InvalidEmailException.class, 
        () -> user.setEmail("invalid-email"));
}
```

**2. Maintenance overhead**
- More tests = more code to maintain
- Trivial tests break when refactoring
- Time spent on low-value tests

**3. Diminishing returns**
- Last 10% coverage often tests trivial code
- 90% coverage might catch 99% of bugs
- Better to write fewer, high-quality tests

**4. False confidence**
```java
// 100% line coverage but doesn't test edge cases
public String processUser(String name) {
    if (name != null) {        // Line 1 - covered
        return name.trim();    // Line 2 - covered  
    }
    return "Unknown";          // Line 3 - covered
}

// Test covers all lines but misses empty string case
@Test
void testProcessUser() {
    assertEquals("John", processUser("John"));    // Covers line 1,2
    assertEquals("Unknown", processUser(null));   // Covers line 3
    // Missing: processUser("") - would cause bug!
}
```

**Better approach:**
- **Focus on critical business logic**
- **Test edge cases and error conditions**
- **Aim for 70-80% coverage with high-quality tests**
- **Use coverage as a guide, not a goal**
