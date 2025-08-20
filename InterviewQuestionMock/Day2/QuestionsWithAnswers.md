# Day 2 Interview Questions and Answers

## Question 1
**What is the difference between abstraction and encapsulation?**

**Answer:**
Abstraction is the process of hiding complex implementation details and showing only the essential features of an object. For example, when you use a car, you only need to know how to drive it, not how the engine works. Encapsulation is the technique of wrapping data (variables) and code (methods) together as a single unit and restricting access to some of the object's components. For example, using private fields in a class and providing public getter/setter methods.

---

## Question 2
**Can we achieve encapsulation without abstraction and how?**

**Answer:**
Yes, encapsulation can be achieved without abstraction. By making class variables private and providing public methods to access or modify them, you encapsulate the data. Abstraction is not necessary for this; you are simply controlling access to the data.

---

## Question 3
**What is the difference between String constant pool and String literal?**

**Answer:**
The String constant pool is a special memory area in Java where string literals are stored to optimize memory usage. A String literal is any string value written directly in the code, like "Hello". When you create a string literal, Java checks the pool first; if it exists, it reuses it, otherwise, it creates a new one.

---

## Question 4
**Can interface have private methods?**

**Answer:**
Yes, since Java 9, interfaces can have private methods. These methods are used to share code between default and static methods within the interface but are not accessible outside the interface.

---

## Question 5
**What is the purpose of private methods inside an interface? If they cannot be accessed externally, why or what not?**

**Answer:**
Private methods in interfaces help avoid code duplication by allowing default and static methods to reuse common logic. They improve code maintainability and readability but are not accessible by implementing classes or outside code.

---

## Question 6
**If you don't use a terminal operation in a stream pipeline, will the intermediate operations be executed or not?**

**Answer:**
No, intermediate operations in a stream are lazy and will not be executed until a terminal operation is called. For example, filter or map will not process data unless you use a terminal operation like collect or forEach.

---

## Question 7
**What is the internal working of a HashMap?**

**Answer:**
HashMap stores key-value pairs in an array of buckets. Each key's hash code determines its bucket. If multiple keys map to the same bucket, they are stored in a linked list or tree (since Java 8). HashMap uses hashCode() and equals() methods to manage keys.

---

## Question 8
**If two keys have the same hash code, how does HashMap store them internally?**

**Answer:**
If two keys have the same hash code, they are placed in the same bucket. HashMap then uses the equals() method to distinguish between them. If equals() returns false, both keys are stored separately in the bucket's linked list or tree.

---

## Question 9
**How many types of class loaders are there?**

**Answer:**
There are mainly three types of class loaders in Java:
1. Bootstrap ClassLoader
2. Extension ClassLoader
3. Application (System) ClassLoader

---

## Question 10
**If two different class loaders load the same class, what will happen?**

**Answer:**
If two different class loaders load the same class, they are treated as two separate classes by the JVM, even if the class name and bytecode are identical. This can lead to ClassCastException if you try to cast between them.

---

## Question 11
**Explain SOLID principles.**

**Answer:**
SOLID stands for:
- **S**ingle Responsibility Principle: A class should have only one reason to change.
- **O**pen/Closed Principle: Software entities should be open for extension but closed for modification.
- **L**iskov Substitution Principle: Subtypes must be substitutable for their base types.
- **I**nterface Segregation Principle: No client should be forced to depend on methods it does not use.
- **D**ependency Inversion Principle: Depend on abstractions, not concretions.

---

## Question 12
**Can violating the OPEN-CLOSE principle still be beneficial in certain situations?**

**Answer:**
Yes, in some cases, violating the Open/Closed Principle can be practical, especially for quick fixes or when requirements are changing rapidly. However, it should be avoided in large or stable systems to maintain code quality and extensibility.

---

## Question 13
**Explain exception hierarchy.**

**Answer:**
All exceptions inherit from Throwable. There are two main branches: Error (serious issues) and Exception. Exception splits into checked and unchecked (RuntimeException).

---

## Question 14
**Why does Java separate checked and unchecked exceptions?**

**Answer:**
Checked exceptions must be handled at compile time, ensuring reliability. Unchecked exceptions are programming errors, handled at runtime.

---

## Question 15
**Can we create our own custom exception?**

**Answer:**
Yes. Example:
```java
class MyException extends Exception {
    public MyException(String msg) { super(msg); }
}
```

---

## Question 16
**Why doesn't the Collection interface extend the Map interface?**

**Answer:**
Collection represents a group of elements; Map represents key-value pairs. Their contracts are different, so Map is not a Collection.

---

## Question 17
**Difference between HashMap and TreeMap, including order, time complexity, and retrieval differences.**

**Answer:**
- HashMap: Unordered, O(1) operations, uses hash table.
- TreeMap: Sorted by keys, O(log n) operations, uses Red-Black tree.

---

## Question 18
**Can we store null as a key in a TreeMap? Why or why not?**

**Answer:**
No. TreeMap uses compareTo() for sorting, which throws NullPointerException for null keys.

---

## Question 19
**Difference between microservices and monolithic architecture.**

**Answer:**
Monolithic: Single unit, tightly coupled. Microservices: Multiple independent services, loosely coupled, scalable.

---

## Question 20
**Are there any scenarios where monolithic architecture is preferable over microservices?**

**Answer:**
Yes. For small teams, simple applications, or when rapid development is needed, monolithic is easier to manage.

---

## Question 21
**Can a Spring Boot application run without annotations?**

**Answer:**
No. Annotations are essential for configuration and dependency injection in Spring Boot.

---

## Question 22
**What is the DispatcherServlet?**

**Answer:**
Central servlet in Spring MVC that routes requests to controllers.

---

## Question 23
**Explain the annotations @RequestParam and @PathVariable.**

**Answer:**
- @RequestParam: Extracts query parameters.
- @PathVariable: Extracts values from URI path.
Example:
```java
@GetMapping("/user/{id}")
public String getUser(@PathVariable int id, @RequestParam String name) { ... }
```

---

## Question 24
**Can we use both @RequestParam and @PathVariable together in a single method?**

**Answer:**
Yes. See example above.

---

## Question 25
**What is the Spring MVC architecture?**

**Answer:**
It follows Model-View-Controller pattern. DispatcherServlet routes requests, controllers handle logic, models hold data, views render output.

---

## Question 26
**Explain REST principles.**

**Answer:**
REST stands for Representational State Transfer. Principles: statelessness, client-server, cacheable, layered system, uniform interface.

---

## Question 27
**Is REST always stateless? Can REST API maintain a session?**

**Answer:**
REST is designed to be stateless, but sessions can be maintained using tokens or cookies, though it's not recommended.

---

## Question 28
**What happens if a Circuit Breaker remains open for too long?**

**Answer:**
Requests are blocked, service remains unavailable, and users may experience downtime. Should be reset or retried after a timeout.

---

## Question 29
**How will you configure your application if you have different databases for different environments? How do you handle exceptions in your application and in Spring Boot?**

**Answer:**
Use separate config files (application-dev.properties, application-prod.properties). Handle exceptions using @ControllerAdvice and custom exception handlers.

---

## Question 30
**Can the finally block override an exception thrown in the try block?**

**Answer:**
Yes, if finally throws another exception, it overrides the one from try.
```java
try { throw new Exception("A"); }
finally { throw new Exception("B"); } // "B" is thrown
```

---

## Question 31
**How is transaction management handled in your application?**

**Answer:**
Using @Transactional annotation in Spring. It manages commit/rollback automatically.

---

## Question 32
**What is TDD and BDD? Explain the testing pyramid in your project.**

**Answer:**
- TDD: Test Driven Development, write tests before code.
- BDD: Behavior Driven Development, focus on behavior/specs.
Testing pyramid: More unit tests, fewer integration, least UI tests.

---

## Question 33
**What are ACID properties?**

**Answer:**
- Atomicity: All or nothing.
- Consistency: Valid state.
- Isolation: Transactions don't affect each other.
- Durability: Changes persist after commit.

---

## Question 34
**How can you optimize a query that is taking a long time?**

**Answer:**
Use indexes, avoid SELECT *, analyze execution plan, optimize joins, and limit result set.

---

## Question 36
**What monitoring tools have you used in your project?**

**Answer:**
Examples: Prometheus, Grafana, ELK Stack, New Relic, AppDynamics.

---

## Question 37
**Can you describe the CI/CD pipeline you have used in your project?**

**Answer:**
Code pushed to repo, automated build/test, deploy to staging, manual/auto approval, deploy to production. Tools: Jenkins, GitHub Actions, GitLab CI.

---

## Question 38
**What is SDLC?**

**Answer:**
Software Development Life Cycle: Requirement, Design, Development, Testing, Deployment, Maintenance.

---

## Question 39
**Why is Agile preferred over the Waterfall model?**

**Answer:**
Agile is iterative, flexible to changes, delivers value faster. Waterfall is rigid and sequential.

---

## Question 40
**How is Agile followed in your project?**

**Answer:**
Work is divided into sprints, daily standups, regular reviews, continuous feedback, and adaptation.

---
