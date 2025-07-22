1. ✅ What is Unit Testing & Why is it Important
Definition of unit test

Benefits: catch bugs early, improve code quality, refactoring safety

2. ✅ JUnit Basics
What is JUnit?

Popular versions: JUnit 4 vs JUnit 5

Maven/Gradle dependencies

Subtopics:

@Test

@BeforeEach, @AfterEach

@BeforeAll, @AfterAll

@Disabled

3. ✅ Assertions
assertEquals(expected, actual)

assertTrue(condition)

assertFalse(condition)

assertThrows(exception.class, () -> { })

assertNotNull() and assertNull()

4. ✅ Testing a Simple Service Layer
Write test for a simple CalculatorService (add, subtract, etc.)

Use mock data, call method, assert result

5. ✅ Mocking with Mockito
Even if not hands-on, know the purpose:

Why mocking is needed (e.g. avoid database/API calls)

Common annotations:

@Mock

@InjectMocks

Mockito.when().thenReturn()

6. ✅ Test Coverage
What is code coverage?

Tools: JaCoCo (just the name is enough)

100% coverage vs meaningful coverage

7. ✅ Best Practices
Unit tests should be fast, repeatable, independent

Follow naming conventions: methodName_expectedBehavior_actualResult()
