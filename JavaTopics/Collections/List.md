# List Interface in Java - Interview Questions

## 1. Difference between ArrayList and LinkedList

### Key Differences:

| Aspect | ArrayList | LinkedList |
|--------|-----------|------------|
| **Internal Structure** | Dynamic array (resizable array) | Doubly linked list |
| **Memory Location** | Contiguous memory | Non-contiguous memory |
| **Access Time** | O(1) random access | O(n) sequential access |
| **Insertion/Deletion at beginning** | O(n) - requires shifting | O(1) |
| **Insertion/Deletion at end** | O(1) amortized | O(1) |
| **Insertion/Deletion at middle** | O(n) - requires shifting | O(n) to find + O(1) to insert |
| **Memory Overhead** | Lower (only stores data) | Higher (stores data + 2 pointers) |
| **Cache Performance** | Better (data locality) | Worse (scattered memory) |

### Code Examples:

```java
import java.util.*;

public class ArrayListVsLinkedList {
    public static void main(String[] args) {
        // ArrayList Example
        List<String> arrayList = new ArrayList<>();
        arrayList.add("A");
        arrayList.add("B");
        arrayList.add("C");
        
        // Fast random access
        System.out.println("ArrayList get(1): " + arrayList.get(1)); // O(1)
        
        // LinkedList Example
        List<String> linkedList = new LinkedList<>();
        linkedList.add("A");
        linkedList.add("B");
        linkedList.add("C");
        
        // Slow random access
        System.out.println("LinkedList get(1): " + linkedList.get(1)); // O(n)
        
        // Performance comparison for insertions at beginning
        performanceTest();
    }
    
    public static void performanceTest() {
        int size = 100000;
        
        // ArrayList - insertion at beginning
        List<Integer> arrayList = new ArrayList<>();
        long start = System.currentTimeMillis();
        for (int i = 0; i < size; i++) {
            arrayList.add(0, i); // O(n) for each insertion
        }
        long arrayListTime = System.currentTimeMillis() - start;
        
        // LinkedList - insertion at beginning
        List<Integer> linkedList = new LinkedList<>();
        start = System.currentTimeMillis();
        for (int i = 0; i < size; i++) {
            linkedList.add(0, i); // O(1) for each insertion
        }
        long linkedListTime = System.currentTimeMillis() - start;
        
        System.out.println("ArrayList insertion time: " + arrayListTime + "ms");
        System.out.println("LinkedList insertion time: " + linkedListTime + "ms");
    }
}
```

### When to use:
- **ArrayList**: When you need frequent random access, more reads than writes, better memory efficiency
- **LinkedList**: When you need frequent insertions/deletions at beginning/middle, implementing queue/deque operations

---

## 2. Difference between ArrayList and Vector

### Key Differences:

| Aspect | ArrayList | Vector |
|--------|-----------|--------|
| **Synchronization** | Not synchronized (not thread-safe) | Synchronized (thread-safe) |
| **Performance** | Faster (no synchronization overhead) | Slower (synchronization overhead) |
| **Growth Rate** | Increases by 50% (1.5x) | Increases by 100% (2x) |
| **Introduction** | Java 1.2 (Collections Framework) | Java 1.0 (Legacy class) |
| **Null Values** | Allows null values | Allows null values |
| **Enumeration** | Only Iterator (fail-fast) | Both Iterator and Enumeration |

### Code Examples:

```java
import java.util.*;

public class ArrayListVsVector {
    public static void main(String[] args) {
        // ArrayList Example
        List<String> arrayList = new ArrayList<>();
        arrayList.add("Item1");
        arrayList.add("Item2");
        arrayList.add(null); // Allows null
        
        // Vector Example
        Vector<String> vector = new Vector<>();
        vector.add("Item1");
        vector.add("Item2");
        vector.add(null); // Allows null
        
        // Thread Safety Demo
        threadSafetyDemo();
        
        // Growth Rate Demo
        growthRateDemo();
    }
    
    public static void threadSafetyDemo() {
        // ArrayList - Not thread-safe
        List<Integer> arrayList = new ArrayList<>();
        
        // Vector - Thread-safe
        Vector<Integer> vector = new Vector<>();
        
        // Multiple threads accessing ArrayList (unsafe)
        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                arrayList.add(i);
            }
        });
        
        Thread t2 = new Thread(() -> {
            for (int i = 1000; i < 2000; i++) {
                arrayList.add(i);
            }
        });
        
        // Multiple threads accessing Vector (safe)
        Thread t3 = new Thread(() -> {
            for (int i = 0; i < 1000; i++) {
                vector.add(i);
            }
        });
        
        Thread t4 = new Thread(() -> {
            for (int i = 1000; i < 2000; i++) {
                vector.add(i);
            }
        });
        
        try {
            t1.start(); t2.start();
            t1.join(); t2.join();
            System.out.println("ArrayList size (may be inconsistent): " + arrayList.size());
            
            t3.start(); t4.start();
            t3.join(); t4.join();
            System.out.println("Vector size (consistent): " + vector.size());
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    
    public static void growthRateDemo() {
        // ArrayList growth rate demonstration
        List<Integer> arrayList = new ArrayList<>(2);
        System.out.println("ArrayList initial capacity: 2");
        
        // Vector growth rate demonstration
        Vector<Integer> vector = new Vector<>(2);
        System.out.println("Vector initial capacity: 2");
        
        // Add elements to trigger growth
        for (int i = 0; i < 10; i++) {
            arrayList.add(i);
            vector.add(i);
        }
        
        // Note: Actual capacity inspection requires reflection or custom implementation
        System.out.println("ArrayList grows by 50% when capacity is exceeded");
        System.out.println("Vector grows by 100% when capacity is exceeded");
    }
}

// Synchronized ArrayList alternative
class SynchronizedArrayListExample {
    public static void main(String[] args) {
        // Making ArrayList thread-safe
        List<String> synchronizedList = Collections.synchronizedList(new ArrayList<>());
        
        // Still need external synchronization for iteration
        synchronized (synchronizedList) {
            Iterator<String> iterator = synchronizedList.iterator();
            while (iterator.hasNext()) {
                System.out.println(iterator.next());
            }
        }
    }
}
```

### When to use:
- **ArrayList**: Modern applications, single-threaded access, better performance
- **Vector**: Legacy code, when you need built-in synchronization (though CopyOnWriteArrayList or Collections.synchronizedList() are preferred)

---

## 3. When to use Stack vs ArrayDeque

### Key Differences:

| Aspect | Stack | ArrayDeque |
|--------|-------|------------|
| **Type** | Legacy class (extends Vector) | Modern implementation |
| **Thread Safety** | Synchronized (thread-safe) | Not synchronized |
| **Performance** | Slower (synchronization + inheritance overhead) | Faster |
| **Memory** | Higher overhead | Lower overhead |
| **Operations** | LIFO operations | Both LIFO and FIFO operations |
| **Recommended** | No (legacy) | Yes (modern alternative) |

### Code Examples:

```java
import java.util.*;

public class StackVsArrayDeque {
    public static void main(String[] args) {
        // Stack Example (Legacy)
        Stack<String> stack = new Stack<>();
        stack.push("First");
        stack.push("Second");
        stack.push("Third");
        
        System.out.println("Stack peek: " + stack.peek()); // Third
        System.out.println("Stack pop: " + stack.pop());   // Third
        System.out.println("Stack size: " + stack.size()); // 2
        
        // ArrayDeque as Stack (Modern)
        Deque<String> dequeStack = new ArrayDeque<>();
        dequeStack.push("First");
        dequeStack.push("Second");
        dequeStack.push("Third");
        
        System.out.println("ArrayDeque peek: " + dequeStack.peek()); // Third
        System.out.println("ArrayDeque pop: " + dequeStack.pop());   // Third
        System.out.println("ArrayDeque size: " + dequeStack.size()); // 2
        
        // Performance comparison
        performanceComparison();
        
        // ArrayDeque versatility
        dequeVersatility();
    }
    
    public static void performanceComparison() {
        int operations = 1000000;
        
        // Stack performance
        Stack<Integer> stack = new Stack<>();
        long start = System.currentTimeMillis();
        for (int i = 0; i < operations; i++) {
            stack.push(i);
        }
        for (int i = 0; i < operations; i++) {
            stack.pop();
        }
        long stackTime = System.currentTimeMillis() - start;
        
        // ArrayDeque performance
        Deque<Integer> deque = new ArrayDeque<>();
        start = System.currentTimeMillis();
        for (int i = 0; i < operations; i++) {
            deque.push(i);
        }
        for (int i = 0; i < operations; i++) {
            deque.pop();
        }
        long dequeTime = System.currentTimeMillis() - start;
        
        System.out.println("Stack time: " + stackTime + "ms");
        System.out.println("ArrayDeque time: " + dequeTime + "ms");
        System.out.println("ArrayDeque is " + (stackTime / (double) dequeTime) + "x faster");
    }
    
    public static void dequeVersatility() {
        Deque<String> deque = new ArrayDeque<>();
        
        // Use as Stack (LIFO)
        deque.push("Stack1");
        deque.push("Stack2");
        System.out.println("Stack pop: " + deque.pop()); // Stack2
        
        // Use as Queue (FIFO)
        deque.offer("Queue1");
        deque.offer("Queue2");
        System.out.println("Queue poll: " + deque.poll()); // Stack1 (first remaining)
        
        // Double-ended operations
        deque.addFirst("First");
        deque.addLast("Last");
        System.out.println("Remove first: " + deque.removeFirst()); // First
        System.out.println("Remove last: " + deque.removeLast());   // Last
    }
}

// Thread-safe alternatives for modern applications
class ThreadSafeStackExample {
    public static void main(String[] args) {
        // Option 1: Synchronized wrapper
        Deque<String> synchronizedDeque = Collections.synchronizedDeque(new ArrayDeque<>());
        
        // Option 2: ConcurrentLinkedDeque (for high concurrency)
        Deque<String> concurrentDeque = new ConcurrentLinkedDeque<>();
        
        // Option 3: Custom thread-safe stack using locks
        ThreadSafeStack<String> customStack = new ThreadSafeStack<>();
        customStack.push("Item1");
        customStack.push("Item2");
        System.out.println(customStack.pop()); // Item2
    }
}

// Custom thread-safe stack implementation
class ThreadSafeStack<T> {
    private final Deque<T> deque = new ArrayDeque<>();
    
    public synchronized void push(T item) {
        deque.push(item);
    }
    
    public synchronized T pop() {
        return deque.pop();
    }
    
    public synchronized T peek() {
        return deque.peek();
    }
    
    public synchronized boolean isEmpty() {
        return deque.isEmpty();
    }
    
    public synchronized int size() {
        return deque.size();
    }
}
```

### When to use:

#### Use ArrayDeque when:
- Building modern applications
- Need better performance
- Want flexibility (stack, queue, or deque operations)
- Single-threaded or handling synchronization externally
- Memory efficiency is important

#### Use Stack when:
- Working with legacy code that requires Stack class
- Need built-in thread safety (though not recommended)
- Maintaining existing codebase that heavily uses Stack

### Best Practices:

```java
public class StackBestPractices {
    public static void main(String[] args) {
        // ✅ Recommended: Use ArrayDeque as Stack
        Deque<String> stack = new ArrayDeque<>();
        
        // ✅ For thread safety, use synchronized wrapper
        Deque<String> threadSafeStack = Collections.synchronizedDeque(new ArrayDeque<>());
        
        // ✅ Or use ConcurrentLinkedDeque for high concurrency
        Deque<String> concurrentStack = new ConcurrentLinkedDeque<>();
        
        // ❌ Avoid: Using Stack class in new code
        // Stack<String> legacyStack = new Stack<>();
        
        // Example usage
        demonstrateStackOperations(stack);
    }
    
    public static void demonstrateStackOperations(Deque<String> stack) {
        // Stack operations
        stack.push("Bottom");
        stack.push("Middle");
        stack.push("Top");
        
        System.out.println("Peek: " + stack.peek());     // Top
        System.out.println("Pop: " + stack.pop());       // Top
        System.out.println("Size: " + stack.size());     // 2
        System.out.println("Empty: " + stack.isEmpty()); // false
        
        // Clear all elements
        while (!stack.isEmpty()) {
            System.out.println("Removing: " + stack.pop());
        }
    }
}
```

## Summary

1. **ArrayList vs LinkedList**: Choose ArrayList for random access and better memory efficiency; LinkedList for frequent insertions/deletions at beginning/middle.

2. **ArrayList vs Vector**: Prefer ArrayList in modern applications; Vector is legacy and has synchronization overhead.

3. **Stack vs ArrayDeque**: Always prefer ArrayDeque over Stack for new development - it's faster, more flexible, and follows modern design principles.

These implementations showcase the practical differences and help you make informed decisions based on your specific use case requirements.