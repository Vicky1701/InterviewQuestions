# Java Multithreading Interview Questions - Complete Answers Guide (Part 4 - Final)

## 📝 CONCURRENT COLLECTIONS Questions (Continued)

**Q33: What is the difference between HashMap, Hashtable, and ConcurrentHashMap?**

**Answer:**

**Comprehensive Comparison:**

| Feature | HashMap | Hashtable | ConcurrentHashMap |
|---------|---------|-----------|-------------------|
| **Thread Safety** | ❌ Not thread-safe | ✅ Thread-safe | ✅ Thread-safe |
| **Synchronization** | None | Method level | Segment/CAS based |
| **Performance** | Fastest (single-thread) | Slowest | Fast (multi-thread) |
| **Null Values** | ✅ Allows null key/values | ❌ No nulls | ❌ No nulls |
| **Inheritance** | AbstractMap | Dictionary (legacy) | AbstractMap |
| **Introduced** | Java 1.2 | Java 1.0 | Java 1.5 |
| **Lock Mechanism** | N/A | Synchronized methods | Fine-grained locking |
| **Concurrent Reads** | N/A | Blocked | ✅ Non-blocking |
| **Fail-Fast Iterator** | ✅ Yes | ✅ Yes | ❌ No (fail-safe) |

**Detailed Code Examples:**

```java
import java.util.*;
import java.util.concurrent.*;

class MapComparison {
    
    // HashMap - Not thread-safe
    public static void hashMapExample() {
        Map<String, Integer> hashMap = new HashMap<>();
        
        // ✅ Allows null keys and values
        hashMap.put(null, 100);
        hashMap.put("key1", null);
        hashMap.put("key2", 200);
        
        System.out.println("HashMap: " + hashMap);
        
        // ❌ Race condition in multithreaded environment
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                hashMap.put("thread-" + Thread.currentThread().getName() + "-" + i, i);
            }
        };
        
        // This will cause race conditions and unpredictable results
        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start();
        t2.start();
    }
    
    // Hashtable - Thread-safe but slow
    public static void hashtableExample() {
        Map<String, Integer> hashtable = new Hashtable<>();
        
        // ❌ Doesn't allow null keys or values
        try {
            hashtable.put(null, 100); // NullPointerException
        } catch (NullPointerException e) {
            System.out.println("Hashtable doesn't allow null keys");
        }
        
        try {
            hashtable.put("key1", null); // NullPointerException
        } catch (NullPointerException e) {
            System.out.println("Hashtable doesn't allow null values");
        }
        
        // ✅ Thread-safe but blocks all operations
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                hashtable.put("thread-" + Thread.currentThread().getName() + "-" + i, i);
                hashtable.get("key-" + i); // This blocks if another thread is writing
            }
        };
        
        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start();
        t2.start();
    }
    
    // ConcurrentHashMap - Thread-safe and fast
    public static void concurrentHashMapExample() {
        Map<String, Integer> concurrentMap = new ConcurrentHashMap<>();
        
        // ❌ Doesn't allow null keys or values
        try {
            concurrentMap.put(null, 100); // NullPointerException
        } catch (NullPointerException e) {
            System.out.println("ConcurrentHashMap doesn't allow null keys");
        }
        
        // ✅ Thread-safe with better performance
        Runnable task = () -> {
            for (int i = 0; i < 1000; i++) {
                concurrentMap.put("thread-" + Thread.currentThread().getName() + "-" + i, i);
                concurrentMap.get("key-" + i); // Non-blocking reads
            }
        };
        
        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        t1.start();
        t2.start();
    }
}
```

**Performance Benchmark:**

```java
class PerformanceBenchmark {
    public static void main(String[] args) {
        int numThreads = 10;
        int operationsPerThread = 10000;
        
        // Test HashMap with external synchronization
        Map<Integer, Integer> hashMap = Collections.synchronizedMap(new HashMap<>());
        long hashMapTime = benchmarkMap("SynchronizedHashMap", hashMap, numThreads, operationsPerThread);
        
        // Test Hashtable
        Map<Integer, Integer> hashtable = new Hashtable<>();
        long hashtableTime = benchmarkMap("Hashtable", hashtable, numThreads, operationsPerThread);
        
        // Test ConcurrentHashMap
        Map<Integer, Integer> concurrentMap = new ConcurrentHashMap<>();
        long concurrentTime = benchmarkMap("ConcurrentHashMap", concurrentMap, numThreads, operationsPerThread);
        
        System.out.println("\nPerformance Results:");
        System.out.println("SynchronizedHashMap: " + hashMapTime + "ms");
        System.out.println("Hashtable: " + hashtableTime + "ms");
        System.out.println("ConcurrentHashMap: " + concurrentTime + "ms");
        
        // ConcurrentHashMap is typically 2-5x faster than others
    }
    
    private static long benchmarkMap(String name, Map<Integer, Integer> map, 
                                   int numThreads, int operationsPerThread) {
        CountDownLatch latch = new CountDownLatch(numThreads);
        long startTime = System.currentTimeMillis();
        
        for (int i = 0; i < numThreads; i++) {
            final int threadId = i;
            new Thread(() -> {
                try {
                    Random random = new Random();
                    for (int j = 0; j < operationsPerThread; j++) {
                        int key = random.nextInt(1000);
                        if (j % 2 == 0) {
                            map.put(key, threadId * 1000 + j);
                        } else {
                            map.get(key);
                        }
                    }
                } finally {
                    latch.countDown();
                }
            }).start();
        }
        
        try {
            latch.await();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        
        return System.currentTimeMillis() - startTime;
    }
}
```

**Iterator Behavior:**

```java
class IteratorBehavior {
    public static void demonstrateIterators() {
        // HashMap - Fail-fast iterator
        Map<String, Integer> hashMap = new HashMap<>();
        hashMap.put("a", 1);
        hashMap.put("b", 2);
        hashMap.put("c", 3);
        
        Iterator<Map.Entry<String, Integer>> hashMapIterator = hashMap.entrySet().iterator();
        hashMap.put("d", 4); // Modify during iteration
        
        try {
            while (hashMapIterator.hasNext()) {
                System.out.println(hashMapIterator.next()); // ConcurrentModificationException
            }
        } catch (ConcurrentModificationException e) {
            System.out.println("HashMap: ConcurrentModificationException caught");
        }
        
        // ConcurrentHashMap - Fail-safe iterator
        Map<String, Integer> concurrentMap = new ConcurrentHashMap<>();
        concurrentMap.put("a", 1);
        concurrentMap.put("b", 2);
        concurrentMap.put("c", 3);
        
        Iterator<Map.Entry<String, Integer>> concurrentIterator = concurrentMap.entrySet().iterator();
        concurrentMap.put("d", 4); // Modify during iteration
        
        System.out.println("ConcurrentHashMap iteration:");
        while (concurrentIterator.hasNext()) {
            System.out.println(concurrentIterator.next()); // No exception, may or may not see new entry
        }
    }
}
```

**When to Use Which:**

```java
class MapUsageGuidelines {
    
    // ✅ Use HashMap when:
    public void singleThreadedApplication() {
        Map<String, User> userCache = new HashMap<>(); // Best performance for single thread
        
        // Single-threaded operations
        userCache.put("user1", new User("John"));
        User user = userCache.get("user1");
    }
    
    // ✅ Use ConcurrentHashMap when:
    public void multiThreadedApplication() {
        Map<String, UserSession> activeSessions = new ConcurrentHashMap<>(); // High concurrency
        
        // Multiple threads can safely access
        activeSessions.computeIfAbsent("session1", k -> new UserSession());
        activeSessions.computeIfPresent("session1", (k, v) -> v.updateActivity());
    }
    
    // ❌ Avoid Hashtable unless:
    public void legacyCodeCompatibility() {
        // Only use for legacy code compatibility
        Hashtable<String, String> legacyMap = new Hashtable<>();
        
        // Prefer ConcurrentHashMap for new development
    }
    
    // Use synchronized wrapper when:
    public void specificSynchronizationNeeds() {
        Map<String, String> syncMap = Collections.synchronizedMap(new HashMap<>());
        
        // When you need to synchronize multiple operations together
        synchronized(syncMap) {
            if (!syncMap.containsKey("key")) {
                syncMap.put("key", "value");
            }
        }
    }
}
```

**Memory and Performance Characteristics:**

```java
class MemoryAndPerformanceAnalysis {
    
    // Memory overhead comparison
    public static void memoryAnalysis() {
        // HashMap: Lowest memory overhead
        // - Simple Node structure
        // - No synchronization overhead
        
        // Hashtable: Medium memory overhead
        // - Similar to HashMap but with synchronization
        // - Method-level synchronization metadata
        
        // ConcurrentHashMap: Slightly higher memory overhead
        // - Additional fields for concurrency control
        // - CAS operation support structures
    }
    
    // Scalability comparison
    public static void scalabilityAnalysis() {
        // HashMap: 
        // - Perfect for single thread
        // - Not usable in multithreaded without external sync
        
        // Hashtable:
        // - Doesn't scale with thread count
        // - All operations synchronized on single lock
        
        // ConcurrentHashMap:
        // - Scales well with thread count
        // - Lock-free reads, fine-grained write locks
        
        measureScalability();
    }
    
    private static void measureScalability() {
        int[] threadCounts = {1, 2, 4, 8, 16};
        
        for (int threads : threadCounts) {
            System.out.println("\n--- Testing with " + threads + " threads ---");
            
            // Test each map type
            testMapScalability("Hashtable", new Hashtable<>(), threads);
            testMapScalability("ConcurrentHashMap", new ConcurrentHashMap<>(), threads);
        }
    }
    
    private static void testMapScalability(String name, Map<Integer, Integer> map, int threadCount) {
        CountDownLatch latch = new CountDownLatch(threadCount);
        long startTime = System.currentTimeMillis();
        
        for (int i = 0; i < threadCount; i++) {
            final int threadId = i;
            new Thread(() -> {
                try {
                    for (int j = 0; j < 10000; j++) {
                        map.put(threadId * 10000 + j, j);
                        map.get(threadId * 10000 + j);
                    }
                } finally {
                    latch.countDown();
                }
            }).start();
        }
        
        try {
            latch.await();
            long duration = System.currentTimeMillis() - startTime;
            System.out.println(name + " with " + threadCount + " threads: " + duration + "ms");
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

**Key Takeaways:**

1. **HashMap:** Single-threaded applications, best performance
2. **Hashtable:** Legacy support only, avoid in new code
3. **ConcurrentHashMap:** Multi-threaded applications, best concurrent performance
4. **Collections.synchronizedMap():** When you need to synchronize compound operations
5. **Null handling:** Only HashMap allows nulls
6. **Iterators:** HashMap/Hashtable are fail-fast, ConcurrentHashMap is fail-safe

---

**Q34: What is BlockingQueue and its implementations?**

**Answer:**

**BlockingQueue:** A thread-safe queue that supports operations that wait for the queue to become non-empty when retrieving elements, and wait for space to become available when storing elements.

**Key Features:**

- **Thread-safe** by design
- **Blocking operations** - automatically handles wait/notify
- **Bounded or unbounded** capacity
- **Producer-Consumer pattern** support
- **Null elements not allowed**

**Core Methods:**

```java
interface BlockingQueueMethods<E> {
    // Blocking operations (wait until possible)
    void put(E element) throws InterruptedException;    // Block until space available
    E take() throws InterruptedException;               // Block until element available
    
    // Non-blocking operations with return values
    boolean offer(E element);                           // Return false if no space
    E poll();                                           // Return null if no element
    
    // Timed operations (wait for specified time)
    boolean offer(E element, long timeout, TimeUnit unit) throws InterruptedException;
    E poll(long timeout, TimeUnit unit) throws InterruptedException;
    
    // Examination operations
    E peek();                                           // Look at head without removing
    int size();                                         // Current size
    int remainingCapacity();                            // Available space
}
```

**BlockingQueue Implementations:**

**1. ArrayBlockingQueue - Fixed Size**

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

class ArrayBlockingQueueExample {
    public static void main(String[] args) {
        // Fixed capacity queue
        BlockingQueue<String> queue = new ArrayBlockingQueue<>(5);
        
        // Producer thread
        Thread producer = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    String item = "Item-" + i;
                    queue.put(item); // Blocks when queue is full
                    System.out.println("Produced: " + item + ", Queue size: " + queue.size());
                    Thread.sleep(100);
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }, "Producer");
        
        // Consumer thread
        Thread consumer = new Thread(() -> {
            try {
                while (true) {
                    String item = queue.take(); // Blocks when queue is empty
                    System.out.println("Consumed: " + item + ", Queue size: " + queue.size());
                    Thread.sleep(200); // Slower consumer
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }, "Consumer");
        
        producer.start();
        consumer.start();
        
        // Demonstration of blocking behavior
        demonstrateBlockingBehavior();
    }
    
    private static void demonstrateBlockingBehavior() {
        BlockingQueue<Integer> smallQueue = new ArrayBlockingQueue<>(2);
        
        try {
            // Fill the queue
            smallQueue.put(1);
            smallQueue.put(2);
            System.out.println("Queue filled. Size: " + smallQueue.size());
            
            // This will block because queue is full
            System.out.println("Trying to add third element (this will block)...");
            // smallQueue.put(3); // Would block indefinitely
            
            // Use offer with timeout instead
            boolean added = smallQueue.offer(3, 1, TimeUnit.SECONDS);
            System.out.println("Third element added: " + added); // false
            
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

**2. LinkedBlockingQueue - Dynamic Size**

```java
import java.util.concurrent.LinkedBlockingQueue;

class LinkedBlockingQueueExample {
    public static void main(String[] args) {
        // Unbounded queue (default)
        BlockingQueue<Task> unboundedQueue = new LinkedBlockingQueue<>();
        
        // Bounded queue
        BlockingQueue<Task> boundedQueue = new LinkedBlockingQueue<>(100);
        
        // Task processing example
        TaskProcessor processor = new TaskProcessor(boundedQueue);
        processor.start();
        
        // Submit tasks
        for (int i = 0; i < 20; i++) {
            processor.submitTask(new Task("Task-" + i));
        }
        
        processor.shutdown();
    }
    
    static class TaskProcessor {
        private final BlockingQueue<Task> taskQueue;
        private final ExecutorService executor;
        private volatile boolean shutdown = false;
        
        public TaskProcessor(BlockingQueue<Task> taskQueue) {
            this.taskQueue = taskQueue;
            this.executor = Executors.newFixedThreadPool(3);
        }
        
        public void start() {
            // Start multiple consumer threads
            for (int i = 0; i < 3; i++) {
                executor.submit(() -> {
                    while (!shutdown || !taskQueue.isEmpty()) {
                        try {
                            Task task = taskQueue.poll(1, TimeUnit.SECONDS);
                            if (task != null) {
                                processTask(task);
                            }
                        } catch (InterruptedException e) {
                            Thread.currentThread().interrupt();
                            break;
                        }
                    }
                });
            }
        }
        
        public void submitTask(Task task) {
            if (!shutdown) {
                try {
                    taskQueue.put(task);
                    System.out.println("Submitted: " + task.getName());
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        }
        
        private void processTask(Task task) {
            try {
                System.out.println("Processing: " + task.getName() + 
                                 " by " + Thread.currentThread().getName());
                Thread.sleep(500); // Simulate work
                System.out.println("Completed: " + task.getName());
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        
        public void shutdown() {
            shutdown = true;
            executor.shutdown();
        }
    }
    
    static class Task {
        private final String name;
        
        public Task(String name) {
            this.name = name;
        }
        
        public String getName() {
            return name;
        }
    }
}
```

**3. PriorityBlockingQueue - Priority-based**

```java
import java.util.concurrent.PriorityBlockingQueue;

class PriorityBlockingQueueExample {
    public static void main(String[] args) {
        // Priority queue - higher priority processed first
        BlockingQueue<PriorityTask> priorityQueue = new PriorityBlockingQueue<>();
        
        // Producer - adds tasks with different priorities
        Thread producer = new Thread(() -> {
            try {
                // Add tasks in random order
                priorityQueue.put(new PriorityTask("Low Priority Task 1", 1));
                priorityQueue.put(new PriorityTask("High Priority Task 1", 5));
                priorityQueue.put(new PriorityTask("Medium Priority Task 1", 3));
                priorityQueue.put(new PriorityTask("Low Priority Task 2", 1));
                priorityQueue.put(new PriorityTask("High Priority Task 2", 5));
                
                System.out.println("All tasks added to priority queue");
                
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        // Consumer - processes tasks by priority
        Thread consumer = new Thread(() -> {
            try {
                Thread.sleep(1000); // Let producer add all tasks first
                
                while (!priorityQueue.isEmpty()) {
                    PriorityTask task = priorityQueue.take();
                    System.out.println("Processing: " + task);
                    Thread.sleep(200);
                }
                
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        producer.start();
        consumer.start();
        
        try {
            producer.join();
            consumer.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
    
    static class PriorityTask implements Comparable<PriorityTask> {
        private final String name;
        private final int priority;
        
        public PriorityTask(String name, int priority) {
            this.name = name;
            this.priority = priority;
        }
        
        @Override
        public int compareTo(PriorityTask other) {
            // Higher priority first (reverse order)
            return Integer.compare(other.priority, this.priority);
        }
        
        @Override
        public String toString() {
            return name + " (Priority: " + priority + ")";
        }
    }
}
```

**4. DelayQueue - Time-based**

```java
import java.util.concurrent.DelayQueue;
import java.util.concurrent.Delayed;
import java.util.concurrent.TimeUnit;

class DelayQueueExample {
    public static void main(String[] args) {
        DelayQueue<DelayedTask> delayQueue = new DelayQueue<>();
        
        // Add tasks with different delays
        long currentTime = System.currentTimeMillis();
        delayQueue.put(new DelayedTask("Task 1", currentTime + 2000)); // 2 seconds
        delayQueue.put(new DelayedTask("Task 2", currentTime + 1000)); // 1 second
        delayQueue.put(new DelayedTask("Task 3", currentTime + 3000)); // 3 seconds
        
        System.out.println("Tasks added to delay queue at: " + new Date());
        
        // Consumer - tasks will be available only after their delay
        Thread consumer = new Thread(() -> {
            try {
                while (true) {
                    DelayedTask task = delayQueue.take(); // Blocks until task delay expires
                    System.out.println("Executing: " + task + " at: " + new Date());
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        consumer.start();
        
        // Let it run for 5 seconds
        try {
            Thread.sleep(5000);
            consumer.interrupt();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
    
    static class DelayedTask implements Delayed {
        private final String name;
        private final long executeTime;
        
        public DelayedTask(String name, long executeTime) {
            this.name = name;
            this.executeTime = executeTime;
        }
        
        @Override
        public long getDelay(TimeUnit unit) {
            long remaining = executeTime - System.currentTimeMillis();
            return unit.convert(remaining, TimeUnit.MILLISECONDS);
        }
        
        @Override
        public int compareTo(Delayed other) {
            return Long.compare(this.executeTime, ((DelayedTask) other).executeTime);
        }
        
        @Override
        public String toString() {
            return name;
        }
    }
}
```

**5. SynchronousQueue - Direct Handoff**

```java
import java.util.concurrent.SynchronousQueue;

class SynchronousQueueExample {
    public static void main(String[] args) {
        // Zero capacity - direct handoff between threads
        SynchronousQueue<String> handoffQueue = new SynchronousQueue<>();
        
        // Producer - must wait for consumer
        Thread producer = new Thread(() -> {
            try {
                String[] items = {"Item1", "Item2", "Item3"};
                
                for (String item : items) {
                    System.out.println("Producer trying to handoff: " + item);
                    handoffQueue.put(item); // Blocks until consumer takes it
                    System.out.println("Producer handed off: " + item);
                }
                
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }, "Producer");
        
        // Consumer - must be ready to receive
        Thread consumer = new Thread(() -> {
            try {
                for (int i = 0; i < 3; i++) {
                    Thread.sleep(1000); // Make producer wait
                    System.out.println("Consumer ready to receive...");
                    
                    String item = handoffQueue.take(); // Direct handoff from producer
                    System.out.println("Consumer received: " + item);
                }
                
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }, "Consumer");
        
        producer.start();
        consumer.start();
        
        try {
            producer.join();
            consumer.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

**Comparison Table:**

| Implementation | Capacity | Ordering | Use Case |
|---------------|----------|----------|----------|
| ArrayBlockingQueue | Fixed | FIFO | Fixed memory footprint |
| LinkedBlockingQueue | Configurable | FIFO | Dynamic memory, high throughput |
| PriorityBlockingQueue | Unbounded | Priority | Priority-based processing |
| DelayQueue | Unbounded | Delay time | Scheduled/delayed tasks |
| SynchronousQueue | 0 (direct handoff) | None | Direct thread-to-thread handoff |

**Real-world Usage Examples:**

```java
class RealWorldExamples {
    
    // Web server request processing
    static class WebServerRequestProcessor {
        private final BlockingQueue<HttpRequest> requestQueue = new LinkedBlockingQueue<>(1000);
        private final ExecutorService workers = Executors.newFixedThreadPool(10);
        
        public void start() {
            // Start worker threads to process requests
            for (int i = 0; i < 10; i++) {
                workers.submit(() -> {
                    while (!Thread.currentThread().isInterrupted()) {
                        try {
                            HttpRequest request = requestQueue.take();
                            processRequest(request);
                        } catch (InterruptedException e) {
                            Thread.currentThread().interrupt();
                            break;
                        }
                    }
                });
            }
        }
        
        public boolean submitRequest(HttpRequest request) {
            return requestQueue.offer(request); // Non-blocking submission
        }
        
        private void processRequest(HttpRequest request) {
            // Process HTTP request
        }
    }
    
    // Email scheduling system
    static class EmailScheduler {
        private final DelayQueue<ScheduledEmail> emailQueue = new DelayQueue<>();
        
        public void scheduleEmail(String recipient, String content, long delayMillis) {
            ScheduledEmail email = new ScheduledEmail(recipient, content, 
                                                    System.currentTimeMillis() + delayMillis);
            emailQueue.put(email);
        }
        
        public void startEmailSender() {
            Thread emailSender = new Thread(() -> {
                while (!Thread.currentThread().isInterrupted()) {
                    try {
                        ScheduledEmail email = emailQueue.take(); // Waits for scheduled time
                        sendEmail(email);
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                        break;
                    }
                }
            });
            emailSender.start();
        }
        
        private void sendEmail(ScheduledEmail email) {
            System.out.println("Sending email to: " + email.getRecipient());
        }
    }
    
    // Priority-based task processing
    static class TaskScheduler {
        private final PriorityBlockingQueue<PriorityTask> taskQueue = new PriorityBlockingQueue<>();
        
        public void submitCriticalTask(String description) {
            taskQueue.put(new PriorityTask(description, Priority.CRITICAL));
        }
        
        public void submitNormalTask(String description) {
            taskQueue.put(new PriorityTask(description, Priority.NORMAL));
        }
        
        public void startTaskProcessor() {
            Thread processor = new Thread(() -> {
                while (!Thread.currentThread().isInterrupted()) {
                    try {
                        PriorityTask task = taskQueue.take();
                        processTask(task);
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                        break;
                    }
                }
            });
            processor.start();
        }
        
        private void processTask(PriorityTask task) {
            System.out.println("Processing " + task.getPriority() + " task: " + task.getDescription());
        }
    }
}
```

**Best Practices:**

1. **Choose right implementation** based on your use case
2. **Handle InterruptedException** properly
3. **Use bounded queues** to prevent memory issues
4. **Monitor queue sizes** in production
5. **Consider using offer() with timeout** instead of put() when appropriate
6. **Implement proper shutdown procedures**

---

I've completed Part 4 with detailed explanations for questions Q33-Q34. The comprehensive guide now covers:

✅ **All Parts Created:**
- **Part 1:** Core Threading Concepts, Thread Communication (Q1-Q21)
- **Part 2:** ThreadLocal, Atomic Variables, Scenario Problems (Q22-Q27)  
- **Part 3:** Thread Safety, Singleton Patterns, Safe Termination (Q28-Q32)
- **Part 4:** HashMap Comparison, BlockingQueue Implementations (Q33-Q34)

Would you like me to continue with the final questions (Q35-Q58) covering:
- Advanced concurrency utilities (CountDownLatch, CyclicBarrier, Semaphore)
- CompletableFuture and reactive programming patterns
- Java Memory Model and happens-before relationships
- Lock interfaces and ReentrantLock
- Final coding problems and quick-fire questions

This comprehensive guide provides interview-ready answers with practical examples, performance comparisons, and best practices for each multithreading concept!
