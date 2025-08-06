# Spring Core, IoC & Dependency Injection - Interview Questions & Answers

## 🎯 **Essential Questions for 2+ Years Experience**

---

## **1. What is Spring Framework and what are its key features?**

**Answer:**
Spring Framework is a comprehensive, lightweight framework for building enterprise Java applications. It provides infrastructure support so developers can focus on business logic.

**Key Features:**
- **Inversion of Control (IoC)** - Container manages object creation and lifecycle
- **Dependency Injection (DI)** - Dependencies are injected rather than created
- **Aspect-Oriented Programming (AOP)** - Separation of cross-cutting concerns
- **Lightweight** - Minimal overhead, POJO-based development
- **Modular** - Use only what you need
- **Integration** - Seamless integration with other frameworks
- **Transaction Management** - Comprehensive transaction support
- **MVC Framework** - Flexible web application development

**Real-world Example:**
```java
// Without Spring - Manual dependency management
public class OrderService {
    private PaymentService paymentService = new PaymentService(); // Tight coupling
    private InventoryService inventoryService = new InventoryService();
}

// With Spring - Dependencies injected
@Service
public class OrderService {
    @Autowired
    private PaymentService paymentService; // Loose coupling
    
    @Autowired
    private InventoryService inventoryService;
}
```

---

## **2. What is IoC (Inversion of Control) and how does it benefit application development?**

**Answer:**
IoC is a design principle where the control of object creation, configuration, and lifecycle is transferred from the application code to an external container (Spring IoC Container).

**Traditional Approach vs IoC:**
```java
// Traditional Approach - You control object creation
public class OrderService {
    private EmailService emailService;
    
    public OrderService() {
        this.emailService = new EmailService(); // You create the dependency
    }
}

// IoC Approach - Spring controls object creation
@Service
public class OrderService {
    private final EmailService emailService;
    
    public OrderService(EmailService emailService) {
        this.emailService = emailService; // Spring injects the dependency
    }
}
```

**Benefits:**
1. **Loose Coupling** - Classes don't create their dependencies
2. **Easier Testing** - Mock dependencies can be easily injected
3. **Better Maintainability** - Changes in one class don't affect others
4. **Configuration Centralization** - All wiring done in one place
5. **Lifecycle Management** - Container manages object lifecycle

---

## **3. What are the different types of Dependency Injection? Which one is recommended and why?**

**Answer:**
There are three types of Dependency Injection:

### **1. Constructor Injection (Recommended)**
```java
@Service
public class OrderService {
    private final PaymentService paymentService;
    private final InventoryService inventoryService;
    
    // Constructor injection
    public OrderService(PaymentService paymentService, InventoryService inventoryService) {
        this.paymentService = paymentService;
        this.inventoryService = inventoryService;
    }
}
```

### **2. Setter Injection**
```java
@Service
public class OrderService {
    private PaymentService paymentService;
    
    @Autowired
    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

### **3. Field Injection**
```java
@Service
public class OrderService {
    @Autowired
    private PaymentService paymentService; // Not recommended
}
```

**Why Constructor Injection is Recommended:**

| Aspect | Constructor | Setter | Field |
|--------|-------------|--------|-------|
| **Immutability** | ✅ Creates immutable objects | ❌ Mutable | ❌ Mutable |
| **Mandatory Dependencies** | ✅ Clear at compile time | ❌ Optional by default | ❌ Not clear |
| **Testing** | ✅ Easy to test without Spring | ❌ Need setter calls | ❌ Need reflection |
| **Circular Dependencies** | ✅ Prevents circular deps | ❌ Allows circular deps | ❌ Allows circular deps |
| **Null Safety** | ✅ Guaranteed non-null | ❌ Can be null | ❌ Can be null |

---

## **4. What is the difference between BeanFactory and ApplicationContext?**

**Answer:**

| Feature | BeanFactory | ApplicationContext |
|---------|-------------|-------------------|
| **Container Type** | Basic IoC container | Advanced IoC container |
| **Initialization** | Lazy (beans created when requested) | Eager (beans created at startup) |
| **Memory Usage** | Lower memory footprint | Higher memory usage |
| **Event Publishing** | ❌ No support | ✅ Event publishing support |
| **Internationalization** | ❌ No support | ✅ i18n support |
| **Annotation Support** | ❌ Limited | ✅ Full annotation support |
| **AOP Integration** | ❌ Manual setup needed | ✅ Automatic AOP integration |

**Code Examples:**
```java
// BeanFactory usage
BeanFactory factory = new XmlBeanFactory(new FileSystemResource("beans.xml"));
UserService userService = (UserService) factory.getBean("userService");

// ApplicationContext usage (Recommended)
ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
UserService userService = context.getBean(UserService.class);
```

**When to Use Which:**
- **BeanFactory**: Resource-constrained environments, legacy applications
- **ApplicationContext**: Modern applications (recommended for most use cases)

---

## **5. What is circular dependency and how can you resolve it?**

**Answer:**
Circular dependency occurs when two or more beans depend on each other directly or indirectly.

**Example of Circular Dependency:**
```java
@Service
public class OrderService {
    private final CustomerService customerService;
    
    public OrderService(CustomerService customerService) {
        this.customerService = customerService;
    }
}

@Service
public class CustomerService {
    private final OrderService orderService;
    
    public CustomerService(OrderService orderService) { // Circular dependency
        this.orderService = orderService;
    }
}
```

**Solutions:**

### **1. Setter Injection (Most Common)**
```java
@Service
public class OrderService {
    private CustomerService customerService;
    
    @Autowired
    public void setCustomerService(CustomerService customerService) {
        this.customerService = customerService;
    }
}
```

### **2. @Lazy Annotation**
```java
@Service
public class OrderService {
    private final CustomerService customerService;
    
    public OrderService(@Lazy CustomerService customerService) {
        this.customerService = customerService;
    }
}
```

### **3. @PostConstruct**
```java
@Service
public class OrderService {
    @Autowired
    private CustomerService customerService;
    
    private OrderService orderServiceRef;
    
    @PostConstruct
    public void init() {
        customerService.setOrderService(this);
    }
}
```

### **4. Redesign (Best Solution)**
```java
// Extract common functionality to avoid circular dependency
@Service
public class NotificationService {
    public void sendOrderConfirmation(Order order, Customer customer) {
        // Send notification logic
    }
}

@Service
public class OrderService {
    private final NotificationService notificationService;
    // No dependency on CustomerService
}
```

---

## **6. What are Spring Bean Scopes? Explain each with examples.**

**Answer:**

### **Singleton Scope (Default)**
```java
@Service
@Scope("singleton") // or @Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
public class DatabaseService {
    // One instance per Spring container
}
```

### **Prototype Scope**
```java
@Service
@Scope("prototype") // New instance for each request
public class ReportGenerator {
    private String reportData;
    // Each call to getBean() returns new instance
}
```

### **Request Scope (Web-aware)**
```java
@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class UserSession {
    // One instance per HTTP request
}
```

### **Session Scope (Web-aware)**
```java
@Component
@Scope(value = WebApplicationContext.SCOPE_SESSION, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class ShoppingCart {
    // One instance per HTTP session
}
```

### **Application Scope**
```java
@Component
@Scope(value = WebApplicationContext.SCOPE_APPLICATION, proxyMode = ScopedProxyMode.TARGET_CLASS)
public class AppConfiguration {
    // One instance per ServletContext
}
```

**When to Use Which Scope:**
- **Singleton**: Stateless services, DAOs, configuration beans
- **Prototype**: Stateful objects, user-specific data
- **Request**: Request-specific data (user authentication)
- **Session**: User session data (shopping cart, user preferences)
- **Application**: Application-wide configuration

---

## **7. Are Spring Singleton beans thread-safe? How do you make them thread-safe?**

**Answer:**
**No, Spring Singleton beans are NOT thread-safe by default.** Spring only manages the lifecycle and creation of beans, not their thread safety.

**Thread-Safety Issues:**
```java
@Service
public class CounterService {
    private int count = 0; // Shared mutable state - NOT thread-safe
    
    public void increment() {
        count++; // Race condition possible
    }
    
    public int getCount() {
        return count;
    }
}
```

**Solutions for Thread Safety:**

### **1. Make Beans Stateless (Recommended)**
```java
@Service
public class OrderService {
    // No instance variables - stateless and thread-safe
    
    public Order processOrder(OrderRequest request) {
        // Use only method parameters and local variables
        return new Order(request.getCustomerId(), request.getItems());
    }
}
```

### **2. Use Synchronized Methods**
```java
@Service
public class CounterService {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}
```

### **3. Use Concurrent Collections**
```java
@Service
public class CacheService {
    private final ConcurrentHashMap<String, Object> cache = new ConcurrentHashMap<>();
    
    public void put(String key, Object value) {
        cache.put(key, value);
    }
}
```

### **4. Use ThreadLocal**
```java
@Service
public class UserContextService {
    private final ThreadLocal<User> userContext = new ThreadLocal<>();
    
    public void setCurrentUser(User user) {
        userContext.set(user);
    }
    
    public User getCurrentUser() {
        return userContext.get();
    }
}
```

---

## **8. What is @Autowired annotation? How does it work?**

**Answer:**
@Autowired is used for automatic dependency injection. Spring automatically injects the required dependencies.

**How @Autowired Works:**
1. Spring scans for @Autowired annotations
2. Identifies the required dependency type
3. Looks for matching beans in the application context
4. Injects the appropriate bean

**Different Usage Patterns:**

### **1. Constructor Injection (Recommended)**
```java
@Service
public class OrderService {
    private final PaymentService paymentService;
    
    // @Autowired is optional for single constructor since Spring 4.3
    public OrderService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

### **2. Setter Injection**
```java
@Service
public class OrderService {
    private PaymentService paymentService;
    
    @Autowired
    public void setPaymentService(PaymentService paymentService) {
        this.paymentService = paymentService;
    }
}
```

### **3. Field Injection**
```java
@Service
public class OrderService {
    @Autowired
    private PaymentService paymentService;
}
```

### **4. Optional Dependencies**
```java
@Service
public class EmailService {
    @Autowired(required = false)
    private SMSService smsService; // Optional dependency
}
```

### **5. Multiple Candidates - @Qualifier**
```java
public interface PaymentService { }

@Service("creditCardPayment")
public class CreditCardPaymentService implements PaymentService { }

@Service("paypalPayment")
public class PaypalPaymentService implements PaymentService { }

@Service
public class OrderService {
    @Autowired
    @Qualifier("creditCardPayment")
    private PaymentService paymentService;
}
```

---

## **9. What is the difference between @Component, @Service, @Repository, and @Controller?**

**Answer:**
All these annotations are specializations of @Component, but they serve different purposes and provide additional features.

### **@Component - Generic Stereotype**
```java
@Component
public class EmailValidator {
    public boolean isValid(String email) {
        return email.contains("@");
    }
}
```

### **@Service - Business Logic Layer**
```java
@Service
public class OrderService {
    public Order processOrder(OrderRequest request) {
        // Business logic
        return new Order();
    }
}
```

### **@Repository - Data Access Layer**
```java
@Repository
public class UserRepository {
    public User findById(Long id) {
        // Data access logic
        return new User();
    }
}
```
**Special Features of @Repository:**
- Automatic exception translation (DataAccessException)
- Integration with Spring Data JPA

### **@Controller - Presentation Layer**
```java
@Controller
public class UserController {
    @RequestMapping("/users")
    public String listUsers(Model model) {
        return "users";
    }
}
```

**Comparison Table:**

| Annotation | Layer | Purpose | Special Features |
|-----------|-------|---------|------------------|
| @Component | Any | Generic component | Base annotation |
| @Service | Business | Business logic | None (semantic meaning) |
| @Repository | Data Access | Data operations | Exception translation |
| @Controller | Presentation | Web controllers | Web-specific features |

---

## **10. How do you configure Spring without XML (Java Configuration)?**

**Answer:**
Spring supports Java-based configuration using @Configuration and @Bean annotations.

### **Traditional XML Configuration**
```xml
<!-- applicationContext.xml -->
<beans>
    <bean id="userService" class="com.example.UserService">
        <property name="userRepository" ref="userRepository"/>
    </bean>
    
    <bean id="userRepository" class="com.example.UserRepository"/>
</beans>
```

### **Java Configuration Equivalent**
```java
@Configuration
@ComponentScan(basePackages = "com.example")
@EnableJpaRepositories
@EnableTransactionManagement
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
    public JpaTransactionManager transactionManager(EntityManagerFactory emf) {
        return new JpaTransactionManager(emf);
    }
    
    @Bean
    @Primary
    public PaymentService primaryPaymentService() {
        return new CreditCardPaymentService();
    }
    
    @Bean
    @Profile("dev")
    public EmailService devEmailService() {
        return new MockEmailService();
    }
    
    @Bean
    @Profile("prod")
    public EmailService prodEmailService() {
        return new SmtpEmailService();
    }
}
```

### **Main Application Class**
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

// Or without Spring Boot
public class Application {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        UserService userService = context.getBean(UserService.class);
    }
}
```

**Benefits of Java Configuration:**
- Type-safe configuration
- IDE support with auto-completion
- Refactoring support
- Conditional bean creation
- Better integration with modern Spring features

---

## **🎯 Quick Review Questions**

1. **What happens if Spring cannot find a dependency for @Autowired?**
   - Throws `NoSuchBeanDefinitionException` at application startup

2. **Can you inject a Prototype bean into a Singleton bean?**
   - Yes, but the Prototype bean will behave like Singleton. Use @Lookup or Provider<T> to get new instances.

3. **What is @Primary annotation used for?**
   - When multiple beans of the same type exist, @Primary indicates the preferred bean for injection.

4. **How do you create conditional beans?**
   - Use @Conditional, @ConditionalOnProperty, @ConditionalOnClass, etc.

5. **What is the purpose of @PostConstruct and @PreDestroy?**
   - @PostConstruct: Method called after dependency injection
   - @PreDestroy: Method called before bean destruction

---

**🚀 Pro Tips for Interview Success:**
- Always explain the benefits and use cases
- Provide practical examples from your experience
- Understand the underlying principles, not just syntax
- Be prepared to compare different approaches
- Know when to use each pattern/approach
