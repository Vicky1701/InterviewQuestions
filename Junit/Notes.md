# JUnit Testing Complete Guide

## 1. What is Unit Testing & Why is it Important

### Definition of Unit Test
A unit test is a type of software testing where individual components or modules of a software application are tested in isolation. The "unit" typically refers to the smallest testable part of an application, such as a function, method, or class.

### Benefits
- **Catch bugs early**: Identify issues during development rather than in production
- **Improve code quality**: Writing testable code often leads to better design and cleaner architecture
- **Refactoring safety**: Tests provide confidence when modifying existing code by ensuring functionality remains intact
- **Documentation**: Tests serve as living documentation showing how code should behave
- **Faster debugging**: Pinpoint exact location of failures quickly

## 2. JUnit Basics

### What is JUnit?
JUnit is a popular open-source testing framework for Java applications. It provides annotations, assertions, and test runners to write and execute unit tests effectively.

### Popular Versions: JUnit 4 vs JUnit 5
- **JUnit 4**: Legacy version, still widely used, simpler annotation set
- **JUnit 5**: Modern version with enhanced features, modular architecture, and better integration with modern Java features

### Maven/Gradle Dependencies

**Maven (JUnit 5):**
```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```

**Gradle (JUnit 5):**
```gradle
testImplementation 'org.junit.jupiter:junit-jupiter:5.10.0'
```

### Key Annotations

#### @Test
Marks a method as a test method that should be executed by the test runner.

#### @BeforeEach, @AfterEach
- `@BeforeEach`: Executed before each test method (setup)
- `@AfterEach`: Executed after each test method (cleanup)

#### @BeforeAll, @AfterAll
- `@BeforeAll`: Executed once before all test methods in the class
- `@AfterAll`: Executed once after all test methods in the class

#### @Disabled
Temporarily disables a test method or entire test class from execution.

## 3. Assertions

Assertions are used to verify expected outcomes in your tests. JUnit provides various assertion methods:

### assertEquals(expected, actual)
Verifies that two values are equal.
```java
assertEquals(5, calculator.add(2, 3));
```

### assertTrue(condition)
Verifies that a condition is true.
```java
assertTrue(result > 0);
```

### assertFalse(condition)
Verifies that a condition is false.
```java
assertFalse(list.isEmpty());
```

### assertThrows(exception.class, () -> { })
Verifies that a specific exception is thrown.
```java
assertThrows(IllegalArgumentException.class, () -> {
    calculator.divide(10, 0);
});
```

### assertNotNull() and assertNull()
- `assertNotNull()`: Verifies that an object is not null
- `assertNull()`: Verifies that an object is null

## 4. Testing a Simple Service Layer

Here's an example of testing a simple CalculatorService:

```java
public class CalculatorService {
    public int add(int a, int b) {
        return a + b;
    }
    
    public int subtract(int a, int b) {
        return a - b;
    }
    
    public int multiply(int a, int b) {
        return a * b;
    }
    
    public double divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("Cannot divide by zero");
        }
        return (double) a / b;
    }
}
```

**Test Class:**
```java
class CalculatorServiceTest {
    
    private CalculatorService calculator;
    
    @BeforeEach
    void setUp() {
        calculator = new CalculatorService();
    }
    
    @Test
    void add_shouldReturnSum_whenValidInputs() {
        // Given
        int a = 5;
        int b = 3;
        
        // When
        int result = calculator.add(a, b);
        
        // Then
        assertEquals(8, result);
    }
    
    @Test
    void divide_shouldThrowException_whenDivideByZero() {
        // Given & When & Then
        assertThrows(IllegalArgumentException.class, () -> {
            calculator.divide(10, 0);
        });
    }
}
```

## 5. Mocking with Mockito

### Why Mocking is Needed
Mocking isolates the unit under test by replacing external dependencies with controlled fake objects. This is essential to:
- Avoid database or API calls during testing
- Control return values and behavior of dependencies
- Test error scenarios that are difficult to reproduce with real objects
- Keep tests fast and reliable

### Common Annotations

#### @Mock
Creates a mock instance of a class or interface.

#### @InjectMocks
Injects mock dependencies into the class being tested.

#### Mockito.when().thenReturn()
Defines the behavior of mock objects.

**Example:**
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private UserRepository userRepository;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    void getUser_shouldReturnUser_whenUserExists() {
        // Given
        User expectedUser = new User("John", "john@example.com");
        Mockito.when(userRepository.findById(1L)).thenReturn(expectedUser);
        
        // When
        User result = userService.getUser(1L);
        
        // Then
        assertEquals(expectedUser, result);
    }
}
```

## 6. Test Coverage

### What is Code Coverage?
Code coverage is a metric that measures the percentage of code that is executed during testing. It helps identify untested parts of your codebase.

### Tools: JaCoCo
JaCoCo (Java Code Coverage) is a popular tool for measuring code coverage in Java applications. It generates reports showing which lines, branches, and methods are covered by tests.

### 100% Coverage vs Meaningful Coverage
While high code coverage is desirable, 100% coverage doesn't guarantee bug-free code. Focus on:
- **Meaningful coverage**: Test important business logic and edge cases
- **Quality over quantity**: Well-written tests that verify correct behavior
- **Critical path coverage**: Ensure core functionality is thoroughly tested

## 7. Best Practices

### Unit Tests Should Be Fast, Repeatable, and Independent
- **Fast**: Tests should execute quickly to encourage frequent running
- **Repeatable**: Tests should produce the same results every time
- **Independent**: Tests should not depend on the order of execution or state from other tests

### Naming Conventions
Follow the pattern: `methodName_expectedBehavior_actualResult()`

**Examples:**
```java
@Test
void add_shouldReturnSum_whenValidInputs() { }

@Test
void divide_shouldThrowException_whenDivideByZero() { }

@Test
void getUserById_shouldReturnNull_whenUserNotFound() { }
```

### Additional Best Practices
- **Arrange, Act, Assert (AAA)**: Structure tests with clear setup, execution, and verification phases
- **One assertion per test**: Focus each test on verifying a single behavior
- **Use descriptive test data**: Make test inputs meaningful and easy to understand
- **Test edge cases**: Include boundary conditions and error scenarios
- **Keep tests simple**: Avoid complex logic in test methods
- **Regular maintenance**: Update tests when code changes and remove obsolete tests
