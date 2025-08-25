# Java 8 Problems - Input and Output Examples

## Lambda Expressions

### Basic Lambda Problems (1-5)

**1. Write a lambda expression to check if a number is even.**
```
Input: 4
Output: true

Input: 7
Output: false
```

**2. Create a lambda expression that takes two integers and returns their sum.**
```
Input: 5, 3
Output: 8

Input: -2, 7
Output: 5
```

**3. Write a lambda expression to convert a string to uppercase.**
```
Input: "hello world"
Output: "HELLO WORLD"

Input: "Java Programming"
Output: "JAVA PROGRAMMING"
```

**4. Create a lambda expression that checks if a string is empty or null.**
```
Input: ""
Output: true

Input: null
Output: true

Input: "Hello"
Output: false
```

**5. Write a lambda expression to calculate the square of a number.**
```
Input: 5
Output: 25

Input: -3
Output: 9
```

### Intermediate Lambda Problems (6-15)

**6. Write a lambda expression to sort a list of strings by length.**
```
Input: ["apple", "hi", "banana", "a"]
Output: ["a", "hi", "apple", "banana"]

Input: ["programming", "java", "code"]
Output: ["java", "code", "programming"]
```

**7. Create a lambda expression that filters even numbers from a list.**
```
Input: [1, 2, 3, 4, 5, 6, 7, 8]
Output: [2, 4, 6, 8]

Input: [10, 15, 20, 25, 30]
Output: [10, 20, 30]
```

**8. Write a lambda expression to find the maximum element in a list.**
```
Input: [3, 7, 1, 9, 4]
Output: 9

Input: [-5, -1, -10, -3]
Output: -1
```

**9. Create a lambda expression that concatenates two strings with a space.**
```
Input: "Hello", "World"
Output: "Hello World"

Input: "Java", "Programming"
Output: "Java Programming"
```

**10. Write a lambda expression to check if a string starts with a specific character.**
```
Input: "Hello", 'H'
Output: true

Input: "world", 'W'
Output: false
```

**11. Create a lambda expression that multiplies all elements in a list.**
```
Input: [2, 3, 4]
Output: 24

Input: [1, 5, 2, 3]
Output: 30
```

**12. Write a lambda expression to find the first element greater than 10 in a list.**
```
Input: [5, 8, 12, 15, 3]
Output: 12

Input: [1, 2, 3, 4, 5]
Output: null (or Optional.empty())
```

**13. Create a lambda expression that removes duplicates from a list.**
```
Input: [1, 2, 3, 2, 4, 1, 5]
Output: [1, 2, 3, 4, 5]

Input: ["apple", "banana", "apple", "orange"]
Output: ["apple", "banana", "orange"]
```

**14. Write a lambda expression to count vowels in a string.**
```
Input: "Hello World"
Output: 3

Input: "Programming"
Output: 3
```

**15. Create a lambda expression that reverses a string.**
```
Input: "hello"
Output: "olleh"

Input: "Java"
Output: "avaJ"
```

### Advanced Lambda Problems (16-20)

**16. Write a lambda expression to group employees by department using custom logic.**
```
Input: [Employee("John", "IT"), Employee("Jane", "HR"), Employee("Bob", "IT")]
Output: {IT=[John, Bob], HR=[Jane]}
```

**17. Create a lambda expression that implements a custom comparator for Person objects.**
```
Input: [Person("Alice", 25), Person("Bob", 30), Person("Charlie", 20)]
Output (sorted by age): [Person("Charlie", 20), Person("Alice", 25), Person("Bob", 30)]
```

**18. Write a lambda expression to validate email format.**
```
Input: "user@example.com"
Output: true

Input: "invalid-email"
Output: false
```

**19. Create a lambda expression that calculates compound interest.**
```
Input: principal=1000, rate=5, time=2
Output: 1102.5

Input: principal=5000, rate=3, time=1
Output: 5150.0
```

**20. Write a lambda expression to implement a recursive factorial function.**
```
Input: 5
Output: 120

Input: 0
Output: 1
```

---

## Stream API

### Basic Stream Problems (1-5)

**1. Create a stream from a list and print all elements.**
```
Input: [1, 2, 3, 4, 5]
Output: 
1
2
3
4
5
```

**2. Filter even numbers from a list using streams.**
```
Input: [1, 2, 3, 4, 5, 6, 7, 8]
Output: [2, 4, 6, 8]
```

**3. Convert all strings in a list to uppercase using streams.**
```
Input: ["hello", "world", "java"]
Output: ["HELLO", "WORLD", "JAVA"]
```

**4. Find the sum of all elements in a list using streams.**
```
Input: [1, 2, 3, 4, 5]
Output: 15

Input: [10, 20, 30]
Output: 60
```

**5. Count the number of elements in a stream that satisfy a condition.**
```
Input: [1, 2, 3, 4, 5, 6] (condition: > 3)
Output: 3

Input: ["apple", "banana", "cherry"] (condition: length > 5)
Output: 2
```

### Intermediate Stream Problems (6-15)

**6. Find the average of all numbers in a list using streams.**
```
Input: [2, 4, 6, 8, 10]
Output: 6.0

Input: [1, 3, 5]
Output: 3.0
```

**7. Collect all even numbers into a new list using streams.**
```
Input: [1, 2, 3, 4, 5, 6, 7, 8]
Output: [2, 4, 6, 8]
```

**8. Find the longest string in a list using streams.**
```
Input: ["cat", "elephant", "dog", "butterfly"]
Output: "butterfly"

Input: ["a", "hello", "hi"]
Output: "hello"
```

**9. Remove duplicates from a list using streams.**
```
Input: [1, 2, 3, 2, 4, 1, 5]
Output: [1, 2, 3, 4, 5]
```

**10. Sort a list of strings in descending order using streams.**
```
Input: ["banana", "apple", "cherry", "date"]
Output: ["date", "cherry", "banana", "apple"]
```

**11. Find the second highest number in a list using streams.**
```
Input: [3, 7, 1, 9, 4, 8]
Output: 8

Input: [10, 5, 15, 3]
Output: 10
```

**12. Group a list of words by their first letter using streams.**
```
Input: ["apple", "banana", "cherry", "apricot", "blueberry"]
Output: {a=[apple, apricot], b=[banana, blueberry], c=[cherry]}
```

**13. Convert a list of strings to a map with string as key and length as value.**
```
Input: ["hello", "world", "java"]
Output: {hello=5, world=5, java=4}
```

**14. Find all strings that contain a specific substring using streams.**
```
Input: ["hello", "world", "help", "java"], substring="el"
Output: ["hello", "help"]
```

**15. Calculate the product of all positive numbers in a list using streams.**
```
Input: [2, -3, 4, 5, -1]
Output: 40

Input: [1, 2, 3, 4]
Output: 24
```

### Advanced Stream Problems (16-20)

**16. Implement parallel processing to find prime numbers in a range using streams.**
```
Input: range 1 to 20
Output: [2, 3, 5, 7, 11, 13, 17, 19]

Input: range 1 to 10
Output: [2, 3, 5, 7]
```

**17. Create a stream of Employee objects and find the employee with highest salary in each department.**
```
Input: [Employee("John", "IT", 5000), Employee("Jane", "HR", 4000), Employee("Bob", "IT", 6000)]
Output: {IT=Employee("Bob", "IT", 6000), HR=Employee("Jane", "HR", 4000)}
```

**18. Flatten a list of lists into a single list using streams.**
```
Input: [[1, 2], [3, 4], [5, 6]]
Output: [1, 2, 3, 4, 5, 6]

Input: [["a", "b"], ["c"], ["d", "e", "f"]]
Output: ["a", "b", "c", "d", "e", "f"]
```

**19. Implement pagination using streams (skip and limit).**
```
Input: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], page=2, pageSize=3
Output: [4, 5, 6]

Input: ["a", "b", "c", "d", "e"], page=1, pageSize=2
Output: ["c", "d"]
```

**20. Create a custom collector to calculate standard deviation of a list of numbers.**
```
Input: [2, 4, 6, 8]
Output: 2.236 (approximately)

Input: [1, 2, 3, 4, 5]
Output: 1.414 (approximately)
```

---

## Functional Interfaces

### Predicate Interface Problems (1-5)

**1. Create a Predicate to check if a number is positive.**
```
Input: 5
Output: true

Input: -3
Output: false

Input: 0
Output: false
```

**2. Write a Predicate to validate if a string has more than 5 characters.**
```
Input: "hello"
Output: false

Input: "programming"
Output: true

Input: "hi"
Output: false
```

**3. Create a Predicate to check if a person is eligible to vote (age >= 18).**
```
Input: Person("John", 20)
Output: true

Input: Person("Alice", 16)
Output: false

Input: Person("Bob", 18)
Output: true
```

**4. Write a Predicate to check if a number is prime.**
```
Input: 7
Output: true

Input: 8
Output: false

Input: 2
Output: true

Input: 1
Output: false
```

**5. Create a complex Predicate using and(), or(), and negate() methods.**
```
Input: 15 (check if divisible by 3 AND not divisible by 5)
Output: false

Input: 9 (check if divisible by 3 AND not divisible by 5)
Output: true
```

### Function Interface Problems (6-10)

**6. Create a Function that converts Celsius to Fahrenheit.**
```
Input: 0
Output: 32.0

Input: 100
Output: 212.0

Input: 25
Output: 77.0
```

**7. Write a Function that extracts the domain from an email address.**
```
Input: "user@example.com"
Output: "example.com"

Input: "john.doe@company.org"
Output: "company.org"
```

**8. Create a Function that calculates the area of a circle given radius.**
```
Input: 5
Output: 78.54 (approximately)

Input: 3
Output: 28.27 (approximately)
```

**9. Write a Function that converts a list of integers to their string representation.**
```
Input: [1, 2, 3, 4]
Output: ["1", "2", "3", "4"]

Input: [10, 20, 30]
Output: ["10", "20", "30"]
```

**10. Create a Function chain that first squares a number and then adds 10 to it.**
```
Input: 5
Output: 35 (5² + 10 = 25 + 10)

Input: 3
Output: 19 (3² + 10 = 9 + 10)
```

### Consumer Interface Problems (11-15)

**11. Create a Consumer that prints employee details in a formatted way.**
```
Input: Employee("John", "IT", 5000)
Output: "Employee: John, Department: IT, Salary: $5000"

Input: Employee("Jane", "HR", 4500)
Output: "Employee: Jane, Department: HR, Salary: $4500"
```

**12. Write a Consumer that logs method execution time.**
```
Input: () -> { Thread.sleep(100); }
Output: "Method executed in 100ms"
```

**13. Create a Consumer that updates all elements in a list by multiplying by 2.**
```
Input: [1, 2, 3, 4]
Output: [2, 4, 6, 8] (list modified in place)
```

**14. Write a Consumer that sends email notifications (simulate with print).**
```
Input: "user@example.com", "Hello World"
Output: "Email sent to: user@example.com, Message: Hello World"
```

**15. Create a chained Consumer that validates and then processes data.**
```
Input: "valid-email@domain.com"
Output: 
"Validation passed"
"Processing: valid-email@domain.com"

Input: "invalid-email"
Output: 
"Validation failed"
```

### Supplier Interface Problems (16-20)

**16. Create a Supplier that generates random numbers between 1 and 100.**
```
Output: 42 (random number between 1-100)
Output: 73 (random number between 1-100)
```

**17. Write a Supplier that returns the current timestamp.**
```
Output: 2025-08-25T10:30:45.123
Output: 2025-08-25T10:30:46.456
```

**18. Create a Supplier that provides default configuration values.**
```
Output: Configuration(host="localhost", port=8080, timeout=30)
```

**19. Write a Supplier that generates UUID strings.**
```
Output: "123e4567-e89b-12d3-a456-426614174000"
Output: "987fcdeb-51a2-43d7-8f9e-123456789abc"
```

**20. Create a Supplier that returns a singleton instance of a class.**
```
Output: DatabaseConnection@12345 (same instance every time)
Output: DatabaseConnection@12345 (same instance every time)
```

---

## Method References

### Static Method Reference Problems (1-5)

**1. Use method reference to convert strings to integers.**
```
Input: ["1", "2", "3", "4"]
Output: [1, 2, 3, 4]

Input: ["10", "20", "30"]
Output: [10, 20, 30]
```

**2. Create a method reference for Math.abs() function.**
```
Input: [-5, 3, -8, 10]
Output: [5, 3, 8, 10]

Input: [-1.5, 2.7, -3.2]
Output: [1.5, 2.7, 3.2]
```

**3. Use method reference to call a static utility method for string validation.**
```
Input: ["", "hello", null, "world"]
Output: [false, true, false, true]
```

**4. Create a method reference for Arrays.sort() method.**
```
Input: [3, 1, 4, 1, 5]
Output: [1, 1, 3, 4, 5] (array sorted in place)
```

**5. Use method reference to call a static factory method.**
```
Input: "John", 25
Output: Person{name="John", age=25}
```

### Instance Method Reference Problems (6-10)

**6. Use method reference to call String.length() on a list of strings.**
```
Input: ["hello", "world", "java"]
Output: [5, 5, 4]

Input: ["a", "hello", "programming"]
Output: [1, 5, 11]
```

**7. Create a method reference for calling trim() on strings.**
```
Input: [" hello ", "  world  ", " java"]
Output: ["hello", "world", "java"]
```

**8. Use method reference to call a custom instance method on objects.**
```
Input: [Employee("John", 5000), Employee("Jane", 6000)]
Output: [5000, 6000] (salaries extracted)
```

**9. Create a method reference for calling toString() method.**
```
Input: [1, 2, 3, 4]
Output: ["1", "2", "3", "4"]

Input: [Person("John"), Person("Jane")]
Output: ["Person{name=John}", "Person{name=Jane}"]
```

**10. Use method reference to sort objects using their natural ordering.**
```
Input: ["banana", "apple", "cherry"]
Output: ["apple", "banana", "cherry"]

Input: [3, 1, 4, 1, 5]
Output: [1, 1, 3, 4, 5]
```

### Constructor Reference Problems (11-15)

**11. Create a constructor reference for ArrayList.**
```
Input: Stream of [1, 2, 3, 4]
Output: ArrayList containing [1, 2, 3, 4]
```

**12. Use constructor reference to create Person objects from strings.**
```
Input: ["John", "Jane", "Bob"]
Output: [Person("John"), Person("Jane"), Person("Bob")]
```

**13. Create a constructor reference for creating Date objects.**
```
Input: [1234567890000L, 1234567891000L]
Output: [Date(1234567890000L), Date(1234567891000L)]
```

**14. Use constructor reference to create HashMap instances.**
```
Input: Stream of key-value pairs
Output: HashMap instances containing the pairs
```

**15. Create a constructor reference for creating custom objects with parameters.**
```
Input: [("John", 25), ("Jane", 30)]
Output: [Person("John", 25), Person("Jane", 30)]
```

### Advanced Method Reference Problems (16-20)

**16. Use method reference in stream operations for complex transformations.**
```
Input: [Employee("John", "IT", 5000), Employee("Jane", "HR", 6000)]
Output: ["John-IT", "Jane-HR"] (name-department format)
```

**17. Create method references for method chaining scenarios.**
```
Input: ["  hello  ", "  world  "]
Output: ["HELLO", "WORLD"] (trim then uppercase)
```

**18. Use method reference with generic types and wildcards.**
```
Input: List<? extends Number> [1, 2.5, 3L]
Output: ["1", "2.5", "3"] (toString applied)
```

**19. Create method reference for calling inherited methods.**
```
Input: [Manager("John"), Developer("Jane")]
Output: ["John", "Jane"] (getName() from Person base class)
```

**20. Implement method reference with exception handling scenarios.**
```
Input: ["file1.txt", "file2.txt", "nonexistent.txt"]
Output: [true, true, false] (file exists check with exception handling)
```

---

## Optional Class

### Basic Optional Problems (1-5)

**1. Create an Optional object and check if value is present.**
```
Input: "Hello"
Output: true

Input: null
Output: false
```

**2. Use Optional.orElse() to provide default values.**
```
Input: Optional.of("Hello")
Output: "Hello"

Input: Optional.empty()
Output: "Default Value"
```

**3. Create an empty Optional and handle it safely.**
```
Input: Optional.empty()
Output: "No value present"

Input: Optional.of(42)
Output: "Value: 42"
```

**4. Use Optional.orElseThrow() to throw custom exceptions.**
```
Input: Optional.of("Hello")
Output: "Hello"

Input: Optional.empty()
Output: CustomException: "Value not found"
```

**5. Chain Optional operations using map() and flatMap().**
```
Input: Optional.of("hello")
Output: Optional.of("HELLO") (mapped to uppercase)

Input: Optional.empty()
Output: Optional.empty()
```

### Intermediate Optional Problems (6-15)

**6. Use Optional.filter() to validate data before processing.**
```
Input: Optional.of(25), condition: age >= 18
Output: Optional.of(25)

Input: Optional.of(15), condition: age >= 18
Output: Optional.empty()
```

**7. Create a method that returns Optional<Employee> and handle null cases.**
```
Input: employeeId = 123 (exists)
Output: Optional.of(Employee("John", "IT"))

Input: employeeId = 999 (doesn't exist)
Output: Optional.empty()
```

**8. Use Optional.ifPresent() to perform actions only when value exists.**
```
Input: Optional.of("Hello")
Output: "Processing: Hello"

Input: Optional.empty()
Output: (no output - action not performed)
```

**9. Convert nullable method returns to Optional returns.**
```
Input: findUserById(123) -> User object or null
Output: Optional.of(User) or Optional.empty()
```

**10. Use Optional.or() to provide alternative Optional values.**
```
Input: Optional.empty()
Output: Optional.of("Alternative Value")

Input: Optional.of("Primary Value")
Output: Optional.of("Primary Value")
```

**11. Create nested Optional handling for complex object structures.**
```
Input: Optional.of(Person with Address)
Output: Optional.of("Street Name")

Input: Optional.of(Person without Address)
Output: Optional.empty()
```

**12. Use Optional with streams to handle null elements.**
```
Input: [1, null, 3, null, 5]
Output: [1, 3, 5] (nulls filtered out)
```

**13. Implement caching mechanism using Optional.**
```
Input: "cache-key"
Output: Optional.of("cached-value") or Optional.empty()
```

**14. Use Optional.ofNullable() vs Optional.of() appropriately.**
```
Input: null (with ofNullable)
Output: Optional.empty()

Input: null (with of)
Output: NullPointerException
```

**15. Handle Optional in method parameters and return types.**
```
Input: processUser(Optional.of(User))
Output: "User processed: John"

Input: processUser(Optional.empty())
Output: "No user to process"
```

### Advanced Optional Problems (16-20)

**16. Create a complex Optional chain for nested object property access.**
```
Input: Optional.of(Company with Employees with Addresses)
Output: Optional.of("City Name") or Optional.empty()
```

**17. Implement Optional-based validation framework.**
```
Input: User data
Output: Optional.of(ValidUser) or Optional.empty() with validation errors
```

**18. Use Optional with CompletableFuture for asynchronous operations.**
```
Input: userId
Output: CompletableFuture<Optional<User>>
```

**19. Create Optional utility methods for common operations.**
```
Input: Optional.of("123")
Output: Optional.of(123) (string to int conversion)

Input: Optional.of("abc")
Output: Optional.empty() (invalid number)
```

**20. Implement null-safe comparison methods using Optional.**
```
Input: Optional.of("hello"), Optional.of("hello")
Output: true

Input: Optional.of("hello"), Optional.empty()
Output: false
```

---

## Date and Time API

### LocalDate Problems (1-5)

**1. Create a LocalDate for your birthday and calculate your age.**
```
Input: Birthday = 1990-05-15, Current = 2025-08-25
Output: 35 years

Input: Birthday = 2000-12-25, Current = 2025-08-25
Output: 24 years
```

**2. Find the number of days between two dates.**
```
Input: 2025-01-01, 2025-01-15
Output: 14 days

Input: 2025-08-01, 2025-08-25
Output: 24 days
```

**3. Get the day of week for a specific date.**
```
Input: 2025-08-25
Output: MONDAY

Input: 2025-12-25
Output: THURSDAY
```

**4. Find all Sundays in a given month.**
```
Input: August 2025
Output: [2025-08-03, 2025-08-10, 2025-08-17, 2025-08-24, 2025-08-31]

Input: February 2025
Output: [2025-02-02, 2025-02-09, 2025-02-16, 2025-02-23]
```

**5. Check if a year is a leap year using LocalDate.**
```
Input: 2024
Output: true

Input: 2025
Output: false

Input: 2000
Output: true
```

### LocalTime Problems (6-10)

**6. Create a LocalTime for current time and format it.**
```
Input: LocalTime.of(14, 30, 45)
Output: "14:30:45" or "2:30:45 PM"

Input: LocalTime.of(9, 15, 0)
Output: "09:15:00" or "9:15:00 AM"
```

**7. Add 2 hours and 30 minutes to a given time.**
```
Input: 10:15:00
Output: 12:45:00

Input: 22:45:00
Output: 01:15:00 (next day)
```

**8. Calculate the difference between two times in minutes.**
```
Input: 10:00:00, 10:45:00
Output: 45 minutes

Input: 23:30:00, 01:15:00 (next day)
Output: 105 minutes
```

**9. Check if a time falls within business hours (9 AM to 5 PM).**
```
Input: 10:30:00
Output: true

Input: 18:30:00
Output: false

Input: 08:30:00
Output: false
```

**10. Create a meeting scheduler that avoids time conflicts.**
```
Input: Existing meetings: [(09:00-10:00), (14:00-15:00)], New meeting: 1 hour
Output: Available slots: [10:00-11:00, 11:00-12:00, 12:00-13:00, 13:00-14:00, 15:00-16:00, 16:00-17:00]
```

### LocalDateTime Problems (11-15)

**11. Create a LocalDateTime and convert it to different time zones.**
```
Input: 2025-08-25T14:30:00 (UTC)
Output: 
UTC: 2025-08-25T14:30:00
EST: 2025-08-25T09:30:00
IST: 2025-08-25T20:00:00
```

**12. Parse a date-time string in custom format.**
```
Input: "25/08/2025 14:30:45"
Output: 2025-08-25T14:30:45

Input: "Aug 25, 2025 2:30 PM"
Output: 2025-08-25T14:30:00
```

**13. Find the start and end of current week.**
```
Input: 2025-08-25 (Monday)
Output: Start: 2025-08-25, End: 2025-08-31

Input: 2025-08-27 (Wednesday)
Output: Start: 2025-08-25, End: 2025-08-31
```

**14. Calculate working hours between two date-times excluding weekends.**
```
Input: 2025-08-25T09:00 to 2025-08-27T17:00
Output: 16 hours (Monday 8h + Tuesday 8h, Wednesday excluded as partial)

Input: 2025-08-22T09:00 to 2025-08-25T17:00
Output: 16 hours (Friday 8h + Monday 8h, weekend excluded)
```

**15. Create a recurring event scheduler.**
```
Input: Start: 2025-08-25T10:00, Frequency: Weekly, Occurrences: 3
Output: [2025-08-25T10:00, 2025-09-01T10:00, 2025-09-08T10:00]
```

### Advanced Date-Time Problems (16-20)

**16. Implement a custom TemporalAdjuster for last working day of month.**
```
Input: 2025-08-15 (any day in August)
Output: 2025-08-29 (last working day of August 2025)

Input: 2025-02-15 (any day in February)
Output: 2025-02-28 (last working day of February 2025)
```

**17. Create a date-time range validator.**
```
Input: Event: 2025-08-25T10:00 to 2025-08-25T12:00, Allowed: 09:00-17:00
Output: true (valid)

Input: Event: 2025-08-25T18:00 to 2025-08-25T20:00, Allowed: 09:00-17:00
Output: false (invalid)
```

**18. Implement time-based cache expiration using Duration.**
```
Input: CacheEntry created at 2025-08-25T10:00, TTL: 1 hour, Check at 2025-08-25T10:30
Output: false (not expired)

Input: CacheEntry created at 2025-08-25T10:00, TTL: 1 hour, Check at 2025-08-25T11:30
Output: true (expired)
```

**19. Create a holiday calendar that excludes business days calculations.**
```
Input: Start: 2025-08-25, Business days to add: 5, Holidays: [2025-08-26]
Output: 2025-09-02 (excluding weekend and holiday)
```

**20. Implement time zone conversion utility for global applications.**
```
Input: "2025-08-25T14:30:00", from="UTC", to="America/New_York"
Output: "2025-08-25T09:30:00-05:00"

Input: "2025-08-25T14:30:00", from="Asia/Kolkata", to="Europe/London"
Output: "2025-08-25T10:00:00+01:00"
```

---

## Default and Static Methods in Interfaces

### Default Method Problems (1-10)

**1. Create an interface with default method for logging.**
```
Input: LoggableService.log("Processing started")
Output: "[2025-08-25 10:30:45] Processing started"
```

**2. Implement multiple inheritance using default methods.**
```
Input: ConcreteClass implements InterfaceA, InterfaceB
Output: Successfully inherits default methods from both interfaces
```

**3. Override a default method in implementing class.**
```
Input: CustomClass overrides defaultMethod()
Output: Custom implementation instead of default behavior
```

**4. Create default methods that call other interface methods.**
```
Input: Processable.processAndLog(data)
Output: Data processed and logged using default implementation
```

**5. Resolve diamond problem using default methods.**
```
Input: Class implements A, B where both have same default method
Output: Compilation requires explicit override to resolve conflict
```

**6. Create utility default methods for data validation.**
```
Input: Validatable.validateEmail("user@example.com")
Output: true

Input: Validatable.validateEmail("invalid-email")
Output: false
```

**7. Implement default sorting behavior in an interface.**
```
Input: Sortable list of comparable items
Output: List sorted using default comparator
```

**8. Create default methods for backward compatibility.**
```
Input: Legacy implementation using interface with new default methods
Output: Works without modification, uses default implementations
```

**9. Use default methods to provide common implementations.**
```
Input: Drawable.drawWithBorder(shape)
Output: Shape drawn with default border implementation
```

**10. Chain default methods for fluent interface design.**
```
Input: FluentBuilder.setName("John").setAge(25).build()
Output: Person{name="John", age=25}
```

### Static Method Problems (11-20)

**11. Create static utility methods in interfaces.**
```
Input: MathUtils.max(5, 3, 8, 1)
Output: 8

Input: StringUtils.isNullOrEmpty("")
Output: true
```

**12. Implement factory methods as static methods in interfaces.**
```
Input: Person.create("John", 25)
Output: Person{name="John", age=25}

Input: Configuration.defaultConfig()
Output: Configuration with default values
```

**13. Create static validation methods in interfaces.**
```
Input: EmailValidator.isValid("user@example.com")
Output: true

Input: EmailValidator.isValid("invalid")
Output: false
```

**14. Use static methods for constants and enums in interfaces.**
```
Input: Constants.DEFAULT_TIMEOUT
Output: 30 (seconds)

Input: Status.fromCode(200)
Output: Status.SUCCESS
```

**15. Create static helper methods for data transformation.**
```
Input: DataTransformer.csvToList("a,b,c")
Output: ["a", "b", "c"]

Input: DataTransformer.listToCsv(["x", "y", "z"])
Output: "x,y,z"
```

**16. Implement static methods for mathematical operations.**
```
Input: Calculator.factorial(5)
Output: 120

Input: Calculator.gcd(12, 18)
Output: 6
```

**17. Create static methods for file operations in interfaces.**
```
Input: FileUtils.readAllLines("test.txt")
Output: ["line1", "line2", "line3"]

Input: FileUtils.exists("nonexistent.txt")
Output: false
```

**18. Use static methods for creating test data.**
```
Input: TestDataFactory.createEmployees(3)
Output: [Employee1, Employee2, Employee3]

Input: TestDataFactory.randomString(5)
Output: "aB3xK" (random 5-character string)
```

**19. Implement static methods for configuration management.**
```
Input: ConfigManager.getProperty("database.url")
Output: "jdbc:mysql://localhost:3306/mydb"

Input: ConfigManager.setProperty("debug", "true")
Output: Property set successfully
```

**20. Create static methods for exception handling utilities.**
```
Input: ExceptionUtils.wrapChecked(() -> riskyOperation())
Output: Result or RuntimeException wrapper

Input: ExceptionUtils.ignoreExceptions(() -> mightFail())
Output: Executes without throwing exceptions
```

---

## Collectors and Grouping

### Basic Collectors Problems (1-5)

**1. Collect stream elements into a List using Collectors.toList().**
```
Input: Stream.of(1, 2, 3, 4, 5)
Output: [1, 2, 3, 4, 5]

Input: Stream.of("a", "b", "c")
Output: ["a", "b", "c"]
```

**2. Collect elements into a Set to remove duplicates.**
```
Input: Stream.of(1, 2, 2, 3, 3, 4)
Output: {1, 2, 3, 4}

Input: Stream.of("apple", "banana", "apple")
Output: {"apple", "banana"}
```

**3. Join strings with a delimiter using Collectors.joining().**
```
Input: Stream.of("Java", "Python", "JavaScript")
Output: "Java, Python, JavaScript"

Input: Stream.of("a", "b", "c")
Output: "a-b-c" (with "-" delimiter)
```

**4. Count elements using Collectors.counting().**
```
Input: Stream.of(1, 2, 3, 4, 5)
Output: 5

Input: Stream.of("hello", "world")
Output: 2
```

**5. Find average of numbers using Collectors.averagingInt().**
```
Input: Stream.of(2, 4, 6, 8)
Output: 5.0

Input: Stream.of(1, 3, 5)
Output: 3.0
```

### Grouping Problems (6-15)

**6. Group employees by department using Collectors.groupingBy().**
```
Input: [Employee("John", "IT"), Employee("Jane", "HR"), Employee("Bob", "IT")]
Output: {IT=[John, Bob], HR=[Jane]}
```

**7. Group students by grade and count students in each grade.**
```
Input: [Student("A", "A"), Student("B", "B"), Student("C", "A")]
Output: {A=2, B=1}
```

**8. Create a nested grouping by department and then by salary range.**
```
Input: [Employee("John", "IT", 5000), Employee("Jane", "IT", 7000), Employee("Bob", "HR", 4000)]
Output: {IT={5000-6000=[John], 7000-8000=[Jane]}, HR={4000-5000=[Bob]}}
```

**9. Partition employees into groups based on salary > 50000.**
```
Input: [Employee("John", 60000), Employee("Jane", 40000), Employee("Bob", 55000)]
Output: {true=[John, Bob], false=[Jane]}
```

**10. Group strings by their length.**
```
Input: ["cat", "elephant", "dog", "butterfly"]
Output: {3=[cat, dog], 8=[elephant], 9=[butterfly]}
```

**11. Group orders by customer and calculate total amount per customer.**
```
Input: [Order("John", 100), Order("Jane", 200), Order("John", 150)]
Output: {John=250, Jane=200}
```

**12. Create age groups (child, adult, senior) from a list of people.**
```
Input: [Person("Alice", 10), Person("Bob", 30), Person("Charlie", 70)]
Output: {child=[Alice], adult=[Bob], senior=[Charlie]}
```

**13. Group products by category and find the most expensive in each.**
```
Input: [Product("Laptop", "Electronics", 1000), Product("Phone", "Electronics", 800), Product("Shirt", "Clothing", 50)]
Output: {Electronics=Product("Laptop", 1000), Clothing=Product("Shirt", 50)}
```

**14. Group transactions by month and calculate monthly totals.**
```
Input: [Transaction("2025-08-15", 100), Transaction("2025-08-20", 200), Transaction("2025-09-05", 150)]
Output: {2025-08=300, 2025-09=150}
```

**15. Group employees by department and collect their names into a list.**
```
Input: [Employee("John", "IT"), Employee("Jane", "HR"), Employee("Bob", "IT")]
Output: {IT=["John", "Bob"], HR=["Jane"]}
```

### Advanced Collectors Problems (16-20)

**16. Create a custom collector to calculate median of a list.**
```
Input: [1, 3, 5, 7, 9]
Output: 5.0

Input: [2, 4, 6, 8]
Output: 5.0 (average of 4 and 6)
```

**17. Implement parallel collection with custom thread pool.**
```
Input: Large list processed in parallel
Output: Result collected using multiple threads efficiently
```

**18. Create a collector that maintains insertion order while grouping.**
```
Input: [Employee("John", "IT"), Employee("Jane", "HR"), Employee("Bob", "IT")]
Output: LinkedHashMap{IT=[John, Bob], HR=[Jane]} (maintains order)
```

**19. Implement a collector for statistical analysis (min, max, avg, count).**
```
Input: [10, 20, 30, 40, 50]
Output: Statistics{min=10, max=50, average=30.0, count=5, sum=150}
```

**20. Create a collector that groups and then applies further transformations.**
```
Input: [Employee("John", "IT", 5000), Employee("Jane", "IT", 6000)]
Output: {IT="John(5000), Jane(6000)"} (grouped and formatted)
```

---

**Note:** These input/output examples are provided to help you understand the expected behavior of each problem. When implementing, make sure to write the actual Java 8 code using the appropriate functional interfaces, lambda expressions, streams, and other Java 8 features.
