# Java Functional Programming Interview Solutions

## 📘 Core Functional Programming + Streams

### ✅ Easy Problems

#### 1. Filter Even Numbers from a List using filter()
```java
import java.util.*;
import java.util.stream.*;

public class FilterEvenNumbers {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        List<Integer> evenNumbers = numbers.stream()
            .filter(n -> n % 2 == 0)
            .collect(Collectors.toList());
        
        System.out.println("Even numbers: " + evenNumbers);
        // Output: [2, 4, 6, 8, 10]
    }
}
```

#### 2. Map Integers to Their Squares using map()
```java
import java.util.*;
import java.util.stream.*;

public class MapToSquares {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        
        List<Integer> squares = numbers.stream()
            .map(n -> n * n)
            .collect(Collectors.toList());
        
        System.out.println("Squares: " + squares);
        // Output: [1, 4, 9, 16, 25]
    }
}
```

#### 3. Convert Strings to Uppercase
```java
import java.util.*;
import java.util.stream.*;

public class StringsToUppercase {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("hello", "world", "java", "streams");
        
        List<String> upperCaseWords = words.stream()
            .map(String::toUpperCase)
            .collect(Collectors.toList());
        
        System.out.println("Uppercase: " + upperCaseWords);
        // Output: [HELLO, WORLD, JAVA, STREAMS]
    }
}
```

#### 4. Create Comma-Separated String using Collectors.joining()
```java
import java.util.*;
import java.util.stream.*;

public class JoinStrings {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("Java", "Python", "C++", "JavaScript");
        
        String result = words.stream()
            .collect(Collectors.joining(", "));
        
        System.out.println("Joined: " + result);
        // Output: Java, Python, C++, JavaScript
    }
}
```

#### 5. Check if Any Number is Even using anyMatch()
```java
import java.util.*;
import java.util.stream.*;

public class AnyEvenNumber {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 3, 5, 7, 8, 9);
        
        boolean hasEven = numbers.stream()
            .anyMatch(n -> n % 2 == 0);
        
        System.out.println("Has even number: " + hasEven);
        // Output: true
    }
}
```

#### 6. Remove Null or Empty Strings using filter()
```java
import java.util.*;
import java.util.stream.*;

public class RemoveNullEmpty {
    public static void main(String[] args) {
        List<String> strings = Arrays.asList("hello", "", null, "world", "java", null, "");
        
        List<String> cleanStrings = strings.stream()
            .filter(s -> s != null && !s.isEmpty())
            .collect(Collectors.toList());
        
        System.out.println("Clean strings: " + cleanStrings);
        // Output: [hello, world, java]
    }
}
```

### 🔶 Medium Problems

#### 7. Find Second Highest Number using sorted(), limit(), skip()
```java
import java.util.*;
import java.util.stream.*;

public class SecondHighest {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 9, 3, 7);
        
        Optional<Integer> secondHighest = numbers.stream()
            .distinct()
            .sorted(Collections.reverseOrder())
            .skip(1)
            .findFirst();
        
        System.out.println("Second highest: " + secondHighest.orElse(-1));
        // Output: 8
    }
}
```

#### 8. Flatten List of Lists using flatMap()
```java
import java.util.*;
import java.util.stream.*;

public class FlattenLists {
    public static void main(String[] args) {
        List<List<Integer>> listOfLists = Arrays.asList(
            Arrays.asList(1, 2, 3),
            Arrays.asList(4, 5),
            Arrays.asList(6, 7, 8, 9)
        );
        
        List<Integer> flattenedList = listOfLists.stream()
            .flatMap(List::stream)
            .collect(Collectors.toList());
        
        System.out.println("Flattened: " + flattenedList);
        // Output: [1, 2, 3, 4, 5, 6, 7, 8, 9]
    }
}
```

#### 9. Top 3 Maximum Elements using sorted() + limit()
```java
import java.util.*;
import java.util.stream.*;

public class Top3Maximum {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 9, 3, 7, 6, 4);
        
        List<Integer> top3 = numbers.stream()
            .sorted(Collections.reverseOrder())
            .limit(3)
            .collect(Collectors.toList());
        
        System.out.println("Top 3: " + top3);
        // Output: [9, 8, 7]
    }
}
```

#### 10. Group Words by Length using groupingBy()
```java
import java.util.*;
import java.util.stream.*;

public class GroupByLength {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("cat", "dog", "elephant", "rat", "tiger", "lion");
        
        Map<Integer, List<String>> groupedByLength = words.stream()
            .collect(Collectors.groupingBy(String::length));
        
        System.out.println("Grouped by length: " + groupedByLength);
        // Output: {3=[cat, dog, rat], 4=[lion], 5=[tiger], 8=[elephant]}
    }
}
```

#### 11. Distinct + Sorted Pipeline on List of Integers
```java
import java.util.*;
import java.util.stream.*;

public class DistinctSorted {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(5, 2, 8, 2, 1, 9, 3, 7, 5, 1);
        
        List<Integer> result = numbers.stream()
            .distinct()
            .sorted()
            .collect(Collectors.toList());
        
        System.out.println("Distinct and sorted: " + result);
        // Output: [1, 2, 3, 5, 7, 8, 9]
    }
}
```

### 🔴 Hard Problems

#### 12. Full Stream Pipeline – filter → map → distinct → sorted → collect
```java
import java.util.*;
import java.util.stream.*;

public class FullStreamPipeline {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "date", "elderberry", "fig");
        
        // Filter words with length > 4, map to uppercase, distinct, sorted
        List<String> result = words.stream()
            .filter(word -> word.length() > 4)
            .map(String::toUpperCase)
            .distinct()
            .sorted()
            .collect(Collectors.toList());
        
        System.out.println("Pipeline result: " + result);
        // Output: [APPLE, BANANA, CHERRY, ELDERBERRY]
    }
}
```

#### 13. Word Frequency Count from Paragraph using Collectors.groupingBy()
```java
import java.util.*;
import java.util.stream.*;

public class WordFrequency {
    public static void main(String[] args) {
        String paragraph = "java is great java is powerful java streams are awesome";
        
        Map<String, Long> wordFrequency = Arrays.stream(paragraph.split("\\s+"))
            .collect(Collectors.groupingBy(
                String::toLowerCase,
                Collectors.counting()
            ));
        
        System.out.println("Word frequency: " + wordFrequency);
        // Output: {java=3, is=2, great=1, powerful=1, streams=1, are=1, awesome=1}
    }
}
```

#### 14. Find First Non-Repeating Character using Streams
```java
import java.util.*;
import java.util.stream.*;

public class FirstNonRepeatingChar {
    public static void main(String[] args) {
        String input = "programming";
        
        Optional<Character> firstNonRepeating = input.chars()
            .mapToObj(c -> (char) c)
            .collect(Collectors.groupingBy(c -> c, LinkedHashMap::new, Collectors.counting()))
            .entrySet().stream()
            .filter(entry -> entry.getValue() == 1)
            .map(Map.Entry::getKey)
            .findFirst();
        
        System.out.println("First non-repeating: " + firstNonRepeating.orElse(null));
        // Output: p
    }
}
```

## 📗 Functional Interfaces & Lambdas

### ✅ Easy Problems

#### 15. Use Predicate<String> to check if string starts with "A"
```java
import java.util.function.Predicate;
import java.util.*;

public class PredicateExample {
    public static void main(String[] args) {
        Predicate<String> startsWithA = s -> s.startsWith("A");
        
        List<String> names = Arrays.asList("Alice", "Bob", "Andrew", "Charlie");
        List<String> aNames = names.stream()
            .filter(startsWithA)
            .collect(Collectors.toList());
        
        System.out.println("Names starting with A: " + aNames);
        // Output: [Alice, Andrew]
    }
}
```

#### 16. Use Function<String, Integer> to get string length
```java
import java.util.function.Function;
import java.util.*;

public class FunctionExample {
    public static void main(String[] args) {
        Function<String, Integer> getLength = String::length;
        
        List<String> words = Arrays.asList("hello", "world", "java");
        List<Integer> lengths = words.stream()
            .map(getLength)
            .collect(Collectors.toList());
        
        System.out.println("Lengths: " + lengths);
        // Output: [5, 5, 4]
    }
}
```

#### 17. Use Consumer<String> to print strings
```java
import java.util.function.Consumer;
import java.util.*;

public class ConsumerExample {
    public static void main(String[] args) {
        Consumer<String> printer = System.out::println;
        
        List<String> words = Arrays.asList("hello", "world", "java");
        words.forEach(printer);
        // Output: hello, world, java (each on new line)
    }
}
```

#### 18. Use Supplier<Double> to return random number
```java
import java.util.function.Supplier;
import java.util.*;

public class SupplierExample {
    public static void main(String[] args) {
        Supplier<Double> randomSupplier = Math::random;
        
        List<Double> randomNumbers = Stream.generate(randomSupplier)
            .limit(5)
            .collect(Collectors.toList());
        
        System.out.println("Random numbers: " + randomNumbers);
    }
}
```

### 🔶 Medium Problems

#### 19. Pass Predicate<Employee> to filter list
```java
import java.util.function.Predicate;
import java.util.*;

class Employee {
    private String name;
    private int age;
    private double salary;
    
    public Employee(String name, int age, double salary) {
        this.name = name;
        this.age = age;
        this.salary = salary;
    }
    
    // Getters
    public String getName() { return name; }
    public int getAge() { return age; }
    public double getSalary() { return salary; }
    
    @Override
    public String toString() {
        return "Employee{name='" + name + "', age=" + age + ", salary=" + salary + "}";
    }
}

public class EmployeePredicate {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
            new Employee("Alice", 25, 50000),
            new Employee("Bob", 35, 60000),
            new Employee("Charlie", 28, 55000)
        );
        
        Predicate<Employee> highSalary = emp -> emp.getSalary() > 52000;
        
        List<Employee> highEarners = employees.stream()
            .filter(highSalary)
            .collect(Collectors.toList());
        
        System.out.println("High earners: " + highEarners);
    }
}
```

#### 20. Use BiPredicate<String, Integer> to check if length matches
```java
import java.util.function.BiPredicate;
import java.util.*;

public class BiPredicateExample {
    public static void main(String[] args) {
        BiPredicate<String, Integer> lengthMatches = (str, len) -> str.length() == len;
        
        List<String> words = Arrays.asList("hello", "world", "java", "stream");
        
        List<String> fiveLetterWords = words.stream()
            .filter(word -> lengthMatches.test(word, 5))
            .collect(Collectors.toList());
        
        System.out.println("Five letter words: " + fiveLetterWords);
        // Output: [hello, world]
    }
}
```

#### 21. Use BinaryOperator<String> to combine strings
```java
import java.util.function.BinaryOperator;
import java.util.*;

public class BinaryOperatorExample {
    public static void main(String[] args) {
        BinaryOperator<String> combiner = (s1, s2) -> s1 + " " + s2;
        
        List<String> words = Arrays.asList("Java", "is", "awesome");
        
        String combined = words.stream()
            .reduce(combiner)
            .orElse("");
        
        System.out.println("Combined: " + combined);
        // Output: Java is awesome
    }
}
```

### 🔴 Hard Problems

#### 22. Compose Functions: trim → uppercase → length
```java
import java.util.function.Function;
import java.util.*;

public class FunctionComposition {
    public static void main(String[] args) {
        Function<String, String> trim = String::trim;
        Function<String, String> uppercase = String::toUpperCase;
        Function<String, Integer> length = String::length;
        
        // Compose functions
        Function<String, Integer> composedFunction = trim
            .andThen(uppercase)
            .andThen(length);
        
        List<String> inputs = Arrays.asList("  hello  ", "  world  ", "  java  ");
        
        List<Integer> results = inputs.stream()
            .map(composedFunction)
            .collect(Collectors.toList());
        
        System.out.println("Results: " + results);
        // Output: [5, 5, 4]
    }
}
```

#### 23. Pass BiFunction<List<Integer>, Predicate<Integer>> to filter list
```java
import java.util.function.*;
import java.util.*;

public class BiFunctionFilter {
    public static void main(String[] args) {
        BiFunction<List<Integer>, Predicate<Integer>, List<Integer>> filterList = 
            (list, predicate) -> list.stream()
                .filter(predicate)
                .collect(Collectors.toList());
        
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        Predicate<Integer> isEven = n -> n % 2 == 0;
        
        List<Integer> evenNumbers = filterList.apply(numbers, isEven);
        System.out.println("Even numbers: " + evenNumbers);
        // Output: [2, 4, 6, 8, 10]
    }
}
```

#### 24. Method to filter based on two Predicate<T>
```java
import java.util.function.Predicate;
import java.util.*;

public class TwoPredicateFilter {
    public static <T> List<T> filterWithTwoPredicates(
        List<T> list, 
        Predicate<T> predicate1, 
        Predicate<T> predicate2
    ) {
        return list.stream()
            .filter(predicate1.and(predicate2))
            .collect(Collectors.toList());
    }
    
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        Predicate<Integer> isEven = n -> n % 2 == 0;
        Predicate<Integer> greaterThan5 = n -> n > 5;
        
        List<Integer> result = filterWithTwoPredicates(numbers, isEven, greaterThan5);
        System.out.println("Even and > 5: " + result);
        // Output: [6, 8, 10]
    }
}
```

## 📙 Custom Classes, Sorting, Grouping

### ✅ Easy Problems

#### 25. Use allMatch to check condition over list of objects
```java
import java.util.*;

class Course {
    private String name;
    private int reviewScore;
    private int noOfStudents;
    
    public Course(String name, int reviewScore, int noOfStudents) {
        this.name = name;
        this.reviewScore = reviewScore;
        this.noOfStudents = noOfStudents;
    }
    
    public String getName() { return name; }
    public int getReviewScore() { return reviewScore; }
    public int getNoOfStudents() { return noOfStudents; }
}

public class AllMatchExample {
    public static void main(String[] args) {
        List<Course> courses = Arrays.asList(
            new Course("Java", 85, 100),
            new Course("Python", 90, 150),
            new Course("JavaScript", 80, 120)
        );
        
        boolean allHighRated = courses.stream()
            .allMatch(course -> course.getReviewScore() >= 80);
        
        System.out.println("All courses highly rated: " + allHighRated);
        // Output: true
    }
}
```

#### 26. Sort custom class by field using Comparator.comparing()
```java
import java.util.*;

public class SortByField {
    public static void main(String[] args) {
        List<Course> courses = Arrays.asList(
            new Course("Java", 85, 100),
            new Course("Python", 90, 150),
            new Course("JavaScript", 80, 120)
        );
        
        List<Course> sortedByScore = courses.stream()
            .sorted(Comparator.comparing(Course::getReviewScore))
            .collect(Collectors.toList());
        
        sortedByScore.forEach(course -> 
            System.out.println(course.getName() + ": " + course.getReviewScore()));
        // Output: JavaScript: 80, Java: 85, Python: 90
    }
}
```

#### 27. Skip/limit operations for pagination
```java
import java.util.*;

public class Pagination {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        int pageSize = 3;
        int pageNumber = 2; // 0-indexed
        
        List<Integer> page = numbers.stream()
            .skip(pageNumber * pageSize)
            .limit(pageSize)
            .collect(Collectors.toList());
        
        System.out.println("Page " + pageNumber + ": " + page);
        // Output: Page 2: [7, 8, 9]
    }
}
```

### 🔶 Medium Problems

#### 28. Find max course by reviewScore
```java
import java.util.*;

public class MaxCourseByScore {
    public static void main(String[] args) {
        List<Course> courses = Arrays.asList(
            new Course("Java", 85, 100),
            new Course("Python", 90, 150),
            new Course("JavaScript", 80, 120)
        );
        
        Optional<Course> maxCourse = courses.stream()
            .max(Comparator.comparing(Course::getReviewScore));
        
        maxCourse.ifPresent(course -> 
            System.out.println("Highest rated: " + course.getName() + " (" + course.getReviewScore() + ")"));
        // Output: Highest rated: Python (90)
    }
}
```

#### 29. Group courses by category using Collectors.groupingBy()
```java
import java.util.*;

class CourseWithCategory {
    private String name;
    private String category;
    private int reviewScore;
    
    public CourseWithCategory(String name, String category, int reviewScore) {
        this.name = name;
        this.category = category;
        this.reviewScore = reviewScore;
    }
    
    public String getName() { return name; }
    public String getCategory() { return category; }
    public int getReviewScore() { return reviewScore; }
}

public class GroupByCategory {
    public static void main(String[] args) {
        List<CourseWithCategory> courses = Arrays.asList(
            new CourseWithCategory("Java", "Programming", 85),
            new CourseWithCategory("Python", "Programming", 90),
            new CourseWithCategory("Photoshop", "Design", 80),
            new CourseWithCategory("Illustrator", "Design", 85)
        );
        
        Map<String, List<CourseWithCategory>> groupedByCategory = courses.stream()
            .collect(Collectors.groupingBy(CourseWithCategory::getCategory));
        
        System.out.println("Grouped by category: " + groupedByCategory);
    }
}
```

#### 30. Average score using Collectors.averagingInt()
```java
import java.util.*;

public class AverageScore {
    public static void main(String[] args) {
        List<Course> courses = Arrays.asList(
            new Course("Java", 85, 100),
            new Course("Python", 90, 150),
            new Course("JavaScript", 80, 120)
        );
        
        Double averageScore = courses.stream()
            .collect(Collectors.averagingInt(Course::getReviewScore));
        
        System.out.println("Average score: " + averageScore);
        // Output: Average score: 85.0
    }
}
```

### 🔴 Hard Problems

#### 31. Find course with max students per category
```java
import java.util.*;

public class MaxStudentsPerCategory {
    public static void main(String[] args) {
        List<CourseWithCategory> courses = Arrays.asList(
            new CourseWithCategory("Java", "Programming", 85, 100),
            new CourseWithCategory("Python", "Programming", 90, 150),
            new CourseWithCategory("Photoshop", "Design", 80, 120),
            new CourseWithCategory("Illustrator", "Design", 85, 80)
        );
        
        Map<String, Optional<CourseWithCategory>> maxStudentsPerCategory = courses.stream()
            .collect(Collectors.groupingBy(
                CourseWithCategory::getCategory,
                Collectors.maxBy(Comparator.comparing(CourseWithCategory::getNoOfStudents))
            ));
        
        maxStudentsPerCategory.forEach((category, course) -> 
            System.out.println(category + ": " + course.get().getName()));
        // Output: Programming: Python, Design: Photoshop
    }
}
```

#### 32. Create map of category → sorted course names by noOfStudents
```java
import java.util.*;

public class CategorySortedCourses {
    public static void main(String[] args) {
        List<CourseWithCategory> courses = Arrays.asList(
            new CourseWithCategory("Java", "Programming", 85, 100),
            new CourseWithCategory("Python", "Programming", 90, 150),
            new CourseWithCategory("C++", "Programming", 80, 80),
            new CourseWithCategory("Photoshop", "Design", 80, 120),
            new CourseWithCategory("Illustrator", "Design", 85, 80)
        );
        
        Map<String, List<String>> categorySortedCourses = courses.stream()
            .collect(Collectors.groupingBy(
                CourseWithCategory::getCategory,
                Collectors.collectingAndThen(
                    Collectors.toList(),
                    list -> list.stream()
                        .sorted(Comparator.comparing(CourseWithCategory::getNoOfStudents).reversed())
                        .map(CourseWithCategory::getName)
                        .collect(Collectors.toList())
                )
            ));
        
        System.out.println("Category sorted courses: " + categorySortedCourses);
    }
}
```

#### 33. Partition courses into high/low-rated using partitioningBy()
```java
import java.util.*;

public class PartitionCourses {
    public static void main(String[] args) {
        List<Course> courses = Arrays.asList(
            new Course("Java", 85, 100),
            new Course("Python", 90, 150),
            new Course("JavaScript", 80, 120),
            new Course("C++", 75, 80)
        );
        
        Map<Boolean, List<Course>> partitioned = courses.stream()
            .collect(Collectors.partitioningBy(course -> course.getReviewScore() >= 85));
        
        System.out.println("High-rated courses: " + partitioned.get(true).size());
        System.out.println("Low-rated courses: " + partitioned.get(false).size());
    }
}
```

## 📒 Performance & Real-World Application

### ✅ Easy Problems

#### 34. Start Thread using Lambda
```java
public class ThreadLambda {
    public static void main(String[] args) {
        // Traditional way
        Thread thread1 = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("Thread 1 running");
            }
        });
        
        // Lambda way
        Thread thread2 = new Thread(() -> System.out.println("Thread 2 running"));
        
        thread1.start();
        thread2.start();
    }
}
```

#### 35. Use List.replaceAll() and removeIf()
```java
import java.util.*;

public class ListOperations {
    public static void main(String[] args) {
        List<String> words = new ArrayList<>(Arrays.asList("hello", "world", "java", "stream"));
        
        // Replace all with uppercase
        words.replaceAll(String::toUpperCase);
        System.out.println("After replaceAll: " + words);
        
        // Remove words with length < 5
        words.removeIf(word -> word.length() < 5);
        System.out.println("After removeIf: " + words);
    }
}
```

### 🔶 Medium Problems

#### 36. Use Files.lines() to read file and count keyword
```java
import java.io.IOException;
import java.nio.file.*;
import java.util.stream.Stream;

public class FileProcessing {
    public static void main(String[] args) {
        try {
            // Create a sample file for demonstration
            Path path = Paths.get("sample.txt");
            Files.write(path, "java is great\njava streams are powerful\npython is also good".getBytes());
            
            long javaCount = Files.lines(path)
                .flatMap(line -> Stream.of(line.split("\\s+")))
                .filter(word -> word.equalsIgnoreCase("java"))
                .count();
            
            System.out.println("Java keyword count: " + javaCount);
            
            // Clean up
            Files.deleteIfExists(path);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### 37. Parallel vs Sequential Stream benchmark
```java
import java.util.*;
import java.util.stream.IntStream;

public class ParallelVsSequential {
    public static void main(String[] args) {
        List<Integer> numbers = IntStream.rangeClosed(1, 10_000_000)
            .boxed()
            .collect(Collectors.toList());
        
        // Sequential processing
        long startTime = System.currentTimeMillis();
        long sequentialSum = numbers.stream()
            .mapToLong(Integer::longValue)
            .sum();
        long sequentialTime = System.currentTimeMillis() - startTime;
        
        // Parallel processing
        startTime = System.currentTimeMillis();
        long parallelSum = numbers.parallelStream()
            .mapToLong(Integer::longValue)
            .sum();
        long parallelTime = System.currentTimeMillis() - startTime;
        
        System.out.println("Sequential time: " + sequentialTime + "ms, Sum: " + sequentialSum);
        System.out.println("Parallel time: " + parallelTime + "ms, Sum: " + parallelSum);
    }
}
```

### 🔴 Hard Problems

#### 38. Use parallelStream() for filtering + sorting
```java
import java.util.*;
import java.util.stream.IntStream;

public class ParallelFilterSort {
    public static void main(String[] args) {
        List<Integer> numbers = IntStream.rangeClosed(1, 1_000_000)
            .boxed()
            .collect(Collectors.toList());
        
        long startTime = System.currentTimeMillis();
        
        List<Integer> result = numbers.parallelStream()
            .filter(n -> n % 2 == 0)
            .filter(n -> n % 3 == 0)
            .sorted()
            .limit(1000)
            .collect(Collectors.toList());
        
        long endTime = System.currentTimeMillis();
        
        System.out.println("Parallel processing time: " + (endTime - startTime) + "ms");
        System.out.println("First 10 results: " + result.subList(0, Math.min(10, result.size())));
    }
}
```

#### 39. Summarize log file entries using Streams
```java
import java.io.IOException;
import java.nio.file.*;
import java.util.*;
import java.util.stream.Collectors;
import java.util.regex.Pattern;

class LogEntry {
    private String level;
    private String message;
    private String timestamp;
    
    public LogEntry(String level, String message, String timestamp) {
        this.level = level;
        this.message = message;
        this.timestamp = timestamp;
    }
    
    public String getLevel() { return level; }
    public String getMessage() { return message; }
    public String getTimestamp() { return timestamp; }
}

public class LogFileAnalysis {
    public static void main(String[] args) {
        try {
            // Create sample log file
            Path logPath = Paths.get("application.log");
            String logContent = "2023-01-01 10:00:00 INFO Application started\n" +
                              "2023-01-01 10:01:00 DEBUG User login attempt\n" +
                              "2023-01-01 10:02:00 ERROR Database connection failed\n" +
                              "2023-01-01 10:03:00 INFO User logged in successfully\n" +
                              "2023-01-01 10:04:00 WARN Memory usage high\n" +
                              "2023-01-01 10:05:00 ERROR Timeout occurred\n";
            
            Files.write(logPath, logContent.getBytes());
            
            Pattern logPattern = Pattern.compile("(\\d{4}-\\d{2}-\\d{2} \\d{2}:\\d{2}:\\d{2}) (\\w+) (.+)");
            
            Map<String, Long> logLevelCounts = Files.lines(logPath)
                .map(logPattern::matcher)
                .filter(matcher -> matcher.matches())
                .map(matcher -> new LogEntry(matcher.group(2), matcher.group(3), matcher.group(1)))
                .collect(Collectors.groupingBy(LogEntry::getLevel, Collectors.counting()));
            
            System.out.println("Log level summary: " + logLevelCounts);
            
            // Find error messages
            List<String> errorMessages = Files.lines(logPath)
                .map(logPattern::matcher)
                .filter(matcher -> matcher.matches())
                .map(matcher -> new LogEntry(matcher.group(2), matcher.group(3), matcher.group(1)))
                .filter(entry -> "ERROR".equals(entry.getLevel()))
                .map(LogEntry::getMessage)
                .collect(Collectors.toList());
            
            System.out.println("Error messages: " + errorMessages);
            
            // Clean up
            Files.deleteIfExists(logPath);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 📋 Additional Core Problems

### ✅ Easy Problems

#### 40. Convert Array to List
```java
import java.util.*;

public class ArrayToList {
    public static void main(String[] args) {
        // Method 1: Using Arrays.asList()
        String[] array1 = {"apple", "banana", "cherry"};
        List<String> list1 = Arrays.asList(array1);
        System.out.println("Method 1: " + list1);
        
        // Method 2: Using Streams
        Integer[] array2 = {1, 2, 3, 4, 5};
        List<Integer> list2 = Arrays.stream(array2)
            .collect(Collectors.toList());
        System.out.println("Method 2: " + list2);
        
        // Method 3: Using Collections.addAll()
        String[] array3 = {"x", "y", "z"};
        List<String> list3 = new ArrayList<>();
        Collections.addAll(list3, array3);
        System.out.println("Method 3: " + list3);
    }
}
```

#### 41. Count frequency of elements in List using Map
```java
import java.util.*;
import java.util.stream.Collectors;

public class ElementFrequency {
    public static void main(String[] args) {
        List<String> elements = Arrays.asList("apple", "banana", "apple", "cherry", "banana", "apple");
        
        // Method 1: Using Streams
        Map<String, Long> frequency1 = elements.stream()
            .collect(Collectors.groupingBy(e -> e, Collectors.counting()));
        
        // Method 2: Manual counting
        Map<String, Integer> frequency2 = new HashMap<>();
        for (String element : elements) {
            frequency2.put(element, frequency2.getOrDefault(element, 0) + 1);
        }
        
        System.out.println("Stream method: " + frequency1);
        System.out.println("Manual method: " + frequency2);
    }
}
```

#### 42. Remove duplicates from List using Set
```java
import java.util.*;
import java.util.stream.Collectors;

public class RemoveDuplicates {
    public static void main(String[] args) {
        List<Integer> listWithDuplicates = Arrays.asList(1, 2, 3, 2, 4, 1, 5, 3);
        
        // Method 1: Using LinkedHashSet (preserves order)
        List<Integer> unique1 = new ArrayList<>(new LinkedHashSet<>(listWithDuplicates));
        
        // Method 2: Using Streams
        List<Integer> unique2 = listWithDuplicates.stream()
            .distinct()
            .collect(Collectors.toList());
        
        // Method 3: Using Set
        Set<Integer> uniqueSet = new HashSet<>(listWithDuplicates);
        List<Integer> unique3 = new ArrayList<>(uniqueSet);
        
        System.out.println("Original: " + listWithDuplicates);
        System.out.println("Method 1 (LinkedHashSet): " + unique1);
        System.out.println("Method 2 (Streams): " + unique2);
        System.out.println("Method 3 (HashSet): " + unique3);
    }
}
```

#### 43. Sort List of Strings alphabetically
```java
import java.util.*;
import java.util.stream.Collectors;

public class SortStrings {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("banana", "apple", "cherry", "date");
        
        // Method 1: Using Collections.sort()
        List<String> sorted1 = new ArrayList<>(words);
        Collections.sort(sorted1);
        
        // Method 2: Using List.sort()
        List<String> sorted2 = new ArrayList<>(words);
        sorted2.sort(String::compareTo);
        
        // Method 3: Using Streams
        List<String> sorted3 = words.stream()
            .sorted()
            .collect(Collectors.toList());
        
        // Method 4: Case-insensitive sorting
        List<String> sorted4 = words.stream()
            .sorted(String.CASE_INSENSITIVE_ORDER)
            .collect(Collectors.toList());
        
        System.out.println("Original: " + words);
        System.out.println("Sorted 1: " + sorted1);
        System.out.println("Sorted 2: " + sorted2);
        System.out.println("Sorted 3: " + sorted3);
        System.out.println("Case-insensitive: " + sorted4);
    }
}
```

#### 44. Check if two Lists are equal (ignoring order)
```java
import java.util.*;

public class ListEquality {
    public static void main(String[] args) {
        List<Integer> list1 = Arrays.asList(1, 2, 3, 4, 5);
        List<Integer> list2 = Arrays.asList(5, 4, 3, 2, 1);
        List<Integer> list3 = Arrays.asList(1, 2, 3, 4, 6);
        
        // Method 1: Convert to sets
        boolean equal1 = new HashSet<>(list1).equals(new HashSet<>(list2));
        
        // Method 2: Sort and compare
        List<Integer> sortedList1 = new ArrayList<>(list1);
        List<Integer> sortedList2 = new ArrayList<>(list2);
        Collections.sort(sortedList1);
        Collections.sort(sortedList2);
        boolean equal2 = sortedList1.equals(sortedList2);
        
        // Method 3: Check containsAll both ways
        boolean equal3 = list1.containsAll(list2) && list2.containsAll(list1) && list1.size() == list2.size();
        
        System.out.println("List1: " + list1);
        System.out.println("List2: " + list2);
        System.out.println("List3: " + list3);
        System.out.println("List1 equals List2 (ignoring order): " + equal1);
        System.out.println("List1 equals List3 (ignoring order): " + new HashSet<>(list1).equals(new HashSet<>(list3)));
    }
}
```

#### 45. Iterate Map and print key-value pairs using forEach
```java
import java.util.*;

public class MapIteration {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 5);
        map.put("banana", 3);
        map.put("cherry", 8);
        
        // Method 1: Using forEach on entrySet
        System.out.println("Method 1 - EntrySet forEach:");
        map.entrySet().forEach(entry -> 
            System.out.println(entry.getKey() + " = " + entry.getValue()));
        
        // Method 2: Using forEach with key-value parameters
        System.out.println("\nMethod 2 - Direct forEach:");
        map.forEach((key, value) -> 
            System.out.println(key + " = " + value));
        
        // Method 3: Traditional for-each loop
        System.out.println("\nMethod 3 - Traditional for-each:");
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }
    }
}
```

### 🟡 Medium Problems

#### 46. Group list of Strings by first character using Map<String, List<String>>
```java
import java.util.*;
import java.util.stream.Collectors;

public class GroupByFirstChar {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "apricot", "blueberry", "cranberry");
        
        // Method 1: Using Streams
        Map<String, List<String>> grouped1 = words.stream()
            .collect(Collectors.groupingBy(word -> String.valueOf(word.charAt(0))));
        
        // Method 2: Manual grouping
        Map<String, List<String>> grouped2 = new HashMap<>();
        for (String word : words) {
            String firstChar = String.valueOf(word.charAt(0));
            grouped2.computeIfAbsent(firstChar, k -> new ArrayList<>()).add(word);
        }
        
        System.out.println("Stream grouping: " + grouped1);
        System.out.println("Manual grouping: " + grouped2);
    }
}
```

#### 47. Sort List<Employee> by salary or name using Comparator
```java
import java.util.*;
import java.util.stream.Collectors;

public class SortEmployees {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
            new Employee("Alice", 25, 50000),
            new Employee("Bob", 35, 60000),
            new Employee("Charlie", 28, 50000),
            new Employee("Diana", 30, 55000)
        );
        
        // Sort by salary (ascending)
        List<Employee> sortedBySalary = employees.stream()
            .sorted(Comparator.comparing(Employee::getSalary))
            .collect(Collectors.toList());
        
        // Sort by name (alphabetically)
        List<Employee> sortedByName = employees.stream()
            .sorted(Comparator.comparing(Employee::getName))
            .collect(Collectors.toList());
        
        // Sort by salary desc, then by name asc
        List<Employee> sortedBySalaryThenName = employees.stream()
            .sorted(Comparator.comparing(Employee::getSalary).reversed()
                    .thenComparing(Employee::getName))
            .collect(Collectors.toList());
        
        System.out.println("Sorted by salary:");
        sortedBySalary.forEach(System.out::println);
        
        System.out.println("\nSorted by name:");
        sortedByName.forEach(System.out::println);
        
        System.out.println("\nSorted by salary desc, then name:");
        sortedBySalaryThenName.forEach(System.out::println);
    }
}
```

#### 48. Merge two Maps into one
```java
import java.util.*;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class MergeMaps {
    public static void main(String[] args) {
        Map<String, Integer> map1 = new HashMap<>();
        map1.put("apple", 5);
        map1.put("banana", 3);
        
        Map<String, Integer> map2 = new HashMap<>();
        map2.put("cherry", 8);
        map2.put("banana", 7); // Duplicate key
        
        // Method 1: Using putAll (second map overwrites first)
        Map<String, Integer> merged1 = new HashMap<>(map1);
        merged1.putAll(map2);
        
        // Method 2: Using Streams (handling conflicts by summing)
        Map<String, Integer> merged2 = Stream.of(map1, map2)
            .flatMap(map -> map.entrySet().stream())
            .collect(Collectors.toMap(
                Map.Entry::getKey,
                Map.Entry::getValue,
                Integer::sum
            ));
        
        // Method 3: Using merge method
        Map<String, Integer> merged3 = new HashMap<>(map1);
        map2.forEach((key, value) -> merged3.merge(key, value, Integer::sum));
        
        System.out.println("Map1: " + map1);
        System.out.println("Map2: " + map2);
        System.out.println("Merged1 (putAll): " + merged1);
        System.out.println("Merged2 (sum conflicts): " + merged2);
        System.out.println("Merged3 (merge method): " + merged3);
    }
}
```

#### 49. Remove duplicate values from Map<K, V>
```java
import java.util.*;
import java.util.stream.Collectors;

public class RemoveDuplicateValues {
    public static void main(String[] args) {
        Map<String, Integer> originalMap = new HashMap<>();
        originalMap.put("apple", 5);
        originalMap.put("banana", 3);
        originalMap.put("cherry", 5);  // Duplicate value
        originalMap.put("date", 8);
        originalMap.put("elderberry", 3);  // Duplicate value
        
        // Method 1: Keep first occurrence
        Set<Integer> seenValues = new HashSet<>();
        Map<String, Integer> uniqueValues1 = originalMap.entrySet().stream()
            .filter(entry -> seenValues.add(entry.getValue()))
            .collect(Collectors.toMap(
                Map.Entry::getKey,
                Map.Entry::getValue,
                (e1, e2) -> e1,
                LinkedHashMap::new
            ));
        
        // Method 2: Remove entries with duplicate values
        Map<Integer, Long> valueCounts = originalMap.values().stream()
            .collect(Collectors.groupingBy(v -> v, Collectors.counting()));
        
        Map<String, Integer> uniqueValues2 = originalMap.entrySet().stream()
            .filter(entry -> valueCounts.get(entry.getValue()) == 1)
            .collect(Collectors.toMap(
                Map.Entry::getKey,
                Map.Entry::getValue
            ));
        
        System.out.println("Original map: " + originalMap);
        System.out.println("Keep first occurrence: " + uniqueValues1);
        System.out.println("Remove duplicate values: " + uniqueValues2);
    }
}
```

#### 50. Count characters in String using Map<Character, Integer>
```java
import java.util.*;
import java.util.stream.Collectors;

public class CharacterCount {
    public static void main(String[] args) {
        String text = "programming";
        
        // Method 1: Using Streams
        Map<Character, Long> charCount1 = text.chars()
            .mapToObj(c -> (char) c)
            .collect(Collectors.groupingBy(c -> c, Collectors.counting()));
        
        // Method 2: Manual counting
        Map<Character, Integer> charCount2 = new HashMap<>();
        for (char c : text.toCharArray()) {
            charCount2.put(c, charCount2.getOrDefault(c, 0) + 1);
        }
        
        // Method 3: Using merge method
        Map<Character, Integer> charCount3 = new HashMap<>();
        for (char c : text.toCharArray()) {
            charCount3.merge(c, 1, Integer::sum);
        }
        
        System.out.println("Text: " + text);
        System.out.println("Stream method: " + charCount1);
        System.out.println("Manual method: " + charCount2);
        System.out.println("Merge method: " + charCount3);
    }
}
```

#### 51. Sort Map by keys or values
```java
import java.util.*;
import java.util.stream.Collectors;

public class SortMap {
    public static void main(String[] args) {
        Map<String, Integer> originalMap = new HashMap<>();
        originalMap.put("banana", 3);
        originalMap.put("apple", 5);
        originalMap.put("cherry", 1);
        originalMap.put("date", 8);
        
        // Sort by keys
        Map<String, Integer> sortedByKeys = originalMap.entrySet().stream()
            .sorted(Map.Entry.comparingByKey())
            .collect(Collectors.toMap(
                Map.Entry::getKey,
                Map.Entry::getValue,
                (e1, e2) -> e1,
                LinkedHashMap::new
            ));
        
        // Sort by values (ascending)
        Map<String, Integer> sortedByValues = originalMap.entrySet().stream()
            .sorted(Map.Entry.comparingByValue())
            .collect(Collectors.toMap(
                Map.Entry::getKey,
                Map.Entry::getValue,
                (e1, e2) -> e1,
                LinkedHashMap::new
            ));
        
        // Sort by values (descending)
        Map<String, Integer> sortedByValuesDesc = originalMap.entrySet().stream()
            .sorted(Map.Entry.<String, Integer>comparingByValue().reversed())
            .collect(Collectors.toMap(
                Map.Entry::getKey,
                Map.Entry::getValue,
                (e1, e2) -> e1,
                LinkedHashMap::new
            ));
        
        System.out.println("Original: " + originalMap);
        System.out.println("Sorted by keys: " + sortedByKeys);
        System.out.println("Sorted by values (asc): " + sortedByValues);
        System.out.println("Sorted by values (desc): " + sortedByValuesDesc);
    }
}
```

#### 52. Filter Map<String, Integer> to retain entries with values > 50
```java
import java.util.*;
import java.util.stream.Collectors;

public class FilterMap {
    public static void main(String[] args) {
        Map<String, Integer> scores = new HashMap<>();
        scores.put("Alice", 85);
        scores.put("Bob", 42);
        scores.put("Charlie", 67);
        scores.put("Diana", 31);
        scores.put("Eve", 78);
        
        // Method 1: Using Streams
        Map<String, Integer> highScores1 = scores.entrySet().stream()
            .filter(entry -> entry.getValue() > 50)
            .collect(Collectors.toMap(
                Map.Entry::getKey,
                Map.Entry::getValue
            ));
        
        // Method 2: Using removeIf (modifies original)
        Map<String, Integer> scoresCopy = new HashMap<>(scores);
        scoresCopy.entrySet().removeIf(entry -> entry.getValue() <= 50);
        
        System.out.println("Original scores: " + scores);
        System.out.println("High scores (>50) - Stream: " + highScores1);
        System.out.println("High scores (>50) - removeIf: " + scoresCopy);
    }
}
```

### 🔴 Hard Problems

#### 53. LRU Cache Implementation using LinkedHashMap
```java
import java.util.*;

class LRUCache<K, V> extends LinkedHashMap<K, V> {
    private final int capacity;
    
    public LRUCache(int capacity) {
        super(capacity, 0.75f, true); // accessOrder = true for LRU
        this.capacity = capacity;
    }
    
    @Override
    protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
        return size() > capacity;
    }
    
    public V get(K key) {
        return super.get(key); // This will move accessed item to end
    }
    
    public V put(K key, V value) {
        return super.put(key, value);
    }
}

public class LRUCacheDemo {
    public static void main(String[] args) {
        LRUCache<String, Integer> cache = new LRUCache<>(3);
        
        cache.put("A", 1);
        cache.put("B", 2);
        cache.put("C", 3);
        System.out.println("After adding A, B, C: " + cache);
        
        cache.get("A"); // Access A, moves it to end
        System.out.println("After accessing A: " + cache);
        
        cache.put("D", 4); // This should remove B (least recently used)
        System.out.println("After adding D: " + cache);
        
        cache.put("E", 5); // This should remove C
        System.out.println("After adding E: " + cache);
    }
}
```

#### 54. Top K Frequent Elements from List
```java
import java.util.*;
import java.util.stream.Collectors;

public class TopKFrequent {
    public static void main(String[] args) {
        List<String> elements = Arrays.asList(
            "apple", "banana", "apple", "cherry", "banana", "apple", 
            "date", "cherry", "banana", "elderberry"
        );
        int k = 3;
        
        // Method 1: Using Streams and sorting
        List<String> topK1 = elements.stream()
            .collect(Collectors.groupingBy(e -> e, Collectors.counting()))
            .entrySet().stream()
            .sorted(Map.Entry.<String, Long>comparingByValue().reversed())
            .limit(k)
            .map(Map.Entry::getKey)
            .collect(Collectors.toList());
        
        // Method 2: Using PriorityQueue (Min-Heap)
        Map<String, Long> frequencyMap = elements.stream()
            .collect(Collectors.groupingBy(e -> e, Collectors.counting()));
        
        PriorityQueue<Map.Entry<String, Long>> minHeap = new PriorityQueue<>(
            Map.Entry.comparingByValue()
        );
        
        for (Map.Entry<String, Long> entry : frequencyMap.entrySet()) {
            minHeap.offer(entry);
            if (minHeap.size() > k) {
                minHeap.poll();
            }
        }
        
        List<String> topK2 = minHeap.stream()
            .sorted(Map.Entry.<String, Long>comparingByValue().reversed())
            .map(Map.Entry::getKey)
            .collect(Collectors.toList());
        
        System.out.println("Elements: " + elements);
        System.out.println("Frequency map: " + frequencyMap);
        System.out.println("Top " + k + " frequent (method 1): " + topK1);
        System.out.println("Top " + k + " frequent (method 2): " + topK2);
    }
}
```

#### 55. Detect cycle in directed graph (Adjacency List)
```java
import java.util.*;

public class CycleDetection {
    private Map<Integer, List<Integer>> graph;
    
    public CycleDetection() {
        this.graph = new HashMap<>();
    }
    
    public void addEdge(int from, int to) {
        graph.computeIfAbsent(from, k -> new ArrayList<>()).add(to);
    }
    
    public boolean hasCycle() {
        Set<Integer> visited = new HashSet<>();
        Set<Integer> recursionStack = new HashSet<>();
        
        for (Integer node : graph.keySet()) {
            if (!visited.contains(node)) {
                if (hasCycleDFS(node, visited, recursionStack)) {
                    return true;
                }
            }
        }
        return false;
    }
    
    private boolean hasCycleDFS(int node, Set<Integer> visited, Set<Integer> recursionStack) {
        visited.add(node);
        recursionStack.add(node);
        
        List<Integer> neighbors = graph.getOrDefault(node, new ArrayList<>());
        for (int neighbor : neighbors) {
            if (!visited.contains(neighbor)) {
                if (hasCycleDFS(neighbor, visited, recursionStack)) {
                    return true;
                }
            } else if (recursionStack.contains(neighbor)) {
                return true; // Back edge found, cycle detected
            }
        }
        
        recursionStack.remove(node);
        return false;
    }
    
    public static void main(String[] args) {
        // Example 1: Graph with cycle
        CycleDetection graph1 = new CycleDetection();
        graph1.addEdge(0, 1);
        graph1.addEdge(1, 2);
        graph1.addEdge(2, 0); // Creates cycle: 0 -> 1 -> 2 -> 0
        
        System.out.println("Graph 1 has cycle: " + graph1.hasCycle());
        
        // Example 2: Graph without cycle
        CycleDetection graph2 = new CycleDetection();
        graph2.addEdge(0, 1);
        graph2.addEdge(1, 2);
        graph2.addEdge(0, 2);
        
        System.out.println("Graph 2 has cycle: " + graph2.hasCycle());
    }
}
```

#### 56. Group Anagrams from List of Strings
```java
import java.util.*;
import java.util.stream.Collectors;

public class GroupAnagrams {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("eat", "tea", "tan", "ate", "nat", "bat");
        
        // Method 1: Using sorting as key
        Map<String, List<String>> anagramGroups1 = words.stream()
            .collect(Collectors.groupingBy(word -> {
                char[] chars = word.toCharArray();
                Arrays.sort(chars);
                return new String(chars);
            }));
        
        // Method 2: Using character frequency as key
        Map<Map<Character, Long>, List<String>> anagramGroups2 = words.stream()
            .collect(Collectors.groupingBy(word -> 
                word.chars()
                    .mapToObj(c -> (char) c)
                    .collect(Collectors.groupingBy(c -> c, Collectors.counting()))
            ));
        
        System.out.println("Original words: " + words);
        System.out.println("Anagram groups (sorting): " + anagramGroups1);
        System.out.println("Anagram groups (frequency): " + anagramGroups2.values());
        
        // Get only groups with more than one word
        List<List<String>> anagramGroupsOnly = anagramGroups1.values().stream()
            .filter(group -> group.size() > 1)
            .collect(Collectors.toList());
        
        System.out.println("Anagram groups only: " + anagramGroupsOnly);
    }
}
```

#### 57. Custom Data Structure - Word Frequency Tracker
```java
import java.util.*;
import java.util.stream.Collectors;

class WordFrequencyTracker {
    private Map<String, Integer> wordCounts;
    private Map<Integer, Set<String>> frequencyToWords;
    
    public WordFrequencyTracker() {
        this.wordCounts = new HashMap<>();
        this.frequencyToWords = new HashMap<>();
    }
    
    public void addWord(String word) {
        int oldFreq = wordCounts.getOrDefault(word, 0);
        int newFreq = oldFreq + 1;
        
        wordCounts.put(word, newFreq);
        
        // Update frequency mappings
        if (oldFreq > 0) {
            frequencyToWords.get(oldFreq).remove(word);
            if (frequencyToWords.get(oldFreq).isEmpty()) {
                frequencyToWords.remove(oldFreq);
            }
        }
        
        frequencyToWords.computeIfAbsent(newFreq, k -> new HashSet<>()).add(word);
    }
    
    public int getFrequency(String word) {
        return wordCounts.getOrDefault(word, 0);
    }
    
    public Set<String> getWordsWithFrequency(int frequency) {
        return frequencyToWords.getOrDefault(frequency, new HashSet<>());
    }
    
    public String getMostFrequentWord() {
        if (frequencyToWords.isEmpty()) return null;
        
        int maxFreq = Collections.max(frequencyToWords.keySet());
        return frequencyToWords.get(maxFreq).iterator().next();
    }
    
    public List<String> getTopKFrequentWords(int k) {
        return wordCounts.entrySet().stream()
            .sorted(Map.Entry.<String, Integer>comparingByValue().reversed()
                    .thenComparing(Map.Entry.comparingByKey()))
            .limit(k)
            .map(Map.Entry::getKey)
            .collect(Collectors.toList());
    }
