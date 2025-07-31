# Java Multithreading Interview Questions - Complete Answers Guide

## 🔥 MUST-KNOW Questions (High Priority)

### Core Threading Concepts

**Q1: What is multithreading and why is it important in Java?**

**Answer:**
Multithreading is the ability of a program to execute multiple threads concurrently within a single process.

**Key Points:**
- Multiple threads share the same memory space but execute independently
- Enables parallel execution of tasks
- Improves application performance and responsiveness

**Why Important:**
- **Performance**: Better CPU utilization, especially on multi-core systems
- **Responsiveness**: UI remains responsive while background tasks run
- **Resource Sharing**: Threads share memory, making communication efficient
- **Scalability**: Handle multiple requests simultaneously (web servers)

**Example:**
```java
// Without multithreading - Sequential execution
downloadFile1(); // Takes 5 seconds
downloadFile2(); // Takes 5 seconds
// Total: 10 seconds

// With multithreading - Parallel execution
Thread t1 = new Thread(() -> downloadFile1());
Thread t2 = new Thread(() -> downloadFile2());
t1.start(); t2.start();
// Total: ~5 seconds (both run simultaneously)
```

---

**Q2: What is the difference between process and thread?**

**Answer:**

| Process | Thread |
|---------|--------|
| Independent execution unit | Lightweight execution unit within a process |
| Has its own memory space | Shares memory space with other threads |
| High creation cost | Low creation cost |
| Inter-process communication is expensive | Inter-thread communication is cheap |
| Crash doesn't affect other processes | One thread crash can affect entire process |
| Examples: Browser, Word, Calculator | Examples: UI thread, Background thread |

**Simple Analogy:**
- **Process** = A restaurant (independent business)
- **Thread** = Waiters in the restaurant (work together, share resources)

---

**Q3: How many ways can you create a thread in Java?**

**Answer:**
There are **4 main ways** to create threads:

**1. Extending Thread Class:**
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread running: " + Thread.currentThread().getName());
    }
}

// Usage
MyThread t = new MyThread();
t.start();
```

**2. Implementing Runnable Interface:**
```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Thread running: " + Thread.currentThread().getName());
    }
}

// Usage
Thread t = new Thread(new MyRunnable());
t.start();
```

**3. Using Callable Interface (with ExecutorService):**
```java
class MyCallable implements Callable<String> {
    public String call() {
        return "Result from thread";
    }
}

// Usage
ExecutorService executor = Executors.newSingleThreadExecutor();
Future<String> future = executor.submit(new MyCallable());
String result = future.get();
```

**4. Using Lambda Expressions (Java 8+):**
```java
// With Runnable
Thread t = new Thread(() -> {
    System.out.println("Lambda thread running");
});
t.start();

// With ExecutorService
ExecutorService executor = Executors.newSingleThreadExecutor();
executor.submit(() -> System.out.println("Lambda with executor"));
```

---

**Q4: What is the difference between extending Thread class and implementing Runnable interface?**

**Answer:**

| Extending Thread | Implementing Runnable |
|------------------|----------------------|
| **Inheritance:** Single inheritance only | **Inheritance:** Can extend other classes |
| **Flexibility:** Less flexible | **Flexibility:** More flexible |
| **Coupling:** Tight coupling | **Coupling:** Loose coupling |
| **Reusability:** Less reusable | **Reusability:** More reusable |
| **Design:** Violates "IS-A" relationship | **Design:** Follows "HAS-A" relationship |

**Example Demonstrating the Difference:**
```java
// ❌ Problem with extending Thread
class MyTask extends Thread {
    // Can't extend any other class now!
    // What if we need to extend DatabaseConnection?
}

// ✅ Better approach with Runnable
class MyTask implements Runnable {
    // Can still extend other classes if needed
    public void run() {
        // Task logic here
    }
}

class DatabaseTask extends DatabaseConnection implements Runnable {
    public void run() {
        // Can extend DatabaseConnection AND implement Runnable
    }
}
```

**Best Practice:** Always prefer Runnable interface over extending Thread class.

---

**Q5: What is the difference between start() and run() method?**

**Answer:**

| start() Method | run() Method |
|----------------|--------------|
| Creates new thread | No new thread created |
| Calls run() method internally | Direct method call |
| Can be called only once | Can be called multiple times |
| Asynchronous execution | Synchronous execution |
| Thread goes to RUNNABLE state | Executes in current thread |

**Code Example:**
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Running in: " + Thread.currentThread().getName());
        for(int i = 0; i < 5; i++) {
            System.out.println("Count: " + i);
            try { Thread.sleep(1000); } catch(Exception e) {}
        }
    }
}

public class ThreadDemo {
    public static void main(String[] args) {
        MyThread t = new MyThread();
        
        // Using start() - Creates new thread
        t.start(); // Output: Running in: Thread-0
        
        // Using run() - Runs in main thread
        // t.run(); // Output: Running in: main
        
        System.out.println("Main thread: " + Thread.currentThread().getName());
    }
}
```

**Key Point:** Always use `start()` to create a new thread. Using `run()` directly defeats the purpose of multithreading.

---

**Q6: What are the different states of a thread in Java?**

**Answer:**
A thread can be in one of **6 states**:

**1. NEW:** Thread created but not started
**2. RUNNABLE:** Thread is executing or ready to execute
**3. BLOCKED:** Thread is blocked waiting for a monitor lock
**4. WAITING:** Thread is waiting indefinitely for another thread
**5. TIMED_WAITING:** Thread is waiting for a specified period
**6. TERMINATED:** Thread has completed execution

**State Transition Diagram:**
```
NEW → start() → RUNNABLE ↔ BLOCKED
                    ↓
              WAITING/TIMED_WAITING
                    ↓
               TERMINATED
```

**Code Example:**
```java
public class ThreadStates {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new Thread(() -> {
            try {
                Thread.sleep(2000); // TIMED_WAITING
                synchronized(ThreadStates.class) {
                    // Some work
                }
            } catch (InterruptedException e) {}
        });
        
        System.out.println("1. " + t.getState()); // NEW
        
        t.start();
        System.out.println("2. " + t.getState()); // RUNNABLE
        
        Thread.sleep(100);
        System.out.println("3. " + t.getState()); // TIMED_WAITING
        
        t.join(); // Wait for thread to complete
        System.out.println("4. " + t.getState()); // TERMINATED
    }
}
```

---

**Q7: What is synchronization in Java and why do we need it?**

**Answer:**
Synchronization is a mechanism to control access to shared resources in a multithreaded environment.

**Why We Need It:**
- **Race Condition Prevention:** Multiple threads accessing shared data simultaneously
- **Data Consistency:** Ensure data integrity and consistency
- **Thread Safety:** Make code safe for concurrent execution

**Problems Without Synchronization:**
```java
class Counter {
    private int count = 0;
    
    public void increment() {
        count++; // Not atomic! Can cause race condition
    }
    
    public int getCount() {
        return count;
    }
}

// Problem: Two threads incrementing simultaneously might lost updates
// Expected: 200, Actual: might be 150-200 (unpredictable)
```

**Solution With Synchronization:**
```java
class SynchronizedCounter {
    private int count = 0;
    
    public synchronized void increment() {
        count++; // Now thread-safe
    }
    
    public synchronized int getCount() {
        return count;
    }
}
```

**Types of Synchronization:**
1. **Method Level:** `synchronized` keyword with method
2. **Block Level:** `synchronized` block with specific object
3. **Static Synchronization:** For static methods/variables

---

**Q8: What is synchronized keyword? Difference between synchronized method and block?**

**Answer:**

**Synchronized Keyword:**
- Provides mutual exclusion (only one thread can access at a time)
- Uses intrinsic lock (monitor) associated with object
- Prevents race conditions and ensures thread safety

**Synchronized Method vs Block:**

| Synchronized Method | Synchronized Block |
|-------------------|-------------------|
| Locks entire method | Locks only specific code block |
| Less granular control | More granular control |
| Uses 'this' object as lock | Can specify any object as lock |
| Less performance efficient | More performance efficient |
| Simpler syntax | More complex syntax |

**Examples:**

**Synchronized Method:**
```java
class BankAccount {
    private double balance = 1000;
    
    // Entire method is synchronized
    public synchronized void withdraw(double amount) {
        if(balance >= amount) {
            System.out.println("Withdrawing: " + amount);
            balance -= amount;
            System.out.println("Remaining balance: " + balance);
        }
    }
}
```

**Synchronized Block:**
```java
class BankAccount {
    private double balance = 1000;
    private Object lock = new Object();
    
    public void withdraw(double amount) {
        // Only critical section is synchronized
        synchronized(lock) {
            if(balance >= amount) {
                balance -= amount;
            }
        }
        // Non-critical code outside synchronized block
        System.out.println("Transaction completed");
    }
}
```

**Best Practice:** Use synchronized blocks for better performance when only part of method needs synchronization.

---

### Thread Communication

**Q9: What are wait(), notify(), and notifyAll() methods?**

**Answer:**
These methods are used for **inter-thread communication** and **coordination**.

**wait():**
- Causes current thread to wait until another thread invokes notify()
- Releases the lock and goes to WAITING state
- Must be called from synchronized context

**notify():**
- Wakes up one thread that is waiting on the object's monitor
- The awakened thread competes for the lock
- Must be called from synchronized context

**notifyAll():**
- Wakes up all threads waiting on the object's monitor
- All threads compete for the lock
- Must be called from synchronized context

**Producer-Consumer Example:**
```java
class SharedResource {
    private int data;
    private boolean available = false;
    
    public synchronized void produce(int value) {
        while(available) {
            try {
                wait(); // Wait until data is consumed
            } catch(InterruptedException e) {}
        }
        data = value;
        available = true;
        System.out.println("Produced: " + value);
        notify(); // Notify consumer
    }
    
    public synchronized int consume() {
        while(!available) {
            try {
                wait(); // Wait until data is available
            } catch(InterruptedException e) {}
        }
        available = false;
        System.out.println("Consumed: " + data);
        notify(); // Notify producer
        return data;
    }
}
```

**Key Rules:**
- Always call wait(), notify(), notifyAll() inside synchronized block
- Always use wait() in a loop (while/do-while)
- Use notifyAll() when multiple threads might be waiting

---

**Q10: Why are wait(), notify(), and notifyAll() methods defined in Object class?**

**Answer:**

**Reasons:**

1. **Every Object Can Be a Monitor:** In Java, every object can serve as a monitor (lock)
2. **Universal Availability:** Since every class inherits from Object, these methods are available everywhere
3. **Lock Association:** These methods work with the intrinsic lock of the object
4. **Design Consistency:** The monitor pattern requires these methods to be tied to the object being synchronized on

**Technical Explanation:**
```java
class Example {
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();
    
    public void method1() {
        synchronized(lock1) {
            lock1.wait(); // Wait on lock1's monitor
            lock1.notify(); // Notify threads waiting on lock1
        }
    }
    
    public void method2() {
        synchronized(lock2) {
            lock2.wait(); // Wait on lock2's monitor
            lock2.notify(); // Notify threads waiting on lock2
        }
    }
}
```

**Why Not in Thread Class:**
- Different objects need different monitors
- Thread class would limit flexibility
- Multiple threads can wait on the same object

**Memory Model:**
```
Object Header → Monitor/Lock Information
             → Wait Set (threads waiting on this object)
             → Entry Set (threads trying to acquire lock)
```

---

**Q11: What is the difference between sleep() and wait() methods?**

**Answer:**

| sleep() | wait() |
|---------|--------|
| **Class:** Thread class (static method) | **Class:** Object class (instance method) |
| **Lock:** Does NOT release lock | **Lock:** Releases the lock |
| **Context:** Can be called anywhere | **Context:** Must be in synchronized block |
| **Purpose:** Pause thread execution | **Purpose:** Inter-thread communication |
| **Wake up:** After time expires | **Wake up:** By notify()/notifyAll() or timeout |
| **Exception:** InterruptedException | **Exception:** InterruptedException |

**Code Example:**
```java
class SleepVsWait {
    private static final Object lock = new Object();
    
    public static void sleepExample() {
        synchronized(lock) {
            System.out.println("Before sleep - Lock held");
            try {
                Thread.sleep(2000); // Holds the lock for 2 seconds
            } catch(InterruptedException e) {}
            System.out.println("After sleep - Lock still held");
        }
    }
    
    public static void waitExample() {
        synchronized(lock) {
            System.out.println("Before wait - Lock held");
            try {
                lock.wait(2000); // Releases lock, waits for 2 seconds or notification
            } catch(InterruptedException e) {}
            System.out.println("After wait - Lock reacquired");
        }
    }
}
```

**When to Use:**
- **sleep():** When you want to pause execution for a specific time
- **wait():** When you want to wait for a condition to be met by another thread

---

**Q12: What is deadlock and how can you avoid it?**

**Answer:**

**Deadlock:** A situation where two or more threads are blocked forever, waiting for each other to release resources.

**Deadlock Example:**
```java
class DeadlockExample {
    private static final Object lock1 = new Object();
    private static final Object lock2 = new Object();
    
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            synchronized(lock1) {
                System.out.println("Thread 1: Acquired lock1");
                try { Thread.sleep(100); } catch(Exception e) {}
                
                synchronized(lock2) { // Waiting for lock2
                    System.out.println("Thread 1: Acquired lock2");
                }
            }
        });
        
        Thread t2 = new Thread(() -> {
            synchronized(lock2) {
                System.out.println("Thread 2: Acquired lock2");
                try { Thread.sleep(100); } catch(Exception e) {}
                
                synchronized(lock1) { // Waiting for lock1
                    System.out.println("Thread 2: Acquired lock1");
                }
            }
        });
        
        t1.start();
        t2.start();
    }
}
```

**How to Avoid Deadlock:**

**1. Lock Ordering:**
```java
// Always acquire locks in the same order
synchronized(lock1) {
    synchronized(lock2) {
        // Critical section
    }
}
```

**2. Timeout:**
```java
if(lock1.tryLock(1000, TimeUnit.MILLISECONDS)) {
    try {
        if(lock2.tryLock(1000, TimeUnit.MILLISECONDS)) {
            try {
                // Critical section
            } finally {
                lock2.unlock();
            }
        }
    } finally {
        lock1.unlock();
    }
}
```

**3. Avoid Nested Locks:**
```java
// Instead of nested synchronization
public void transfer(Account from, Account to, int amount) {
    // Get both locks atomically
    acquireLocks(from, to);
    try {
        from.withdraw(amount);
        to.deposit(amount);
    } finally {
        releaseLocks(from, to);
    }
}
```

**Best Practices:**
- Avoid nested locks when possible
- Use timeout with tryLock()
- Follow consistent lock ordering
- Keep synchronized blocks small
- Consider using higher-level concurrency utilities

---

## 💡 LIKELY Questions (Medium Priority)

### Thread Safety & Synchronization

**Q13: What is thread safety and how do you achieve it?**

**Answer:**

**Thread Safety:** A code is thread-safe if it behaves correctly when accessed concurrently by multiple threads.

**Characteristics of Thread-Safe Code:**
- No race conditions
- Consistent state across threads
- Predictable behavior in concurrent environment

**Ways to Achieve Thread Safety:**

**1. Synchronization:**
```java
class ThreadSafeCounter {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
    
    public synchronized int getCount() {
        return count;
    }
}
```

**2. Immutable Objects:**
```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    
    public ImmutablePerson(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
    // No setters - object is immutable
}
```

**3. Thread-Local Storage:**
```java
class ThreadSafeService {
    private ThreadLocal<SimpleDateFormat> formatter = 
        ThreadLocal.withInitial(() -> new SimpleDateFormat("yyyy-MM-dd"));
    
    public String formatDate(Date date) {
        return formatter.get().format(date); // Thread-safe
    }
}
```

**4. Atomic Variables:**
```java
class AtomicCounter {
    private AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet(); // Thread-safe atomic operation
    }
    
    public int getCount() {
        return count.get();
    }
}
```

**5. Concurrent Collections:**
```java
// Thread-safe collections
Map<String, String> map = new ConcurrentHashMap<>();
List<String> list = new CopyOnWriteArrayList<>();
Queue<String> queue = new ConcurrentLinkedQueue<>();
```

---

**Q14: What is volatile keyword and when to use it?**

**Answer:**

**Volatile Keyword:** Ensures that changes to a variable are immediately visible to all threads.

**Key Features:**
- **Visibility:** Changes are immediately visible across threads
- **No Caching:** Variable is always read from main memory
- **Prevents Reordering:** Compiler optimizations that reorder operations are prevented
- **Not Atomic:** Does not guarantee atomicity for compound operations

**When to Use:**
1. **Flags/Status Variables:**
```java
class TaskManager {
    private volatile boolean shutdown = false;
    
    public void shutdown() {
        shutdown = true; // Immediately visible to all threads
    }
    
    public void doWork() {
        while(!shutdown) { // Always reads latest value
            // Do some work
        }
    }
}
```

2. **Double-Checked Locking:**
```java
class Singleton {
    private static volatile Singleton instance;
    
    public static Singleton getInstance() {
        if(instance == null) { // First check
            synchronized(Singleton.class) {
                if(instance == null) { // Second check
                    instance = new Singleton(); // volatile ensures proper initialization
                }
            }
        }
        return instance;
    }
}
```

**What Volatile Does NOT Do:**
```java
class Counter {
    private volatile int count = 0;
    
    // ❌ This is NOT thread-safe even with volatile
    public void increment() {
        count++; // This is actually: count = count + 1 (3 operations)
    }
    
    // ✅ This IS thread-safe with volatile
    public void setCount(int newCount) {
        count = newCount; // Single assignment operation
    }
}
```

**Memory Model Visualization:**
```
Without volatile:
Thread 1: [Local Cache] ← → [Main Memory] ← → [Local Cache] :Thread 2
          count = 5                count = 0              count = 0

With volatile:
Thread 1: [No Cache] ← → [Main Memory] ← → [No Cache] :Thread 2
          count = 5         count = 5         count = 5
```

---

**Q15: What is the difference between synchronized and volatile?**

**Answer:**

| synchronized | volatile |
|-------------|----------|
| **Mutual Exclusion:** YES | **Mutual Exclusion:** NO |
| **Visibility:** YES | **Visibility:** YES |
| **Atomicity:** YES | **Atomicity:** NO |
| **Performance:** Slower (blocking) | **Performance:** Faster (non-blocking) |
| **Scope:** Method/Block level | **Scope:** Variable level |
| **Deadlock:** Possible | **Deadlock:** Not possible |

**Use Cases Comparison:**

**Use synchronized when:**
```java
class BankAccount {
    private double balance = 1000;
    
    // Need atomicity for multiple operations
    public synchronized void transfer(double amount) {
        balance -= amount; // Multiple operations need to be atomic
        // Update transaction log
        // Send notification
    }
}
```

**Use volatile when:**
```java
class GameEngine {
    private volatile boolean gameRunning = true;
    
    // Simple flag that needs immediate visibility
    public void stopGame() {
        gameRunning = false; // Simple assignment
    }
    
    public void gameLoop() {
        while(gameRunning) { // Need latest value
            // Game logic
        }
    }
}
```

**Can be used together:**
```java
class StatefulService {
    private volatile boolean initialized = false;
    private final Object lock = new Object();
    
    public void initialize() {
        synchronized(lock) { // Synchronized for atomicity
            if(!initialized) {
                // Initialization logic
                initialized = true; // Volatile for visibility
            }
        }
    }
}
```

**Q16: Which collection classes are thread-safe in Java?**

**Answer:**

**Thread-Safe Collections (Legacy):**

- **Vector:** Thread-safe version of ArrayList
- **Hashtable:** Thread-safe version of HashMap
- **Stack:** Thread-safe stack implementation

```java
// Legacy thread-safe collections (synchronized methods)
Vector<String> vector = new Vector<>(); // Thread-safe ArrayList
Hashtable<String, String> hashtable = new Hashtable<>(); // Thread-safe HashMap
Stack<String> stack = new Stack<>(); // Thread-safe stack
```

**Modern Concurrent Collections:**

```java
// Better performance concurrent collections
ConcurrentHashMap<String, String> concurrentMap = new ConcurrentHashMap<>();
CopyOnWriteArrayList<String> copyOnWriteList = new CopyOnWriteArrayList<>();
ConcurrentLinkedQueue<String> concurrentQueue = new ConcurrentLinkedQueue<>();
ArrayBlockingQueue<String> blockingQueue = new ArrayBlockingQueue<>(10);
```

**Making Non-Thread-Safe Collections Thread-Safe:**

```java
// Using Collections.synchronizedXXX()
List<String> syncList = Collections.synchronizedList(new ArrayList<>());
Map<String, String> syncMap = Collections.synchronizedMap(new HashMap<>());
Set<String> syncSet = Collections.synchronizedSet(new HashSet<>());

// Important: Manual synchronization needed for iteration
synchronized(syncList) {
    for(String item : syncList) {
        // Safe iteration
    }
}
```

**Performance Comparison:**

| Collection Type | Thread Safety | Performance | Best For |
|----------------|---------------|-------------|----------|
| ArrayList | No | Fastest | Single-threaded |
| Vector | Yes (synchronized) | Slower | Legacy code |
| CopyOnWriteArrayList | Yes (copy-on-write) | Read-heavy | More reads than writes |
| Collections.synchronizedList() | Yes (wrapper) | Medium | General purpose |

---

**Q17: What is race condition in multithreading?**

**Answer:**

**Race Condition:** Occurs when multiple threads access shared data concurrently and the final result depends on the timing/order of execution.

**Example of Race Condition:**

```java
class UnsafeCounter {
    private int count = 0;
    
    public void increment() {
        count++; // This is actually 3 operations:
                 // 1. Read count value
                 // 2. Add 1 to it
                 // 3. Write back to count
    }
    
    public int getCount() {
        return count;
    }
}

// Race condition demo
public class RaceConditionDemo {
    public static void main(String[] args) throws InterruptedException {
        UnsafeCounter counter = new UnsafeCounter();
        
        // Create 1000 threads, each incrementing 1000 times
        Thread[] threads = new Thread[1000];
        for(int i = 0; i < 1000; i++) {
            threads[i] = new Thread(() -> {
                for(int j = 0; j < 1000; j++) {
                    counter.increment();
                }
            });
            threads[i].start();
        }
        
        // Wait for all threads to complete
        for(Thread t : threads) {
            t.join();
        }
        
        System.out.println("Expected: 1000000");
        System.out.println("Actual: " + counter.getCount()); // Usually less than 1000000
    }
}
```

**Why Race Condition Occurs:**

```
Thread 1: Read count (0) → Add 1 → Write (1)
Thread 2: Read count (0) → Add 1 → Write (1)
Result: count = 1 (should be 2)
```

**Solutions to Race Condition:**

**1. Synchronized Methods:**
```java
class SafeCounter {
    private int count = 0;
    
    public synchronized void increment() {
        count++; // Now atomic
    }
    
    public synchronized int getCount() {
        return count;
    }
}
```

**2. Atomic Variables:**
```java
class AtomicCounter {
    private AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet(); // Atomic operation
    }
    
    public int getCount() {
        return count.get();
    }
}
```

**3. Locks:**
```java
class LockCounter {
    private int count = 0;
    private final ReentrantLock lock = new ReentrantLock();
    
    public void increment() {
        lock.lock();
        try {
            count++;
        } finally {
            lock.unlock();
        }
    }
}
```

---

**Q18: What is monitor or intrinsic lock in Java?**

**Answer:**

**Monitor/Intrinsic Lock:** Every object in Java has an associated monitor (lock) that provides mutual exclusion for synchronized methods and blocks.

**Key Concepts:**

- **One Lock Per Object:** Each object has exactly one monitor
- **Mutual Exclusion:** Only one thread can hold the monitor at a time
- **Reentrant:** Same thread can acquire the same lock multiple times
- **Automatic Release:** Lock is automatically released when leaving synchronized block/method

**How Monitor Works:**

```java
class MonitorExample {
    private int value = 0;
    
    // Method level synchronization - uses 'this' object's monitor
    public synchronized void methodA() {
        value++;
        methodB(); // Same thread can call another synchronized method
    }
    
    public synchronized void methodB() {
        value *= 2; // Uses same monitor as methodA
    }
    
    // Block level synchronization - can specify different objects
    public void methodC() {
        synchronized(this) { // Uses 'this' object's monitor
            value--;
        }
    }
    
    // Using different object as monitor
    private final Object lock = new Object();
    public void methodD() {
        synchronized(lock) { // Uses 'lock' object's monitor
            value += 10;
        }
    }
}
```

**Monitor States:**

```
Object Monitor:
├── Owner: Thread currently holding the lock (null if available)
├── Entry Set: Threads waiting to acquire the lock
├── Wait Set: Threads that called wait() on this object
└── Recursion Count: How many times current thread acquired the lock
```

**Static Synchronization:**

```java
class StaticSync {
    private static int staticValue = 0;
    
    // Uses Class object's monitor (StaticSync.class)
    public static synchronized void staticMethod() {
        staticValue++;
    }
    
    // Equivalent to above
    public static void staticMethod2() {
        synchronized(StaticSync.class) {
            staticValue++;
        }
    }
}
```

**Monitor Example with Multiple Objects:**

```java
class MultipleMonitors {
    private final Object lock1 = new Object();
    private final Object lock2 = new Object();
    
    public void method1() {
        synchronized(lock1) {
            // Only threads waiting on lock1 are blocked
            System.out.println("Using lock1");
        }
    }
    
    public void method2() {
        synchronized(lock2) {
            // Can run concurrently with method1
            System.out.println("Using lock2");
        }
    }
}
```

---

### Advanced Threading

**Q19: What is ExecutorService and its advantages?**

**Answer:**

**ExecutorService:** A higher-level framework for managing and controlling thread execution, part of `java.util.concurrent` package.

**Advantages:**

- **Thread Pool Management:** Reuses threads instead of creating new ones
- **Resource Control:** Limits the number of concurrent threads
- **Task Submission:** Provides flexible ways to submit tasks
- **Lifecycle Management:** Proper shutdown and termination
- **Exception Handling:** Better error handling mechanisms
- **Future Support:** Asynchronous task execution with results

**Basic Usage:**

```java
import java.util.concurrent.*;

public class ExecutorServiceExample {
    public static void main(String[] args) {
        // Create thread pool with 3 threads
        ExecutorService executor = Executors.newFixedThreadPool(3);
        
        // Submit tasks
        for(int i = 0; i < 10; i++) {
            final int taskId = i;
            executor.submit(() -> {
                System.out.println("Task " + taskId + " executed by " + 
                                 Thread.currentThread().getName());
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            });
        }
        
        // Shutdown executor
        executor.shutdown();
        try {
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow();
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
    }
}
```

**Types of Task Submission:**

```java
ExecutorService executor = Executors.newFixedThreadPool(3);

// 1. Submit Runnable (no return value)
Future<?> future1 = executor.submit(() -> {
    System.out.println("Runnable task");
});

// 2. Submit Callable (returns value)
Future<String> future2 = executor.submit(() -> {
    Thread.sleep(1000);
    return "Task completed";
});

// 3. Submit Runnable with result
Future<String> future3 = executor.submit(() -> {
    System.out.println("Task with result");
}, "Default result");

// Get results
try {
    String result = future2.get(5, TimeUnit.SECONDS);
    System.out.println("Result: " + result);
} catch (TimeoutException e) {
    System.out.println("Task timed out");
}
```

**Comparison: Manual Thread vs ExecutorService:**

```java
// ❌ Manual thread creation (not recommended)
class ManualThreads {
    public static void processRequests(List<Request> requests) {
        for(Request request : requests) {
            Thread thread = new Thread(() -> {
                processRequest(request);
            });
            thread.start(); // Creates new thread for each request
        }
    }
}

// ✅ Using ExecutorService (recommended)
class ExecutorServiceExample {
    private final ExecutorService executor = Executors.newFixedThreadPool(10);
    
    public void processRequests(List<Request> requests) {
        for(Request request : requests) {
            executor.submit(() -> {
                processRequest(request); // Reuses existing threads
            });
        }
    }
}
```

---

**Q20: What are different types of thread pools in Java?**

**Answer:**

**Thread Pool Types:**

**1. Fixed Thread Pool:**
```java
// Creates fixed number of threads
ExecutorService fixedPool = Executors.newFixedThreadPool(5);

// Use case: Known workload, controlled resource usage
// Good for: Web servers, database connection pools
```

**2. Cached Thread Pool:**
```java
// Creates threads as needed, reuses existing ones
ExecutorService cachedPool = Executors.newCachedThreadPool();

// Use case: Short-lived asynchronous tasks
// Good for: I/O intensive tasks, varying workload
```

**3. Single Thread Executor:**
```java
// Single worker thread with unbounded queue
ExecutorService singlePool = Executors.newSingleThreadExecutor();

// Use case: Sequential execution, event processing
// Good for: Background tasks, file processing
```

**4. Scheduled Thread Pool:**
```java
// For delayed and periodic task execution
ScheduledExecutorService scheduledPool = Executors.newScheduledThreadPool(3);

// Schedule one-time task
scheduledPool.schedule(() -> {
    System.out.println("Executed after 5 seconds");
}, 5, TimeUnit.SECONDS);

// Schedule periodic task
scheduledPool.scheduleAtFixedRate(() -> {
    System.out.println("Executed every 10 seconds");
}, 0, 10, TimeUnit.SECONDS);
```

**5. Work Stealing Pool (Java 8+):**
```java
// Uses work-stealing algorithm for better performance
ExecutorService workStealingPool = Executors.newWorkStealingPool();

// Use case: Recursive tasks, divide-and-conquer algorithms
// Good for: Parallel processing, CPU-intensive tasks
```

**Custom Thread Pool:**
```java
// Create custom thread pool with specific parameters
ThreadPoolExecutor customPool = new ThreadPoolExecutor(
    2,                      // Core pool size
    10,                     // Maximum pool size
    60L,                    // Keep alive time
    TimeUnit.SECONDS,       // Time unit
    new LinkedBlockingQueue<>(100), // Work queue
    new ThreadFactoryBuilder()
        .setNameFormat("Custom-Thread-%d")
        .build(),           // Thread factory
    new ThreadPoolExecutor.AbortPolicy() // Rejection policy
);
```

**Choosing the Right Thread Pool:**

| Pool Type | Best For | Thread Count | Queue |
|-----------|----------|--------------|-------|
| Fixed | Steady workload | Fixed | Unbounded |
| Cached | Variable workload | Unlimited | Direct handoff |
| Single | Sequential processing | 1 | Unbounded |
| Scheduled | Timed tasks | Fixed | Delayed queue |
| Work Stealing | Parallel tasks | CPU cores | Deque per thread |

**Real-world Example:**
```java
class WebServer {
    // Fixed pool for handling HTTP requests
    private final ExecutorService requestHandler = 
        Executors.newFixedThreadPool(50);
    
    // Scheduled pool for maintenance tasks
    private final ScheduledExecutorService maintenancePool = 
        Executors.newScheduledThreadPool(2);
    
    public void start() {
        // Schedule cleanup every hour
        maintenancePool.scheduleAtFixedRate(this::cleanup, 
            1, 1, TimeUnit.HOURS);
    }
    
    public void handleRequest(HttpRequest request) {
        requestHandler.submit(() -> {
            processRequest(request);
        });
    }
}
```

---

**Q21: What is Future and Callable interface?**

**Answer:**

**Callable Interface:** Similar to Runnable but can return a value and throw checked exceptions.

**Future Interface:** Represents the result of an asynchronous computation.

**Callable vs Runnable:**

| Runnable | Callable |
|----------|----------|
| `void run()` | `V call() throws Exception` |
| No return value | Returns a value |
| Cannot throw checked exceptions | Can throw checked exceptions |
| Used with Thread, ExecutorService | Used with ExecutorService only |

**Basic Example:**

```java
// Callable implementation
class CalculationTask implements Callable<Integer> {
    private int number;
    
    public CalculationTask(int number) {
        this.number = number;
    }
    
    @Override
    public Integer call() throws Exception {
        Thread.sleep(1000); // Simulate work
        return number * number;
    }
}

// Using Callable with Future
public class CallableFutureExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        
        // Submit Callable task
        Future<Integer> future = executor.submit(new CalculationTask(10));
        
        try {
            // Do other work while task executes
            System.out.println("Task submitted, doing other work...");
            
            // Get result (blocks if not ready)
            Integer result = future.get(5, TimeUnit.SECONDS);
            System.out.println("Result: " + result);
            
        } catch (TimeoutException e) {
            System.out.println("Task timed out");
            future.cancel(true); // Cancel the task
        } catch (Exception e) {
            System.out.println("Task failed: " + e.getMessage());
        } finally {
            executor.shutdown();
        }
    }
}
```

**Future Methods:**

```java
Future<String> future = executor.submit(someCallable);

// Check if task is completed
boolean isDone = future.isDone();

// Check if task was cancelled
boolean isCancelled = future.isCancelled();

// Cancel the task
boolean cancelled = future.cancel(true); // true = interrupt if running

// Get result (blocking)
String result = future.get();

// Get result with timeout
String result = future.get(10, TimeUnit.SECONDS);
```

**Multiple Callable Tasks:**

```java
public class MultipleCallables {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newFixedThreadPool(3);
        
        // Create multiple tasks
        List<Callable<String>> tasks = Arrays.asList(
            () -> { Thread.sleep(1000); return "Task 1"; },
            () -> { Thread.sleep(2000); return "Task 2"; },
            () -> { Thread.sleep(1500); return "Task 3"; }
        );
        
        // Submit all tasks
        List<Future<String>> futures = executor.invokeAll(tasks);
        
        // Get all results
        for(Future<String> future : futures) {
            System.out.println("Result: " + future.get());
        }
        
        executor.shutdown();
    }
}
```

**Exception Handling:**

```java
class RiskyTask implements Callable<String> {
    @Override
    public String call() throws Exception {
        if(Math.random() > 0.5) {
            throw new RuntimeException("Task failed!");
        }
        return "Success";
    }
}

// Exception handling with Future
Future<String> future = executor.submit(new RiskyTask());
try {
    String result = future.get();
    System.out.println("Result: " + result);
} catch (ExecutionException e) {
    Throwable cause = e.getCause(); // Get the original exception
    System.out.println("Task failed: " + cause.getMessage());
}
```

---

I've created a comprehensive answers file covering the most important multithreading questions. The file includes:

1. **Core Threading Concepts** (Q1-Q8)
2. **Thread Communication** (Q9-Q12) 
3. **Thread Safety & Synchronization** (Q13-Q18)
4. **Advanced Threading** (Q19-Q21)

Each answer includes:
- Clear explanations
- Code examples
- Practical use cases
- Best practices
- Common pitfalls to avoid

Would you like me to continue with the remaining questions (Q22-Q58) covering topics like:
- ThreadLocal
- Atomic variables
- Scenario-based questions
- Concurrent collections
- Advanced concurrency utilities
- Coding problems

The format is designed to be interview-friendly with easy-to-remember explanations and practical examples you can discuss during interviews.
