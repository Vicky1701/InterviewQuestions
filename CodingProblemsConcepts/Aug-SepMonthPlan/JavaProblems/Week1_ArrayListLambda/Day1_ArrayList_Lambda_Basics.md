# ☕ Day 1: ArrayList + Lambda Basics

## **Session Overview**
- **Focus**: Introduction to functional programming with ArrayList
- **Duration**: 90 minutes (20 min theory + 60 min practice + 10 min review)
- **Problems**: 6 hands-on exercises
- **Goal**: Master basic lambda expressions and stream operations

---

## 📚 **Theory Review (20 minutes)**

### **ArrayList Refresher**:
```java
List<Integer> numbers = new ArrayList<>();
numbers.add(1);
numbers.add(2);
numbers.add(3);

// Traditional iteration
for (int i = 0; i < numbers.size(); i++) {
    System.out.println(numbers.get(i));
}

// Enhanced for loop
for (Integer num : numbers) {
    System.out.println(num);
}
```

### **Lambda Expressions Introduction**:
```java
// Traditional approach
Comparator<String> traditionalComparator = new Comparator<String>() {
    public int compare(String a, String b) {
        return a.compareTo(b);
    }
};

// Lambda approach
Comparator<String> lambdaComparator = (a, b) -> a.compareTo(b);

// Method reference approach
Comparator<String> methodRefComparator = String::compareTo;
```

### **Basic Stream Operations**:
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Filter even numbers
List<Integer> evens = numbers.stream()
    .filter(n -> n % 2 == 0)
    .collect(Collectors.toList());

// Transform to squares
List<Integer> squares = numbers.stream()
    .map(n -> n * n)
    .collect(Collectors.toList());
```

---

## 💻 **Practice Problems (60 minutes)**

### **Problem 1: Remove Duplicates from ArrayList using Java 8**
**Time**: 10 minutes | **Difficulty**: Easy

**Traditional Approach**:
```java
public static List<Integer> removeDuplicatesTraditional(List<Integer> list) {
    List<Integer> result = new ArrayList<>();
    for (Integer num : list) {
        if (!result.contains(num)) {
            result.add(num);
        }
    }
    return result;
}
```

**Your Java 8 Solution**:
```java
public static List<Integer> removeDuplicates(List<Integer> list) {
    // TODO: Implement using Java 8 streams and distinct()
    
}
```

**Test Case**:
```java
List<Integer> input = Arrays.asList(1, 2, 2, 3, 4, 4, 5);
// Expected output: [1, 2, 3, 4, 5]
```

**Solution**:
<details>
<summary>Click to reveal</summary>

```java
public static List<Integer> removeDuplicates(List<Integer> list) {
    return list.stream()
              .distinct()
              .collect(Collectors.toList());
}
```
</details>

---

### **Problem 2: Find Second Largest Element using Streams**
**Time**: 10 minutes | **Difficulty**: Easy

**Traditional Approach**:
```java
public static Integer findSecondLargestTraditional(List<Integer> list) {
    if (list.size() < 2) return null;
    
    Collections.sort(list, Collections.reverseOrder());
    return list.get(1);
}
```

**Your Java 8 Solution**:
```java
public static Optional<Integer> findSecondLargest(List<Integer> list) {
    // TODO: Implement using streams, distinct, sorted, skip
    
}
```

**Test Case**:
```java
List<Integer> input = Arrays.asList(3, 1, 4, 1, 5, 9, 2, 6);
// Expected output: Optional[6] (after removing duplicates: [9, 6, 5, 4, 3, 2, 1])
```

**Solution**:
<details>
<summary>Click to reveal</summary>

```java
public static Optional<Integer> findSecondLargest(List<Integer> list) {
    return list.stream()
              .distinct()
              .sorted(Collections.reverseOrder())
              .skip(1)
              .findFirst();
}
```
</details>

---

### **Problem 3: Sort ArrayList of Custom Objects using Lambda**
**Time**: 10 minutes | **Difficulty**: Medium

**Person Class**:
```java
class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    // Getters and toString method
    public String getName() { return name; }
    public int getAge() { return age; }
    
    @Override
    public String toString() {
        return name + "(" + age + ")";
    }
}
```

**Traditional Approach**:
```java
public static List<Person> sortByAgeTraditional(List<Person> people) {
    Collections.sort(people, new Comparator<Person>() {
        public int compare(Person p1, Person p2) {
            return Integer.compare(p1.getAge(), p2.getAge());
        }
    });
    return people;
}
```

**Your Java 8 Solutions**:
```java
// Sort by age using lambda
public static List<Person> sortByAge(List<Person> people) {
    // TODO: Implement using lambda comparator
    
}

// Sort by name using method reference
public static List<Person> sortByName(List<Person> people) {
    // TODO: Implement using method reference
    
}
```

**Test Case**:
```java
List<Person> people = Arrays.asList(
    new Person("Alice", 30),
    new Person("Bob", 25),
    new Person("Charlie", 35)
);
```

**Solutions**:
<details>
<summary>Click to reveal</summary>

```java
// Sort by age using lambda
public static List<Person> sortByAge(List<Person> people) {
    return people.stream()
                .sorted((p1, p2) -> Integer.compare(p1.getAge(), p2.getAge()))
                .collect(Collectors.toList());
}

// Sort by name using method reference
public static List<Person> sortByName(List<Person> people) {
    return people.stream()
                .sorted(Comparator.comparing(Person::getName))
                .collect(Collectors.toList());
}
```
</details>

---

### **Problem 4: Create Lambda to Check if Number is Even**
**Time**: 5 minutes | **Difficulty**: Easy

**Traditional Approach**:
```java
public static boolean isEvenTraditional(int number) {
    return number % 2 == 0;
}
```

**Your Lambda Solutions**:
```java
// Create a Predicate for even numbers
Predicate<Integer> isEven = // TODO: Implement lambda

// Create a Function that returns boolean
Function<Integer, Boolean> checkEven = // TODO: Implement lambda

// Test your predicates
public static void testEvenCheckers() {
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
    
    // Filter even numbers using your predicate
    List<Integer> evens = // TODO: Use stream and filter
    
    System.out.println("Even numbers: " + evens);
}
```

**Solutions**:
<details>
<summary>Click to reveal</summary>

```java
// Create a Predicate for even numbers
Predicate<Integer> isEven = n -> n % 2 == 0;

// Create a Function that returns boolean
Function<Integer, Boolean> checkEven = n -> n % 2 == 0;

// Test your predicates
public static void testEvenCheckers() {
    List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
    
    // Filter even numbers using your predicate
    List<Integer> evens = numbers.stream()
                                .filter(isEven)
                                .collect(Collectors.toList());
    
    System.out.println("Even numbers: " + evens);
}
```
</details>

---

### **Problem 5: Lambda Expression for String to Uppercase Conversion**
**Time**: 5 minutes | **Difficulty**: Easy

**Traditional Approach**:
```java
public static List<String> toUppercaseTraditional(List<String> strings) {
    List<String> result = new ArrayList<>();
    for (String str : strings) {
        result.add(str.toUpperCase());
    }
    return result;
}
```

**Your Lambda Solutions**:
```java
// Using Function interface
Function<String, String> toUppercase = // TODO: Implement lambda

// Using method reference
Function<String, String> toUppercaseRef = // TODO: Use method reference

// Apply to list using streams
public static List<String> convertToUppercase(List<String> strings) {
    // TODO: Use stream and map
    
}
```

**Test Case**:
```java
List<String> input = Arrays.asList("hello", "world", "java", "lambda");
// Expected output: ["HELLO", "WORLD", "JAVA", "LAMBDA"]
```

**Solutions**:
<details>
<summary>Click to reveal</summary>

```java
// Using Function interface
Function<String, String> toUppercase = s -> s.toUpperCase();

// Using method reference
Function<String, String> toUppercaseRef = String::toUpperCase;

// Apply to list using streams
public static List<String> convertToUppercase(List<String> strings) {
    return strings.stream()
                 .map(String::toUpperCase)
                 .collect(Collectors.toList());
}
```
</details>

---

### **Problem 6: Lambda to Calculate Sum of Two Integers**
**Time**: 10 minutes | **Difficulty**: Easy

**Traditional Approach**:
```java
interface Calculator {
    int calculate(int a, int b);
}

class Addition implements Calculator {
    public int calculate(int a, int b) {
        return a + b;
    }
}
```

**Your Lambda Solutions**:
```java
// Create different calculator lambdas
Function<int[], Integer> sumCalculator = // TODO: Takes array, returns sum

BinaryOperator<Integer> addOperator = // TODO: Takes two integers, returns sum

// Create a more complex calculator
interface MathOperation {
    int operate(int a, int b);
}

MathOperation addition = // TODO: Lambda for addition
MathOperation multiplication = // TODO: Lambda for multiplication
MathOperation subtraction = // TODO: Lambda for subtraction

// Use with ArrayList operations
public static int calculateListSum(List<Integer> numbers) {
    // TODO: Use stream to sum all elements
    
}

public static List<Integer> pairwiseAdd(List<Integer> list1, List<Integer> list2) {
    // TODO: Add corresponding elements from both lists
    
}
```

**Test Cases**:
```java
List<Integer> nums1 = Arrays.asList(1, 2, 3, 4);
List<Integer> nums2 = Arrays.asList(5, 6, 7, 8);
// Expected pairwise sum: [6, 8, 10, 12]

List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
// Expected sum: 15
```

**Solutions**:
<details>
<summary>Click to reveal</summary>

```java
// Create different calculator lambdas
Function<int[], Integer> sumCalculator = arr -> Arrays.stream(arr).sum();

BinaryOperator<Integer> addOperator = (a, b) -> a + b;

// Create a more complex calculator
MathOperation addition = (a, b) -> a + b;
MathOperation multiplication = (a, b) -> a * b;
MathOperation subtraction = (a, b) -> a - b;

// Use with ArrayList operations
public static int calculateListSum(List<Integer> numbers) {
    return numbers.stream()
                 .mapToInt(Integer::intValue)
                 .sum();
    // Or simply: numbers.stream().reduce(0, Integer::sum);
}

public static List<Integer> pairwiseAdd(List<Integer> list1, List<Integer> list2) {
    return IntStream.range(0, Math.min(list1.size(), list2.size()))
                   .mapToObj(i -> list1.get(i) + list2.get(i))
                   .collect(Collectors.toList());
}
```
</details>

---

## 🤝 **Combined Challenge (10 minutes)**

**Use lambda with ArrayList to filter and transform data**:

```java
public class StudentGradeProcessor {
    static class Student {
        private String name;
        private int grade;
        
        public Student(String name, int grade) {
            this.name = name;
            this.grade = grade;
        }
        
        // Getters
        public String getName() { return name; }
        public int getGrade() { return grade; }
        
        @Override
        public String toString() {
            return name + ": " + grade;
        }
    }
    
    public static void processStudentGrades() {
        List<Student> students = Arrays.asList(
            new Student("Alice", 85),
            new Student("Bob", 92),
            new Student("Charlie", 78),
            new Student("Diana", 96),
            new Student("Eve", 67)
        );
        
        // TODO: Complete these operations using lambdas and streams
        
        // 1. Find students with grades >= 80
        List<Student> goodStudents = // TODO
        
        // 2. Get names of students with grades >= 90
        List<String> excellentStudents = // TODO
        
        // 3. Calculate average grade of all students
        double averageGrade = // TODO
        
        // 4. Find the highest grade
        Optional<Integer> maxGrade = // TODO
        
        // 5. Count students with grades < 70
        long strugglingCount = // TODO
        
        System.out.println("Good students (>=80): " + goodStudents);
        System.out.println("Excellent students (>=90): " + excellentStudents);
        System.out.println("Average grade: " + averageGrade);
        System.out.println("Highest grade: " + maxGrade.orElse(0));
        System.out.println("Students struggling (<70): " + strugglingCount);
    }
}
```

**Solution**:
<details>
<summary>Click to reveal</summary>

```java
public static void processStudentGrades() {
    List<Student> students = Arrays.asList(
        new Student("Alice", 85),
        new Student("Bob", 92),
        new Student("Charlie", 78),
        new Student("Diana", 96),
        new Student("Eve", 67)
    );
    
    // 1. Find students with grades >= 80
    List<Student> goodStudents = students.stream()
        .filter(s -> s.getGrade() >= 80)
        .collect(Collectors.toList());
    
    // 2. Get names of students with grades >= 90
    List<String> excellentStudents = students.stream()
        .filter(s -> s.getGrade() >= 90)
        .map(Student::getName)
        .collect(Collectors.toList());
    
    // 3. Calculate average grade of all students
    double averageGrade = students.stream()
        .mapToInt(Student::getGrade)
        .average()
        .orElse(0.0);
    
    // 4. Find the highest grade
    Optional<Integer> maxGrade = students.stream()
        .map(Student::getGrade)
        .max(Integer::compareTo);
    
    // 5. Count students with grades < 70
    long strugglingCount = students.stream()
        .filter(s -> s.getGrade() < 70)
        .count();
}
```
</details>

---

## 📝 **Your Learning Notes**

### **Key Concepts Learned**:
- [ ] Lambda expression syntax: `(parameters) -> expression`
- [ ] Method references: `ClassName::methodName`
- [ ] Stream operations: `filter`, `map`, `collect`
- [ ] Functional interfaces: `Predicate`, `Function`, `BinaryOperator`

### **Time Tracking**:
- **Theory Review**: _____ minutes
- **Problem 1**: _____ minutes
- **Problem 2**: _____ minutes  
- **Problem 3**: _____ minutes
- **Problem 4**: _____ minutes
- **Problem 5**: _____ minutes
- **Problem 6**: _____ minutes
- **Combined Challenge**: _____ minutes
- **Total Time**: _____ minutes (Target: 90 minutes)

### **Difficulty Assessment**:
- **Easiest Problem**: ________________
- **Hardest Problem**: ________________
- **Most Interesting**: ________________

### **Your Insights**:
- **Biggest "Aha!" Moment**: ________________
- **Most Confusing Concept**: ________________
- **Real-world Application**: ________________

---

## 🔗 **Connection to DSA (Integration Thinking)**

### **How Today's Java Learning Connects to Two Sum**:

1. **HashMap Pattern** (from DSA) ↔ **Map Operations** (Java 8)
2. **Array Iteration** (from DSA) ↔ **Stream Processing** (Java 8)
3. **Pair Finding Logic** (from DSA) ↔ **Filter & Match** (Java 8)

**Example Integration**:
```java
// DSA Two Sum with Java 8 flavor (for learning)
public int[] twoSumJava8Style(int[] nums, int target) {
    Map<Integer, Integer> map = new HashMap<>();
    
    return IntStream.range(0, nums.length)
        .filter(i -> {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return true;
            }
            map.put(nums[i], i);
            return false;
        })
        .mapToObj(i -> new int[]{map.get(target - nums[i]), i})
        .findFirst()
        .orElse(new int[]{});
}
```

---

## ✅ **Completion Checklist**

- [ ] 📚 Completed theory review (20 min)
- [ ] 💻 Solved all 6 problems
- [ ] 🤝 Completed combined challenge
- [ ] 📝 Documented learning insights
- [ ] 🔗 Made connection to DSA concepts
- [ ] ⏰ Finished within 90 minutes
- [ ] 🎯 Ready for tomorrow's advanced topics

---

**🎉 Congratulations on completing Day 1 Java session! 🎉**

**Key Takeaway**: Functional programming makes code more readable and expressive!

**Tomorrow**: ArrayList Advanced + Stream Operations (filter, map, collect deep dive)
