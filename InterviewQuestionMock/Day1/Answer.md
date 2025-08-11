# Day 1 Interview Questions - Answers

## 🔹 Java Core Concepts

### Answer 1: Call by value vs call by reference

**Java is always call by value**, but this can be confusing when dealing with objects.

- **Primitive types**: The actual value is copied. Changes to the parameter don't affect the original variable.
- **Objects**: The reference (memory address) is copied by value. You can modify the object's state through this reference, but you cannot change what the original reference points to.

```java
// Primitive example
void modifyPrimitive(int x) {
    x = 10; // Original variable unchanged
}

// Object example
void modifyObject(List<String> list) {
    list.add("new"); // Original list is modified
    list = new ArrayList<>(); // Original reference unchanged
}
```

### Answer 2: Can we change the scope of an overridden method? Rules?

**No, you cannot reduce the visibility of an overridden method**, but you can increase it.

**Rules:**
- `private` → `private, protected, public`
- `protected` → `protected, public`
- `public` → `public` only
- `default` → `default, protected, public`

```java
class Parent {
    protected void method() {}
}

class Child extends Parent {
    public void method() {} // Valid - increasing visibility
    // private void method() {} // Invalid - reducing visibility
}
```

### Answer 3: Can we write default and static methods inside a function? Can lambdas use them?

**No, you cannot write default and static methods inside functions.** These can only be declared in interfaces or classes.

**Lambdas and interface methods:**
- Lambdas **can call** static methods from interfaces
- Lambdas **can call** default methods if the functional interface has them
- Lambdas **cannot define** new default or static methods

```java
interface MyInterface {
    void abstractMethod();
    
    default void defaultMethod() {
        System.out.println("Default method");
    }
    
    static void staticMethod() {
        System.out.println("Static method");
    }
}

// Lambda can use these methods
MyInterface lambda = () -> {
    MyInterface.staticMethod(); // Can call static method
    // this.defaultMethod(); // Cannot call default method directly
};
```

### Answer 4: About Thread life cycle?

**Thread Life Cycle States:**

1. **NEW**: Thread created but not started
2. **RUNNABLE**: Thread is executing or ready to execute
3. **BLOCKED**: Thread blocked waiting for monitor lock
4. **WAITING**: Thread waiting indefinitely for another thread
5. **TIMED_WAITING**: Thread waiting for specified time
6. **TERMINATED**: Thread completed execution

```java
Thread t = new Thread(() -> {
    try {
        Thread.sleep(1000); // TIMED_WAITING
        synchronized(this) {
            wait(); // WAITING
        }
    } catch (InterruptedException e) {}
}); // NEW state

t.start(); // RUNNABLE state
// Eventually TERMINATED
```

### Answer 5: If int a = 5 is outside, can we use it inside .filter() of Stream API?

**Yes, but the variable must be effectively final** (not modified after initialization).

```java
int a = 5; // Effectively final
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

List<Integer> result = numbers.stream()
    .filter(x -> x > a) // Valid - 'a' is effectively final
    .collect(Collectors.toList());

// a = 6; // This would make 'a' not effectively final
```

### Answer 6: What is object cloning?

**Object cloning creates a copy of an object.** Java provides two types:

**Shallow Cloning**: Copies object but not nested objects (references are shared)
**Deep Cloning**: Copies object and all nested objects

```java
class Person implements Cloneable {
    String name;
    Address address;
    
    // Shallow clone
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
    
    // Deep clone
    public Person deepClone() {
        Person cloned = new Person();
        cloned.name = this.name;
        cloned.address = this.address.clone(); // Clone nested objects
        return cloned;
    }
}
```

### Answer 7: Method overriding – values and behavior?

**Method overriding allows a subclass to provide specific implementation of a method defined in parent class.**

**Rules:**
- Same method signature (name, parameters, return type)
- Return type can be covariant (subtype)
- Cannot reduce visibility
- Cannot override static, final, or private methods

```java
class Animal {
    public Animal makeSound() {
        System.out.println("Animal sound");
        return this;
    }
}

class Dog extends Animal {
    @Override
    public Dog makeSound() { // Covariant return type
        System.out.println("Woof!");
        return this;
    }
}
```

### Answer 8: Difference between .reduce() and .map() in Stream API

| Aspect | map() | reduce() |
|--------|-------|----------|
| **Purpose** | Transform each element | Combine elements into single result |
| **Return Type** | Stream | Optional or single value |
| **Operation** | 1-to-1 transformation | Many-to-1 aggregation |

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// map() - transforms each element
List<Integer> doubled = numbers.stream()
    .map(x -> x * 2) // [2, 4, 6, 8, 10]
    .collect(Collectors.toList());

// reduce() - combines all elements
Optional<Integer> sum = numbers.stream()
    .reduce((a, b) -> a + b); // 15
```

### Answer 9: Working of Optional.of()

**Optional.of()** creates an Optional containing the given non-null value.

```java
// Valid usage
Optional<String> opt1 = Optional.of("Hello"); // Contains "Hello"

// Will throw NullPointerException
Optional<String> opt2 = Optional.of(null); // Throws NPE

// Safe alternatives
Optional<String> opt3 = Optional.ofNullable(null); // Empty Optional
Optional<String> opt4 = Optional.empty(); // Empty Optional

// Usage
opt1.ifPresent(System.out::println); // Prints "Hello"
String value = opt1.orElse("Default"); // Returns "Hello"
```

### Answer 10: What is lazy loading in streams?

**Lazy loading means stream operations are not executed until a terminal operation is called.**

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

Stream<Integer> stream = numbers.stream()
    .filter(x -> {
        System.out.println("Filtering: " + x);
        return x > 2;
    })
    .map(x -> {
        System.out.println("Mapping: " + x);
        return x * 2;
    }); // Nothing printed yet - operations are lazy

// Only when terminal operation is called, processing begins
List<Integer> result = stream.collect(Collectors.toList());
```

### Answer 11: What is a terminal operation in streams?

**Terminal operations trigger the execution of the stream pipeline and produce a final result.**

**Common terminal operations:**
- `collect()` - Collects elements into a collection
- `forEach()` - Performs action on each element
- `reduce()` - Reduces stream to single value
- `count()` - Counts elements
- `findFirst()`, `findAny()` - Finds elements
- `anyMatch()`, `allMatch()`, `noneMatch()` - Matching operations

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Terminal operations
long count = names.stream().count(); // Terminal
names.stream().forEach(System.out::println); // Terminal
Optional<String> first = names.stream().findFirst(); // Terminal
```

### Answer 12: Where we can use flatMap()?

**flatMap()** is used when you have nested structures and want to flatten them into a single stream.

**Use cases:**
- List of Lists → Single List
- Optional of Optional → Single Optional
- Stream of Streams → Single Stream

```java
// List of Lists
List<List<String>> listOfLists = Arrays.asList(
    Arrays.asList("a", "b"),
    Arrays.asList("c", "d"),
    Arrays.asList("e", "f")
);

List<String> flattened = listOfLists.stream()
    .flatMap(List::stream) // Flattens nested lists
    .collect(Collectors.toList()); // [a, b, c, d, e, f]

// String to characters
List<String> words = Arrays.asList("Hello", "World");
List<Character> chars = words.stream()
    .flatMap(word -> word.chars().mapToObj(c -> (char) c))
    .collect(Collectors.toList());
```

### Answer 13: Code using Stream API to sort a list of strings based on their length

```java
import java.util.*;
import java.util.stream.Collectors;

public class StringSorting {
    public static void main(String[] args) {
        List<String> strings = Arrays.asList("apple", "hi", "banana", "a", "watermelon");
        
        // Sort by length (ascending)
        List<String> sortedByLength = strings.stream()
            .sorted(Comparator.comparing(String::length))
            .collect(Collectors.toList());
        // Result: [a, hi, apple, banana, watermelon]
        
        // Sort by length (descending)
        List<String> sortedByLengthDesc = strings.stream()
            .sorted(Comparator.comparing(String::length).reversed())
            .collect(Collectors.toList());
        // Result: [watermelon, banana, apple, hi, a]
        
        // Sort by length, then alphabetically
        List<String> sortedByLengthThenAlpha = strings.stream()
            .sorted(Comparator.comparing(String::length)
                   .thenComparing(String::compareTo))
            .collect(Collectors.toList());
    }
}
```

## 🔹 Database Concepts

### Answer 14: Max limit of varchar, and difference between bytes vs characters

**VARCHAR limits:**
- **MySQL**: 65,535 bytes (characters depend on charset)
- **PostgreSQL**: ~1GB (1,073,741,824 characters)
- **SQL Server**: 8,000 bytes (VARCHAR), 2GB (VARCHAR(MAX))
- **Oracle**: 4,000 bytes (VARCHAR2)

**Bytes vs Characters:**
- **Byte**: Basic unit of data (8 bits)
- **Character**: Logical unit that may require 1-4 bytes depending on encoding
- **UTF-8**: 1 byte for ASCII, 2-4 bytes for other characters
- **UTF-16**: 2-4 bytes per character

```sql
-- Example in MySQL
-- With UTF-8, one character can be 1-3 bytes
VARCHAR(255) -- Can store 255 characters, but byte limit varies
```

### Answer 15: Difference between 2NF and 3NF (Normalization)

**Second Normal Form (2NF):**
- Must be in 1NF
- No partial dependencies (non-key attributes fully depend on entire primary key)
- Eliminates redundancy from composite keys

**Third Normal Form (3NF):**
- Must be in 2NF
- No transitive dependencies (non-key attributes don't depend on other non-key attributes)
- Eliminates redundancy from functional dependencies

```sql
-- Example violating 2NF
StudentCourse (StudentID, CourseID, StudentName, CourseName, Grade)
-- StudentName depends only on StudentID (partial dependency)

-- 2NF Solution
Student (StudentID, StudentName)
Course (CourseID, CourseName)
Enrollment (StudentID, CourseID, Grade)

-- Example violating 3NF
Employee (EmpID, EmpName, DeptID, DeptName, DeptLocation)
-- DeptName and DeptLocation depend on DeptID (transitive dependency)

-- 3NF Solution
Employee (EmpID, EmpName, DeptID)
Department (DeptID, DeptName, DeptLocation)
```

### Answer 16: Query to find top 10 employee details having salary > average salary

```sql
-- Method 1: Using subquery
SELECT TOP 10 *
FROM Employee
WHERE Salary > (SELECT AVG(Salary) FROM Employee)
ORDER BY Salary DESC;

-- Method 2: Using window function (more efficient)
WITH AvgSalary AS (
    SELECT AVG(Salary) AS avg_sal FROM Employee
),
HighEarners AS (
    SELECT e.*, a.avg_sal
    FROM Employee e
    CROSS JOIN AvgSalary a
    WHERE e.Salary > a.avg_sal
)
SELECT TOP 10 *
FROM HighEarners
ORDER BY Salary DESC;

-- For MySQL/PostgreSQL (using LIMIT)
SELECT *
FROM Employee
WHERE Salary > (SELECT AVG(Salary) FROM Employee)
ORDER BY Salary DESC
LIMIT 10;
```

### Answer 17: Why is searching by primary key faster in databases?

**Primary key search is faster because:**

1. **Unique Index**: Primary key automatically creates a unique clustered index
2. **Clustered Storage**: Data is physically organized by primary key order
3. **B-Tree Structure**: Index uses balanced tree for O(log n) search complexity
4. **No Duplicates**: Unique constraint eliminates duplicate checks
5. **Memory Optimization**: Index is loaded in memory for faster access

```sql
-- Fast search (uses primary key index)
SELECT * FROM Employee WHERE EmpID = 123;

-- Slower search (may require full table scan)
SELECT * FROM Employee WHERE EmpName = 'John';

-- To make name search faster, create index
CREATE INDEX idx_emp_name ON Employee(EmpName);
```

## 🔹 Design Patterns & System Design

### Answer 18: Singleton design pattern

**Singleton ensures only one instance of a class exists globally.**

```java
// Thread-safe Singleton (Bill Pugh Solution)
public class Singleton {
    private Singleton() {}
    
    private static class SingletonHelper {
        private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}

// Enum Singleton (Best approach)
public enum SingletonEnum {
    INSTANCE;
    
    public void doSomething() {
        // Business logic
    }
}

// Double-Checked Locking
public class ThreadSafeSingleton {
    private static volatile ThreadSafeSingleton instance;
    
    private ThreadSafeSingleton() {}
    
    public static ThreadSafeSingleton getInstance() {
        if (instance == null) {
            synchronized (ThreadSafeSingleton.class) {
                if (instance == null) {
                    instance = new ThreadSafeSingleton();
                }
            }
        }
        return instance;
    }
}
```

### Answer 19: Factory design pattern

**Factory pattern creates objects without specifying their exact class.**

```java
// Product interface
interface Shape {
    void draw();
}

// Concrete products
class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Circle");
    }
}

class Rectangle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing Rectangle");
    }
}

// Factory
class ShapeFactory {
    public static Shape createShape(String type) {
        switch (type.toLowerCase()) {
            case "circle":
                return new Circle();
            case "rectangle":
                return new Rectangle();
            default:
                throw new IllegalArgumentException("Unknown shape: " + type);
        }
    }
}

// Usage
public class FactoryDemo {
    public static void main(String[] args) {
        Shape circle = ShapeFactory.createShape("circle");
        circle.draw(); // Drawing Circle
    }
}
```

### Answer 20: Decomposition design pattern in microservices

**Decomposition patterns break down monolithic applications into microservices.**

**Types of Decomposition:**

1. **Decompose by Business Capability**
   - Services based on business functions
   - Example: OrderService, PaymentService, InventoryService

2. **Decompose by Subdomain (DDD)**
   - Based on Domain-Driven Design
   - Example: User Management, Order Processing, Billing

3. **Decompose by Transaction**
   - Services handling specific transactions
   - Example: CreateOrder, ProcessPayment, UpdateInventory

```
Monolith E-commerce App
├── User Management
├── Product Catalog
├── Order Processing
├── Payment
└── Inventory

Decomposed Microservices:
├── User Service (Registration, Authentication)
├── Catalog Service (Product information)
├── Order Service (Order management)
├── Payment Service (Payment processing)
└── Inventory Service (Stock management)
```

### Answer 21: Challenging aspects of microservices vs monolith

**Microservices Challenges:**

| Challenge | Description | Solution |
|-----------|-------------|----------|
| **Network Latency** | Service-to-service calls over network | Caching, async communication |
| **Data Consistency** | Distributed transactions complexity | Eventual consistency, Saga pattern |
| **Service Discovery** | Finding and connecting services | Service registry (Eureka, Consul) |
| **Monitoring** | Tracking across multiple services | Distributed tracing (Zipkin, Jaeger) |
| **Testing** | Integration testing complexity | Contract testing, service virtualization |
| **Deployment** | Managing multiple deployments | CI/CD pipelines, containerization |

**Monolith Advantages:**
- Simpler deployment and testing
- ACID transactions
- Better performance (no network calls)
- Easier debugging

### Answer 22: How microservices handle logs and health checks?

**Centralized Logging:**
```java
// Using SLF4J with correlation ID
@RestController
public class OrderController {
    private static final Logger logger = LoggerFactory.getLogger(OrderController.class);
    
    @PostMapping("/orders")
    public ResponseEntity<Order> createOrder(@RequestBody Order order) {
        String correlationId = UUID.randomUUID().toString();
        MDC.put("correlationId", correlationId);
        
        logger.info("Creating order: {}", order.getId());
        // Business logic
        logger.info("Order created successfully: {}", order.getId());
        
        MDC.clear();
        return ResponseEntity.ok(order);
    }
}
```

**Health Checks:**
```java
// Spring Boot Actuator
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    @Autowired
    private DataSource dataSource;
    
    @Override
    public Health health() {
        try {
            Connection connection = dataSource.getConnection();
            connection.close();
            return Health.up()
                .withDetail("database", "Available")
                .build();
        } catch (Exception e) {
            return Health.down()
                .withDetail("database", "Unavailable")
                .withException(e)
                .build();
        }
    }
}

// Configuration
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics
  health:
    show-details: always
```

**Log Aggregation Stack:**
- **ELK Stack**: Elasticsearch, Logstash, Kibana
- **Fluentd**: Log collector and forwarder
- **Splunk**: Enterprise log management

### Answer 23: Give a real-world scenario to use Platform.runLater() (JavaFX platform usage)

**Platform.runLater()** schedules a task to run on the JavaFX Application Thread.

**Real-world scenario: File Upload Progress with UI Update**

```java
public class FileUploadService {
    @FXML
    private ProgressBar progressBar;
    @FXML
    private Label statusLabel;
    
    public void uploadFile(File file) {
        Task<Void> uploadTask = new Task<Void>() {
            @Override
            protected Void call() throws Exception {
                long totalBytes = file.length();
                long uploadedBytes = 0;
                
                // Simulate file upload
                try (FileInputStream fis = new FileInputStream(file)) {
                    byte[] buffer = new byte[1024];
                    int bytesRead;
                    
                    while ((bytesRead = fis.read(buffer)) != -1) {
                        // Simulate network upload
                        Thread.sleep(10);
                        uploadedBytes += bytesRead;
                        
                        final double progress = (double) uploadedBytes / totalBytes;
                        final long uploaded = uploadedBytes;
                        
                        // Update UI on JavaFX thread
                        Platform.runLater(() -> {
                            progressBar.setProgress(progress);
                            statusLabel.setText(
                                String.format("Uploaded: %d/%d bytes (%.1f%%)", 
                                uploaded, totalBytes, progress * 100)
                            );
                        });
                    }
                }
                
                // Final UI update
                Platform.runLater(() -> {
                    statusLabel.setText("Upload completed!");
                    showSuccessNotification();
                });
                
                return null;
            }
        };
        
        // Run upload task in background thread
        new Thread(uploadTask).start();
    }
    
    private void showSuccessNotification() {
        Alert alert = new Alert(Alert.AlertType.INFORMATION);
        alert.setTitle("Upload Complete");
        alert.setContentText("File uploaded successfully!");
        alert.show();
    }
}
```

## 🔹 Spring / Spring Boot Concepts

### Answer 24: Difference between @Bean and @Component

| Aspect | @Component | @Bean |
|--------|------------|--------|
| **Usage** | Class-level annotation | Method-level annotation |
| **Auto-detection** | Detected by component scanning | Manually defined in @Configuration |
| **Control** | Less control over instantiation | Full control over object creation |
| **Third-party** | Cannot be used for external libraries | Can be used for external libraries |

```java
// @Component - Spring manages the entire lifecycle
@Component
public class EmailService {
    public void sendEmail(String message) {
        // Implementation
    }
}

// @Bean - Manual control over instantiation
@Configuration
public class AppConfig {
    
    @Bean
    public DataSource dataSource() {
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/mydb");
        dataSource.setUsername("user");
        dataSource.setPassword("password");
        return dataSource;
    }
    
    @Bean
    @Scope("prototype")
    public ReportGenerator reportGenerator() {
        return new ReportGenerator();
    }
}
```

### Answer 25: Managing logging in Spring applications

**Configuration approaches:**

**1. application.yml/properties:**
```yaml
logging:
  level:
    com.example.myapp: DEBUG
    org.springframework: INFO
    org.hibernate: DEBUG
  pattern:
    console: "%d{yyyy-MM-dd HH:mm:ss} - %msg%n"
    file: "%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n"
  file:
    name: application.log
    max-size: 10MB
    max-history: 30
```

**2. Logback configuration (logback-spring.xml):**
```xml
<configuration>
    <springProfile name="dev">
        <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
            <encoder>
                <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
            </encoder>
        </appender>
        <root level="DEBUG">
            <appender-ref ref="CONSOLE"/>
        </root>
    </springProfile>
    
    <springProfile name="prod">
        <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
            <file>logs/application.log</file>
            <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
                <fileNamePattern>logs/application.%d{yyyy-MM-dd}.log</fileNamePattern>
                <maxHistory>30</maxHistory>
            </rollingPolicy>
            <encoder>
                <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
            </encoder>
        </appender>
        <root level="INFO">
            <appender-ref ref="FILE"/>
        </root>
    </springProfile>
</configuration>
```

**3. Programmatic logging:**
```java
@RestController
@Slf4j // Lombok annotation
public class UserController {
    
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        log.info("Fetching user with ID: {}", id);
        
        try {
            User user = userService.findById(id);
            log.debug("User found: {}", user);
            return ResponseEntity.ok(user);
        } catch (UserNotFoundException e) {
            log.error("User not found with ID: {}", id, e);
            return ResponseEntity.notFound().build();
        }
    }
}
```

### Answer 26: Difference between @ResponseBody, @Inject, and @RequestParam

| Annotation | Purpose | Usage |
|------------|---------|--------|
| **@ResponseBody** | Converts return value to HTTP response body | Method/Class level |
| **@Inject** | Dependency injection (JSR-330 standard) | Field/Constructor/Method |
| **@RequestParam** | Binds HTTP request parameters to method parameters | Method parameter |

```java
@RestController // Contains @ResponseBody
public class UserController {
    
    @Inject // or @Autowired
    private UserService userService;
    
    @GetMapping("/users")
    @ResponseBody // Not needed with @RestController
    public List<User> getUsers(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size,
        @RequestParam(required = false) String name
    ) {
        return userService.findUsers(page, size, name);
    }
    
    // For form data
    @PostMapping("/users/search")
    public ResponseEntity<List<User>> searchUsers(
        @RequestParam Map<String, String> searchParams
    ) {
        return ResponseEntity.ok(userService.search(searchParams));
    }
}

// @Inject vs @Autowired
@Service
public class UserService {
    
    // JSR-330 standard - more portable
    @Inject
    private UserRepository userRepository;
    
    // Spring-specific
    @Autowired
    private EmailService emailService;
}
```

### Answer 27: Spring REST Security and securing endpoints

**Spring Security Configuration:**

```java
@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class SecurityConfig {
    
    @Autowired
    private JwtAuthenticationEntryPoint jwtAuthenticationEntryPoint;
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    
    @Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/api/auth/**").permitAll()
                .requestMatchers(HttpMethod.GET, "/api/public/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .requestMatchers("/api/user/**").hasAnyRole("USER", "ADMIN")
                .anyRequest().authenticated()
            )
            .sessionManagement(session -> session
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
            )
            .exceptionHandling(ex -> ex
                .authenticationEntryPoint(jwtAuthenticationEntryPoint)
            );
            
        http.addFilterBefore(jwtRequestFilter, UsernamePasswordAuthenticationFilter.class);
        
        return http.build();
    }
}

// Method-level security
@RestController
@RequestMapping("/api")
public class SecureController {
    
    @GetMapping("/admin/users")
    @PreAuthorize("hasRole('ADMIN')")
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @GetMapping("/user/profile")
    @PreAuthorize("hasRole('USER') or hasRole('ADMIN')")
    public User getCurrentUser(Authentication authentication) {
        return userService.findByUsername(authentication.getName());
    }
    
    @PostMapping("/user/{id}/delete")
    @PreAuthorize("hasRole('ADMIN') or #id == authentication.principal.id")
    public ResponseEntity<?> deleteUser(@PathVariable Long id, Authentication auth) {
        userService.deleteUser(id);
        return ResponseEntity.ok().build();
    }
}

// JWT Token Configuration
@Component
public class JwtTokenUtil {
    private String secret = "mySecretKey";
    private int jwtExpiration = 86400; // 24 hours
    
    public String generateToken(UserDetails userDetails) {
        Map<String, Object> claims = new HashMap<>();
        return createToken(claims, userDetails.getUsername());
    }
    
    private String createToken(Map<String, Object> claims, String subject) {
        return Jwts.builder()
            .setClaims(claims)
            .setSubject(subject)
            .setIssuedAt(new Date(System.currentTimeMillis()))
            .setExpiration(new Date(System.currentTimeMillis() + jwtExpiration * 1000))
            .signWith(SignatureAlgorithm.HS512, secret)
            .compact();
    }
}
```

### Answer 28: Difference between PUT and PATCH HTTP methods

| Aspect | PUT | PATCH |
|--------|-----|-------|
| **Purpose** | Complete resource replacement | Partial resource update |
| **Idempotent** | Yes | No (usually) |
| **Payload** | Complete resource representation | Only fields to update |
| **Safe** | No | No |

```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    // PUT - Complete replacement
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(
        @PathVariable Long id, 
        @RequestBody User user
    ) {
        // Replace entire user object
        user.setId(id);
        User updatedUser = userService.replaceUser(user);
        return ResponseEntity.ok(updatedUser);
    }
    
    // PATCH - Partial update
    @PatchMapping("/{id}")
    public ResponseEntity<User> patchUser(
        @PathVariable Long id,
        @RequestBody Map<String, Object> updates
    ) {
        User updatedUser = userService.partialUpdate(id, updates);
        return ResponseEntity.ok(updatedUser);
    }
}

// Service implementation
@Service
public class UserService {
    
    public User replaceUser(User user) {
        // Complete replacement - all fields updated
        return userRepository.save(user);
    }
    
    public User partialUpdate(Long id, Map<String, Object> updates) {
        User existingUser = userRepository.findById(id)
            .orElseThrow(() -> new UserNotFoundException(id));
            
        // Update only provided fields
        updates.forEach((key, value) -> {
            switch (key) {
                case "name":
                    existingUser.setName((String) value);
                    break;
                case "email":
                    existingUser.setEmail((String) value);
                    break;
                case "age":
                    existingUser.setAge((Integer) value);
                    break;
            }
        });
        
        return userRepository.save(existingUser);
    }
}

// Example requests:
// PUT /api/users/1
// {
//   "name": "John Doe",
//   "email": "john@example.com",
//   "age": 30,
//   "address": "123 Main St"
// }

// PATCH /api/users/1
// {
//   "email": "newemail@example.com"
// }
```

### Answer 29: Difference between JDBC and Spring JPA

| Aspect | JDBC | Spring JPA |
|--------|------|------------|
| **Abstraction Level** | Low-level database access | High-level ORM abstraction |
| **Code Amount** | More boilerplate code | Less boilerplate code |
| **SQL Writing** | Manual SQL queries | Auto-generated queries |
| **Object Mapping** | Manual mapping | Automatic ORM mapping |
| **Transaction Management** | Manual | Declarative (@Transactional) |

**JDBC Example:**
```java
@Repository
public class UserRepositoryJdbc {
    
    @Autowired
    private JdbcTemplate jdbcTemplate;
    
    public User findById(Long id) {
        String sql = "SELECT * FROM users WHERE id = ?";
        return jdbcTemplate.queryForObject(sql, new Object[]{id}, 
            (rs, rowNum) -> {
                User user = new User();
                user.setId(rs.getLong("id"));
                user.setName(rs.getString("name"));
                user.setEmail(rs.getString("email"));
                return user;
            });
    }
    
    public void save(User user) {
        String sql = "INSERT INTO users (name, email) VALUES (?, ?)";
        jdbcTemplate.update(sql, user.getName(), user.getEmail());
    }
    
    public List<User> findAll() {
        String sql = "SELECT * FROM users";
        return jdbcTemplate.query(sql, (rs, rowNum) -> {
            User user = new User();
            user.setId(rs.getLong("id"));
            user.setName(rs.getString("name"));
            user.setEmail(rs.getString("email"));
            return user;
        });
    }
}
```

**Spring JPA Example:**
```java
// Entity
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    @Column(unique = true, nullable = false)
    private String email;
    
    // getters and setters
}

// Repository
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    
    // Auto-generated methods: save(), findById(), findAll(), deleteById()
    
    // Custom query methods
    List<User> findByName(String name);
    Optional<User> findByEmail(String email);
    
    @Query("SELECT u FROM User u WHERE u.name LIKE %:name%")
    List<User> findByNameContaining(@Param("name") String name);
    
    @Modifying
    @Query("UPDATE User u SET u.email = :email WHERE u.id = :id")
    int updateEmailById(@Param("id") Long id, @Param("email") String email);
}

// Service
@Service
@Transactional
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    public User save(User user) {
        return userRepository.save(user); // Auto-generated SQL
    }
    
    public Optional<User> findById(Long id) {
        return userRepository.findById(id);
    }
    
    public List<User> findAll() {
        return userRepository.findAll();
    }
}
```

### Answer 30: Cascade types in JPA – what are they, what is default?

**Cascade types define how operations are propagated from parent to child entities.**

**Cascade Types:**

```java
@Entity
public class Department {
    @Id
    @GeneratedValue
    private Long id;
    
    // Different cascade types
    @OneToMany(mappedBy = "department", cascade = CascadeType.ALL)
    private List<Employee> employees;
    
    @OneToOne(cascade = {CascadeType.PERSIST, CascadeType.MERGE})
    private Address address;
}
```

| Cascade Type | Description | When Applied |
|--------------|-------------|--------------|
| **PERSIST** | Save child when parent is saved | em.persist(parent) |
| **MERGE** | Update child when parent is updated | em.merge(parent) |
| **REMOVE** | Delete child when parent is deleted | em.remove(parent) |
| **REFRESH** | Refresh child when parent is refreshed | em.refresh(parent) |
| **DETACH** | Detach child when parent is detached | em.detach(parent) |
| **ALL** | Apply all cascade types | All operations |

**Default Cascade**: **NONE** - No operations are cascaded by default.

**Examples:**
```java
@Entity
public class Order {
    @Id
    @GeneratedValue
    private Long id;
    
    // When order is saved, order items are also saved
    @OneToMany(mappedBy = "order", cascade = CascadeType.PERSIST)
    private List<OrderItem> orderItems;
    
    // When order is deleted, order items are also deleted
    @OneToMany(mappedBy = "order", cascade = CascadeType.REMOVE)
    private List<OrderItem> orderItems;
    
    // All operations cascaded
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL)
    private List<OrderItem> orderItems;
    
    // Multiple specific cascades
    @OneToMany(mappedBy = "order", 
               cascade = {CascadeType.PERSIST, CascadeType.MERGE, CascadeType.REMOVE})
    private List<OrderItem> orderItems;
}

// Usage example
@Service
@Transactional
public class OrderService {
    
    public void createOrder(Order order) {
        // With CASCADE.PERSIST, saving order also saves all orderItems
        orderRepository.save(order);
    }
    
    public void deleteOrder(Long orderId) {
        Order order = orderRepository.findById(orderId).orElseThrow();
        // With CASCADE.REMOVE, deleting order also deletes all orderItems
        orderRepository.delete(order);
    }
}
```

### Answer 31: Explain @CrossOrigin annotation

**@CrossOrigin enables Cross-Origin Resource Sharing (CORS)** to allow requests from different domains.

**CORS Problem:**
- Browsers block requests from different origins (protocol + domain + port)
- Example: Frontend on `http://localhost:3000` calling API on `http://localhost:8080`

```java
// Method level
@RestController
@RequestMapping("/api")
public class UserController {
    
    @CrossOrigin(origins = "http://localhost:3000")
    @GetMapping("/users")
    public List<User> getUsers() {
        return userService.findAll();
    }
    
    @CrossOrigin(
        origins = {"http://localhost:3000", "https://myapp.com"},
        methods = {RequestMethod.GET, RequestMethod.POST},
        allowedHeaders = {"Content-Type", "Authorization"},
        exposedHeaders = {"X-Total-Count"},
        allowCredentials = "true",
        maxAge = 3600
    )
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
}

// Class level
@RestController
@CrossOrigin(origins = "http://localhost:3000")
@RequestMapping("/api/products")
public class ProductController {
    // All methods in this controller allow CORS from localhost:3000
}

// Global CORS configuration
@Configuration
public class CorsConfig {
    
    @Bean
    public CorsConfigurationSource corsConfigurationSource() {
        CorsConfiguration configuration = new CorsConfiguration();
        configuration.setAllowedOriginPatterns(Arrays.asList("*"));
        configuration.setAllowedMethods(Arrays.asList("GET", "POST", "PUT", "DELETE", "OPTIONS"));
        configuration.setAllowedHeaders(Arrays.asList("*"));
        configuration.setAllowCredentials(true);
        configuration.setMaxAge(3600L);
        
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        source.registerCorsConfiguration("/**", configuration);
        return source;
    }
}

// Or using WebMvcConfigurer
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/api/**")
                .allowedOrigins("http://localhost:3000", "https://myapp.com")
                .allowedMethods("GET", "POST", "PUT", "DELETE")
                .allowedHeaders("*")
                .allowCredentials(true)
                .maxAge(3600);
    }
}
```

**@CrossOrigin Parameters:**

| Parameter | Description | Default |
|-----------|-------------|---------|
| **origins** | Allowed origin URLs | `{}` (none) |
| **originPatterns** | Origin patterns with wildcards | `{}` |
| **methods** | Allowed HTTP methods | All |
| **allowedHeaders** | Allowed request headers | All |
| **exposedHeaders** | Headers exposed to client | `{}` |
| **allowCredentials** | Allow cookies/credentials | `false` |
| **maxAge** | Preflight cache duration (seconds) | 1800 |

### Answer 32: What is the endpoint of a curator? (Apache Curator / Zookeeper)

**Apache Curator is a ZooKeeper client library** that provides higher-level abstractions and utilities.

**Curator doesn't have "endpoints" in the REST sense** - it's a Java library that connects to ZooKeeper clusters.

**ZooKeeper Connection:**
```java
@Configuration
public class CuratorConfig {
    
    @Bean
    public CuratorFramework curatorFramework() {
        CuratorFramework client = CuratorFrameworkFactory.newClient(
            "localhost:2181", // ZooKeeper connection string
            new ExponentialBackoffRetry(1000, 3)
        );
        client.start();
        return client;
    }
}

// Service Discovery with Curator
@Service
public class ServiceDiscovery {
    
    @Autowired
    private CuratorFramework curatorFramework;
    
    private ServiceDiscovery<ServiceInstance<Void>> serviceDiscovery;
    
    @PostConstruct
    public void init() throws Exception {
        serviceDiscovery = ServiceDiscoveryBuilder.builder(Void.class)
            .client(curatorFramework)
            .basePath("/services")
            .build();
        serviceDiscovery.start();
    }
    
    // Register service
    public void registerService(String serviceName, String host, int port) throws Exception {
        ServiceInstance<Void> serviceInstance = ServiceInstance.<Void>builder()
            .name(serviceName)
            .address(host)
            .port(port)
            .build();
        
        serviceDiscovery.registerService(serviceInstance);
    }
    
    // Discover services
    public Collection<ServiceInstance<Void>> getInstances(String serviceName) throws Exception {
        return serviceDiscovery.queryForInstances(serviceName);
    }
}

// Distributed Lock with Curator
@Service
public class DistributedLockService {
    
    @Autowired
    private CuratorFramework curatorFramework;
    
    public void executeWithLock(String lockPath, Runnable task) {
        InterProcessMutex lock = new InterProcessMutex(curatorFramework, lockPath);
        try {
            if (lock.acquire(10, TimeUnit.SECONDS)) {
                try {
                    task.run();
                } finally {
                    lock.release();
                }
            } else {
                throw new RuntimeException("Could not acquire lock");
            }
        } catch (Exception e) {
            throw new RuntimeException("Error with distributed lock", e);
        }
    }
}

// Configuration Management
@Service
public class ConfigurationService {
    
    @Autowired
    private CuratorFramework curatorFramework;
    
    public void setConfig(String path, String value) throws Exception {
        if (curatorFramework.checkExists().forPath(path) == null) {
            curatorFramework.create()
                .creatingParentsIfNeeded()
                .forPath(path, value.getBytes());
        } else {
            curatorFramework.setData().forPath(path, value.getBytes());
        }
    }
    
    public String getConfig(String path) throws Exception {
        byte[] data = curatorFramework.getData().forPath(path);
        return new String(data);
    }
    
    // Watch for configuration changes
    public void watchConfig(String path, ConfigChangeListener listener) throws Exception {
        PathChildrenCache cache = new PathChildrenCache(curatorFramework, path, true);
        cache.getListenable().addListener(new PathChildrenCacheListener() {
            @Override
            public void childEvent(CuratorFramework client, PathChildrenCacheEvent event) throws Exception {
                listener.onConfigChange(event);
            }
        });
        cache.start();
    }
}
```

**ZooKeeper Connection Endpoints:**
- **Default ZooKeeper port**: 2181
- **Connection string examples**:
  - Single node: `localhost:2181`
  - Cluster: `zk1:2181,zk2:2181,zk3:2181`
  - With chroot: `localhost:2181/myapp`

## 🔹 General Java Runtime / JVM

### Answer 33: What is System.exit()? How does it work?

**System.exit()** terminates the currently running Java Virtual Machine.

**Syntax:**
```java
public static void exit(int status)
```

**Exit Status Codes:**
- **0**: Normal termination (success)
- **Non-zero**: Abnormal termination (error)

**How it works:**
1. Calls `Runtime.getRuntime().exit(status)`
2. Triggers shutdown hooks (if any)
3. Runs finalizers (if finalization-on-exit is enabled)
4. Terminates the JVM
5. Returns control to the operating system

```java
public class SystemExitDemo {
    
    public static void main(String[] args) {
        
        // Add shutdown hook
        Runtime.getRuntime().addShutdownHook(new Thread(() -> {
            System.out.println("Shutdown hook executed - cleaning up resources");
            // Cleanup code here
        }));
        
        try {
            // Some application logic
            processData();
            
            // Normal exit
            System.out.println("Application completed successfully");
            System.exit(0); // Success
            
        } catch (Exception e) {
            System.err.println("Application failed: " + e.getMessage());
            System.exit(1); // Error
        }
    }
    
    private static void processData() throws Exception {
        // Simulate some work
        System.out.println("Processing data...");
        
        // Simulate error condition
        boolean errorOccurred = false;
        if (errorOccurred) {
            throw new Exception("Data processing failed");
        }
    }
}

// Example with different exit codes
public class ExitCodeExample {
    
    public static final int SUCCESS = 0;
    public static final int INVALID_ARGUMENTS = 1;
    public static final int FILE_NOT_FOUND = 2;
    public static final int NETWORK_ERROR = 3;
    
    public static void main(String[] args) {
        try {
            if (args.length == 0) {
                System.err.println("Usage: java ExitCodeExample <filename>");
                System.exit(INVALID_ARGUMENTS);
            }
            
            String filename = args[0];
            if (!new File(filename).exists()) {
                System.err.println("File not found: " + filename);
                System.exit(FILE_NOT_FOUND);
            }
            
            // Process file
            System.out.println("File processed successfully");
            System.exit(SUCCESS);
            
        } catch (Exception e) {
            System.err.println("Unexpected error: " + e.getMessage());
            System.exit(-1); // General error
        }
    }
}

// Security Manager considerations
public class SecurityManagerExample {
    
    public static void main(String[] args) {
        // Set security manager
        System.setSecurityManager(new SecurityManager() {
            @Override
            public void checkExit(int status) {
                if (status != 0) {
                    throw new SecurityException("Exit with non-zero status not allowed");
                }
            }
        });
        
        try {
            System.exit(1); // Will throw SecurityException
        } catch (SecurityException e) {
            System.out.println("Exit blocked by security manager: " + e.getMessage());
        }
    }
}
```

**Important Notes:**
- **Use sparingly**: Calling `System.exit()` terminates the entire JVM
- **Spring Boot**: Prefer `SpringApplication.exit()` for graceful shutdown
- **Web applications**: Should not call `System.exit()` in servlets/controllers
- **Testing**: Can make unit tests difficult (use dependency injection for testability)
- **Shutdown hooks**: Will be executed before JVM termination
- **Security**: Security managers can prevent `System.exit()` calls

**Alternatives to System.exit():**
```java
// For Spring Boot applications
@Autowired
private ApplicationContext applicationContext;

public void shutdown() {
    SpringApplication.exit(applicationContext, () -> 0);
}

// For returning from main method
public static void main(String[] args) {
    if (args.length == 0) {
        System.err.println("No arguments provided");
        return; // Instead of System.exit(1)
    }
    // Continue processing
}

// For throwing exceptions
public void processFile(String filename) throws ProcessingException {
    if (!new File(filename).exists()) {
        throw new ProcessingException("File not found: " + filename);
    }
    // Process file
}
```

---

*These answers cover all the Day 1 interview questions with comprehensive explanations, code examples, and best practices. Each answer is structured to provide both theoretical understanding and practical implementation details.*
