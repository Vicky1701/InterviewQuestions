# Java Multithreading Interview Questions - Complete Guide

## 📚 ORAL/THEORETICAL QUESTIONS

### Basic Level (1-15)

1. **What is multithreading in Java? What are its advantages?**
2. **What is the difference between Thread and Runnable interface?**
3. **What are the different states of a thread in Java?**
4. **What is the difference between start() and run() method?**
5. **What is thread synchronization? Why is it needed?**
6. **What is race condition? How can it be prevented?**
7. **What is the synchronized keyword? How does it work?**
8. **What is the difference between synchronized method and synchronized block?**
9. **What is deadlock? How can it be prevented?**
10. **What is the difference between wait() and sleep() methods?**
11. **What is the purpose of join() method?**
12. **What is daemon thread? How to create one?**
13. **What is ThreadLocal? When would you use it?**
14. **What is the difference between notify() and notifyAll()?**
15. **What is volatile keyword? How does it work?**

### Intermediate Level (16-35)

16. **What is the difference between synchronized and volatile?**
17. **What is the happens-before relationship in Java?**
18. **What is the Java Memory Model?**
19. **What is thread starvation? How can it be avoided?**
20. **What is priority inversion? How does Java handle thread priorities?**
21. **What is the difference between interrupting a thread and stopping it?**
22. **What is the Executor framework? What are its benefits?**
23. **What is the difference between ExecutorService and ThreadPoolExecutor?**
24. **What are the different types of thread pools in Java?**
25. **What is Future and CompletableFuture? How do they differ?**
26. **What is the difference between CountDownLatch and CyclicBarrier?**
27. **What is Semaphore? When would you use it?**
28. **What is ReentrantLock? How does it differ from synchronized?**
29. **What is ReentrantReadWriteLock? When is it useful?**
30. **What is the producer-consumer problem? How to solve it in Java?**
31. **What is BlockingQueue? What are its implementations?**
32. **What is the difference between fail-fast and fail-safe iterators?**
33. **What is ConcurrentHashMap? How does it achieve thread safety?**
34. **What is the difference between Collections.synchronizedMap() and ConcurrentHashMap?**
35. **What is fork-join framework? How does it work?**

### Advanced Level (36-50)

36. **What is lock-free programming? How is it implemented in Java?**
37. **What are atomic classes in Java? How do they work?**
38. **What is the ABA problem? How can it be solved?**
39. **What is memory consistency error? How to avoid it?**
40. **What is the double-checked locking pattern? What are its issues?**
41. **What is the compare-and-swap (CAS) operation?**
42. **What is false sharing? How to avoid it in Java?**
43. **What is the difference between parallel and concurrent processing?**
44. **What is work-stealing in ForkJoinPool?**
45. **What is StampedLock? How does it differ from ReadWriteLock?**
46. **What is the difference between blocking and non-blocking algorithms?**
47. **What is the Java 8 Stream API parallel processing?**
48. **What is CompletableFuture chaining and composition?**
49. **What is the difference between asynchronous and parallel processing?**
50. **What are the best practices for writing thread-safe code in Java?**

## 💻 IMPLEMENTATION QUESTIONS

### Basic Implementation (1-15)

1. **Create a simple thread using Thread class**
```java
// Implement a thread that prints numbers 1-10
```

2. **Create a thread using Runnable interface**
```java
// Implement using Runnable and demonstrate the difference
```

3. **Implement alternating odd-even number printing**
```java
// Two threads print odd and even numbers alternately up to 100
```

4. **Create a thread-safe counter**
```java
// Implement a counter that can be safely incremented by multiple threads
```

5. **Demonstrate race condition and fix it**
```java
// Show race condition, then fix using synchronization
```

6. **Implement producer-consumer using wait() and notify()**
```java
// Basic producer-consumer with shared buffer
```

7. **Create a simple thread pool**
```java
// Implement basic thread pool with fixed number of threads
```

8. **Demonstrate deadlock between two threads**
```java
// Create and then resolve deadlock situation
```

9. **Implement thread-safe singleton pattern**
```java
// Different approaches: synchronized, double-checked locking, enum
```

10. **Create a thread that can be interrupted**
```java
// Implement proper thread interruption handling
```

11. **Implement custom thread with lifecycle management**
```java
// Thread that can be paused, resumed, and stopped gracefully
```

12. **Create a thread-safe stack**
```java
// Implement push/pop operations with synchronization
```

13. **Implement a simple barrier synchronization**
```java
// Make multiple threads wait for each other
```

14. **Create a thread-safe queue**
```java
// Implement enqueue/dequeue with proper synchronization
```

15. **Implement thread communication using wait/notify**
```java
// Two threads communicating through shared object
```

### Intermediate Implementation (16-35)

16. **Implement producer-consumer using BlockingQueue**
```java
// Use ArrayBlockingQueue or LinkedBlockingQueue
```

17. **Create a thread-safe LRU cache**
```java
// Implement with proper synchronization
```

18. **Implement dining philosophers problem**
```java
// Solve with proper deadlock prevention
```

19. **Create a custom CountDownLatch**
```java
// Implement your own version of CountDownLatch
```

20. **Implement readers-writers problem**
```java
// Multiple readers, single writer with ReentrantReadWriteLock
```

21. **Create a thread pool with dynamic sizing**
```java
// Pool that grows and shrinks based on workload
```

22. **Implement a concurrent hash table**
```java
// Thread-safe hash table implementation
```

23. **Create a timeout-based blocking queue**
```java
// Queue with timeout for put/take operations
```

24. **Implement parallel merge sort**
```java
// Use multiple threads to sort different parts
```

25. **Create a thread-safe object pool**
```java
// Pool of reusable objects with proper synchronization
```

26. **Implement a simple MapReduce framework**
```java
// Basic map-reduce using thread pools
```

27. **Create a event-driven thread communication system**
```java
// Observer pattern with multiple threads
```

28. **Implement a concurrent circular buffer**
```java
// Ring buffer with multiple producers and consumers
```

29. **Create a thread-safe logger with batching**
```java
// Logger that batches log entries for performance
```

30. **Implement a simple scheduler**
```java
// Schedule tasks to run at specific times
```

31. **Create a thread-safe reference counter**
```java
// Reference counting with proper memory management
```

32. **Implement a concurrent skip list**
```java
// Lock-free or fine-grained locking skip list
```

33. **Create a parallel matrix multiplication**
```java
// Multiply large matrices using multiple threads
```

34. **Implement a work-stealing queue**
```java
// Queue where idle threads can steal work
```

35. **Create a thread-safe connection pool**
```java
// Database connection pool with proper resource management
```

### Advanced Implementation (36-50)

36. **Implement a lock-free stack using atomic operations**
```java
// Use AtomicReference and CAS operations
```

37. **Create a lock-free queue (Michael & Scott algorithm)**
```java
// Implement using atomic references
```

38. **Implement a non-blocking cache with CAS**
```java
// Cache that doesn't use locks
```

39. **Create a parallel quicksort using ForkJoinPool**
```java
// Use divide-and-conquer with work stealing
```

40. **Implement a futures/promises mechanism**
```java
// Custom implementation of Future pattern
```

41. **Create a thread-safe memory allocator**
```java
// Custom memory pool with thread safety
```

42. **Implement a distributed task executor**
```java
// Execute tasks across multiple JVMs
```

43. **Create a reactive stream processor**
```java
// Process stream of data reactively
```

44. **Implement a lock-free hash table**
```java
// Using atomic operations and clever algorithms
```

45. **Create a parallel tree traversal**
```java
// Traverse large tree structures in parallel
```

46. **Implement a thread-safe bloom filter**
```java
// Probabilistic data structure with thread safety
```

47. **Create a concurrent trie (prefix tree)**
```java
// Thread-safe implementation of trie
```

48. **Implement a parallel graph algorithms**
```java
// BFS/DFS using multiple threads
```

49. **Create a high-performance message passing system**
```java
// Inter-thread communication with minimal overhead
```

50. **Implement a custom synchronization primitive**
```java
// Create your own lock or barrier using low-level primitives
```

## 🎯 JAVA-SPECIFIC CONCEPTS TO MASTER

### Core Threading APIs
- **Thread class methods**: start(), run(), join(), interrupt(), isAlive()
- **Runnable interface**: lambda expressions, method references
- **Object methods**: wait(), notify(), notifyAll()
- **Thread states**: NEW, RUNNABLE, BLOCKED, WAITING, TIMED_WAITING, TERMINATED

### Synchronization Mechanisms
- **synchronized keyword**: methods, blocks, static synchronization
- **volatile keyword**: visibility, happens-before relationship
- **ReentrantLock**: lock(), unlock(), tryLock(), condition variables
- **ReentrantReadWriteLock**: read/write lock separation
- **StampedLock**: optimistic reading capability

### Executor Framework
- **ExecutorService**: submit(), invokeAll(), invokeAny(), shutdown()
- **ThreadPoolExecutor**: core pool size, maximum pool size, keep-alive time
- **ScheduledExecutorService**: schedule(), scheduleAtFixedRate()
- **ForkJoinPool**: work-stealing, parallel streams

### Concurrent Collections
- **ConcurrentHashMap**: segment-based locking, computeIfAbsent()
- **CopyOnWriteArrayList**: read-heavy scenarios
- **BlockingQueue implementations**: ArrayBlockingQueue, LinkedBlockingQueue
- **ConcurrentLinkedQueue**: lock-free implementation

### Atomic Classes
- **AtomicInteger/Long/Boolean**: compareAndSet(), getAndIncrement()
- **AtomicReference**: CAS operations, ABA problem
- **AtomicIntegerArray**: atomic operations on arrays
- **LongAdder/DoubleAdder**: high-contention scenarios

### Synchronization Utilities
- **CountDownLatch**: one-time barrier
- **CyclicBarrier**: reusable barrier with optional action
- **Semaphore**: resource access control
- **Exchanger**: data exchange between threads
- **Phaser**: flexible multi-phase barrier

## 📝 SAMPLE INTERVIEW SCENARIOS

### Scenario 1: System Design
**Question**: "Design a thread-safe cache with TTL (Time To Live) functionality"
**Expected**: Discuss ConcurrentHashMap, scheduled cleanup, memory management

### Scenario 2: Performance Optimization
**Question**: "You have a multi-threaded application with poor performance. How would you diagnose and fix it?"
**Expected**: Profiling tools, identifying bottlenecks, lock contention analysis

### Scenario 3: Debugging
**Question**: "Your application occasionally hangs. How would you debug this?"
**Expected**: Thread dumps, deadlock detection, monitoring tools

### Scenario 4: Architecture Decision
**Question**: "When would you choose CompletableFuture over traditional threading?"
**Expected**: Asynchronous programming, reactive systems, composition

## 🚀 STUDY PLAN

### Week 1-2: Foundation
- Basic thread concepts and simple implementations
- Questions 1-15 (Oral) and 1-15 (Implementation)
- Focus on: Thread, Runnable, synchronization basics

### Week 3-4: Intermediate Concepts
- Executor framework and concurrent collections
- Questions 16-35 (Oral) and 16-35 (Implementation)
- Focus on: ExecutorService, BlockingQueue, locks

### Week 5-6: Advanced Topics
- Atomic classes and lock-free programming
- Questions 36-50 (Oral) and 36-50 (Implementation)
- Focus on: CAS operations, performance optimization

### Week 7-8: Integration & Practice
- Complex scenarios and system design
- Mock interviews with mixed questions
- Focus on: Real-world applications, debugging

## 💡 CODING BEST PRACTICES

### Always Remember:
1. **Use higher-level constructs** when possible (ExecutorService over Thread)
2. **Avoid synchronization** when you can use concurrent collections
3. **Handle interruptions** properly in long-running tasks
4. **Use try-with-resources** for automatic resource management
5. **Prefer immutable objects** to reduce synchronization needs
6. **Test thoroughly** with multiple threads and edge cases
7. **Document thread safety** guarantees in your code
8. **Use appropriate visibility modifiers** (volatile, synchronized)

### Common Pitfalls to Avoid:
- Calling run() instead of start()
- Not handling InterruptedException properly
- Creating too many threads
- Improper use of wait/notify
- Ignoring thread safety in constructors
- Memory leaks in thread pools
- Deadlocks due to lock ordering

Good luck with your Java multithreading interview preparation! Focus on understanding the concepts deeply and practice implementing the solutions yourself.
