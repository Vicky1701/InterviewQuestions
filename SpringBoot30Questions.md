Here are detailed answers to the 30 most important Spring Framework interview questions for a candidate with 3 years of experience:

---

### 1. What is the Spring Framework and what problems does it solve?

Spring is a lightweight, open-source framework for building Java applications. It solves issues related to tight coupling, boilerplate code, and enterprise complexity through dependency injection (DI), aspect-oriented programming (AOP), and modularity.

---

### 2. What is Inversion of Control (IoC)? How is it implemented in Spring?

IoC is a design principle where control of object creation is shifted from the application code to the framework. In Spring, it is implemented using the IoC container which injects dependencies into objects via XML or annotations.

---

### 3. What are the different types of Dependency Injection in Spring? Which one is preferred and why?

* Constructor Injection (Preferred for immutability and mandatory dependencies)
* Setter Injection (Used for optional dependencies)
* Field Injection (Not recommended due to testability issues)

---

### 4. What is a Spring Bean? How do you define and manage them?

A Spring Bean is an object managed by the Spring container. Beans can be defined using annotations like `@Component`, `@Service`, or explicitly in XML or Java config with `@Bean`.

---

### 5. What is the difference between `@Component`, `@Service`, `@Repository`, and `@Controller`?

All are specializations of `@Component`:

* `@Component`: Generic stereotype for a Spring-managed component.
* `@Service`: Business logic layer.
* `@Repository`: Persistence layer, integrates with Spring DataException translation.
* `@Controller`: Web layer for Spring MVC.

---

### 6. What is the Bean Lifecycle in Spring?

* Instantiation
* Populate properties
* `BeanNameAware`, `BeanFactoryAware`, etc.
* `@PostConstruct` or `InitializingBean.afterPropertiesSet()`
* Custom init method
* Bean is ready
* `@PreDestroy` or `DisposableBean.destroy()`

---

### 7. What are the different Bean scopes in Spring?

* Singleton (default)
* Prototype
* Request
* Session
* Application

---

### 8. How does Spring handle circular dependencies?

Spring can handle circular dependencies for field injection using proxies. Constructor injection will fail with a `BeanCurrentlyInCreationException`.

---

### 9. What is the difference between `@Autowired`, `@Qualifier`, and `@Primary`?

* `@Autowired`: Injects dependencies by type.
* `@Qualifier`: Specifies which bean to inject when multiple beans of the same type exist.
* `@Primary`: Declares a default bean when multiple candidates exist.

---

### 10. What is the role of `ApplicationContext` vs `BeanFactory`?

`ApplicationContext` is a superset of `BeanFactory` with additional features like internationalization, event propagation, and AOP support.

---

### 11. What is Java-based configuration in Spring?

Using `@Configuration` and `@Bean` annotations to define beans instead of XML. More type-safe and refactor-friendly.

---

### 12. What is the role of `@Profile` in Spring?

Used to load beans conditionally based on the active environment (e.g., dev, test, prod).

---

### 13. Difference between `@Value` and `@ConfigurationProperties`?

* `@Value`: For injecting individual property values.
* `@ConfigurationProperties`: For mapping entire property groups to Java beans (preferred for structured data).

---

### 14. How to implement lazy initialization?

Use `@Lazy` on bean class or field to defer initialization until it's first needed.

---

### 15. Constructor vs Field Injection?

Constructor injection is preferred for immutability and testability. Field injection is discouraged because it hides dependencies.

---

### 16. What is AOP? What problems does it solve?

Aspect-Oriented Programming modularizes cross-cutting concerns like logging, security, and transactions. Spring implements AOP using proxies.

---

### 17. Types of advice in AOP?

* `@Before`
* `@After`
* `@AfterReturning`
* `@AfterThrowing`
* `@Around`

---

### 18. AOP use-case example?

Logging method execution time using `@Around` advice.

---

### 19. What is a pointcut?

Expression that matches join points (method executions) where advice should be applied.

---

### 20. Spring AOP vs AspectJ?

* Spring AOP: Proxy-based, method-level, runtime weaving.
* AspectJ: Compile-time or load-time weaving, more powerful.

---

### 21. How does Spring simplify JDBC?

Via `JdbcTemplate`, which handles resource management, exception translation, and reduces boilerplate code.

---

### 22. Spring with JPA/Hibernate?

Use `@Entity`, `@Repository`, and `JpaRepository`. Spring Boot auto-configures JPA setup.

---

### 23. What is `@Transactional`?

Marks methods or classes as transactional. Manages rollback, commit, and propagation automatically.

---

### 24. Propagation types?

* `REQUIRED`
* `REQUIRES_NEW`
* `NESTED`
* `MANDATORY`
* `NEVER`
* `SUPPORTS`
* `NOT_SUPPORTED`

---

### 25. Isolation levels?

Defines how transaction reads are isolated from others:

* READ\_UNCOMMITTED
* READ\_COMMITTED (default)
* REPEATABLE\_READ
* SERIALIZABLE

---

### 26. Spring MVC flow?

`DispatcherServlet` -> HandlerMapping -> Controller -> ViewResolver -> View

---

### 27. Difference between `@RequestParam`, `@PathVariable`, and `@ModelAttribute`?

* `@RequestParam`: Query parameters
* `@PathVariable`: URI template variables
* `@ModelAttribute`: Bind form data to model object

---

### 28. How to do form validation?

Use `@Valid`, `BindingResult`, and JSR-303 annotations like `@NotNull`, `@Size`, etc.

---

### 29. Difference between `@RestController` and `@Controller`?

* `@RestController` = `@Controller` + `@ResponseBody`
* Used for REST APIs

---

### 30. Exception handling in Spring?

* `@ControllerAdvice` for global exception handling
* `@ExceptionHandler` for specific exception handling

---

Let me know if you want this exported as a PDF or explained as flashcards.
