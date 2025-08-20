# Day 4 Interview Questions and Answers

## 🔹 Project Technology Stack

### Question 1
**What is the Spring version you are using in your project?**

**Answer:**
Spring Boot 3.x and Spring Framework 6.x are commonly used in modern projects (as of 2025). The exact version depends on your project's requirements and compatibility needs.

### Question 2
**What is the Java version you are using?**

**Answer:**
Java 17 and Java 21 are the latest LTS versions widely adopted in enterprise projects. Specify the version used in your project (e.g., Java 17).

### Question 8
**What are the stacks/tech stacks you have worked on in your company?**

**Answer:**
Typical stacks include Java (Spring Boot), Hibernate, REST APIs, MySQL/PostgreSQL, React/Angular (frontend), Docker, Kubernetes, Jenkins (CI/CD), Git, and cloud platforms (AWS/Azure/GCP).

## 🔹 Java 8+ Features & Versions

### Question 3
**What are the new features in Java 11?**

**Answer:**
Java 11 introduced features like local variable syntax for lambda parameters, new String methods, HTTP Client API, removal of deprecated APIs, and performance improvements.

### Question 6
**What Java 8 features are used in your project?**

**Answer:**
Common Java 8 features: Lambda expressions, Stream API, Functional interfaces, Optional, new Date/Time API, and default/static methods in interfaces.

## 🔹 Stream API & Functional Programming

### Question 4
**What is the Stream API?**

**Answer:**
Stream API allows processing collections of objects in a functional style (filter, map, reduce, etc.), enabling efficient, readable, and parallelizable data operations.

### Question 5
**What is the difference between map() and flatMap() with examples?**

**Answer:**
`map()` transforms each element; `flatMap()` flattens nested structures. Example:
- `map`: List<String> to List<Integer> (lengths)
- `flatMap`: List<List<String>> to List<String> (flattened)

### Question 7
**What is a Stream function?**

**Answer:**
A Stream function is any method that operates on a stream, such as `filter`, `map`, `reduce`, `collect`, etc., enabling functional-style operations on data.

### Question 9
**When should you use a lambda function vs a normal function?**

**Answer:**
Use lambda functions for short, inline implementations (especially with functional interfaces). Use normal functions for reusable, complex logic.

### Question 10
**What is streaming in Java (not Stream API)?**

**Answer:**
"Streaming" refers to reading/writing data in a continuous flow (e.g., InputStream/OutputStream for files, network, etc.), not related to Stream API.

### Question 12
**What is the difference between normal stream and parallel stream?**

**Answer:**
Normal streams process data sequentially; parallel streams split data and process in multiple threads for faster execution (on multicore CPUs).

### Question 13
**What is the thread limitation in a parallel stream?**

**Answer:**
Parallel streams use the ForkJoinPool (default size = number of CPU cores). Too many threads can cause contention; control with `System.setProperty` or custom pool.

## 🔹 Hibernate & Database Operations

### Question 14
**How does Hibernate handle queries in the database?**

**Answer:**
Hibernate translates HQL/Criteria queries to SQL, manages connections, executes queries, and maps results to Java objects (ORM).

### Question 15
**What is the difference between select and update in Hibernate?**

**Answer:**
`select` retrieves data; `update` modifies existing records. Hibernate provides methods for both (e.g., `session.createQuery("from Entity")` vs `session.update(entity)`).

### Question 16
**How does Hibernate complete a query?**

**Answer:**
Hibernate parses the query, generates SQL, executes it, and maps results to entities. Transaction management ensures consistency.

### Question 17
**What is HQL (Hibernate Query Language)?**

**Answer:**
HQL is an object-oriented query language similar to SQL but operates on entity objects, not tables.

### Question 18
**How does Spring call a Hibernate query?**

**Answer:**
Spring uses HibernateTemplate or JPA repositories to execute queries, managing sessions and transactions automatically.

### Question 19
**How does Spring write or invoke a Hibernate query internally? Can you explain with an example?**

**Answer:**
Spring Data JPA uses method naming conventions or `@Query` annotation to define queries. Example:
```java
@Query("SELECT e FROM Entity e WHERE e.status = ?1")
List<Entity> findByStatus(String status);
```

## 🔹 Transaction Management

### Question 20
**What is transaction management?**

**Answer:**
Transaction management ensures data consistency and integrity by grouping operations into a single unit (commit/rollback).

### Question 21
**What is a rollback? Is it the default behavior?**

**Answer:**
Rollback undoes changes if an error occurs. In Spring, rollback is default for unchecked exceptions; can be customized with `@Transactional`.

## 🔹 Spring Framework & Annotations

### Question 22
**Can you explain the annotations used in your Spring project?**

**Answer:**
Common annotations: `@SpringBootApplication`, `@RestController`, `@Service`, `@Component`, `@Repository`, `@Autowired`, `@RequestMapping`, etc.

### Question 23
**What is @RequestMapping and what are its uses?**

**Answer:**
`@RequestMapping` maps HTTP requests to handler methods in controllers. It supports method, path, params, headers, etc.

### Question 24
**How do you map a request when a lot of data is coming in?**

**Answer:**
Use `@RequestBody` to map large/complex JSON payloads to POJOs. For file uploads, use `MultipartFile`.

### Question 25
**How does a POJO class understand the request API?**

**Answer:**
Spring uses Jackson to map JSON/XML request data to POJO fields via setters/getters or annotations.

### Question 29
**Explain the difference between Spring MVC and Spring Boot.**

**Answer:**
Spring MVC is a web framework; Spring Boot simplifies setup, configuration, and deployment with auto-configuration and embedded servers.

### Question 30
**What are different types of autowiring in Spring?**

**Answer:**
Types: `byType`, `byName`, `constructor`, and `@Autowired` annotation (default byType).

## 🔹 Dependency Injection & Spring Concepts

### Question 26
**What is dependency injection?**

**Answer:**
Dependency injection is a design pattern where dependencies are provided by the framework, not created by the class itself, improving modularity and testability.

### Question 27
**How does Spring create an object every time you create one?**

**Answer:**
Spring manages bean scopes. For prototype scope, a new object is created each time; for singleton, only one instance exists.

### Question 28
**What is the difference between different Spring scopes?**

**Answer:**
Scopes: `singleton` (one instance), `prototype` (new instance per request), `request`, `session`, `application` (web contexts).

## 🔹 Java Collections

### Question 31
**What is the difference between ArrayList and LinkedList?**

**Answer:**
ArrayList uses a dynamic array, fast for random access, slow for insert/delete. LinkedList uses nodes, fast for insert/delete, slow for access.

## 🔹 Team Processes & Development

### Question 11
**What are the processes you are involved in with your team?**

**Answer:**
Agile ceremonies (standups, sprint planning), code reviews, deployments, testing, documentation, and client communication.

### Question 33
**What is the development and deployment process used in your project?**

**Answer:**
Development: coding, testing, code review. Deployment: CI/CD pipeline (Jenkins/GitHub Actions), containerization (Docker), cloud deployment.

## 🔹 Testing & Quality Assurance

### Question 32
**What testing tools have you used?**

**Answer:**
JUnit, Mockito, Selenium, Postman, SonarQube, JaCoCo, Cucumber.

## 🔹 DevOps & CI/CD

### Question 34
**What repositories have you used in your project?**

**Answer:**
GitHub, GitLab, Bitbucket for version control and collaboration.

### Question 35
**What CI/CD pipeline have you used?**

**Answer:**
Jenkins, GitHub Actions, GitLab CI for automated build, test, and deployment.

## 🔹 Frontend Development

### Question 36
**Have you worked on any front-end scripts?**

**Answer:**
Yes, with JavaScript, React, Angular, or basic HTML/CSS for UI development and integration.

---

*Date: Day 4 Mock Interview*
*Topics: Java 8+, Stream API, Spring Framework, Hibernate, Transaction Management, Testing, DevOps, Full-Stack Development*
