# Spring Framework Interview Questions - Complete Study Guide

## 📋 Table of Contents

1. [Core Spring Framework](#core-spring-framework)
2. [Spring IoC & Dependency Injection](#spring-ioc--dependency-injection)
3. [Spring Beans & Scopes](#spring-beans--scopes)
4. [Spring AOP (Aspect-Oriented Programming)](#spring-aop-aspect-oriented-programming)
5. [Spring Boot](#spring-boot)
6. [Spring MVC](#spring-mvc)
7. [Spring REST API](#spring-rest-api)
8. [Spring Data JPA & Hibernate](#spring-data-jpa--hibernate)
9. [Microservices with Spring](#microservices-with-spring)
10. [Spring Security](#spring-security)
11. [Spring Annotations](#spring-annotations)

---

## 🔥 Core Spring Framework

### Basic Concepts

**Q1: What is Spring Framework? What are its advantages?**
- Spring is a lightweight, open-source framework for enterprise Java development
- **Advantages:**
  - Inversion of Control (IoC)
  - Dependency Injection
  - Aspect-Oriented Programming (AOP)
  - Simplified testing
  - Integration with various frameworks
  - Non-invasive framework

**Q2: What is the difference between Spring Framework, Spring, Spring MVC, and Spring Boot?**
- **Spring Framework**: Core framework with IoC, DI, AOP
- **Spring**: Same as Spring Framework
- **Spring MVC**: Web module of Spring for building web applications
- **Spring Boot**: Auto-configuration framework built on top of Spring

**Q3: What are the modules of Spring Framework?**
- Core Container (Core, Beans, Context, SpEL)
- Data Access/Integration (JDBC, ORM, OXM, JMS, Transaction)
- Web (Web, Web-MVC, Web-Socket, Web-Portlet)
- AOP and Instrumentation
- Test

---

## 🔄 Spring IoC & Dependency Injection

### IoC Container

**Q4: What is IoC (Inversion of Control) and DI (Dependency Injection)?**
- **IoC**: Design principle where control of object creation is transferred to external entity
- **DI**: Implementation of IoC where dependencies are injected rather than created

**Q5: What is the role of IoC Container?**
- Creates objects
- Manages object lifecycle
- Injects dependencies
- Provides configuration metadata

**Q6: What are types of IoC containers in Spring?**
- **BeanFactory**: Basic container with lazy initialization
- **ApplicationContext**: Advanced container with eager initialization, event publishing, internationalization

**Q7: What is the difference between BeanFactory and ApplicationContext?**
| BeanFactory | ApplicationContext |
|-------------|-------------------|
| Lazy initialization | Eager initialization |
| Basic DI support | Advanced features |
| No event publishing | Event publishing |
| Manual registration | Automatic registration |

### Dependency Injection Types

**Q8: What are the types of Dependency Injection?**
- **Constructor Injection**: Dependencies injected through constructor
- **Setter Injection**: Dependencies injected through setter methods
- **Field Injection**: Dependencies injected directly into fields

**Q9: Which is the best way to inject beans and why?**
- **Constructor Injection** is preferred because:
  - Ensures immutability
  - Mandatory dependencies are clear
  - Prevents partial object creation
  - Better for testing

**Q10: What is the difference between Constructor Injection and Setter Injection?**
| Constructor Injection | Setter Injection |
|----------------------|------------------|
| Mandatory dependencies | Optional dependencies |
| Immutable objects | Mutable objects |
| Fails fast | May create partial objects |
| Thread-safe | Not inherently thread-safe |

**Q11: What is circular dependency issue and how to resolve it?**
- **Issue**: When two beans depend on each other
- **Solutions**:
  - Use setter injection instead of constructor injection
  - Use @Lazy annotation
  - Redesign to avoid circular dependency
  - Use @PostConstruct for initialization

---

## 🫘 Spring Beans & Scopes

### Bean Basics

**Q12: What is a Spring Bean?**
- Object managed by Spring IoC container
- Instantiated, assembled, and managed by container
- Defined in configuration metadata

**Q13: What is @Configuration and @Bean annotation?**
- **@Configuration**: Indicates class contains bean definitions
- **@Bean**: Method-level annotation to define a bean

```java
@Configuration
public class AppConfig {
    @Bean
    public UserService userService() {
        return new UserService();
    }
}
```

### Bean Scopes

**Q14: What are different bean scopes in Spring?**
- **Singleton**: One instance per Spring container (default)
- **Prototype**: New instance for each request
- **Request**: One instance per HTTP request (web-aware)
- **Session**: One instance per HTTP session (web-aware)
- **GlobalSession**: One instance per global HTTP session
- **Application**: One instance per ServletContext

**Q15: What is the default bean scope in Spring Framework?**
- **Singleton** is the default scope

**Q16: In which scenarios would you use Singleton and Prototype scope?**
- **Singleton**: Stateless services, DAOs, configuration objects
- **Prototype**: Stateful objects, objects with user-specific data

**Q17: Are Singleton beans thread-safe?**
- No, Spring doesn't guarantee thread safety
- Developer must ensure thread safety for stateful singleton beans

**Q18: Can we have multiple Spring configurations in one project?**
- Yes, using:
  - Multiple @Configuration classes
  - @Import annotation
  - Component scanning
  - Profile-based configurations

### Spring Profiles

**Q19: What are Spring Profiles and how do you use them?**
- Mechanism to segregate application configuration
- Used for different environments (dev, test, prod)

```java
@Profile("dev")
@Configuration
public class DevConfig {
    // Development specific beans
}
```

---

## 🎯 Spring AOP (Aspect-Oriented Programming)

**Q20: What is AOP (Aspect-Oriented Programming)?**
- Programming paradigm for cross-cutting concerns
- Separates business logic from system services (logging, security, transactions)

**Q21: What are the advantages and disadvantages of AOP?**
- **Advantages**: Code modularity, reusability, cleaner code
- **Disadvantages**: Complexity, debugging difficulty, performance overhead

**Q22: What is the difference between JoinPoint and PointCut?**
- **JoinPoint**: Execution point in application (method execution, exception handling)
- **PointCut**: Expression that selects JoinPoints

**Q23: What are AOP terminologies?**
- **Aspect**: Cross-cutting concern implementation
- **Advice**: Action taken at JoinPoint (@Before, @After, @Around)
- **Weaving**: Process of linking aspects with objects

---

## 🚀 Spring Boot

### Spring Boot Basics

**Q24: What is Spring Boot?**
- Framework built on top of Spring Framework
- Provides auto-configuration and convention over configuration
- Embedded server support

**Q25: What are the features of Spring Boot?**
- Auto-configuration
- Starter dependencies
- Embedded servers
- Production-ready features (actuator)
- No XML configuration required

**Q26: What are the advantages of Spring Boot?**
- Rapid development
- Microservices friendly
- Reduced boilerplate code
- Easy testing
- Production-ready

**Q27: What are the key components of Spring Boot?**
- Spring Boot Starter
- Auto-configuration
- Spring Boot CLI
- Spring Boot Actuator

**Q28: Why do we prefer Spring Boot over Spring Framework?**
- Less configuration
- Embedded server
- Auto-configuration
- Production-ready features
- Faster development

### Spring Boot Internals

**Q29: What is the internal working of Spring Boot?**
1. @SpringBootApplication triggers auto-configuration
2. SpringApplication.run() starts the application
3. Auto-configuration classes are loaded
4. Embedded server is started
5. Application context is created

**Q30: What are Spring Boot Starter Dependencies?**
- Pre-configured dependency descriptors
- Examples: spring-boot-starter-web, spring-boot-starter-data-jpa
- Simplify dependency management

**Q31: How does a Spring Boot application get started?**
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

**Q32: What does @SpringBootApplication do internally?**
- Combines @Configuration, @EnableAutoConfiguration, @ComponentScan
- Triggers auto-configuration
- Enables component scanning

**Q33: What is Spring Initializr?**
- Web-based tool to generate Spring Boot project structure
- Provides project metadata and dependencies selection

### Auto-configuration

**Q34: What is autowiring in Spring?**
- Automatic injection of dependencies by Spring container
- Types: @Autowired, @Resource, @Inject

**Q35: What is the difference between @SpringBootApplication and @EnableAutoConfiguration?**
- **@SpringBootApplication**: Includes @EnableAutoConfiguration + @Configuration + @ComponentScan
- **@EnableAutoConfiguration**: Only enables auto-configuration

**Q36: What is the difference between WAR and JAR deployment?**
| WAR | JAR |
|-----|-----|
| External server required | Embedded server |
| Traditional deployment | Self-contained |
| Larger file size | Smaller file size |
| Complex deployment | Simple deployment |

---

## 🌐 Spring MVC

### MVC Architecture

**Q37: What is Spring MVC? Explain its architecture.**
- Web framework based on Model-View-Controller pattern
- **Components**: DispatcherServlet, Controller, ViewResolver, View

**Q38: What is DispatcherServlet and its role?**
- Front controller that handles all HTTP requests
- Routes requests to appropriate controllers
- Manages the entire request-response cycle

**Q39: What is the Spring MVC request flow?**
1. Request comes to DispatcherServlet
2. DispatcherServlet consults HandlerMapping
3. Controller processes request
4. Controller returns ModelAndView
5. ViewResolver resolves view
6. View renders response

### Controllers and Request Mapping

**Q40: What is @Controller and @RestController?**
- **@Controller**: Traditional MVC controller, returns view names
- **@RestController**: REST controller, returns data directly (@Controller + @ResponseBody)

**Q41: What are @RequestMapping annotations?**
- @RequestMapping: Generic mapping
- @GetMapping: GET requests
- @PostMapping: POST requests
- @PutMapping: PUT requests
- @DeleteMapping: DELETE requests

**Q42: What is @PathVariable and @RequestParam?**
- **@PathVariable**: Extract values from URL path
- **@RequestParam**: Extract values from query parameters

```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id, @RequestParam String format) {
    // Implementation
}
```

---

## 🔗 Spring REST API

### REST Fundamentals

**Q43: What is REST and its principles?**
- **REST**: Representational State Transfer
- **Principles**: Stateless, Client-Server, Cacheable, Uniform Interface, Layered System

**Q44: What are HTTP methods in REST?**
- **GET**: Retrieve data
- **POST**: Create new resource
- **PUT**: Update existing resource (complete)
- **PATCH**: Partial update
- **DELETE**: Remove resource

**Q45: What is the difference between PUT and POST?**
| PUT | POST |
|-----|------|
| Idempotent | Not idempotent |
| Update operation | Create operation |
| Complete resource | Partial data allowed |
| Known resource ID | System generates ID |

### Request/Response Handling

**Q46: What is @RequestBody and @ResponseBody?**
- **@RequestBody**: Converts HTTP request body to Java object
- **@ResponseBody**: Converts Java object to HTTP response body

**Q47: How do you handle validation in REST API?**
```java
@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
    // Implementation
}
```

**Q48: What are HTTP status codes? Name important ones.**
- **2xx**: Success (200 OK, 201 Created, 204 No Content)
- **4xx**: Client Error (400 Bad Request, 401 Unauthorized, 404 Not Found)
- **5xx**: Server Error (500 Internal Server Error, 503 Service Unavailable)

### Error Handling

**Q49: How do you handle exceptions in Spring REST API?**
- @ExceptionHandler
- @ControllerAdvice
- ResponseEntityExceptionHandler

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleUserNotFound(UserNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(new ErrorResponse(ex.getMessage()));
    }
}
```

**Q50: What is @CrossOrigin annotation?**
- Enables Cross-Origin Resource Sharing (CORS)
- Allows requests from different domains

---

## 💾 Spring Data JPA & Hibernate

### JPA Basics

**Q51: What is Spring Data JPA? How is it different from plain JPA?**
- Spring Data JPA provides repository abstraction over JPA
- Eliminates boilerplate code
- Automatic query generation from method names

**Q52: What is the repository hierarchy in Spring Data JPA?**
- Repository (marker interface)
- CrudRepository (basic CRUD)
- PagingAndSortingRepository (pagination and sorting)
- JpaRepository (JPA-specific methods)

**Q53: What is the difference between CrudRepository, JpaRepository, and PagingAndSortingRepository?**
| Repository | Features |
|------------|----------|
| CrudRepository | Basic CRUD operations |
| PagingAndSortingRepository | + Pagination and sorting |
| JpaRepository | + JPA-specific methods (flush, batch operations) |

### Query Methods

**Q54: How does query method generation work in Spring Data JPA?**
- Spring Data JPA parses method names
- Generates JPQL queries automatically
- Uses keywords like findBy, countBy, deleteBy

**Q55: What are query method naming conventions?**
```java
findByFirstName(String firstName)
findByFirstNameAndLastName(String first, String last)
findByAgeGreaterThan(int age)
countByFirstName(String firstName)
deleteByFirstName(String firstName)
```

### Custom Queries

**Q56: How do you write custom queries in Spring Data JPA?**
- @Query annotation with JPQL
- @Query annotation with native SQL
- Specifications for dynamic queries

```java
@Query("SELECT u FROM User u WHERE u.email = ?1")
User findByEmail(String email);

@Query(value = "SELECT * FROM users WHERE age > ?1", nativeQuery = true)
List<User> findUsersOlderThan(int age);
```

### Hibernate Concepts

**Q57: What is the difference between JPA and Hibernate?**
- **JPA**: Specification for ORM
- **Hibernate**: Implementation of JPA specification

**Q58: What are Hibernate entity states?**
- **Transient**: Not associated with session
- **Persistent**: Associated with session and database
- **Detached**: Was persistent but session closed
- **Removed**: Marked for deletion

**Q59: What is the difference between save() and saveAndFlush()?**
- **save()**: Persists entity, may not immediately sync to database
- **saveAndFlush()**: Persists entity and immediately flushes to database

**Q60: What is N+1 query problem and how to solve it?**
- **Problem**: One query to fetch entities + N queries to fetch related entities
- **Solutions**: @Fetch(FetchMode.JOIN), @EntityGraph, JOIN FETCH in JPQL

---

## 🏗️ Microservices with Spring

### Microservices Basics

**Q61: What are microservices and what problems do they solve?**
- Architectural style with small, independent services
- **Problems solved**: Scalability, technology diversity, team autonomy, fault isolation

**Q62: What is the difference between monolithic and microservices architecture?**
| Monolithic | Microservices |
|------------|---------------|
| Single deployable unit | Multiple deployable units |
| Shared database | Database per service |
| Technology coupling | Technology independence |
| Difficult scaling | Independent scaling |

### Spring Cloud Components

**Q63: What is Spring Cloud and its components?**
- Framework for building microservices
- **Components**: Eureka, Zuul, Ribbon, Hystrix, Config Server

**Q64: What is service discovery and why is it important?**
- Mechanism for services to find and communicate with each other
- **Tools**: Eureka, Consul, Zookeeper

**Q65: What is API Gateway and its role?**
- Single entry point for all client requests
- **Functions**: Routing, load balancing, authentication, rate limiting

**Q66: What is circuit breaker pattern?**
- Prevents cascading failures in microservices
- **States**: Closed, Open, Half-Open
- **Tool**: Hystrix, Resilience4j

### Communication Patterns

**Q67: How do microservices communicate with each other?**
- **Synchronous**: REST API, gRPC
- **Asynchronous**: Message queues, Event streaming

**Q68: What is the difference between orchestration and choreography?**
- **Orchestration**: Central coordinator controls workflow
- **Choreography**: Services coordinate themselves through events

---

## 🔐 Spring Security

**Q69: What is Spring Security?**
- Comprehensive security framework for Java applications
- Provides authentication, authorization, and protection against attacks

**Q70: What is the difference between authentication and authorization?**
- **Authentication**: Verifying user identity (who you are)
- **Authorization**: Checking user permissions (what you can do)

**Q71: What are different authentication mechanisms in Spring Security?**
- Form-based authentication
- HTTP Basic authentication
- JWT tokens
- OAuth2
- LDAP authentication

**Q72: How do you implement JWT authentication in Spring Boot?**
```java
@Component
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                  HttpServletResponse response, 
                                  FilterChain filterChain) {
        // JWT validation logic
    }
}
```

---

## 📝 Spring Annotations

### Core Annotations

**Q73: What are all important Spring annotations?**

#### Component Annotations
- **@Component**: Generic component
- **@Service**: Service layer component
- **@Repository**: Data access layer component
- **@Controller**: Web controller
- **@RestController**: REST controller

#### Configuration Annotations
- **@Configuration**: Configuration class
- **@Bean**: Bean definition
- **@ComponentScan**: Component scanning
- **@PropertySource**: Property file location

#### Dependency Injection Annotations
- **@Autowired**: Automatic dependency injection
- **@Qualifier**: Specify bean name for injection
- **@Value**: Inject property values
- **@Primary**: Primary bean for injection

#### Spring Boot Annotations
- **@SpringBootApplication**: Main application class
- **@EnableAutoConfiguration**: Enable auto-configuration
- **@ConditionalOnProperty**: Conditional bean creation

#### Web Annotations
- **@RequestMapping**: Map web requests
- **@PathVariable**: Extract path variables
- **@RequestParam**: Extract request parameters
- **@RequestBody**: Convert request body to object
- **@ResponseBody**: Convert object to response body

#### Validation Annotations
- **@Valid**: Enable validation
- **@NotNull**: Not null validation
- **@Size**: Size validation
- **@Email**: Email validation

#### Transaction Annotations
- **@Transactional**: Transaction management
- **@Rollback**: Rollback transaction in tests

#### Testing Annotations
- **@SpringBootTest**: Integration test
- **@WebMvcTest**: Web layer test
- **@DataJpaTest**: JPA repository test
- **@MockBean**: Mock bean in tests

---

## 🎯 Additional Advanced Topics

### Performance and Optimization

**Q74: How do you optimize Spring Boot application performance?**
- Use appropriate connection pooling
- Implement caching (@Cacheable)
- Optimize database queries
- Use async processing (@Async)
- Profile and monitor application

**Q75: What is Spring WebFlux and how is it different from Spring MVC?**
- **Spring WebFlux**: Reactive programming model
- **Spring MVC**: Traditional servlet-based model
- **Differences**: Non-blocking vs blocking, reactive streams vs traditional

### Testing

**Q76: How do you test Spring applications?**
- **Unit Testing**: @MockBean, Mockito
- **Integration Testing**: @SpringBootTest
- **Web Layer Testing**: @WebMvcTest
- **Data Layer Testing**: @DataJpaTest

**Q77: What is @MockBean vs @Mock?**
- **@MockBean**: Spring Boot test annotation, adds mock to application context
- **@Mock**: Mockito annotation, creates mock object

### Configuration

**Q78: How do you externalize configuration in Spring Boot?**
- application.properties/yml
- Environment variables
- Command line arguments
- @ConfigurationProperties

**Q79: What is @ConfigurationProperties?**
- Type-safe configuration properties binding
- Maps external properties to Java objects

```java
@ConfigurationProperties(prefix = "app")
public class AppProperties {
    private String name;
    private String version;
    // getters and setters
}
```

---

## 📚 Study Tips for Interview Success

### High Priority Topics (Must Know)
1. **Core Spring**: IoC, DI, Bean lifecycle
2. **Spring Boot**: Auto-configuration, starters
3. **REST API**: HTTP methods, status codes, exception handling
4. **Spring Data JPA**: Repository pattern, query methods
5. **Microservices**: Basic concepts, communication patterns

### Medium Priority Topics (Should Know)
1. **Spring MVC**: Request flow, controllers
2. **Spring Security**: Authentication, authorization
3. **Spring AOP**: Cross-cutting concerns
4. **Testing**: Unit and integration testing
5. **Performance**: Caching, optimization

### Practice Areas
1. Create REST APIs with proper error handling
2. Implement JPA repositories with custom queries
3. Write unit and integration tests
4. Configure Spring Security
5. Build microservices with Spring Cloud

### Common Pitfalls to Avoid
1. Not understanding bean scopes and lifecycle
2. Confusion between @Component annotations
3. Not knowing when to use constructor vs setter injection
4. Misunderstanding REST principles
5. Not handling exceptions properly in REST APIs

---

*This guide covers the most commonly asked Spring Framework interview questions for 2+ years experience. Practice these concepts hands-on and understand the underlying principles for interview success.*
