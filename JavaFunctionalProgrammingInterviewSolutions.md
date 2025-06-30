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

