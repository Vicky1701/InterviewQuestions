# Java Functional Programming Interview Questions

## 📘 Core Functional Programming + Streams

### ✅ Easy Problems

#### 1. Filter Even Numbers
**Question:** Given a list of integers, filter out only the even numbers using Java Streams.

**Input:** `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`

**Expected Output:** `[2, 4, 6, 8, 10]`

---

#### 2. Square Numbers
**Question:** Transform a list of integers into their squares using map operation.

**Input:** `[1, 2, 3, 4, 5]`

**Expected Output:** `[1, 4, 9, 16, 25]`

---

#### 3. Convert to Uppercase
**Question:** Convert all strings in a list to uppercase using streams.

**Input:** `["hello", "world", "java", "streams"]`

**Expected Output:** `["HELLO", "WORLD", "JAVA", "STREAMS"]`

---

#### 4. Join Strings with Comma
**Question:** Create a comma-separated string from a list of strings using Collectors.joining().

**Input:** `["Java", "Python", "C++", "JavaScript"]`

**Expected Output:** `"Java, Python, C++, JavaScript"`

---

#### 5. Check Any Even Number
**Question:** Check if any number in the list is even using anyMatch().

**Input:** `[1, 3, 5, 7, 8, 9]`

**Expected Output:** `true`

---

#### 6. Remove Null and Empty Strings
**Question:** Filter out null and empty strings from a list.

**Input:** `["hello", "", null, "world", "java", null, ""]`

**Expected Output:** `["hello", "world", "java"]`

---

### 🔶 Medium Problems

#### 7. Find Second Highest Number
**Question:** Find the second highest number from a list of integers using streams.

**Input:** `[5, 2, 8, 1, 9, 3, 7]`

**Expected Output:** `8`

---

#### 8. Flatten List of Lists
**Question:** Flatten a nested list structure using flatMap().

**Input:** `[[1, 2, 3], [4, 5], [6, 7, 8, 9]]`

**Expected Output:** `[1, 2, 3, 4, 5, 6, 7, 8, 9]`

---

#### 9. Top 3 Maximum Elements
**Question:** Find the top 3 maximum elements from a list using sorted() and limit().

**Input:** `[5, 2, 8, 1, 9, 3, 7, 6, 4]`

**Expected Output:** `[9, 8, 7]`

---

#### 10. Group Words by Length
**Question:** Group words by their length using groupingBy().

**Input:** `["cat", "dog", "elephant", "rat", "tiger", "lion"]`

**Expected Output:** `{3=[cat, dog, rat], 4=[lion], 5=[tiger], 8=[elephant]}`

---

#### 11. Distinct and Sort
**Question:** Remove duplicates and sort a list of integers.

**Input:** `[5, 2, 8, 2, 1, 9, 3, 7, 5, 1]`

**Expected Output:** `[1, 2, 3, 5, 7, 8, 9]`

---

### 🔴 Hard Problems

#### 12. Complete Stream Pipeline
**Question:** Create a complete stream pipeline that filters words with length > 4, converts to uppercase, removes duplicates, and sorts them.

**Input:** `["apple", "banana", "cherry", "date", "elderberry", "fig"]`

**Expected Output:** `["APPLE", "BANANA", "CHERRY", "ELDERBERRY"]`

---

#### 13. Word Frequency Count
**Question:** Count the frequency of each word in a paragraph using streams and groupingBy().

**Input:** `"java is great java is powerful java streams are awesome"`

**Expected Output:** `{java=3, is=2, great=1, powerful=1, streams=1, are=1, awesome=1}`

---

#### 14. First Non-Repeating Character
**Question:** Find the first non-repeating character in a string using streams.

**Input:** `"programming"`

**Expected Output:** `'p'`

---

## 📗 Functional Interfaces & Lambdas

### ✅ Easy Problems

#### 15. Predicate for String Starting with "A"
**Question:** Create a Predicate to check if a string starts with "A" and use it to filter a list.

**Input:** `["Alice", "Bob", "Andrew", "Charlie"]`

**Expected Output:** `["Alice", "Andrew"]`

---

#### 16. Function to Get String Length
**Question:** Create a Function that returns the length of a string and apply it to a list.

**Input:** `["hello", "world", "java"]`

**Expected Output:** `[5, 5, 4]`

---

#### 17. Consumer to Print Strings
**Question:** Use a Consumer to print each string in a list.

**Input:** `["hello", "world", "java"]`

**Expected Output:** Each string printed on a new line

---

#### 18. Supplier for Random Numbers
**Question:** Create a Supplier that generates random numbers and create a list of 5 random numbers.

**Input:** None

**Expected Output:** List of 5 random double values

---

### 🔶 Medium Problems

#### 19. Employee Filter with Predicate
**Question:** Given a list of Employee objects with name, age, and salary, create a Predicate to filter employees with salary > 52000.

**Input:** 
```
[
  Employee("Alice", 25, 50000),
  Employee("Bob", 35, 60000),
  Employee("Charlie", 28, 55000)
]
```

**Expected Output:** `[Employee("Bob", 35, 60000), Employee("Charlie", 28, 55000)]`

---

#### 20. BiPredicate for Length Matching
**Question:** Create a BiPredicate that checks if a string's length matches a given integer.

**Input:** `["hello", "world", "java", "stream"]` with target length `5`

**Expected Output:** `["hello", "world"]`

---

#### 21. BinaryOperator for String Combination
**Question:** Use BinaryOperator to combine strings with a space separator.

**Input:** `["Java", "is", "awesome"]`

**Expected Output:** `"Java is awesome"`

---

### 🔴 Hard Problems

#### 22. Function Composition
**Question:** Compose three functions: trim → uppercase → length, and apply to a list of strings.

**Input:** `["  hello  ", "  world  ", "  java  "]`

**Expected Output:** `[5, 5, 4]`

---

#### 23. BiFunction for List Filtering
**Question:** Create a BiFunction that takes a List and a Predicate, then returns filtered results.

**Input:** `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]` with even number predicate

**Expected Output:** `[2, 4, 6, 8, 10]`

---

#### 24. Multiple Predicate Filtering
**Question:** Create a method that filters a list using two predicates combined with AND operation.

**Input:** `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]` with predicates: isEven AND greaterThan5

**Expected Output:** `[6, 8, 10]`

---

## 📙 Custom Classes, Sorting, Grouping

### ✅ Easy Problems

#### 25. AllMatch with Custom Objects
**Question:** Check if all courses have a review score >= 80 using allMatch().

**Input:** 
```
[
  Course("Java", 85, 100),
  Course("Python", 90, 150),
  Course("JavaScript", 80, 120)
]
```

**Expected Output:** `true`

---

#### 26. Sort by Field
**Question:** Sort a list of Course objects by review score using Comparator.comparing().

**Input:** 
```
[
  Course("Java", 85, 100),
  Course("Python", 90, 150),
  Course("JavaScript", 80, 120)
]
```

**Expected Output:** Courses sorted by score: JavaScript(80), Java(85), Python(90)

---

#### 27. Pagination with Skip/Limit
**Question:** Implement pagination using skip() and limit() operations.

**Input:** `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]` with pageSize=3, pageNumber=2

**Expected Output:** `[7, 8, 9]`

---

### 🔶 Medium Problems

#### 28. Find Maximum by Field
**Question:** Find the course with the highest review score using max() and Comparator.

**Input:** 
```
[
  Course("Java", 85, 100),
  Course("Python", 90, 150),
  Course("JavaScript", 80, 120)
]
```

**Expected Output:** `Course("Python", 90, 150)`

---

#### 29. Group by Category
**Question:** Group courses by their category using groupingBy().

**Input:** 
```
[
  Course("Java", "Programming", 85),
  Course("Python", "Programming", 90),
  Course("Photoshop", "Design", 80),
  Course("Illustrator", "Design", 85)
]
```

**Expected Output:** `{Programming=[Java, Python], Design=[Photoshop, Illustrator]}`

---

#### 30. Calculate Average Score
**Question:** Calculate the average review score of all courses using Collectors.averagingInt().

**Input:** 
```
[
  Course("Java", 85, 100),
  Course("Python", 90, 150),
  Course("JavaScript", 80, 120)
]
```

**Expected Output:** `85.0`

---

### 🔴 Hard Problems

#### 31. Max Students per Category
**Question:** Find the course with maximum students in each category.

**Input:** 
```
[
  Course("Java", "Programming", 85, 100),
  Course("Python", "Programming", 90, 150),
  Course("Photoshop", "Design", 80, 120),
  Course("Illustrator", "Design", 85, 80)
]
```

**Expected Output:** `{Programming=Python, Design=Photoshop}`

---

#### 32. Category with Sorted Course Names
**Question:** Create a map where each category maps to a list of course names sorted by number of students (descending).

**Input:** 
```
[
  Course("Java", "Programming", 85, 100),
  Course("Python", "Programming", 90, 150),
  Course("C++", "Programming", 80, 80),
  Course("Photoshop", "Design", 80, 120),
  Course("Illustrator", "Design", 85, 80)
]
```

**Expected Output:** `{Programming=[Python, Java, C++], Design=[Photoshop, Illustrator]}`

---

#### 33. Partition by Rating
**Question:** Partition courses into high-rated (>=85) and low-rated (<85) using partitioningBy().

**Input:** 
```
[
  Course("Java", 85, 100),
  Course("Python", 90, 150),
  Course("JavaScript", 80, 120),
  Course("C++", 75, 80)
]
```

**Expected Output:** `{true=[Java, Python], false=[JavaScript, C++]}`

---

## 📒 Performance & Real-World Application

### ✅ Easy Problems

#### 34. Thread with Lambda
**Question:** Create and start a thread using lambda expression instead of anonymous inner class.

**Input:** Task to print "Hello from Lambda Thread"

**Expected Output:** Thread executes and prints the message

---

#### 35. List Operations with Lambdas
**Question:** Use replaceAll() to convert all strings to uppercase, then removeIf() to remove strings shorter than 5 characters.

**Input:** `["hello", "world", "java", "stream"]`

**Expected Output:** After replaceAll: `["HELLO", "WORLD", "JAVA", "STREAM"]`, After removeIf: `["HELLO", "WORLD", "STREAM"]`

---

### 🔶 Medium Problems

#### 36. File Processing with Streams
**Question:** Read a file line by line using Files.lines() and count occurrences of a specific keyword.

**Input:** File content: "java is great\njava streams are powerful\npython is also good"

**Expected Output:** Count of "java": 2

---

#### 37. Parallel vs Sequential Performance
**Question:** Compare performance between sequential and parallel stream processing for a large dataset.

**Input:** List of 10,000,000 integers

**Expected Output:** Time comparison showing parallel vs sequential processing times

---

### 🔴 Hard Problems

#### 38. Parallel Stream Processing
**Question:** Use parallelStream() to filter even numbers divisible by 3, sort them, and take the first 1000.

**Input:** Numbers from 1 to 1,000,000

**Expected Output:** First 1000 numbers that are even and divisible by 3, sorted

---

#### 39. Log File Analysis
**Question:** Parse a log file and create a summary of log levels and extract all error messages.

**Input:** 
```
2023-01-01 10:00:00 INFO Application started
2023-01-01 10:01:00 DEBUG User login attempt
2023-01-01 10:02:00 ERROR Database connection failed
2023-01-01 10:03:00 INFO User logged in successfully
2023-01-01 10:04:00 WARN Memory usage high
2023-01-01 10:05:00 ERROR Timeout occurred
```

**Expected Output:** 
- Log level counts: `{INFO=2, DEBUG=1, ERROR=2, WARN=1}`
- Error messages: `["Database connection failed", "Timeout occurred"]`
