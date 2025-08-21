# Java Collection Framework - Complete Guide

## 1. What is the Collection Framework in Java?

The Java Collection Framework is a unified architecture for representing and manipulating collections of objects. It provides a set of interfaces, implementations, and algorithms to work with groups of objects efficiently.

### Key Components:
- **Interfaces**: Define abstract data types (Collection, List, Set, Map, Queue, etc.)
- **Implementations**: Concrete classes that implement these interfaces (ArrayList, HashMap, HashSet, etc.)
- **Algorithms**: Static methods that perform useful operations like sorting and searching

### Collection Hierarchy:
```
Collection (Interface)
├── List (Interface)
│   ├── ArrayList
│   ├── LinkedList
│   └── Vector
├── Set (Interface)
│   ├── HashSet
│   ├── LinkedHashSet
│   └── TreeSet
└── Queue (Interface)
    ├── PriorityQueue
    └── LinkedList

Map (Interface) - Separate hierarchy
├── HashMap
├── LinkedHashMap
└── TreeMap
```

## 2. Difference between Collection and Collections class

### Collection (Interface)
- Root interface of the collection hierarchy
- Defines basic operations like add(), remove(), size(), isEmpty()
- Extended by List, Set, and Queue interfaces
- Cannot be instantiated directly

```java
import java.util.*;

public class CollectionExample {
    public static void main(String[] args) {
        // Collection is an interface - cannot instantiate directly
        // Collection<String> collection = new Collection<String>(); // ERROR
        
        // But we can use it as reference type
        Collection<String> collection = new ArrayList<>();
        collection.add("Hello");
        collection.add("World");
        
        System.out.println("Size: " + collection.size());
        System.out.println("Contains 'Hello': " + collection.contains("Hello"));
        
        // Iterate through collection
        for(String item : collection) {
            System.out.println(item);
        }
    }
}
```

### Collections (Class)
- Utility class in java.util package
- Contains static methods for operating on collections
- Provides algorithms like sort(), reverse(), shuffle(), binarySearch()

```java
import java.util.*;

public class CollectionsExample {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>();
        numbers.add(5);
        numbers.add(2);
        numbers.add(8);
        numbers.add(1);
        
        System.out.println("Original: " + numbers);
        
        // Using Collections utility methods
        Collections.sort(numbers);
        System.out.println("Sorted: " + numbers);
        
        Collections.reverse(numbers);
        System.out.println("Reversed: " + numbers);
        
        Collections.shuffle(numbers);
        System.out.println("Shuffled: " + numbers);
        
        int max = Collections.max(numbers);
        int min = Collections.min(numbers);
        System.out.println("Max: " + max + ", Min: " + min);
        
        // Binary search (requires sorted list)
        Collections.sort(numbers);
        int index = Collections.binarySearch(numbers, 5);
        System.out.println("Index of 5: " + index);
    }
}
```

## 3. Difference between List, Set, and Map

### List
- **Ordered collection** that allows duplicates
- Elements are **indexed** (accessible by position)
- **Implementations**: ArrayList, LinkedList, Vector

```java
import java.util.*;

public class ListExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        
        // Adding elements (duplicates allowed)
        list.add("Apple");
        list.add("Banana");
        list.add("Apple");  // Duplicate allowed
        list.add("Cherry");
        
        System.out.println("List: " + list);
        System.out.println("Size: " + list.size());
        
        // Accessing by index
        System.out.println("Element at index 0: " + list.get(0));
        System.out.println("Element at index 2: " + list.get(2));
        
        // Modifying by index
        list.set(1, "Blueberry");
        System.out.println("After modification: " + list);
        
        // Finding index
        System.out.println("Index of 'Apple': " + list.indexOf("Apple"));
        System.out.println("Last index of 'Apple': " + list.lastIndexOf("Apple"));
        
        // Removing by index
        list.remove(2);
        System.out.println("After removing index 2: " + list);
    }
}
```

### Set
- Collection that **does not allow duplicates**
- **No indexing** (elements not accessible by position)
- **Implementations**: HashSet, LinkedHashSet, TreeSet

```java
import java.util.*;

public class SetExample {
    public static void main(String[] args) {
        // HashSet - no order guarantee
        Set<String> hashSet = new HashSet<>();
        hashSet.add("Apple");
        hashSet.add("Banana");
        hashSet.add("Apple");  // Duplicate - will be ignored
        hashSet.add("Cherry");
        System.out.println("HashSet: " + hashSet);
        
        // LinkedHashSet - maintains insertion order
        Set<String> linkedHashSet = new LinkedHashSet<>();
        linkedHashSet.add("Apple");
        linkedHashSet.add("Banana");
        linkedHashSet.add("Apple");  // Duplicate - will be ignored
        linkedHashSet.add("Cherry");
        System.out.println("LinkedHashSet: " + linkedHashSet);
        
        // TreeSet - sorted order
        Set<String> treeSet = new TreeSet<>();
        treeSet.add("Cherry");
        treeSet.add("Apple");
        treeSet.add("Banana");
        treeSet.add("Apple");  // Duplicate - will be ignored
        System.out.println("TreeSet: " + treeSet);
        
        // Set operations
        Set<Integer> set1 = new HashSet<>(Arrays.asList(1, 2, 3, 4));
        Set<Integer> set2 = new HashSet<>(Arrays.asList(3, 4, 5, 6));
        
        // Union
        Set<Integer> union = new HashSet<>(set1);
        union.addAll(set2);
        System.out.println("Union: " + union);
        
        // Intersection
        Set<Integer> intersection = new HashSet<>(set1);
        intersection.retainAll(set2);
        System.out.println("Intersection: " + intersection);
        
        // Difference
        Set<Integer> difference = new HashSet<>(set1);
        difference.removeAll(set2);
        System.out.println("Difference: " + difference);
    }
}
```

### Map
- **Not part of Collection interface** hierarchy
- Stores **key-value pairs**
- **Keys must be unique**, values can be duplicates
- **Implementations**: HashMap, LinkedHashMap, TreeMap

```java
import java.util.*;

public class MapExample {
    public static void main(String[] args) {
        // HashMap - no order guarantee
        Map<String, Integer> hashMap = new HashMap<>();
        hashMap.put("Alice", 25);
        hashMap.put("Bob", 30);
        hashMap.put("Charlie", 25);  // Duplicate value allowed
        hashMap.put("Alice", 26);    // Overwrites previous value
        
        System.out.println("HashMap: " + hashMap);
        System.out.println("Alice's age: " + hashMap.get("Alice"));
        System.out.println("Contains key 'Bob': " + hashMap.containsKey("Bob"));
        System.out.println("Contains value 30: " + hashMap.containsValue(30));
        
        // LinkedHashMap - maintains insertion order
        Map<String, Integer> linkedHashMap = new LinkedHashMap<>();
        linkedHashMap.put("First", 1);
        linkedHashMap.put("Second", 2);
        linkedHashMap.put("Third", 3);
        System.out.println("LinkedHashMap: " + linkedHashMap);
        
        // TreeMap - sorted by keys
        Map<String, Integer> treeMap = new TreeMap<>();
        treeMap.put("Charlie", 25);
        treeMap.put("Alice", 26);
        treeMap.put("Bob", 30);
        System.out.println("TreeMap: " + treeMap);
        
        // Iterating through Map
        System.out.println("\nIterating through HashMap:");
        for(Map.Entry<String, Integer> entry : hashMap.entrySet()) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }
        
        // Working with keys and values
        System.out.println("Keys: " + hashMap.keySet());
        System.out.println("Values: " + hashMap.values());
    }
}
```

## 4. Difference between Iterator and ListIterator

### Iterator
- Works with **all Collection types**
- **Unidirectional** (forward only)
- Methods: hasNext(), next(), remove()
- Cannot modify elements during iteration (except remove)

```java
import java.util.*;

public class IteratorExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("A");
        list.add("B");
        list.add("C");
        list.add("D");
        
        System.out.println("Original list: " + list);
        
        // Using Iterator
        System.out.println("\nUsing Iterator (forward only):");
        Iterator<String> iterator = list.iterator();
        while(iterator.hasNext()) {
            String element = iterator.next();
            System.out.println(element);
            
            // Remove elements that equal "B"
            if(element.equals("B")) {
                iterator.remove();  // Safe removal during iteration
            }
        }
        
        System.out.println("After removal: " + list);
        
        // Iterator with Set
        Set<Integer> set = new HashSet<>();
        set.add(1);
        set.add(2);
        set.add(3);
        
        System.out.println("\nIterating Set:");
        Iterator<Integer> setIterator = set.iterator();
        while(setIterator.hasNext()) {
            System.out.println(setIterator.next());
        }
    }
}
```

### ListIterator
- Works **only with List implementations**
- **Bidirectional** (forward and backward)
- Methods: hasNext(), next(), hasPrevious(), previous(), add(), set(), remove()
- Can modify list during iteration

```java
import java.util.*;

public class ListIteratorExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("A");
        list.add("B");
        list.add("C");
        list.add("D");
        
        System.out.println("Original list: " + list);
        
        // Forward iteration with ListIterator
        System.out.println("\nForward iteration:");
        ListIterator<String> listIterator = list.listIterator();
        while(listIterator.hasNext()) {
            int index = listIterator.nextIndex();
            String element = listIterator.next();
            System.out.println("Index " + index + ": " + element);
            
            // Modify elements during iteration
            if(element.equals("B")) {
                listIterator.set("MODIFIED_B");  // Replace current element
            }
            
            // Add new element after "C"
            if(element.equals("C")) {
                listIterator.add("NEW_ELEMENT");
            }
        }
        
        System.out.println("After forward iteration: " + list);
        
        // Backward iteration
        System.out.println("\nBackward iteration:");
        while(listIterator.hasPrevious()) {
            int index = listIterator.previousIndex();
            String element = listIterator.previous();
            System.out.println("Index " + index + ": " + element);
        }
        
        // Starting from specific position
        System.out.println("\nStarting from index 2:");
        ListIterator<String> listIterator2 = list.listIterator(2);
        while(listIterator2.hasNext()) {
            System.out.println(listIterator2.next());
        }
    }
}
```

## 5. Difference between fail-fast and fail-safe iterators

### Fail-fast iterators
- **Immediately throw `ConcurrentModificationException`** if collection is modified during iteration
- Work on **original collection**
- Used by ArrayList, HashMap, HashSet, etc.
- Detect structural modifications through **modCount field**
- **Memory efficient** but **not thread-safe**

```java
import java.util.*;

public class FailFastExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("A");
        list.add("B");
        list.add("C");
        
        System.out.println("Fail-fast Iterator Example:");
        
        try {
            Iterator<String> iterator = list.iterator();
            while(iterator.hasNext()) {
                String element = iterator.next();
                System.out.println(element);
                
                // This will cause ConcurrentModificationException
                if(element.equals("B")) {
                    list.add("D");  // Modifying collection during iteration
                }
            }
        } catch(ConcurrentModificationException e) {
            System.out.println("ConcurrentModificationException caught!");
            System.out.println("Cannot modify collection during iteration with fail-fast iterator");
        }
        
        // Safe way to modify during iteration
        System.out.println("\nSafe modification using Iterator.remove():");
        list = new ArrayList<>(Arrays.asList("A", "B", "C", "D"));
        Iterator<String> safeIterator = list.iterator();
        while(safeIterator.hasNext()) {
            String element = safeIterator.next();
            if(element.equals("B")) {
                safeIterator.remove();  // Safe removal
            }
        }
        System.out.println("After safe removal: " + list);
        
        // Demonstrating with HashMap
        System.out.println("\nFail-fast with HashMap:");
        Map<String, Integer> map = new HashMap<>();
        map.put("key1", 1);
        map.put("key2", 2);
        map.put("key3", 3);
        
        try {
            for(String key : map.keySet()) {
                System.out.println(key + " -> " + map.get(key));
                if(key.equals("key2")) {
                    map.put("key4", 4);  // This will cause exception
                }
            }
        } catch(ConcurrentModificationException e) {
            System.out.println("ConcurrentModificationException in HashMap iteration!");
        }
    }
}
```

### Fail-safe iterators
- **Do not throw exceptions** when collection is modified during iteration
- Work on a **copy/snapshot** of the collection
- Used by ConcurrentHashMap, CopyOnWriteArrayList, etc.
- Allow concurrent modifications but **may not reflect latest changes**
- **Higher memory overhead** but **thread-safe**

```java
import java.util.*;
import java.util.concurrent.*;

public class FailSafeExample {
    public static void main(String[] args) {
        System.out.println("Fail-safe Iterator Example:");
        
        // ConcurrentHashMap - fail-safe
        ConcurrentHashMap<String, Integer> concurrentMap = new ConcurrentHashMap<>();
        concurrentMap.put("key1", 1);
        concurrentMap.put("key2", 2);
        concurrentMap.put("key3", 3);
        
        System.out.println("Original map: " + concurrentMap);
        
        // Safe modification during iteration
        for(String key : concurrentMap.keySet()) {
            System.out.println(key + " -> " + concurrentMap.get(key));
            if(key.equals("key2")) {
                concurrentMap.put("key4", 4);  // Safe modification
                System.out.println("Added key4 during iteration");
            }
        }
        
        System.out.println("Map after iteration: " + concurrentMap);
        
        // CopyOnWriteArrayList - fail-safe
        System.out.println("\nCopyOnWriteArrayList Example:");
        CopyOnWriteArrayList<String> copyOnWriteList = new CopyOnWriteArrayList<>();
        copyOnWriteList.add("A");
        copyOnWriteList.add("B");
        copyOnWriteList.add("C");
        
        System.out.println("Original list: " + copyOnWriteList);
        
        for(String element : copyOnWriteList) {
            System.out.println(element);
            if(element.equals("B")) {
                copyOnWriteList.add("D");  // Safe modification
                System.out.println("Added D during iteration");
            }
        }
        
        System.out.println("List after iteration: " + copyOnWriteList);
        
        // Demonstrating snapshot behavior
        System.out.println("\nSnapshot behavior demonstration:");
        CopyOnWriteArrayList<Integer> numbers = new CopyOnWriteArrayList<>();
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
        
        Iterator<Integer> iterator = numbers.iterator();
        numbers.add(4);  // Added after iterator creation
        numbers.add(5);  // Added after iterator creation
        
        System.out.print("Iterator sees: ");
        while(iterator.hasNext()) {
            System.out.print(iterator.next() + " ");  // Will only see 1, 2, 3
        }
        
        System.out.println("\nActual list: " + numbers);  // Shows 1, 2, 3, 4, 5
    }
}
```

### Comparison Table

| Feature | Fail-Fast | Fail-Safe |
|---------|-----------|-----------|
| **Exception on modification** | Yes (ConcurrentModificationException) | No |
| **Memory usage** | Lower | Higher (works on copy) |
| **Thread safety** | Not thread-safe | Thread-safe |
| **Performance** | Better | Slower due to copying |
| **Reflects latest changes** | Yes (if no modification) | No (works on snapshot) |
| **Examples** | ArrayList, HashMap, HashSet | ConcurrentHashMap, CopyOnWriteArrayList |

### Thread Safety Example

```java
import java.util.*;
import java.util.concurrent.*;

public class ThreadSafetyExample {
    public static void main(String[] args) throws InterruptedException {
        // Fail-fast - not thread-safe
        System.out.println("Testing fail-fast (ArrayList) with multiple threads:");
        List<Integer> arrayList = new ArrayList<>();
        for(int i = 0; i < 1000; i++) {
            arrayList.add(i);
        }
        
        // This may cause issues with multiple threads
        Thread readerThread = new Thread(() -> {
            try {
                for(Integer num : arrayList) {
                    Thread.sleep(1); // Small delay
                }
            } catch(Exception e) {
                System.out.println("Exception in reader: " + e.getClass().getSimpleName());
            }
        });
        
        Thread writerThread = new Thread(() -> {
            try {
                Thread.sleep(50);
                arrayList.add(1001);
            } catch(Exception e) {
                System.out.println("Exception in writer: " + e.getClass().getSimpleName());
            }
        });
        
        readerThread.start();
        writerThread.start();
        readerThread.join();
        writerThread.join();
        
        // Fail-safe - thread-safe
        System.out.println("\nTesting fail-safe (CopyOnWriteArrayList) with multiple threads:");
        CopyOnWriteArrayList<Integer> copyOnWriteList = new CopyOnWriteArrayList<>();
        for(int i = 0; i < 100; i++) {
            copyOnWriteList.add(i);
        }
        
        Thread safeReaderThread = new Thread(() -> {
            int count = 0;
            for(Integer num : copyOnWriteList) {
                count++;
                try {
                    Thread.sleep(1);
                } catch(InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
            System.out.println("Reader processed " + count + " elements");
        });
        
        Thread safeWriterThread = new Thread(() -> {
            try {
                Thread.sleep(50);
                copyOnWriteList.add(1001);
                System.out.println("Writer added element. List size: " + copyOnWriteList.size());
            } catch(InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        });
        
        safeReaderThread.start();
        safeWriterThread.start();
        safeReaderThread.join();
        safeWriterThread.join();
        
        System.out.println("Final list size: " + copyOnWriteList.size());
    }
}
```

## Summary

The Java Collection Framework provides a comprehensive set of data structures and algorithms for handling groups of objects. Understanding the differences between these components is crucial for writing efficient and safe Java applications:

- **Use List** when you need ordered, indexed collections that allow duplicates
- **Use Set** when you need collections without duplicates
- **Use Map** when you need key-value associations
- **Use Iterator** for general collection traversal
- **Use ListIterator** when you need bidirectional traversal of lists with modification capabilities
- **Choose fail-fast** for single-threaded applications where you want immediate error detection
- **Choose fail-safe** for multi-threaded applications where thread safety is more important than immediate consistency
