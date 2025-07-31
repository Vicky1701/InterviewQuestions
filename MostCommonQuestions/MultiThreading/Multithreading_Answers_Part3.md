# Java Multithreading Interview Questions - Complete Answers Guide (Part 3)

## 🎯 SCENARIO-BASED Questions (Continued)

**Q28: How do you create a thread-safe Singleton class?**

**Answer:**

**Thread-Safe Singleton:** Ensures only one instance exists and is safely created in multithreaded environment.

**Solution 1: Synchronized Method (Simple but slower)**

```java
class ThreadSafeSingleton1 {
    private static ThreadSafeSingleton1 instance;
    
    // Private constructor to prevent instantiation
    private ThreadSafeSingleton1() {}
    
    // Synchronized method - thread-safe but slower
    public static synchronized ThreadSafeSingleton1 getInstance() {
        if (instance == null) {
            instance = new ThreadSafeSingleton1();
        }
        return instance;
    }
}
```

**Problem:** Every call to `getInstance()` is synchronized, causing performance bottleneck.

**Solution 2: Double-Checked Locking (Recommended)**

```java
class ThreadSafeSingleton2 {
    // volatile ensures proper initialization across threads
    private static volatile ThreadSafeSingleton2 instance;
    
    private ThreadSafeSingleton2() {}
    
    public static ThreadSafeSingleton2 getInstance() {
        // First check (no synchronization)
        if (instance == null) {
            synchronized (ThreadSafeSingleton2.class) {
                // Second check (synchronized)
                if (instance == null) {
                    instance = new ThreadSafeSingleton2();
                }
            }
        }
        return instance;
    }
}
```

**Why Double-Checked Locking Works:**
- First check avoids synchronization for subsequent calls
- Second check ensures only one thread creates the instance
- `volatile` prevents instruction reordering

**Solution 3: Eager Initialization (Thread-safe by JVM)**

```java
class ThreadSafeSingleton3 {
    // Instance created at class loading time - thread-safe by JVM
    private static final ThreadSafeSingleton3 INSTANCE = new ThreadSafeSingleton3();
    
    private ThreadSafeSingleton3() {}
    
    public static ThreadSafeSingleton3 getInstance() {
        return INSTANCE; // No synchronization needed
    }
}
```

**Pros:** Simple, thread-safe
**Cons:** Instance created even if never used

**Solution 4: Initialization-on-demand holder (Best Performance)**

```java
class ThreadSafeSingleton4 {
    private ThreadSafeSingleton4() {}
    
    // Static inner class - loaded only when accessed
    private static class SingletonHolder {
        private static final ThreadSafeSingleton4 INSTANCE = new ThreadSafeSingleton4();
    }
    
    public static ThreadSafeSingleton4 getInstance() {
        return SingletonHolder.INSTANCE; // Thread-safe, lazy, fast
    }
}
```

**Advantages:**
- Lazy initialization
- Thread-safe without synchronization
- Best performance
- JVM handles thread safety

**Solution 5: Enum Singleton (Bulletproof)**

```java
enum ThreadSafeSingleton5 {
    INSTANCE;
    
    public void doSomething() {
        System.out.println("Doing something...");
    }
}

// Usage
class SingletonDemo {
    public static void main(String[] args) {
        ThreadSafeSingleton5 singleton = ThreadSafeSingleton5.INSTANCE;
        singleton.doSomething();
    }
}
```

**Benefits:**
- Thread-safe by design
- Serialization-safe
- Reflection-proof
- Simplest code

**Complete Example with Testing:**

```java
class SingletonTester {
    public static void main(String[] args) throws InterruptedException {
        int numThreads = 1000;
        CountDownLatch latch = new CountDownLatch(numThreads);
        Set<ThreadSafeSingleton2> instances = Collections.synchronizedSet(new HashSet<>());
        
        // Create multiple threads trying to get singleton instance
        for (int i = 0; i < numThreads; i++) {
            new Thread(() -> {
                try {
                    instances.add(ThreadSafeSingleton2.getInstance());
                } finally {
                    latch.countDown();
                }
            }).start();
        }
        
        latch.await(); // Wait for all threads to complete
        
        System.out.println("Number of instances created: " + instances.size());
        // Should print: Number of instances created: 1
    }
}
```

**Comparison Table:**

| Method | Thread Safety | Performance | Lazy Loading | Complexity |
|--------|---------------|-------------|--------------|------------|
| Synchronized Method | ✅ | ❌ Slow | ✅ | Low |
| Double-Checked Locking | ✅ | ✅ Fast | ✅ | Medium |
| Eager Initialization | ✅ | ✅ Fast | ❌ | Low |
| Holder Pattern | ✅ | ✅ Fastest | ✅ | Medium |
| Enum | ✅ | ✅ Fast | ❌ | Low |

**Best Practice:** Use **Initialization-on-demand holder pattern** for most cases, or **Enum** if you need additional safety features.

---

**Q29: What happens if we call start() method twice on the same thread?**

**Answer:**

**IllegalThreadStateException** is thrown when you call `start()` method twice on the same thread.

**Demonstration:**

```java
class DoubleStartDemo {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            System.out.println("Thread running: " + Thread.currentThread().getName());
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            System.out.println("Thread completed");
        });
        
        System.out.println("Thread state before start: " + thread.getState()); // NEW
        
        // First call to start() - works fine
        thread.start();
        System.out.println("Thread state after first start: " + thread.getState()); // RUNNABLE
        
        try {
            Thread.sleep(100); // Let thread start execution
            
            // Second call to start() - throws exception
            thread.start(); // IllegalThreadStateException
            
        } catch (IllegalThreadStateException e) {
            System.out.println("Exception caught: " + e.getClass().getSimpleName());
            System.out.println("Message: " + e.getMessage());
            System.out.println("Thread state when exception occurred: " + thread.getState());
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

**Output:**
```
Thread state before start: NEW
Thread running: Thread-0
Thread state after first start: RUNNABLE
Exception caught: IllegalThreadStateException
Message: null
Thread state when exception occurred: RUNNABLE
Thread completed
```

**Why This Happens:**

1. **Thread Lifecycle:** A thread can only be started once
2. **State Transition:** NEW → RUNNABLE → TERMINATED
3. **JVM Restriction:** Once started, thread cannot return to NEW state

**Thread State Explanation:**

```java
class ThreadStateDemo {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(() -> {
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        // Thread states demonstration
        System.out.println("1. Initial state: " + thread.getState()); // NEW
        
        thread.start();
        System.out.println("2. After start(): " + thread.getState()); // RUNNABLE
        
        Thread.sleep(100);
        System.out.println("3. While running: " + thread.getState()); // TIMED_WAITING
        
        thread.join(); // Wait for completion
        System.out.println("4. After completion: " + thread.getState()); // TERMINATED
        
        // Now try to start again
        try {
            thread.start(); // This will fail
        } catch (IllegalThreadStateException e) {
            System.out.println("5. Cannot restart TERMINATED thread");
        }
    }
}
```

**Workaround - Creating New Thread:**

```java
class ThreadRestartDemo {
    public static void main(String[] args) throws InterruptedException {
        Runnable task = () -> {
            System.out.println("Task executed by: " + Thread.currentThread().getName());
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        };
        
        // First execution
        Thread thread1 = new Thread(task, "Thread-1");
        thread1.start();
        thread1.join();
        
        System.out.println("First thread completed. State: " + thread1.getState());
        
        // Cannot restart the same thread, so create a new one
        Thread thread2 = new Thread(task, "Thread-2");
        thread2.start();
        thread2.join();
        
        System.out.println("Second thread completed. State: " + thread2.getState());
    }
}
```

**Best Practice for Reusable Tasks:**

```java
class ReusableTaskManager {
    private final Runnable task;
    
    public ReusableTaskManager(Runnable task) {
        this.task = task;
    }
    
    public void executeTask() {
        Thread thread = new Thread(task);
        thread.start();
        try {
            thread.join(); // Wait for completion
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
    
    public static void main(String[] args) {
        Runnable printTask = () -> {
            System.out.println("Executing task in: " + Thread.currentThread().getName());
        };
        
        ReusableTaskManager manager = new ReusableTaskManager(printTask);
        
        // Execute the same task multiple times with different threads
        manager.executeTask(); // Creates Thread-0
        manager.executeTask(); // Creates Thread-1
        manager.executeTask(); // Creates Thread-2
    }
}
```

**Using ExecutorService for Better Approach:**

```java
class BetterTaskManagement {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        
        Runnable task = () -> {
            System.out.println("Task executed by: " + Thread.currentThread().getName());
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        };
        
        // Submit the same task multiple times - no thread creation overhead
        for (int i = 0; i < 5; i++) {
            executor.submit(task);
        }
        
        executor.shutdown();
        try {
            executor.awaitTermination(10, TimeUnit.SECONDS);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

**Key Takeaways:**

1. **Cannot restart a thread** - start() can only be called once
2. **Create new thread** if you need to run the same task again
3. **Use ExecutorService** for better thread management
4. **Thread states are irreversible** - NEW → RUNNABLE → TERMINATED
5. **IllegalThreadStateException** indicates violation of thread lifecycle

---

**Q30: How would you stop a running thread safely?**

**Answer:**

**Safe Thread Termination** requires cooperation from the thread itself. You cannot forcibly kill a thread safely.

**❌ Wrong Approaches (Deprecated/Dangerous):**

```java
// DON'T USE THESE - They are deprecated and dangerous
class UnsafeThreadStopping {
    public static void demonstrateWrongWays() {
        Thread thread = new Thread(() -> {
            while (true) {
                // Some work
            }
        });
        
        thread.start();
        
        // ❌ NEVER USE - Deprecated and dangerous
        // thread.stop();    // Can cause data corruption
        // thread.suspend(); // Can cause deadlocks
        // thread.resume();  // Also deprecated
    }
}
```

**✅ Correct Approaches:**

**Method 1: Using Boolean Flag (Cooperative Termination)**

```java
class CooperativeThreadStopping {
    private volatile boolean stopRequested = false;
    
    public void requestStop() {
        stopRequested = true;
    }
    
    public void startWorker() {
        Thread worker = new Thread(() -> {
            while (!stopRequested) {
                try {
                    // Do some work
                    System.out.println("Working... " + Thread.currentThread().getName());
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    System.out.println("Thread interrupted, stopping...");
                    Thread.currentThread().interrupt(); // Restore interrupt status
                    break;
                }
            }
            System.out.println("Thread stopped gracefully");
        }, "WorkerThread");
        
        worker.start();
        
        // Stop the thread after 5 seconds
        new Thread(() -> {
            try {
                Thread.sleep(5000);
                requestStop();
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }).start();
    }
    
    public static void main(String[] args) {
        new CooperativeThreadStopping().startWorker();
    }
}
```

**Method 2: Using Thread Interruption (Recommended)**

```java
class InterruptBasedStopping {
    public static void main(String[] args) throws InterruptedException {
        Thread worker = new Thread(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try {
                    // Simulate work
                    System.out.println("Working...");
                    Thread.sleep(1000); // This will throw InterruptedException when interrupted
                    
                } catch (InterruptedException e) {
                    System.out.println("Received interrupt signal, cleaning up...");
                    
                    // Cleanup code here
                    cleanup();
                    
                    // Restore interrupt status and exit
                    Thread.currentThread().interrupt();
                    break;
                }
            }
            System.out.println("Thread stopped gracefully");
        });
        
        worker.start();
        
        // Let it work for 3 seconds, then interrupt
        Thread.sleep(3000);
        System.out.println("Requesting thread to stop...");
        worker.interrupt(); // Send interrupt signal
        
        // Wait for thread to finish
        worker.join(2000); // Wait max 2 seconds
        
        if (worker.isAlive()) {
            System.out.println("Thread didn't stop in time!");
        } else {
            System.out.println("Thread stopped successfully");
        }
    }
    
    private static void cleanup() {
        System.out.println("Performing cleanup...");
        // Close files, database connections, etc.
    }
}
```

**Method 3: Using ExecutorService (Best Practice)**

```java
class ExecutorServiceStopping {
    public static void main(String[] args) throws InterruptedException {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        
        // Submit a long-running task
        Future<?> future = executor.submit(() -> {
            while (!Thread.currentThread().isInterrupted()) {
                try {
                    System.out.println("Processing...");
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    System.out.println("Task interrupted");
                    Thread.currentThread().interrupt(); // Restore interrupt status
                    return; // Exit the task
                }
            }
        });
        
        // Let it run for 3 seconds
        Thread.sleep(3000);
        
        // Method 1: Cancel the specific task
        System.out.println("Canceling task...");
        future.cancel(true); // true = interrupt if running
        
        // Method 2: Shutdown the executor
        System.out.println("Shutting down executor...");
        executor.shutdown(); // Graceful shutdown
        
        // Wait for tasks to complete
        if (!executor.awaitTermination(2, TimeUnit.SECONDS)) {
            System.out.println("Tasks didn't complete in time, forcing shutdown...");
            executor.shutdownNow(); // Force shutdown
            
            // Wait a bit more
            if (!executor.awaitTermination(1, TimeUnit.SECONDS)) {
                System.out.println("Executor didn't terminate");
            }
        }
        
        System.out.println("All tasks completed");
    }
}
```

**Method 4: Combining Multiple Stopping Conditions**

```java
class AdvancedThreadStopping {
    private volatile boolean stopRequested = false;
    private volatile boolean errorOccurred = false;
    
    public void startAdvancedWorker() {
        Thread worker = new Thread(() -> {
            int workCount = 0;
            
            while (!shouldStop() && workCount < 100) { // Multiple stop conditions
                try {
                    // Simulate work that might fail
                    if (Math.random() < 0.1) { // 10% chance of error
                        throw new RuntimeException("Simulated error");
                    }
                    
                    System.out.println("Work item " + ++workCount + " completed");
                    Thread.sleep(500);
                    
                } catch (InterruptedException e) {
                    System.out.println("Worker interrupted");
                    Thread.currentThread().interrupt();
                    break;
                    
                } catch (Exception e) {
                    System.out.println("Error occurred: " + e.getMessage());
                    errorOccurred = true;
                    break;
                }
            }
            
            // Cleanup and report final status
            cleanup();
            reportStatus(workCount);
        }, "AdvancedWorker");
        
        worker.start();
        
        // Monitor and control thread
        new Thread(() -> {
            try {
                Thread.sleep(10000); // Let it work for 10 seconds max
                if (worker.isAlive()) {
                    System.out.println("Requesting stop due to timeout");
                    requestStop();
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }, "Monitor").start();
    }
    
    private boolean shouldStop() {
        return stopRequested || errorOccurred || Thread.currentThread().isInterrupted();
    }
    
    public void requestStop() {
        stopRequested = true;
    }
    
    private void cleanup() {
        System.out.println("Performing cleanup operations...");
        // Close resources, save state, etc.
    }
    
    private void reportStatus(int workCount) {
        if (errorOccurred) {
            System.out.println("Thread stopped due to error after " + workCount + " items");
        } else if (stopRequested) {
            System.out.println("Thread stopped on request after " + workCount + " items");
        } else if (Thread.currentThread().isInterrupted()) {
            System.out.println("Thread interrupted after " + workCount + " items");
        } else {
            System.out.println("Thread completed normally after " + workCount + " items");
        }
    }
    
    public static void main(String[] args) {
        new AdvancedThreadStopping().startAdvancedWorker();
    }
}
```

**Producer-Consumer Safe Stopping:**

```java
class SafeProducerConsumerStopping {
    private final BlockingQueue<String> queue = new ArrayBlockingQueue<>(10);
    private volatile boolean shutdown = false;
    private final String POISON_PILL = "SHUTDOWN";
    
    public void startProducerConsumer() {
        // Producer
        Thread producer = new Thread(() -> {
            try {
                for (int i = 1; i <= 50; i++) {
                    if (shutdown) break;
                    
                    queue.put("Item-" + i);
                    System.out.println("Produced: Item-" + i);
                    Thread.sleep(100);
                }
            } catch (InterruptedException e) {
                System.out.println("Producer interrupted");
                Thread.currentThread().interrupt();
            } finally {
                // Send poison pill to stop consumer
                try {
                    queue.put(POISON_PILL);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        }, "Producer");
        
        // Consumer
        Thread consumer = new Thread(() -> {
            try {
                while (true) {
                    String item = queue.take();
                    
                    if (POISON_PILL.equals(item)) {
                        System.out.println("Consumer received shutdown signal");
                        break;
                    }
                    
                    System.out.println("Consumed: " + item);
                    Thread.sleep(150);
                }
            } catch (InterruptedException e) {
                System.out.println("Consumer interrupted");
                Thread.currentThread().interrupt();
            }
        }, "Consumer");
        
        producer.start();
        consumer.start();
        
        // Shutdown after 5 seconds
        new Thread(() -> {
            try {
                Thread.sleep(5000);
                System.out.println("Requesting shutdown...");
                shutdown = true;
                producer.interrupt(); // Interrupt producer if blocked
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }).start();
    }
    
    public static void main(String[] args) {
        new SafeProducerConsumerStopping().startProducerConsumer();
    }
}
```

**Best Practices for Safe Thread Stopping:**

1. **Use cooperative termination** with boolean flags
2. **Handle InterruptedException properly** - restore interrupt status
3. **Use ExecutorService** for better lifecycle management
4. **Implement proper cleanup** in finally blocks
5. **Use poison pills** for producer-consumer scenarios
6. **Set reasonable timeouts** for shutdown operations
7. **Never use deprecated stop(), suspend(), resume()**

**Key Points:**

- **Threads must cooperate** in their own termination
- **Check interruption status** regularly in long-running loops
- **Handle InterruptedException** properly
- **Use ExecutorService** for production applications
- **Implement graceful shutdown** with cleanup operations

---

## 📝 CONCURRENT COLLECTIONS Questions

**Q32: What is ConcurrentHashMap and how does it work?**

**Answer:**

**ConcurrentHashMap:** A thread-safe version of HashMap that provides better performance than Hashtable by using segment-based locking (Java 7) and CAS operations with synchronized blocks (Java 8+).

**Key Features:**

- **Thread-safe** without synchronizing the entire map
- **Better performance** than Hashtable
- **No blocking** for read operations
- **Allows concurrent reads and writes**
- **Null values not allowed** (both key and value)

**Evolution of ConcurrentHashMap:**

**Java 7 - Segment-based Locking:**

```java
// Java 7 approach (simplified explanation)
class Java7ConcurrentHashMapConcept {
    // Map is divided into segments (default 16)
    // Each segment has its own lock
    // Multiple threads can access different segments simultaneously
    
    private static class Segment<K,V> {
        private volatile HashEntry<K,V>[] table;
        private final ReentrantLock lock = new ReentrantLock();
        
        V put(K key, V value) {
            lock.lock();
            try {
                // Only this segment is locked, not entire map
                return putInternal(key, value);
            } finally {
                lock.unlock();
            }
        }
        
        V get(K key) {
            // No locking needed for reads in most cases
            return getInternal(key);
        }
    }
}
```

**Java 8+ - CAS + Synchronized Nodes:**

```java
// Java 8+ approach (simplified explanation)
class Java8ConcurrentHashMapConcept {
    // Uses CAS operations for atomic updates
    // Synchronized blocks only when needed (collision handling)
    // Better scalability and performance
    
    private volatile Node<K,V>[] table;
    
    V put(K key, V value) {
        // Try CAS operation first
        if (casTabAt(table, index, null, newNode)) {
            return null; // Success with CAS, no locking needed
        }
        
        // If collision, synchronize on the head node
        synchronized (headNode) {
            // Handle collision (linked list or tree)
        }
    }
    
    V get(K key) {
        // Always lock-free for reads
        Node<K,V> node = tabAt(table, index);
        return searchNode(node, key);
    }
}
```

**Basic Usage Examples:**

```java
import java.util.concurrent.ConcurrentHashMap;

class ConcurrentHashMapBasics {
    public static void main(String[] args) {
        ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
        
        // Basic operations
        map.put("apple", 10);
        map.put("banana", 20);
        map.put("orange", 15);
        
        // Thread-safe operations
        Integer apples = map.get("apple");
        Integer removed = map.remove("banana");
        
        // Atomic operations (Java 8+)
        map.putIfAbsent("grape", 25); // Only put if key doesn't exist
        map.replace("apple", 10, 12); // Replace only if current value is 10
        map.computeIfAbsent("mango", k -> k.length() * 5); // Compute if absent
        map.computeIfPresent("apple", (k, v) -> v + 1); // Compute if present
        
        // Merge operation
        map.merge("apple", 5, Integer::sum); // Add 5 to existing value
        
        System.out.println("Final map: " + map);
    }
}
```

**Advanced Operations:**

```java
class ConcurrentHashMapAdvanced {
    private final ConcurrentHashMap<String, AtomicInteger> counters = new ConcurrentHashMap<>();
    
    // Thread-safe counter increment
    public void incrementCounter(String key) {
        counters.computeIfAbsent(key, k -> new AtomicInteger(0))
                .incrementAndGet();
    }
    
    // Batch operations
    public void performBatchOperations() {
        ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
        
        // Populate map
        for (int i = 0; i < 1000; i++) {
            map.put("key" + i, i);
        }
        
        // Parallel operations (Java 8+)
        
        // 1. forEach in parallel
        map.forEach(100, (key, value) -> {
            System.out.println("Processing: " + key + " = " + value);
        });
        
        // 2. Search in parallel
        String result = map.search(100, (key, value) -> {
            return value > 500 ? key : null;
        });
        
        // 3. Reduce in parallel
        Integer sum = map.reduce(100,
            (key, value) -> value, // Transform function
            Integer::sum          // Reduce function
        );
        
        System.out.println("Sum: " + sum);
    }
    
    // Real-world example: Web request counting
    public static class RequestCounter {
        private final ConcurrentHashMap<String, AtomicLong> requestCounts = new ConcurrentHashMap<>();
        
        public void recordRequest(String endpoint) {
            requestCounts.computeIfAbsent(endpoint, k -> new AtomicLong(0))
                        .incrementAndGet();
        }
        
        public long getRequestCount(String endpoint) {
            AtomicLong counter = requestCounts.get(endpoint);
            return counter != null ? counter.get() : 0;
        }
        
        public Map<String, Long> getAllCounts() {
            return requestCounts.entrySet().stream()
                .collect(Collectors.toMap(
                    Map.Entry::getKey,
                    entry -> entry.getValue().get()
                ));
        }
    }
}
```

**Performance Comparison:**

```java
class ConcurrentHashMapPerformance {
    public static void performanceTest() {
        int numThreads = 10;
        int operationsPerThread = 100000;
        
        // Test ConcurrentHashMap
        ConcurrentHashMap<Integer, Integer> concurrentMap = new ConcurrentHashMap<>();
        long concurrentTime = measurePerformance("ConcurrentHashMap", 
            concurrentMap, numThreads, operationsPerThread);
        
        // Test Hashtable
        Hashtable<Integer, Integer> hashtable = new Hashtable<>();
        long hashtableTime = measurePerformance("Hashtable", 
            hashtable, numThreads, operationsPerThread);
        
        // Test Collections.synchronizedMap
        Map<Integer, Integer> syncMap = Collections.synchronizedMap(new HashMap<>());
        long syncMapTime = measurePerformance("SynchronizedMap", 
            syncMap, numThreads, operationsPerThread);
        
        System.out.println("Performance Results:");
        System.out.println("ConcurrentHashMap: " + concurrentTime + "ms");
        System.out.println("Hashtable: " + hashtableTime + "ms");
        System.out.println("SynchronizedMap: " + syncMapTime + "ms");
    }
    
    private static long measurePerformance(String name, Map<Integer, Integer> map, 
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
                        if (j % 3 == 0) {
                            map.put(key, threadId);
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

**When to Use ConcurrentHashMap:**

**✅ Use ConcurrentHashMap when:**
- High concurrent read/write operations
- Better performance than Hashtable needed
- Null values are not required
- Want atomic operations like putIfAbsent, replace, etc.

**❌ Don't use when:**
- Single-threaded application (use HashMap)
- Need to store null values
- Need to synchronize multiple operations together

**Best Practices:**

```java
class ConcurrentHashMapBestPractices {
    private final ConcurrentHashMap<String, UserSession> activeSessions = new ConcurrentHashMap<>();
    
    // ✅ Good: Use atomic operations
    public UserSession getOrCreateSession(String sessionId) {
        return activeSessions.computeIfAbsent(sessionId, id -> new UserSession(id));
    }
    
    // ✅ Good: Use merge for updates
    public void updateSessionActivity(String sessionId) {
        activeSessions.computeIfPresent(sessionId, (id, session) -> {
            session.updateLastActivity();
            return session;
        });
    }
    
    // ❌ Bad: Multiple operations not atomic
    public void badUpdateSession(String sessionId) {
        if (activeSessions.containsKey(sessionId)) {
            UserSession session = activeSessions.get(sessionId); // Race condition!
            session.updateLastActivity();
            activeSessions.put(sessionId, session);
        }
    }
    
    // ✅ Good: Atomic check and update
    public boolean extendSession(String sessionId, long extensionTime) {
        return activeSessions.computeIfPresent(sessionId, (id, session) -> {
            if (session.canExtend()) {
                session.extend(extensionTime);
                return session;
            }
            return null; // Remove if can't extend
        }) != null;
    }
}
```

**Key Benefits:**

1. **Scalability:** Better performance with multiple threads
2. **Lock-free reads:** No blocking for get operations
3. **Fine-grained locking:** Only locks specific portions when needed
4. **Atomic operations:** Built-in support for common concurrent patterns
5. **Memory consistency:** Proper happens-before relationships

**Internal Working (Java 8+):**
- Uses array of Node objects
- CAS operations for lock-free updates when possible
- Synchronized blocks only for collision handling
- Tree structure (Red-Black tree) for buckets with many collisions
- Resizing happens concurrently without blocking other operations

---

I've completed Part 3 covering questions Q28-Q32 with detailed explanations for:

✅ **Questions covered:**
- Thread-safe Singleton patterns (5 different approaches)
- What happens when calling start() twice
- Safe thread termination techniques (4 methods)
- ConcurrentHashMap working and usage

Would you like me to continue with the remaining questions (Q33-Q58) covering:
- HashMap vs Hashtable vs ConcurrentHashMap comparison
- BlockingQueue implementations
- Advanced concurrency utilities (CountDownLatch, CyclicBarrier, Semaphore)
- CompletableFuture and reactive programming
- Java Memory Model
- Coding problems and interview scenarios

Each answer maintains the same comprehensive format with practical examples and best practices!
