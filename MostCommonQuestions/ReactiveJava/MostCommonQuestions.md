# Java Reactive Programming Interview Questions - Study Guide

## 🔥 MUST-KNOW Questions (High Priority)

### Reactive Programming Fundamentals
**Q1: What is Reactive Programming? Why do we need it?**

**Q2: What are the core principles of Reactive Programming (Reactive Manifesto)?**

**Q3: What is the difference between imperative and reactive programming?**

**Q4: What are the benefits of reactive programming?**

**Q5: What is backpressure in reactive programming?**

**Q6: What is asynchronous vs synchronous programming?**

**Q7: What is non-blocking I/O? How does it differ from blocking I/O?**

**Q8: What are the main reactive libraries in Java ecosystem?**

### Reactive Streams API
**Q9: What is Reactive Streams specification?**

**Q10: What are the core interfaces in Reactive Streams? (Publisher, Subscriber, Subscription, Processor)**

**Q11: Explain the Publisher-Subscriber pattern in reactive programming.**

**Q12: What is the difference between hot and cold streams?**

---

## 💡 LIKELY Questions (Medium Priority)

### Project Reactor (Core Library)
**Q13: What is Project Reactor? What are Mono and Flux?**

**Q14: When would you use Mono vs Flux?**

**Q15: How do you create Mono and Flux instances?**

**Q16: What are the common operators in Reactor? (map, flatMap, filter, etc.)**

**Q17: What's the difference between map and flatMap?**

**Q18: How do you handle errors in reactive streams?**

**Q19: What are schedulers in Project Reactor?**

**Q20: What's the difference between subscribeOn and publishOn?**

### Spring WebFlux
**Q21: What is Spring WebFlux? How is it different from Spring MVC?**

**Q22: When should you choose WebFlux over Spring MVC?**

**Q23: What are the two programming models in WebFlux?**

**Q24: How do you create reactive REST APIs using WebFlux?**

**Q25: What is RouterFunction and HandlerFunction?**

**Q26: How do you handle reactive database operations?**

**Q27: What is WebClient? How is it different from RestTemplate?**

---

## 🎯 SCENARIO-BASED Questions (Medium Priority)

### Practical Implementation
**Q28: How would you convert a blocking operation to reactive?**

**Q29: How do you handle multiple parallel API calls reactively?**

**Q30: How would you implement retry logic in reactive programming?**

**Q31: How do you combine multiple reactive streams?**

**Q32: How do you implement timeout in reactive streams?**

**Q33: How would you implement caching in reactive applications?**

**Q34: How do you test reactive code?**

### Performance & Optimization
**Q35: How does reactive programming improve performance?**

**Q36: What are the threading implications in reactive programming?**

**Q37: How do you debug reactive applications?**

**Q38: What are common pitfalls in reactive programming?**

**Q39: How do you monitor reactive applications?**

---

## 📝 CODING Questions (Be Ready to Write)

**Q40: Create a simple Flux that emits numbers 1 to 5.**

**Q41: Transform a List to Flux and apply map operation.**

**Q42: Create a reactive REST endpoint using WebFlux.**

**Q43: Implement error handling in a reactive stream.**

**Q44: Create a Mono that makes an HTTP call using WebClient.**

**Q45: Combine two Flux streams using zip operation.**

---

## 🔧 ADVANCED Questions (Lower Priority)

### Advanced Operators & Patterns
**Q46: What are advanced operators like window, buffer, groupBy?**

**Q47: What is the difference between merge and concat?**

**Q48: How do you implement custom operators?**

**Q49: What are reactive design patterns?**

**Q50: How do you handle context in reactive streams?**

### Integration & Architecture
**Q51: How do you integrate reactive programming with databases?**

**Q52: What is R2DBC? How is it different from JDBC?**

**Q53: How do you implement reactive microservices?**

**Q54: How do you handle distributed transactions in reactive systems?**

**Q55: How do you implement reactive security?**

---

## ⚡ QUICK-FIRE Questions (Know the Answers)

**Q56: What does "reactive" mean in Reactive Manifesto?**
*Answer: Responsive, Resilient, Elastic, Message-driven*

**Q57: What is the default scheduler for Reactor operations?**
*Answer: Schedulers.parallel()*

**Q58: Can you block in reactive code?**
*Answer: No, it defeats the purpose and can cause issues*

**Q59: What happens if you don't subscribe to a reactive stream?**
*Answer: Nothing - reactive streams are lazy*

**Q60: What's the difference between Mono.empty() and Mono.just(null)?**
*Answer: empty() completes without value, just(null) emits null*

**Q61: What port does WebFlux use by default?**
*Answer: 8080 (same as Spring Boot)*

---

## 🎪 ECOSYSTEM Questions (If Time Permits)

### Other Reactive Libraries
**Q62: What is RxJava? How is it different from Project Reactor?**

**Q63: What is Akka? When would you use it?**

**Q64: What is Vert.x? How does it support reactive programming?**

**Q65: What are reactive database drivers?**

### Modern Java Features
**Q66: How do CompletableFuture and reactive programming relate?**

**Q67: What are Virtual Threads (Project Loom) and how do they affect reactive programming?**

**Q68: How do Streams API and reactive streams differ?**

**Q69: What is Flow API in Java 9?**

---

## 📋 PRIORITY STUDY ORDER

### Week 1 (Essential - Focus Here First)
- Q1-Q12 (Reactive fundamentals and Reactive Streams)
- Understand the "why" behind reactive programming
- Learn basic Mono and Flux concepts

### Week 2 (If You Have More Time)
- Q13-Q27 (Project Reactor and Spring WebFlux)
- Q28-Q39 (Practical scenarios and performance)
- Practice coding questions Q40-Q45

### Additional Time
- Q46-Q69 (Advanced topics and ecosystem)

---

## 🚀 INTERVIEW SURVIVAL TIPS

### Core Concepts You Must Explain Clearly:
1. **Reactive Programming**: Asynchronous programming with data streams and event propagation
2. **Non-blocking**: Operations don't block threads while waiting for I/O
3. **Backpressure**: Mechanism to handle slow consumers
4. **Mono vs Flux**: Single value vs stream of values
5. **Publisher-Subscriber**: Core pattern for reactive streams

### Safe Answers to Have Ready:
- "Reactive programming is about building asynchronous, non-blocking applications"
- "I understand the benefits of reactive programming for scalable applications"
- "I'm familiar with Project Reactor and Spring WebFlux concepts"
- "I know how reactive programming helps with resource efficiency"

### When Asked About Experience:
- "I understand reactive programming principles and have studied Project Reactor"
- "I'm familiar with the difference between blocking and non-blocking operations"
- "I know when reactive programming is beneficial and when traditional approaches are better"

### Code Writing Strategy:
- Start with simple Mono/Flux creation
- Use common operators like map, filter, flatMap
- Always remember to subscribe to make streams work
- Explain the non-blocking nature of operations

---

## 🎯 MINIMUM VIABLE PREPARATION

**If you have LIMITED time, focus ONLY on these 15 questions:**
Q1, Q2, Q3, Q5, Q9, Q10, Q13, Q14, Q15, Q21, Q22, Q24, Q27, Q40, Q41

**Master these and you can handle 70% of Java Reactive interviews!**

---

## 📚 QUICK REFERENCE - KEY DEFINITIONS

**Reactive Programming**: Programming paradigm focused on asynchronous data streams and event propagation
**Mono**: Reactive type representing 0 or 1 element
**Flux**: Reactive type representing 0 to N elements
**Publisher**: Produces data stream
**Subscriber**: Consumes data stream
**Backpressure**: Mechanism to handle fast publishers and slow subscribers
**Non-blocking**: Operations that don't block the calling thread
**WebFlux**: Spring's reactive web framework
**WebClient**: Reactive HTTP client
**Scheduler**: Manages threading in reactive operations

---

## 🔑 ESSENTIAL CODE PATTERNS TO KNOW

### Basic Mono/Flux Creation
```java
// Creating Mono
Mono<String> mono = Mono.just("Hello");
Mono<String> empty = Mono.empty();
Mono<String> error = Mono.error(new Exception("Error"));

// Creating Flux
Flux<Integer> flux = Flux.just(1, 2, 3, 4, 5);
Flux<Integer> range = Flux.range(1, 10);
Flux<String> fromList = Flux.fromIterable(Arrays.asList("a", "b", "c"));
```

### Common Operators
```java
// Transformation
Flux<Integer> numbers = Flux.range(1, 5)
    .map(n -> n * 2)                    // Transform each element
    .filter(n -> n > 5)                 // Filter elements
    .doOnNext(System.out::println);     // Side effect

// FlatMap for async operations
Flux<String> result = Flux.just(1, 2, 3)
    .flatMap(n -> callAsyncService(n))  // Flatten async results
    .subscribe();
```

### Error Handling
```java
Flux<String> withErrorHandling = Flux.just("1", "2", "invalid", "4")
    .map(Integer::parseInt)
    .map(n -> "Number: " + n)
    .onErrorContinue((error, item) -> {
        System.err.println("Error processing: " + item);
    });
```

### WebFlux Controller
```java
@RestController
public class ReactiveController {
    
    @GetMapping("/users")
    public Flux<User> getUsers() {
        return userService.findAll();  // Returns Flux<User>
    }
    
    @GetMapping("/users/{id}")
    public Mono<User> getUser(@PathVariable String id) {
        return userService.findById(id);  // Returns Mono<User>
    }
    
    @PostMapping("/users")
    public Mono<User> createUser(@RequestBody Mono<User> userMono) {
        return userMono.flatMap(userService::save);
    }
}
```

### WebClient Usage
```java
WebClient client = WebClient.create("http://api.example.com");

Mono<String> response = client
    .get()
    .uri("/users/{id}", userId)
    .retrieve()
    .bodyToMono(String.class);
```

---

## 🎲 COMMON REACTIVE PATTERNS TO PRACTICE

### Combining Streams
```java
// Zip - combine latest from each stream
Mono<String> combined = Mono.zip(mono1, mono2)
    .map(tuple -> tuple.getT1() + tuple.getT2());

// Merge - merge multiple streams
Flux<String> merged = Flux.merge(flux1, flux2, flux3);
```

### Async Processing
```java
Flux<String> async = Flux.range(1, 10)
    .flatMap(n -> processAsync(n))  // Process each element asynchronously
    .subscribeOn(Schedulers.parallel());  // Use parallel scheduler
```

### Retry Logic
```java
Mono<String> withRetry = webClient
    .get()
    .uri("/api/data")
    .retrieve()
    .bodyToMono(String.class)
    .retry(3)  // Retry up to 3 times
    .timeout(Duration.ofSeconds(5));  // Timeout after 5 seconds
```

---

## 🏗️ REACTIVE MANIFESTO PRINCIPLES

1. **Responsive**: System responds in a timely manner
2. **Resilient**: System stays responsive in the face of failure
3. **Elastic**: System stays responsive under varying workload
4. **Message-driven**: System relies on asynchronous message-passing

---

## ⚠️ COMMON INTERVIEW MISTAKES TO AVOID

- Don't confuse reactive programming with multi-threading
- Remember that reactive streams are lazy - nothing happens without subscription
- Don't block in reactive code (avoid .block() in production)
- Understand the difference between hot and cold streams
- Know when NOT to use reactive programming (simple CRUD apps)
- Don't forget to handle errors properly in reactive streams
- Remember that WebFlux is not always better than Spring MVC
- Understand that reactive programming has a learning curve
- Know that debugging reactive code can be challenging
- Don't ignore backpressure handling in high-throughput scenarios

---

## 🔥 WHEN TO USE REACTIVE PROGRAMMING

### Good Use Cases:
- High concurrency applications
- I/O intensive operations
- Streaming data processing
- Microservices with many service calls
- Real-time applications
- Applications with unpredictable load

### When NOT to Use:
- Simple CRUD applications
- CPU-intensive tasks
- Small applications with low concurrency
- When team lacks reactive programming experience
- Legacy systems with blocking dependencies

---

## 🎯 KEY DIFFERENCES TO REMEMBER

### Reactive vs Traditional
| Traditional | Reactive |
|------------|----------|
| Blocking I/O | Non-blocking I/O |
| Thread-per-request | Event loop |
| Pull-based | Push-based |
| Synchronous | Asynchronous |
| Resource intensive | Resource efficient |

### Spring MVC vs WebFlux
| Spring MVC | WebFlux |
|-----------|---------|
| Servlet API | Reactive Streams |
| Blocking | Non-blocking |
| Thread pool | Event loop |
| RestTemplate | WebClient |
| @RequestMapping | @RequestMapping + RouterFunction |
