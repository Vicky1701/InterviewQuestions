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
    assertNull(mockList.get(0
