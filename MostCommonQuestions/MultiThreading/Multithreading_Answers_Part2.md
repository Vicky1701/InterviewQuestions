# Java Multithreading Interview Questions - Complete Answers Guide (Part 2)

## 💡 LIKELY Questions (Continued)

### ThreadLocal and Atomic Variables

**Q22: What is the difference between Runnable and Callable?**

**Answer:**

| Runnable | Callable |
|----------|----------|
| **Method:** `void run()` | **Method:** `V call() throws Exception` |
| **Return Value:** None | **Return Value:** Generic type V |
| **Exception:** Cannot throw checked exceptions | **Exception:** Can throw checked exceptions |
| **Usage:** Thread, ExecutorService | **Usage:** ExecutorService only |
| **Result:** No result handling | **Result:** Returns Future object |
| **Since:** Java 1.0 | **Since:** Java 1.5 |

**Practical Example:**

```java
// Runnable - For tasks that don't return results
class FileDownloader implements Runnable {
    private String url;
    
    public FileDownloader(String url) {
        this.url = url;
    }
    
    @Override
    public void run() {
        try {
            // Download file logic
            System.out.println("Downloaded: " + url);
        } catch (Exception e) {
            // Can only handle runtime exceptions
            throw new RuntimeException(e);
        }
    }
}

// Callable - For tasks that return results
class DataProcessor implements Callable<ProcessResult> {
    private String data;
    
    public DataProcessor(String data) {
        this.data = data;
    }
    
    @Override
    public ProcessResult call() throws ProcessingException {
        // Can throw checked exceptions
        if (data == null) {
            throw new ProcessingException("Data cannot be null");
        }
        
        // Process data and return result
        return new ProcessResult(data.toUpperCase());
    }
}

// Usage comparison
public class RunnableVsCallableDemo {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        
        // Using Runnable - Fire and forget
        executor.submit(new FileDownloader("http://example.com/file.pdf"));
        
        // Using Callable - Get result
        Future<ProcessResult> future = executor.submit(new DataProcessor("hello"));
        try {
            ProcessResult result = future.get();
            System.out.println("Processed result: " + result.getValue());
        } catch (Exception e) {
            System.out.println("Processing failed: " + e.getMessage());
        }
        
        executor.shutdown();
    }
}
```

**When to Use Which:**

- **Use Runnable when:**
  - Task doesn't need to return a result
  - Fire-and-forget operations
  - Background processing without result tracking

- **Use Callable when:**
  - Need to get a result from the task
  - Task might throw checked exceptions
  - Need to track completion status

---

**Q23: What is ThreadLocal and when would you use it?**

**Answer:**

**ThreadLocal:** Provides thread-local variables where each thread has its own copy of the variable, isolated from other threads.

**Key Features:**

- Each thread has its own instance of the variable
- No synchronization needed
- Automatic cleanup when thread terminates
- Useful for maintaining per-thread context

**Common Use Cases:**

**1. Database Connections:**
```java
class DatabaseManager {
    private static ThreadLocal<Connection> connectionHolder = 
        ThreadLocal.withInitial(() -> {
            try {
                return DriverManager.getConnection(
                    "jdbc:mysql://localhost/db", "user", "password");
            } catch (SQLException e) {
                throw new RuntimeException(e);
            }
        });
    
    public static Connection getConnection() {
        return connectionHolder.get(); // Each thread gets its own connection
    }
    
    public static void closeConnection() {
        Connection conn = connectionHolder.get();
        if (conn != null) {
            try {
                conn.close();
            } catch (SQLException e) {
                // Log error
            }
            connectionHolder.remove(); // Important: cleanup
        }
    }
}
```

**2. User Context/Session Management:**
```java
class UserContext {
    private static ThreadLocal<User> currentUser = new ThreadLocal<>();
    
    public static void setCurrentUser(User user) {
        currentUser.set(user);
    }
    
    public static User getCurrentUser() {
        return currentUser.get();
    }
    
    public static void clear() {
        currentUser.remove(); // Prevent memory leaks
    }
}

// Usage in web application
class UserController {
    public void handleRequest(HttpRequest request) {
        try {
            User user = authenticateUser(request);
            UserContext.setCurrentUser(user);
            
            // Now any method in this request thread can access current user
            processUserRequest();
            
        } finally {
            UserContext.clear(); // Always cleanup
        }
    }
    
    private void processUserRequest() {
        User currentUser = UserContext.getCurrentUser();
        // Use current user without passing it as parameter
    }
}
```

**3. Thread-Safe SimpleDateFormat:**
```java
class DateUtils {
    // SimpleDateFormat is not thread-safe, but ThreadLocal makes it safe
    private static ThreadLocal<SimpleDateFormat> dateFormatter = 
        ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyy-MM-dd HH:mm:ss"));
    
    public static String formatDate(Date date) {
        return dateFormatter.get().format(date); // Thread-safe
    }
    
    public static Date parseDate(String dateString) throws ParseException {
        return dateFormatter.get().parse(dateString); // Thread-safe
    }
}
```

**Memory Leak Prevention:**

```java
class ThreadLocalExample {
    private static ThreadLocal<LargeObject> threadLocal = new ThreadLocal<>();
    
    public void processRequest() {
        try {
            LargeObject obj = new LargeObject();
            threadLocal.set(obj);
            
            // Do processing
            
        } finally {
            // ⚠️ IMPORTANT: Always remove ThreadLocal values
            threadLocal.remove(); // Prevents memory leaks
        }
    }
}
```

**ThreadLocal vs Synchronization:**

```java
// ❌ Using synchronization - Performance bottleneck
class SynchronizedCounter {
    private int counter = 0;
    
    public synchronized int getNextId() {
        return ++counter; // All threads wait for each other
    }
}

// ✅ Using ThreadLocal - No contention
class ThreadLocalCounter {
    private static ThreadLocal<Integer> counter = 
        ThreadLocal.withInitial(() -> 0);
    
    public int getNextId() {
        int current = counter.get();
        counter.set(current + 1);
        return current + 1; // Each thread has its own counter
    }
}
```

**Best Practices:**

- Always call `remove()` when done to prevent memory leaks
- Use `ThreadLocal.withInitial()` for lazy initialization
- Be careful in thread pool environments
- Consider using try-finally blocks for cleanup

---

**Q24: What are atomic variables in Java? (AtomicInteger, AtomicBoolean)**

**Answer:**

**Atomic Variables:** Thread-safe variables that support lock-free, atomic operations using compare-and-swap (CAS) operations.

**Key Features:**

- **Lock-free:** No synchronization overhead
- **Atomic Operations:** Indivisible operations
- **Better Performance:** Faster than synchronized blocks
- **Wait-free:** Non-blocking algorithms

**Common Atomic Classes:**

```java
import java.util.concurrent.atomic.*;

// Atomic primitives
AtomicInteger atomicInt = new AtomicInteger(0);
AtomicLong atomicLong = new AtomicLong(0L);
AtomicBoolean atomicBoolean = new AtomicBoolean(false);

// Atomic references
AtomicReference<String> atomicRef = new AtomicReference<>("initial");
AtomicStampedReference<String> stampedRef = new AtomicStampedReference<>("value", 0);
```

**AtomicInteger Example:**

```java
class AtomicCounterExample {
    private AtomicInteger counter = new AtomicInteger(0);
    
    // Thread-safe increment
    public int increment() {
        return counter.incrementAndGet(); // Atomic operation
    }
    
    // Thread-safe decrement
    public int decrement() {
        return counter.decrementAndGet();
    }
    
    // Compare and set (CAS operation)
    public boolean updateCounter(int expectedValue, int newValue) {
        return counter.compareAndSet(expectedValue, newValue);
    }
    
    // Get current value
    public int getValue() {
        return counter.get();
    }
    
    // Atomic add
    public int addValue(int delta) {
        return counter.addAndGet(delta);
    }
}
```

**Performance Comparison:**

```java
// Performance test: Atomic vs Synchronized
class PerformanceTest {
    private AtomicInteger atomicCounter = new AtomicInteger(0);
    private int syncCounter = 0;
    
    // Atomic version - Faster
    public void atomicIncrement() {
        atomicCounter.incrementAndGet(); // Lock-free
    }
    
    // Synchronized version - Slower
    public synchronized void synchronizedIncrement() {
        syncCounter++; // Requires lock acquisition
    }
    
    public static void benchmark() {
        PerformanceTest test = new PerformanceTest();
        int numThreads = 10;
        int iterations = 1_000_000;
        
        // Test atomic performance
        long start = System.currentTimeMillis();
        runTest(test::atomicIncrement, numThreads, iterations);
        long atomicTime = System.currentTimeMillis() - start;
        
        // Test synchronized performance
        start = System.currentTimeMillis();
        runTest(test::synchronizedIncrement, numThreads, iterations);
        long syncTime = System.currentTimeMillis() - start;
        
        System.out.println("Atomic time: " + atomicTime + "ms");
        System.out.println("Synchronized time: " + syncTime + "ms");
        // Atomic is typically 2-5x faster
    }
}
```

**AtomicBoolean Example:**

```java
class ConnectionManager {
    private AtomicBoolean connected = new AtomicBoolean(false);
    
    public boolean connect() {
        // Try to change from false to true atomically
        if (connected.compareAndSet(false, true)) {
            // Successfully acquired connection
            establishConnection();
            return true;
        }
        return false; // Already connected
    }
    
    public void disconnect() {
        connected.set(false); // Atomic set operation
        closeConnection();
    }
    
    public boolean isConnected() {
        return connected.get(); // Atomic read
    }
}
```

**AtomicReference Example:**

```java
class AtomicReferenceExample {
    private AtomicReference<Node> head = new AtomicReference<>();
    
    // Lock-free linked list insertion
    public void addFirst(String data) {
        Node newNode = new Node(data);
        Node currentHead;
        
        do {
            currentHead = head.get();
            newNode.next = currentHead;
        } while (!head.compareAndSet(currentHead, newNode));
        // Retry until successful CAS operation
    }
    
    private static class Node {
        volatile String data;
        volatile Node next;
        
        Node(String data) {
            this.data = data;
        }
    }
}
```

**When to Use Atomic Variables:**

**✅ Use when:**
- Simple operations (increment, decrement, compare-and-set)
- High contention scenarios
- Performance is critical
- Want lock-free algorithms

**❌ Don't use when:**
- Complex operations requiring multiple steps
- Need to coordinate multiple variables
- Operations are infrequent

**Atomic Operations vs Synchronized:**

```java
// ❌ Synchronized - Can cause thread blocking
class SynchronizedExample {
    private int value = 0;
    
    public synchronized int incrementAndGet() {
        return ++value; // Thread blocks if another thread holds lock
    }
}

// ✅ Atomic - Lock-free, better performance
class AtomicExample {
    private AtomicInteger value = new AtomicInteger(0);
    
    public int incrementAndGet() {
        return value.incrementAndGet(); // No blocking, uses CAS
    }
}
```

---

## 🎯 SCENARIO-BASED Questions

**Q25: How would you implement Producer-Consumer problem?**

**Answer:**

**Producer-Consumer Problem:** Classic synchronization problem where producers generate data and consumers process it, with a shared buffer.

**Solution 1: Using wait() and notify()**

```java
class ProducerConsumerWaitNotify {
    private final int[] buffer;
    private final int capacity;
    private int count = 0;
    private int in = 0;  // Producer index
    private int out = 0; // Consumer index
    
    public ProducerConsumerWaitNotify(int capacity) {
        this.capacity = capacity;
        this.buffer = new int[capacity];
    }
    
    // Producer method
    public synchronized void produce(int item) throws InterruptedException {
        // Wait while buffer is full
        while (count == capacity) {
            System.out.println("Buffer full, producer waiting...");
            wait();
        }
        
        // Add item to buffer
        buffer[in] = item;
        in = (in + 1) % capacity;
        count++;
        
        System.out.println("Produced: " + item + ", Buffer size: " + count);
        
        // Notify waiting consumers
        notifyAll();
    }
    
    // Consumer method
    public synchronized int consume() throws InterruptedException {
        // Wait while buffer is empty
        while (count == 0) {
            System.out.println("Buffer empty, consumer waiting...");
            wait();
        }
        
        // Remove item from buffer
        int item = buffer[out];
        out = (out + 1) % capacity;
        count--;
        
        System.out.println("Consumed: " + item + ", Buffer size: " + count);
        
        // Notify waiting producers
        notifyAll();
        
        return item;
    }
}

// Usage
class ProducerConsumerDemo {
    public static void main(String[] args) {
        ProducerConsumerWaitNotify pc = new ProducerConsumerWaitNotify(5);
        
        // Producer thread
        Thread producer = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    pc.produce(i);
                    Thread.sleep(100);
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        // Consumer thread
        Thread consumer = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    pc.consume();
                    Thread.sleep(150);
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        producer.start();
        consumer.start();
    }
}
```

**Solution 2: Using BlockingQueue (Recommended)**

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ArrayBlockingQueue;

class ProducerConsumerBlockingQueue {
    private final BlockingQueue<Integer> queue;
    
    public ProducerConsumerBlockingQueue(int capacity) {
        this.queue = new ArrayBlockingQueue<>(capacity);
    }
    
    // Producer - Much simpler with BlockingQueue
    public void produce(int item) throws InterruptedException {
        queue.put(item); // Automatically blocks when full
        System.out.println("Produced: " + item + ", Queue size: " + queue.size());
    }
    
    // Consumer - Much simpler with BlockingQueue
    public int consume() throws InterruptedException {
        int item = queue.take(); // Automatically blocks when empty
        System.out.println("Consumed: " + item + ", Queue size: " + queue.size());
        return item;
    }
}

// Multiple producers and consumers
class MultipleProducerConsumerDemo {
    public static void main(String[] args) {
        ProducerConsumerBlockingQueue pc = new ProducerConsumerBlockingQueue(10);
        
        // Multiple producers
        for (int i = 0; i < 3; i++) {
            final int producerId = i;
            new Thread(() -> {
                try {
                    for (int j = 0; j < 5; j++) {
                        int item = producerId * 10 + j;
                        pc.produce(item);
                        Thread.sleep(200);
                    }
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }, "Producer-" + i).start();
        }
        
        // Multiple consumers
        for (int i = 0; i < 2; i++) {
            new Thread(() -> {
                try {
                    for (int j = 0; j < 7; j++) {
                        pc.consume();
                        Thread.sleep(300);
                    }
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }, "Consumer-" + i).start();
        }
    }
}
```

**Solution 3: Using Lock and Condition**

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.Condition;

class ProducerConsumerLockCondition {
    private final int[] buffer;
    private final int capacity;
    private int count = 0;
    private int in = 0;
    private int out = 0;
    
    private final Lock lock = new ReentrantLock();
    private final Condition notFull = lock.newCondition();
    private final Condition notEmpty = lock.newCondition();
    
    public ProducerConsumerLockCondition(int capacity) {
        this.capacity = capacity;
        this.buffer = new int[capacity];
    }
    
    public void produce(int item) throws InterruptedException {
        lock.lock();
        try {
            // Wait while buffer is full
            while (count == capacity) {
                notFull.await();
            }
            
            // Add item
            buffer[in] = item;
            in = (in + 1) % capacity;
            count++;
            
            System.out.println("Produced: " + item);
            
            // Signal consumers
            notEmpty.signal();
            
        } finally {
            lock.unlock();
        }
    }
    
    public int consume() throws InterruptedException {
        lock.lock();
        try {
            // Wait while buffer is empty
            while (count == 0) {
                notEmpty.await();
            }
            
            // Remove item
            int item = buffer[out];
            out = (out + 1) % capacity;
            count--;
            
            System.out.println("Consumed: " + item);
            
            // Signal producers
            notFull.signal();
            
            return item;
            
        } finally {
            lock.unlock();
        }
    }
}
```

**Key Concepts:**

- **Bounded Buffer:** Fixed-size shared buffer
- **Synchronization:** Prevent race conditions
- **Blocking:** Wait when buffer full/empty
- **Notification:** Wake up waiting threads

**Best Practice:** Use `BlockingQueue` for production code as it's simpler and more reliable.

---

**Q26: How do you handle exceptions in multithreaded applications?**

**Answer:**

**Exception Handling in Multithreading** requires special consideration because exceptions in one thread don't automatically propagate to other threads.

**Key Challenges:**

- Uncaught exceptions terminate the thread
- Main thread doesn't know about exceptions in child threads
- Need proper cleanup and notification mechanisms

**Solution 1: UncaughtExceptionHandler**

```java
class ThreadExceptionHandling {
    public static void main(String[] args) {
        // Set default exception handler for all threads
        Thread.setDefaultUncaughtExceptionHandler((thread, exception) -> {
            System.err.println("Uncaught exception in thread " + 
                             thread.getName() + ": " + exception.getMessage());
            exception.printStackTrace();
            
            // Log to file, send alert, cleanup resources, etc.
            handleException(thread, exception);
        });
        
        // Create thread with specific exception handler
        Thread workerThread = new Thread(() -> {
            throw new RuntimeException("Something went wrong!");
        });
        
        // Set thread-specific exception handler
        workerThread.setUncaughtExceptionHandler((thread, exception) -> {
            System.err.println("Worker thread failed: " + exception.getMessage());
            // Specific handling for this thread
        });
        
        workerThread.start();
        
        // Main thread continues normally
        System.out.println("Main thread continues...");
    }
    
    private static void handleException(Thread thread, Throwable exception) {
        // Centralized exception handling
        // - Log error
        // - Send notifications
        // - Cleanup resources
        // - Restart thread if needed
    }
}
```

**Solution 2: Exception Handling with ExecutorService**

```java
class ExecutorServiceExceptionHandling {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3);
        
        // Method 1: Using Future to catch exceptions
        Future<String> future = executor.submit(() -> {
            Thread.sleep(1000);
            if (Math.random() > 0.5) {
                throw new RuntimeException("Task failed!");
            }
            return "Success";
        });
        
        try {
            String result = future.get(); // Exception propagates here
            System.out.println("Result: " + result);
        } catch (ExecutionException e) {
            Throwable cause = e.getCause();
            System.err.println("Task failed: " + cause.getMessage());
            handleTaskException(cause);
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
        
        // Method 2: Submit Runnable with exception handling
        executor.submit(() -> {
            try {
                // Risky operation
                performRiskyOperation();
            } catch (Exception e) {
                handleTaskException(e);
            }
        });
        
        executor.shutdown();
    }
    
    private static void performRiskyOperation() throws Exception {
        if (Math.random() > 0.7) {
            throw new Exception("Risky operation failed!");
        }
    }
    
    private static void handleTaskException(Throwable exception) {
        // Handle the exception appropriately
        System.err.println("Handling exception: " + exception.getMessage());
    }
}
```

**Solution 3: Custom Thread Pool with Exception Handling**

```java
class CustomThreadPoolWithExceptionHandling {
    public static void main(String[] args) {
        // Custom ThreadFactory with exception handler
        ThreadFactory threadFactory = new ThreadFactory() {
            private int counter = 0;
            
            @Override
            public Thread newThread(Runnable r) {
                Thread thread = new Thread(r, "CustomThread-" + ++counter);
                
                // Set exception handler for all threads
                thread.setUncaughtExceptionHandler((t, e) -> {
                    System.err.println("Exception in " + t.getName() + ": " + e.getMessage());
                    // Handle exception (log, alert, etc.)
                });
                
                return thread;
            }
        };
        
        // Create thread pool with custom thread factory
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
            2, 4, 60L, TimeUnit.SECONDS,
            new LinkedBlockingQueue<>(),
            threadFactory
        ) {
            // Override afterExecute to handle exceptions
            @Override
            protected void afterExecute(Runnable r, Throwable t) {
                super.afterExecute(r, t);
                
                if (t != null) {
                    System.err.println("Task execution failed: " + t.getMessage());
                }
                
                // Handle exceptions from Future tasks
                if (r instanceof FutureTask<?>) {
                    try {
                        ((FutureTask<?>) r).get();
                    } catch (ExecutionException e) {
                        t = e.getCause();
                        System.err.println("Future task failed: " + t.getMessage());
                    } catch (InterruptedException e) {
                        Thread.currentThread().interrupt();
                    }
                }
            }
        };
        
        // Submit tasks
        executor.submit(() -> {
            throw new RuntimeException("Task 1 failed!");
        });
        
        executor.submit(() -> {
            System.out.println("Task 2 completed successfully");
        });
        
        executor.shutdown();
    }
}
```

**Solution 4: Producer-Consumer with Exception Handling**

```java
class ProducerConsumerWithExceptionHandling {
    private final BlockingQueue<String> queue = new ArrayBlockingQueue<>(10);
    private final AtomicBoolean producerFailed = new AtomicBoolean(false);
    private final AtomicBoolean consumerFailed = new AtomicBoolean(false);
    
    public void startProducer() {
        Thread producer = new Thread(() -> {
            try {
                for (int i = 0; i < 100; i++) {
                    if (Math.random() > 0.95) { // Simulate random failure
                        throw new RuntimeException("Producer failed at item " + i);
                    }
                    queue.put("Item-" + i);
                    Thread.sleep(10);
                }
            } catch (Exception e) {
                producerFailed.set(true);
                System.err.println("Producer failed: " + e.getMessage());
                // Notify consumers to stop
                queue.offer("POISON_PILL");
            }
        });
        
        producer.setUncaughtExceptionHandler((t, e) -> {
            System.err.println("Uncaught exception in producer: " + e.getMessage());
            producerFailed.set(true);
        });
        
        producer.start();
    }
    
    public void startConsumer() {
        Thread consumer = new Thread(() -> {
            try {
                while (!consumerFailed.get()) {
                    String item = queue.take();
                    
                    if ("POISON_PILL".equals(item)) {
                        break; // Producer signaled shutdown
                    }
                    
                    if (Math.random() > 0.98) { // Simulate random failure
                        throw new RuntimeException("Consumer failed processing " + item);
                    }
                    
                    System.out.println("Processed: " + item);
                    Thread.sleep(20);
                }
            } catch (Exception e) {
                consumerFailed.set(true);
                System.err.println("Consumer failed: " + e.getMessage());
            }
        });
        
        consumer.setUncaughtExceptionHandler((t, e) -> {
            System.err.println("Uncaught exception in consumer: " + e.getMessage());
            consumerFailed.set(true);
        });
        
        consumer.start();
    }
    
    public boolean isSystemHealthy() {
        return !producerFailed.get() && !consumerFailed.get();
    }
}
```

**Best Practices for Exception Handling:**

1. **Always set UncaughtExceptionHandler**
2. **Use Future.get() to catch exceptions from Callable tasks**
3. **Implement proper cleanup in exception handlers**
4. **Use try-catch within Runnable tasks**
5. **Consider circuit breaker patterns for fault tolerance**
6. **Log exceptions with context information**
7. **Implement graceful shutdown mechanisms**

---

**Q27: How would you print numbers 1-10 using two threads alternately?**

**Answer:**

**Problem:** Two threads should print numbers 1-10 alternately (Thread1: 1,3,5,7,9 and Thread2: 2,4,6,8,10).

**Solution 1: Using wait() and notify()**

```java
class AlternatePrintingWaitNotify {
    private int number = 1;
    private boolean isOddTurn = true;
    private final int MAX_NUMBER = 10;
    
    // Method for odd numbers (1, 3, 5, 7, 9)
    public synchronized void printOdd() {
        while (number <= MAX_NUMBER) {
            // Wait if it's not odd's turn
            while (!isOddTurn && number <= MAX_NUMBER) {
                try {
                    wait();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    return;
                }
            }
            
            // Print odd number if within range
            if (number <= MAX_NUMBER) {
                System.out.println("Odd Thread: " + number);
                number++;
                isOddTurn = false; // Switch turn to even
                notify(); // Wake up even thread
            }
        }
    }
    
    // Method for even numbers (2, 4, 6, 8, 10)
    public synchronized void printEven() {
        while (number <= MAX_NUMBER) {
            // Wait if it's not even's turn
            while (isOddTurn && number <= MAX_NUMBER) {
                try {
                    wait();
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    return;
                }
            }
            
            // Print even number if within range
            if (number <= MAX_NUMBER) {
                System.out.println("Even Thread: " + number);
                number++;
                isOddTurn = true; // Switch turn to odd
                notify(); // Wake up odd thread
            }
        }
    }
    
    public static void main(String[] args) {
        AlternatePrintingWaitNotify printer = new AlternatePrintingWaitNotify();
        
        Thread oddThread = new Thread(printer::printOdd, "OddThread");
        Thread evenThread = new Thread(printer::printEven, "EvenThread");
        
        oddThread.start();
        evenThread.start();
        
        // Wait for both threads to complete
        try {
            oddThread.join();
            evenThread.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }
    }
}
```

**Solution 2: Using Lock and Condition**

```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
import java.util.concurrent.locks.Condition;

class AlternatePrintingLockCondition {
    private int number = 1;
    private boolean isOddTurn = true;
    private final int MAX_NUMBER = 10;
    
    private final Lock lock = new ReentrantLock();
    private final Condition oddCondition = lock.newCondition();
    private final Condition evenCondition = lock.newCondition();
    
    public void printOdd() {
        lock.lock();
        try {
            while (number <= MAX_NUMBER) {
                // Wait for odd turn
                while (!isOddTurn && number <= MAX_NUMBER) {
                    oddCondition.await();
                }
                
                if (number <= MAX_NUMBER) {
                    System.out.println("Odd Thread: " + number);
                    number++;
                    isOddTurn = false;
                    evenCondition.signal(); // Signal even thread
                }
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            lock.unlock();
        }
    }
    
    public void printEven() {
        lock.lock();
        try {
            while (number <= MAX_NUMBER) {
                // Wait for even turn
                while (isOddTurn && number <= MAX_NUMBER) {
                    evenCondition.await();
                }
                
                if (number <= MAX_NUMBER) {
                    System.out.println("Even Thread: " + number);
                    number++;
                    isOddTurn = true;
                    oddCondition.signal(); // Signal odd thread
                }
            }
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        } finally {
            lock.unlock();
        }
    }
    
    public static void main(String[] args) {
        AlternatePrintingLockCondition printer = new AlternatePrintingLockCondition();
        
        new Thread(printer::printOdd, "OddThread").start();
        new Thread(printer::printEven, "EvenThread").start();
    }
}
```

**Solution 3: Using Semaphore**

```java
import java.util.concurrent.Semaphore;

class AlternatePrintingSemaphore {
    private int number = 1;
    private final int MAX_NUMBER = 10;
    
    private final Semaphore oddSemaphore = new Semaphore(1); // Start with odd
    private final Semaphore evenSemaphore = new Semaphore(0); // Even waits
    
    public void printOdd() {
        while (number <= MAX_NUMBER) {
            try {
                oddSemaphore.acquire(); // Wait for odd turn
                
                if (number <= MAX_NUMBER) {
                    System.out.println("Odd Thread: " + number);
                    number++;
                    evenSemaphore.release(); // Give turn to even
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                break;
            }
        }
    }
    
    public void printEven() {
        while (number <= MAX_NUMBER) {
            try {
                evenSemaphore.acquire(); // Wait for even turn
                
                if (number <= MAX_NUMBER) {
                    System.out.println("Even Thread: " + number);
                    number++;
                    oddSemaphore.release(); // Give turn to odd
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                break;
            }
        }
    }
    
    public static void main(String[] args) {
        AlternatePrintingSemaphore printer = new AlternatePrintingSemaphore();
        
        new Thread(printer::printOdd, "OddThread").start();
        new Thread(printer::printEven, "EvenThread").start();
    }
}
```

**Solution 4: Using BlockingQueue**

```java
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.LinkedBlockingQueue;

class AlternatePrintingBlockingQueue {
    private final BlockingQueue<String> oddQueue = new LinkedBlockingQueue<>();
    private final BlockingQueue<String> evenQueue = new LinkedBlockingQueue<>();
    
    public void printNumbers() {
        // Start odd thread
        Thread oddThread = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i += 2) {
                    oddQueue.take(); // Wait for signal
                    System.out.println("Odd Thread: " + i);
                    evenQueue.put("even_turn"); // Signal even thread
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }, "OddThread");
        
        // Start even thread
        Thread evenThread = new Thread(() -> {
            try {
                for (int i = 2; i <= 10; i += 2) {
                    evenQueue.take(); // Wait for signal
                    System.out.println("Even Thread: " + i);
                    if (i < 10) { // Don't signal after last number
                        oddQueue.put("odd_turn"); // Signal odd thread
                    }
                }
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }, "EvenThread");
        
        oddThread.start();
        evenThread.start();
        
        // Start the process
        oddQueue.offer("start");
    }
    
    public static void main(String[] args) {
        new AlternatePrintingBlockingQueue().printNumbers();
    }
}
```

**Solution 5: Using AtomicInteger (More Complex but Lock-free)**

```java
import java.util.concurrent.atomic.AtomicInteger;

class AlternatePrintingAtomic {
    private final AtomicInteger counter = new AtomicInteger(1);
    private final int MAX_NUMBER = 10;
    
    public void printOdd() {
        while (counter.get() <= MAX_NUMBER) {
            int current = counter.get();
            
            // Check if it's odd's turn (odd numbers)
            if (current % 2 == 1 && current <= MAX_NUMBER) {
                if (counter.compareAndSet(current, current + 1)) {
                    System.out.println("Odd Thread: " + current);
                }
            } else {
                // Yield to give even thread a chance
                Thread.yield();
            }
        }
    }
    
    public void printEven() {
        while (counter.get() <= MAX_NUMBER) {
            int current = counter.get();
            
            // Check if it's even's turn (even numbers)
            if (current % 2 == 0 && current <= MAX_NUMBER) {
                if (counter.compareAndSet(current, current + 1)) {
                    System.out.println("Even Thread: " + current);
                }
            } else {
                // Yield to give odd thread a chance
                Thread.yield();
            }
        }
    }
    
    public static void main(String[] args) {
        AlternatePrintingAtomic printer = new AlternatePrintingAtomic();
        
        new Thread(printer::printOdd, "OddThread").start();
        new Thread(printer::printEven, "EvenThread").start();
    }
}
```

**Expected Output (All Solutions):**
```
Odd Thread: 1
Even Thread: 2
Odd Thread: 3
Even Thread: 4
Odd Thread: 5
Even Thread: 6
Odd Thread: 7
Even Thread: 8
Odd Thread: 9
Even Thread: 10
```

**Key Concepts:**

- **Coordination:** Threads must coordinate their execution
- **Turn-taking:** Only one thread should print at a time
- **Synchronization:** Prevent race conditions
- **Communication:** Threads need to signal each other

**Best Practice:** Use `wait()/notify()` or `Lock/Condition` for learning, but prefer higher-level utilities like `Semaphore` or `BlockingQueue` in production code.

---

I've created Part 2 of the multithreading answers covering:

✅ **Questions 22-27 completed:**
- Runnable vs Callable differences
- ThreadLocal usage and examples
- Atomic variables (AtomicInteger, AtomicBoolean)
- Producer-Consumer implementation (3 different approaches)
- Exception handling in multithreaded applications
- Alternate printing problem (5 different solutions)

Would you like me to continue with the remaining questions (Q28-Q58) covering:
- Thread-safe Singleton patterns
- Concurrent collections (ConcurrentHashMap, BlockingQueue)
- Advanced concurrency utilities (CountDownLatch, CyclicBarrier, Semaphore)
- CompletableFuture and Java Memory Model
- Coding problems and interview scenarios

Each solution includes multiple approaches and best practices for real interview situations!
