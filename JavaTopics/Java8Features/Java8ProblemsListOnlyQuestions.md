# Java 8 Problems List - Interview Preparation

## Table of Contents
1. [Lambda Expressions (20 Questions)](#lambda-expressions)
2. [Stream API (20 Questions)](#stream-api)
3. [Functional Interfaces (20 Questions)](#functional-interfaces)
4. [Method References (20 Questions)](#method-references)
5. [Optional Class (20 Questions)](#optional-class)
6. [Date and Time API (20 Questions)](#date-and-time-api)
7. [Default and Static Methods in Interfaces (20 Questions)](#default-and-static-methods)
8. [Collectors and Grouping (20 Questions)](#collectors-and-grouping)

---

## Lambda Expressions

### Basic Lambda Problems (1-5)
1. Write a lambda expression to check if a number is even.
2. Create a lambda expression that takes two integers and returns their sum.
3. Write a lambda expression to convert a string to uppercase.
4. Create a lambda expression that checks if a string is empty or null.
5. Write a lambda expression to calculate the square of a number.

### Intermediate Lambda Problems (6-15)
6. Write a lambda expression to sort a list of strings by length.
7. Create a lambda expression that filters even numbers from a list.
8. Write a lambda expression to find the maximum element in a list.
9. Create a lambda expression that concatenates two strings with a space.
10. Write a lambda expression to check if a string starts with a specific character.
11. Create a lambda expression that multiplies all elements in a list.
12. Write a lambda expression to find the first element greater than 10 in a list.
13. Create a lambda expression that removes duplicates from a list.
14. Write a lambda expression to count vowels in a string.
15. Create a lambda expression that reverses a string.

### Advanced Lambda Problems (16-20)
16. Write a lambda expression to group employees by department using custom logic.
17. Create a lambda expression that implements a custom comparator for Person objects.
18. Write a lambda expression to validate email format.
19. Create a lambda expression that calculates compound interest.
20. Write a lambda expression to implement a recursive factorial function.

---

## Stream API

### Basic Stream Problems (1-5)
1. Create a stream from a list and print all elements.
2. Filter even numbers from a list using streams.
3. Convert all strings in a list to uppercase using streams.
4. Find the sum of all elements in a list using streams.
5. Count the number of elements in a stream that satisfy a condition.

### Intermediate Stream Problems (6-15)
6. Find the average of all numbers in a list using streams.
7. Collect all even numbers into a new list using streams.
8. Find the longest string in a list using streams.
9. Remove duplicates from a list using streams.
10. Sort a list of strings in descending order using streams.
11. Find the second highest number in a list using streams.
12. Group a list of words by their first letter using streams.
13. Convert a list of strings to a map with string as key and length as value.
14. Find all strings that contain a specific substring using streams.
15. Calculate the product of all positive numbers in a list using streams.

### Advanced Stream Problems (16-20)
16. Implement parallel processing to find prime numbers in a range using streams.
17. Create a stream of Employee objects and find the employee with highest salary in each department.
18. Flatten a list of lists into a single list using streams.
19. Implement pagination using streams (skip and limit).
20. Create a custom collector to calculate standard deviation of a list of numbers.

---

## Functional Interfaces

### Predicate Interface Problems (1-5)
1. Create a Predicate to check if a number is positive.
2. Write a Predicate to validate if a string has more than 5 characters.
3. Create a Predicate to check if a person is eligible to vote (age >= 18).
4. Write a Predicate to check if a number is prime.
5. Create a complex Predicate using and(), or(), and negate() methods.

### Function Interface Problems (6-10)
6. Create a Function that converts Celsius to Fahrenheit.
7. Write a Function that extracts the domain from an email address.
8. Create a Function that calculates the area of a circle given radius.
9. Write a Function that converts a list of integers to their string representation.
10. Create a Function chain that first squares a number and then adds 10 to it.

### Consumer Interface Problems (11-15)
11. Create a Consumer that prints employee details in a formatted way.
12. Write a Consumer that logs method execution time.
13. Create a Consumer that updates all elements in a list by multiplying by 2.
14. Write a Consumer that sends email notifications (simulate with print).
15. Create a chained Consumer that validates and then processes data.

### Supplier Interface Problems (16-20)
16. Create a Supplier that generates random numbers between 1 and 100.
17. Write a Supplier that returns the current timestamp.
18. Create a Supplier that provides default configuration values.
19. Write a Supplier that generates UUID strings.
20. Create a Supplier that returns a singleton instance of a class.

---

## Method References

### Static Method Reference Problems (1-5)
1. Use method reference to convert strings to integers.
2. Create a method reference for Math.abs() function.
3. Use method reference to call a static utility method for string validation.
4. Create a method reference for Arrays.sort() method.
5. Use method reference to call a static factory method.

### Instance Method Reference Problems (6-10)
6. Use method reference to call String.length() on a list of strings.
7. Create a method reference for calling trim() on strings.
8. Use method reference to call a custom instance method on objects.
9. Create a method reference for calling toString() method.
10. Use method reference to sort objects using their natural ordering.

### Constructor Reference Problems (11-15)
11. Create a constructor reference for ArrayList.
12. Use constructor reference to create Person objects from strings.
13. Create a constructor reference for creating Date objects.
14. Use constructor reference to create HashMap instances.
15. Create a constructor reference for creating custom objects with parameters.

### Advanced Method Reference Problems (16-20)
16. Use method reference in stream operations for complex transformations.
17. Create method references for method chaining scenarios.
18. Use method reference with generic types and wildcards.
19. Create method reference for calling inherited methods.
20. Implement method reference with exception handling scenarios.

---

## Optional Class

### Basic Optional Problems (1-5)
1. Create an Optional object and check if value is present.
2. Use Optional.orElse() to provide default values.
3. Create an empty Optional and handle it safely.
4. Use Optional.orElseThrow() to throw custom exceptions.
5. Chain Optional operations using map() and flatMap().

### Intermediate Optional Problems (6-15)
6. Use Optional.filter() to validate data before processing.
7. Create a method that returns Optional<Employee> and handle null cases.
8. Use Optional.ifPresent() to perform actions only when value exists.
9. Convert nullable method returns to Optional returns.
10. Use Optional.or() to provide alternative Optional values.
11. Create nested Optional handling for complex object structures.
12. Use Optional with streams to handle null elements.
13. Implement caching mechanism using Optional.
14. Use Optional.ofNullable() vs Optional.of() appropriately.
15. Handle Optional in method parameters and return types.

### Advanced Optional Problems (16-20)
16. Create a complex Optional chain for nested object property access.
17. Implement Optional-based validation framework.
18. Use Optional with CompletableFuture for asynchronous operations.
19. Create Optional utility methods for common operations.
20. Implement null-safe comparison methods using Optional.

---

## Date and Time API

### LocalDate Problems (1-5)
1. Create a LocalDate for your birthday and calculate your age.
2. Find the number of days between two dates.
3. Get the day of week for a specific date.
4. Find all Sundays in a given month.
5. Check if a year is a leap year using LocalDate.

### LocalTime Problems (6-10)
6. Create a LocalTime for current time and format it.
7. Add 2 hours and 30 minutes to a given time.
8. Calculate the difference between two times in minutes.
9. Check if a time falls within business hours (9 AM to 5 PM).
10. Create a meeting scheduler that avoids time conflicts.

### LocalDateTime Problems (11-15)
11. Create a LocalDateTime and convert it to different time zones.
12. Parse a date-time string in custom format.
13. Find the start and end of current week.
14. Calculate working hours between two date-times excluding weekends.
15. Create a recurring event scheduler.

### Advanced Date-Time Problems (16-20)
16. Implement a custom TemporalAdjuster for last working day of month.
17. Create a date-time range validator.
18. Implement time-based cache expiration using Duration.
19. Create a holiday calendar that excludes business days calculations.
20. Implement time zone conversion utility for global applications.

---

## Default and Static Methods in Interfaces

### Default Method Problems (1-10)
1. Create an interface with default method for logging.
2. Implement multiple inheritance using default methods.
3. Override a default method in implementing class.
4. Create default methods that call other interface methods.
5. Resolve diamond problem using default methods.
6. Create utility default methods for data validation.
7. Implement default sorting behavior in an interface.
8. Create default methods for backward compatibility.
9. Use default methods to provide common implementations.
10. Chain default methods for fluent interface design.

### Static Method Problems (11-20)
11. Create static utility methods in interfaces.
12. Implement factory methods as static methods in interfaces.
13. Create static validation methods in interfaces.
14. Use static methods for constants and enums in interfaces.
15. Create static helper methods for data transformation.
16. Implement static methods for mathematical operations.
17. Create static methods for file operations in interfaces.
18. Use static methods for creating test data.
19. Implement static methods for configuration management.
20. Create static methods for exception handling utilities.

---

## Collectors and Grouping

### Basic Collectors Problems (1-5)
1. Collect stream elements into a List using Collectors.toList().
2. Collect elements into a Set to remove duplicates.
3. Join strings with a delimiter using Collectors.joining().
4. Count elements using Collectors.counting().
5. Find average of numbers using Collectors.averagingInt().

### Grouping Problems (6-15)
6. Group employees by department using Collectors.groupingBy().
7. Group students by grade and count students in each grade.
8. Create a nested grouping by department and then by salary range.
9. Partition employees into groups based on salary > 50000.
10. Group strings by their length.
11. Group orders by customer and calculate total amount per customer.
12. Create age groups (child, adult, senior) from a list of people.
13. Group products by category and find the most expensive in each.
14. Group transactions by month and calculate monthly totals.
15. Group employees by department and collect their names into a list.

### Advanced Collectors Problems (16-20)
16. Create a custom collector to calculate median of a list.
17. Implement parallel collection with custom thread pool.
18. Create a collector that maintains insertion order while grouping.
19. Implement a collector for statistical analysis (min, max, avg, count).
20. Create a collector that groups and then applies further transformations.

---

## Daily Practice Schedule

### Week 1: Lambda Expressions + Stream API Basics
- **Day 1**: Lambda Problems 1-6
- **Day 2**: Lambda Problems 7-12  
- **Day 3**: Lambda Problems 13-18
- **Day 4**: Lambda Problems 19-20 + Stream Problems 1-4
- **Day 5**: Stream Problems 5-10
- **Weekend**: Review and practice difficult problems

### Week 2: Stream API Advanced + Functional Interfaces
- **Day 1**: Stream Problems 11-16
- **Day 2**: Stream Problems 17-20 + Functional Interface Problems 1-2
- **Day 3**: Functional Interface Problems 3-8
- **Day 4**: Functional Interface Problems 9-14
- **Day 5**: Functional Interface Problems 15-20
- **Weekend**: Review and practice difficult problems

### Week 3: Method References + Optional
- **Day 1**: Method Reference Problems 1-6
- **Day 2**: Method Reference Problems 7-12
- **Day 3**: Method Reference Problems 13-18
- **Day 4**: Method Reference Problems 19-20 + Optional Problems 1-2
- **Day 5**: Optional Problems 3-8
- **Weekend**: Review and practice difficult problems

### Week 4: Optional + Date-Time API
- **Day 1**: Optional Problems 9-14
- **Day 2**: Optional Problems 15-20
- **Day 3**: Date-Time Problems 1-6
- **Day 4**: Date-Time Problems 7-12
- **Day 5**: Date-Time Problems 13-18
- **Weekend**: Review and practice difficult problems

### Week 5: Date-Time API + Interface Methods
- **Day 1**: Date-Time Problems 19-20 + Interface Methods 1-4
- **Day 2**: Interface Methods Problems 5-10
- **Day 3**: Interface Methods Problems 11-16
- **Day 4**: Interface Methods Problems 17-20
- **Day 5**: Review all Interface Methods
- **Weekend**: Review and practice difficult problems

### Week 6: Collectors and Final Review
- **Day 1**: Collectors Problems 1-6
- **Day 2**: Collectors Problems 7-12
- **Day 3**: Collectors Problems 13-18
- **Day 4**: Collectors Problems 19-20
- **Day 5**: Mixed problems from all topics
- **Weekend**: Final review and mock interview practice

---

## Tips for Daily Practice

1. **Start with easier problems** and gradually move to complex ones
2. **Write actual code** for each problem, don't just think through them
3. **Test your solutions** with different inputs
4. **Time yourself** to simulate interview conditions
5. **Review and optimize** your solutions
6. **Practice explaining** your approach out loud
7. **Create variations** of problems to deepen understanding

## Interview Focus Areas

### High Priority Topics (Practice More)
- Stream API operations and chaining
- Lambda expressions with collections
- Optional handling in real scenarios
- Functional interfaces in practical applications

### Medium Priority Topics
- Method references usage
- Date-Time API for business logic
- Collectors for data transformation

### Advanced Topics (For Senior Positions)
- Custom collectors implementation
- Parallel streams and performance
- Complex stream operations
- Integration with existing codebases
---

**Good Luck with your Interview Preparation! 🚀**