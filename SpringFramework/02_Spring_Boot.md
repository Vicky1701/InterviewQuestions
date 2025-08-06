# Spring Boot - Interview Questions & Answers

## 🎯 **Essential Questions for 2+ Years Experience**

---

## **1. What is Spring Boot and how is it different from Spring Framework?**

**Answer:**
Spring Boot is an opinionated framework built on top of Spring Framework that simplifies application development through auto-configuration and convention over configuration.

**Key Differences:**

| Aspect | Spring Framework | Spring Boot |
|--------|------------------|-------------|
| **Configuration** | Extensive XML/Java config | Auto-configuration |
| **Dependency Management** | Manual dependency versions | Starter dependencies |
| **Server** | External server required | Embedded server |
| **Deployment** | WAR files to server | Standalone JAR |
| **Development Time** | Longer setup time | Rapid development |
| **Boilerplate Code** | More configuration code | Minimal configuration |

**Example - Setting up Web Application:**

**Spring Framework (Traditional):**
```xml
<!-- web.xml -->
<web-app>
    <servlet>
        <servlet-name>dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    </servlet>
</web-app>

<!-- applicationContext.xml -->
<beans>
    <context:component-scan base-package="com.example"/>
    <mvc:annotation-driven/>
</beans>
```

**Spring Boot:**
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@RestController
public class HelloController {
    @GetMapping("/hello")
    public String hello() {
        return "Hello World!";
    }
}
```

---

## **2. What is @SpringBootApplication annotation? What does it do internally?**

**Answer:**
@SpringBootApplication is a convenience annotation that combines three annotations:

```java
@SpringBootApplication
// Equivalent to:
@Configuration          // Makes the class a configuration class
@EnableAutoConfiguration // Enables Spring Boot's auto-configuration
@ComponentScan          // Enables component scanning
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

**What happens internally:**

1. **@Configuration**: Makes the class a source of bean definitions
2. **@EnableAutoConfiguration**: 
   - Analyzes classpath dependencies
   - Automatically configures beans based on dependencies found
   - Uses conditional annotations (@ConditionalOnClass, @ConditionalOnProperty)
3. **@ComponentScan**: 
   - Scans for @Component, @Service, @Repository, @Controller
   - Default scan: current package and sub-packages

**Detailed Internal Process:**
```java
@SpringBootApplication(
    scanBasePackages = "com.example",  // Custom scan packages
    exclude = {DataSourceAutoConfiguration.class}  // Exclude specific auto-configs
)
public class Application {
    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(Application.class);
        app.setDefaultProperties(Collections.singletonMap("server.port", "8081"));
        app.run(args);
    }
}
```

---

## **3. How does Spring Boot Auto-Configuration work?**

**Answer:**
Auto-configuration automatically configures Spring application based on dependencies present in classpath.

**Auto-Configuration Process:**

1. **Classpath Scanning**: Spring Boot scans META-INF/spring.factories
2. **Conditional Evaluation**: Checks @Conditional annotations
3. **Bean Creation**: Creates beans if conditions are met
4. **Configuration Override**: User configuration takes precedence

**Example Auto-Configuration:**
```java
@Configuration
@ConditionalOnClass({DataSource.class, JdbcTemplate.class})
@ConditionalOnSingleCandidate(DataSource.class)
public class JdbcTemplateAutoConfiguration {
    
    @Bean
    @Primary
    @ConditionalOnMissingBean(JdbcOperations.class)
    public JdbcTemplate jdbcTemplate(DataSource dataSource) {
        return new JdbcTemplate(dataSource);
    }
}
```

**Key Auto-Configuration Annotations:**

| Annotation | Purpose |
|------------|---------|
| @ConditionalOnClass | Bean created only if specified class is present |
| @ConditionalOnMissingBean | Bean created only if no such bean exists |
| @ConditionalOnProperty | Bean created based on property value |
| @ConditionalOnWebApplication | Bean created only in web applications |

**Viewing Auto-Configuration Report:**
```properties
# application.properties
debug=true  # Shows auto-configuration report
```

**Custom Auto-Configuration:**
```java
@Configuration
@ConditionalOnClass(RedisTemplate.class)
@EnableConfigurationProperties(RedisProperties.class)
public class RedisAutoConfiguration {
    
    @Bean
    @ConditionalOnMissingBean
    public RedisTemplate<Object, Object> redisTemplate(LettuceConnectionFactory connectionFactory) {
        RedisTemplate<Object, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);
        return template;
    }
}
```

---

## **4. What are Spring Boot Starters? Why are they useful?**

**Answer:**
Spring Boot Starters are dependency descriptors that bring in all necessary dependencies for a specific functionality.

**Popular Starters:**

| Starter | Purpose | Key Dependencies |
|---------|---------|------------------|
| spring-boot-starter-web | Web applications | Spring MVC, Tomcat, Jackson |
| spring-boot-starter-data-jpa | JPA with Hibernate | Hibernate, Spring Data JPA |
| spring-boot-starter-security | Security | Spring Security |
| spring-boot-starter-test | Testing | JUnit, Mockito, Spring Test |
| spring-boot-starter-actuator | Production monitoring | Actuator endpoints |

**Example - Web Starter:**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

**This single dependency brings:**
- Spring MVC
- Embedded Tomcat
- Jackson for JSON
- Spring Boot Auto-configuration
- Validation framework

**Benefits:**
1. **Simplified Dependency Management**: No need to manage individual versions
2. **Compatible Versions**: All dependencies are tested together
3. **Reduced Configuration**: Works out of the box
4. **Best Practices**: Follows Spring team recommendations

**Creating Custom Starter:**
```java
// auto-configuration
@Configuration
@ConditionalOnProperty(name = "custom.service.enabled", havingValue = "true")
@EnableConfigurationProperties(CustomProperties.class)
public class CustomServiceAutoConfiguration {
    
    @Bean
    @ConditionalOnMissingBean
    public CustomService customService(CustomProperties properties) {
        return new CustomService(properties);
    }
}

// META-INF/spring.factories
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.CustomServiceAutoConfiguration
```

---

## **5. What is the difference between @Component and @Bean?**

**Answer:**

| Aspect | @Component | @Bean |
|--------|------------|-------|
| **Usage** | Class-level annotation | Method-level annotation |
| **Bean Creation** | Spring creates instance automatically | Developer controls instance creation |
| **Configuration** | Used with @ComponentScan | Used with @Configuration |
| **Flexibility** | Limited control over instantiation | Full control over instantiation |
| **Third-party Classes** | Cannot annotate third-party classes | Can create beans for third-party classes |

**@Component Example:**
```java
@Component
public class EmailService {
    private final String apiKey;
    
    public EmailService() {
        this.apiKey = "default-key"; // Limited control
    }
}
```

**@Bean Example:**
```java
@Configuration
public class AppConfig {
    
    @Bean
    public EmailService emailService() {
        EmailService service = new EmailService();
        service.setApiKey(getApiKeyFromVault()); // Full control
        service.setRetryCount(3);
        return service;
    }
    
    @Bean
    @Profile("prod")
    public DataSource prodDataSource() {
        // Third-party class configuration
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setJdbcUrl("jdbc:mysql://prod-server:3306/db");
        return dataSource;
    }
    
    @Bean
    @ConditionalOnProperty("feature.cache.enabled")
    public CacheManager cacheManager() {
        return new ConcurrentMapCacheManager("users", "products");
    }
}
```

**When to Use Which:**
- **@Component**: Your own classes with simple instantiation
- **@Bean**: Complex instantiation logic, third-party classes, conditional creation

---

## **6. What is Spring Boot Actuator? What are its important endpoints?**

**Answer:**
Spring Boot Actuator provides production-ready features like monitoring, metrics, and health checks.

**Adding Actuator:**
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**Important Endpoints:**

| Endpoint | Purpose | Example Response |
|----------|---------|------------------|
| /actuator/health | Application health status | {"status": "UP"} |
| /actuator/metrics | Application metrics | Memory, CPU, HTTP metrics |
| /actuator/info | Application information | Version, build info |
| /actuator/env | Environment properties | All configuration properties |
| /actuator/beans | All Spring beans | Bean names and dependencies |
| /actuator/mappings | Request mappings | All controller mappings |
| /actuator/loggers | Logger configuration | Current log levels |

**Configuration:**
```properties
# application.properties
management.endpoints.web.exposure.include=health,metrics,info
management.endpoint.health.show-details=always
management.info.env.enabled=true

# Custom info
info.app.name=My Application
info.app.version=1.0.0
```

**Custom Health Indicator:**
```java
@Component
public class DatabaseHealthIndicator implements HealthIndicator {
    
    @Autowired
    private DataSource dataSource;
    
    @Override
    public Health health() {
        try (Connection connection = dataSource.getConnection()) {
            if (connection.isValid(1000)) {
                return Health.up()
                    .withDetail("database", "Available")
                    .withDetail("validationQuery", "SELECT 1")
                    .build();
            }
        } catch (Exception e) {
            return Health.down()
                .withDetail("database", "Unavailable")
                .withException(e)
                .build();
        }
        return Health.down().build();
    }
}
```

**Custom Metrics:**
```java
@RestController
public class OrderController {
    
    private final MeterRegistry meterRegistry;
    private final Counter orderCounter;
    
    public OrderController(MeterRegistry meterRegistry) {
        this.meterRegistry = meterRegistry;
        this.orderCounter = Counter.builder("orders.created")
            .description("Number of orders created")
            .register(meterRegistry);
    }
    
    @PostMapping("/orders")
    public ResponseEntity<Order> createOrder(@RequestBody Order order) {
        // Business logic
        orderCounter.increment();
        
        Timer.Sample sample = Timer.start(meterRegistry);
        // Process order
        sample.stop(Timer.builder("order.processing.time")
            .description("Order processing time")
            .register(meterRegistry));
            
        return ResponseEntity.ok(order);
    }
}
```

---

## **7. How do you externalize configuration in Spring Boot?**

**Answer:**
Spring Boot provides multiple ways to externalize configuration with a specific precedence order.

**Configuration Sources (High to Low Priority):**

1. **Command Line Arguments**
2. **Java System Properties**
3. **OS Environment Variables**
4. **Profile-specific properties**
5. **Application properties**

**1. Application Properties:**
```properties
# application.properties
server.port=8080
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.datasource.username=user
spring.datasource.password=password

app.name=My Application
app.version=1.0.0
app.features.cache-enabled=true
```

**2. YAML Configuration:**
```yaml
# application.yml
server:
  port: 8080
  
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: user
    password: password
  profiles:
    active: dev

app:
  name: My Application
  version: 1.0.0
  features:
    cache-enabled: true
    
---
spring:
  profiles: dev
server:
  port: 8081
  
---
spring:
  profiles: prod
server:
  port: 80
```

**3. @ConfigurationProperties:**
```java
@ConfigurationProperties(prefix = "app")
@Component
public class AppProperties {
    private String name;
    private String version;
    private Features features;
    
    // Getters and setters
    
    public static class Features {
        private boolean cacheEnabled;
        private int maxRetries = 3;
        
        // Getters and setters
    }
}

@Service
public class AppService {
    private final AppProperties appProperties;
    
    public AppService(AppProperties appProperties) {
        this.appProperties = appProperties;
    }
    
    public void printConfig() {
        System.out.println("App: " + appProperties.getName());
        System.out.println("Cache enabled: " + appProperties.getFeatures().isCacheEnabled());
    }
}
```

**4. Environment Variables:**
```bash
export SERVER_PORT=8082
export SPRING_DATASOURCE_URL=jdbc:mysql://prod-server:3306/mydb
export APP_FEATURES_CACHE_ENABLED=false
```

**5. Command Line Arguments:**
```bash
java -jar myapp.jar --server.port=8083 --spring.profiles.active=prod
```

**6. Profile-specific Configuration:**
```properties
# application-dev.properties
spring.datasource.url=jdbc:h2:mem:testdb
logging.level.com.example=DEBUG

# application-prod.properties
spring.datasource.url=jdbc:mysql://prod-server:3306/mydb
logging.level.com.example=WARN
```

**7. Using @Value:**
```java
@Service
public class EmailService {
    
    @Value("${app.email.from:noreply@example.com}")
    private String fromEmail;
    
    @Value("${app.email.retry-count:3}")
    private int retryCount;
    
    @Value("#{${app.email.templates}}")
    private Map<String, String> templates;
}
```

---

## **8. What are Spring Profiles and how do you use them?**

**Answer:**
Spring Profiles provide a way to segregate parts of application configuration and make it available only in certain environments.

**Defining Profiles:**

**1. Using @Profile on Beans:**
```java
@Configuration
public class DatabaseConfig {
    
    @Bean
    @Profile("dev")
    public DataSource devDataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.H2)
            .build();
    }
    
    @Bean
    @Profile("prod")
    public DataSource prodDataSource() {
        HikariDataSource dataSource = new HikariDataSource();
        dataSource.setJdbcUrl("jdbc:mysql://prod-server:3306/mydb");
        return dataSource;
    }
    
    @Bean
    @Profile("!prod") // NOT prod profile
    public DataSource testDataSource() {
        return new EmbeddedDatabaseBuilder()
            .setType(EmbeddedDatabaseType.H2)
            .build();
    }
}
```

**2. Profile-specific Configuration Files:**
```properties
# application.properties
spring.profiles.active=dev

# application-dev.properties
logging.level.com.example=DEBUG
spring.jpa.show-sql=true

# application-test.properties
spring.datasource.url=jdbc:h2:mem:testdb

# application-prod.properties
logging.level.com.example=WARN
spring.jpa.show-sql=false
```

**3. Activating Profiles:**

**Method 1: Properties File**
```properties
spring.profiles.active=dev,cache
```

**Method 2: Command Line**
```bash
java -jar app.jar --spring.profiles.active=prod
java -Dspring.profiles.active=dev -jar app.jar
```

**Method 3: Environment Variable**
```bash
export SPRING_PROFILES_ACTIVE=prod
```

**Method 4: Programmatically**
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication app = new SpringApplication(Application.class);
        app.setAdditionalProfiles("dev");
        app.run(args);
    }
}
```

**4. Complex Profile Expressions:**
```java
@Component
@Profile("dev & cache") // dev AND cache profiles
public class DevCacheService { }

@Component
@Profile("dev | test") // dev OR test profiles
public class NonProdService { }

@Component
@Profile("!prod") // NOT prod profile
public class DebugService { }
```

**5. Using Profiles in Tests:**
```java
@SpringBootTest
@ActiveProfiles("test")
class OrderServiceTest {
    
    @Autowired
    private OrderService orderService;
    
    @Test
    void testCreateOrder() {
        // Test with test profile configuration
    }
}
```

---

## **9. What is the difference between @SpringBootTest and other test annotations?**

**Answer:**

Spring Boot provides specialized test annotations for different layers:

**1. @SpringBootTest - Full Integration Test:**
```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class FullIntegrationTest {
    
    @Autowired
    private TestRestTemplate restTemplate;
    
    @LocalServerPort
    private int port;
    
    @Test
    void testCompleteFlow() {
        String url = "http://localhost:" + port + "/api/users";
        ResponseEntity<String> response = restTemplate.getForEntity(url, String.class);
        assertEquals(HttpStatus.OK, response.getStatusCode());
    }
}
```

**2. @WebMvcTest - Web Layer Test:**
```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Test
    void testGetUser() throws Exception {
        when(userService.findById(1L)).thenReturn(new User("John"));
        
        mockMvc.perform(get("/users/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.name").value("John"));
    }
}
```

**3. @DataJpaTest - JPA Repository Test:**
```java
@DataJpaTest
class UserRepositoryTest {
    
    @Autowired
    private TestEntityManager entityManager;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void testFindByEmail() {
        User user = new User("john@example.com", "John");
        entityManager.persistAndFlush(user);
        
        Optional<User> found = userRepository.findByEmail("john@example.com");
        assertTrue(found.isPresent());
        assertEquals("John", found.get().getName());
    }
}
```

**4. @JsonTest - JSON Serialization Test:**
```java
@JsonTest
class UserJsonTest {
    
    @Autowired
    private JacksonTester<User> json;
    
    @Test
    void testSerialize() throws Exception {
        User user = new User("john@example.com", "John");
        
        assertThat(json.write(user))
            .hasJsonPathStringValue("@.email")
            .extractingJsonPathStringValue("@.email")
            .isEqualTo("john@example.com");
    }
}
```

**Comparison Table:**

| Annotation | Loads | Use Case | Speed |
|------------|--------|----------|-------|
| @SpringBootTest | Full application context | End-to-end testing | Slow |
| @WebMvcTest | Web layer only | Controller testing | Fast |
| @DataJpaTest | JPA components only | Repository testing | Fast |
| @JsonTest | JSON components only | Serialization testing | Very Fast |

---

## **10. How does Spring Boot handle logging?**

**Answer:**

Spring Boot provides auto-configured logging with sensible defaults.

**Default Logging Configuration:**
- **Default Framework**: Logback
- **Default Level**: INFO
- **Default Pattern**: Timestamp, Level, Thread, Logger, Message

**1. Basic Logging Configuration:**
```properties
# application.properties
logging.level.com.example=DEBUG
logging.level.org.springframework=WARN
logging.level.root=INFO

# Log file
logging.file.name=app.log
logging.file.path=/var/logs

# Pattern
logging.pattern.console=%d{yyyy-MM-dd HH:mm:ss} - %msg%n
logging.pattern.file=%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n
```

**2. Using Different Logging Frameworks:**
```xml
<!-- Exclude default logging -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<!-- Add Log4j2 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-log4j2</artifactId>
</dependency>
```

**3. Custom Logback Configuration:**
```xml
<!-- logback-spring.xml -->
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
        <appender name="FILE" class="ch.qos.logback.core.FileAppender">
            <file>app.log</file>
            <encoder>
                <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
            </encoder>
        </appender>
        <root level="WARN">
            <appender-ref ref="FILE"/>
        </root>
    </springProfile>
</configuration>
```

**4. Logging in Code:**
```java
@Service
public class OrderService {
    
    private static final Logger logger = LoggerFactory.getLogger(OrderService.class);
    
    public Order createOrder(OrderRequest request) {
        logger.info("Creating order for customer: {}", request.getCustomerId());
        
        try {
            Order order = processOrder(request);
            logger.debug("Order created successfully: {}", order.getId());
            return order;
        } catch (Exception e) {
            logger.error("Failed to create order for customer: {}", request.getCustomerId(), e);
            throw e;
        }
    }
}
```

---

## **🎯 Quick Review Questions**

1. **What happens during Spring Boot application startup?**
   - Load property sources → Auto-configuration → Bean creation → Embedded server start

2. **How do you disable specific auto-configuration?**
   - Use `@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})`

3. **What is the difference between JAR and WAR deployment?**
   - JAR: Self-contained with embedded server
   - WAR: Requires external application server

4. **How do you create a custom starter?**
   - Create auto-configuration class + META-INF/spring.factories

5. **What is @ConditionalOnProperty used for?**
   - Creates beans conditionally based on property values

---

**🚀 Pro Tips for Interview Success:**
- Understand the "why" behind Spring Boot features
- Know the auto-configuration mechanism deeply
- Practice with different starter combinations
- Be familiar with production-ready features
- Understand the testing slice annotations
