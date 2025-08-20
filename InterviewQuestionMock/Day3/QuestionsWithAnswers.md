
# Day 3 Interview Questions and Answers

## 🔹 Java Core Concepts & OOPs

### Question 1
**What is the difference between an interface and an abstract class?**

**Answer:**
An interface defines a contract with abstract methods that implementing classes must fulfill. It cannot have any method implementations (until Java 8, which introduced default and static methods). An abstract class can have both abstract methods (without implementation) and concrete methods (with implementation). Abstract classes can have state (fields), constructors, and access modifiers, while interfaces cannot (except static/final fields). Use interfaces for pure abstraction and abstract classes for partial abstraction and code reuse.

### Question 2
**Can an abstract class implement multiple interfaces and also extend another abstract class simultaneously?**

**Answer:**
Yes, an abstract class can implement multiple interfaces and extend one abstract class at the same time. Java supports single inheritance for classes (including abstract classes) and multiple inheritance for interfaces. Syntax: `abstract class A extends B implements C, D {}`

### Question 3
**Why are OOPs (Object-Oriented Programming concepts) very important in Java?**

**Answer:**
OOPs concepts (Encapsulation, Inheritance, Polymorphism, Abstraction) provide modularity, code reusability, scalability, and maintainability. They help organize complex code, enable easier debugging, and support real-world modeling, making Java applications robust and flexible.

## 🔹 JVM & Runtime Environment

### Question 4
**What is the difference between JDK, JRE, and JVM?**

**Answer:**
JDK (Java Development Kit) includes tools for developing Java applications (compiler, debugger, etc.), JRE (Java Runtime Environment) provides libraries and JVM for running Java applications, and JVM (Java Virtual Machine) executes Java bytecode. JDK = JRE + development tools; JRE = JVM + libraries.

### Question 5
**Can we install JRE separately without JDK? If yes, how will it impact Java development and execution?**

**Answer:**
Yes, JRE can be installed separately. JRE is sufficient for running Java applications but not for development (compiling code). Without JDK, you cannot compile Java source files; you can only execute pre-compiled bytecode.

### Question 6
**If the static block throws an exception, what happens to the class loading process?**

**Answer:**
If a static block throws a checked exception, the class fails to initialize, and a `ExceptionInInitializerError` is thrown. The class will not be loaded, and further attempts to use it will result in errors.

## 🔹 Web Services & API Design

### Question 7
**What is the difference between SOAP and REST?**

**Answer:**
SOAP is a protocol using XML for messaging, with strict standards and built-in error handling, security, and transaction support. REST is an architectural style using HTTP methods (GET, POST, etc.), typically with JSON, and is lightweight, flexible, and easier to scale.

### Question 8
**Why do some financial services still prefer SOAP over REST?**

**Answer:**
SOAP provides advanced security (WS-Security), transactional reliability, and strict standards, which are crucial for financial services. It supports ACID transactions and is better for complex, enterprise-level integrations.

### Question 15
**How can you maintain session in a stateless REST API while ensuring scalability?**

**Answer:**
Use token-based authentication (JWT, OAuth) where the client stores the token and sends it with each request. Server does not store session state, ensuring scalability. For additional data, use distributed caches (Redis) or databases.

## 🔹 Spring Boot Framework

### Question 9
**How to run a Spring Boot application?**

**Answer:**
You can run a Spring Boot application using the `main` method, Maven/Gradle command (`mvn spring-boot:run`), or by executing the packaged JAR (`java -jar app.jar`).

### Question 12
**Which annotations are used in your Spring Boot project?**

**Answer:**
Common annotations: `@SpringBootApplication`, `@RestController`, `@Service`, `@Component`, `@Repository`, `@Autowired`, `@RequestMapping`, `@GetMapping`, `@PostMapping`, `@Entity`, `@Table`, etc.

### Question 13
**How to use two databases in a Spring Boot application?**

**Answer:**
Configure multiple `DataSource` beans and use `@Primary` for the default. Define separate `EntityManagerFactory` and `TransactionManager` for each database. Use `@Qualifier` to inject the correct beans.

### Question 16
**Expand and explain the purpose of @Service and @Component annotations.**

**Answer:**
`@Component` is a generic stereotype for any Spring-managed bean. `@Service` is a specialization of `@Component` for service-layer beans, indicating business logic. Both enable auto-detection and dependency injection.

### Question 17
**Can a class be annotated with both @Service and @Component annotations?**

**Answer:**
Technically, yes, but it's redundant. `@Service` is itself a `@Component`. Use only one for clarity and best practice.

### Question 18
**Which is the main annotation in a Spring project?**

**Answer:**
`@SpringBootApplication` (or `@Configuration` in core Spring) is the main annotation, enabling auto-configuration, component scanning, and configuration.

### Question 19
**How do you handle exceptions in Spring Boot?**

**Answer:**
Use `@ControllerAdvice` with `@ExceptionHandler` methods to handle exceptions globally. You can also use `ResponseEntityExceptionHandler` for REST APIs.

### Question 24
**What is the current Spring version/duration (context unclear—version or usage period)?**

**Answer:**
Spring Boot 3.x and Spring Framework 6.x are current (as of 2025). Duration refers to how long you've used Spring in your projects.

## 🔹 Session Management

### Question 14
**How to use session methods?**

**Answer:**
In web applications, use `HttpSession` to store/retrieve session attributes: `session.setAttribute("key", value)` and `session.getAttribute("key")`. In REST APIs, prefer stateless approaches (tokens).

## 🔹 Microservices Architecture

### Question 11
**What are the steps to create a microservice?**

**Answer:**
1. Identify business capability
2. Design API contract
3. Choose tech stack (Spring Boot, etc.)
4. Implement service logic
5. Configure database
6. Add inter-service communication (REST, messaging)
7. Containerize (Docker)
8. Deploy and monitor

## 🔹 Testing & Quality Assurance

### Question 20
**How do you test your own Spring Boot project?**

**Answer:**
Use JUnit and Mockito for unit/integration tests. Use `@SpringBootTest` for context loading, and tools like Postman for API testing.

### Question 21
**How can you ensure 100% test coverage in your Spring Boot application?**

**Answer:**
Write tests for all code paths (unit, integration, edge cases). Use coverage tools (JaCoCo, SonarQube) to measure and improve coverage.

### Question 22
**If there's a problem in your application while running, how would you find it?**

**Answer:**
Check logs, use debugging tools, analyze stack traces, and use monitoring solutions (Actuator, Prometheus, ELK stack) to identify and resolve issues.

## 🔹 Project Development & Tools

### Question 10
**What are the tools used in project development?**

**Answer:**
IDE (IntelliJ, Eclipse), build tools (Maven, Gradle), version control (Git), CI/CD (Jenkins, GitHub Actions), containerization (Docker), monitoring (Prometheus), testing (JUnit, Mockito).

## 🔹 Project Management & SDLC

### Question 23
**What is Agile and SDLC?**

**Answer:**
Agile is an iterative software development methodology focused on collaboration, flexibility, and customer feedback. SDLC (Software Development Life Cycle) is a process for planning, creating, testing, and deploying software, including phases like requirements, design, development, testing, deployment, and maintenance.

## 🔹 Design Patterns & Programming

### Question 25
**Write a program for the Singleton Design Pattern.**

**Answer:**
```java
public class Singleton {
	private static Singleton instance;
	private Singleton() {}
	public static synchronized Singleton getInstance() {
		if (instance == null) {
			instance = new Singleton();
		}
		return instance;
	}
}
```

---

*Date: Day 3 Mock Interview*
*Topics: Java Core, OOPs, JVM, Spring Boot, Web Services, Microservices, Testing, Design Patterns, SDLC*
