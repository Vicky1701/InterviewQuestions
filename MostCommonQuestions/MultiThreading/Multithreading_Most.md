# Java Multithreading Interview Questions - Complete Study Guide

## 🔥 MUST-KNOW Questions (High Priority)

### Core Threading Concepts
**Q1: What is multithreading and why is it important in Java?**

**Q2: What is the difference between process and thread?**

**Q3: How many ways can you create a thread in Java?**

**Q4: What is the difference between extending Thread class and implementing Runnable interface?**

**Q5: What is the difference between start() and run() method?**

**Q6: What are the different states of a thread in Java?**

**Q7: What is synchronization in Java and why do we need it?**

**Q8: What is synchronized keyword? Difference between synchronized method and block?**

### Thread Communication
**Q9: What are wait(), notify(), and notifyAll() methods?**

**Q10: Why are wait(), notify(), and notifyAll() methods defined in Object class?**

**Q11: What is the difference between sleep() and wait() methods?**

**Q12: What is deadlock and how can you avoid it?**

---

## 💡 LIKELY Questions (Medium Priority)

### Thread Safety & Synchronization
**Q13: What is thread safety and how do you achieve it?**

**Q14: What is volatile keyword and when to use it?**

**Q15: What is the difference between synchronized and volatile?**

**Q16: Which collection classes are thread-safe in Java?**

**Q17: What is race condition in multithreading?**

**Q18: What is monitor or intrinsic lock in Java?**

### Advanced Threading
**Q19: What is ExecutorService and its advantages?**

**Q20: What are different types of thread pools in Java?**

**Q21: What is Future and Callable interface?**

**Q22: What is the difference between Runnable and Callable?**

**Q23: What is ThreadLocal and when would you use it?**

**Q24: What are atomic variables in Java? (AtomicInteger, AtomicBoolean)**

---

## 🎯 SCENARIO-BASED Questions (Medium Priority)

**Q25: How would you implement Producer-Consumer problem?**

**Q26: How do you handle exceptions in multithreaded applications?**

**Q27: How would you print numbers 1-10 using two threads alternately?**

**Q28: How do you create a thread-safe Singleton class?**

**Q29: What happens if we call start() method twice on the same thread?**

**Q30: How would you stop a running thread safely?**

**Q31: How do you handle thread pool sizing in production applications?**

---

## 📝 CONCURRENT COLLECTIONS Questions (Be Ready)

**Q32: What is ConcurrentHashMap and how does it work?**

**Q33: What is the difference between HashMap, Hashtable, and ConcurrentHashMap?**

**Q34: What is BlockingQueue and its implementations?**

**Q35: What is CopyOnWriteArrayList and when to use it?**

**Q36: What is ArrayBlockingQueue vs LinkedBlockingQueue?**

---

## 🔧 ADVANCED Questions (Lower Priority)

**Q37: What is Lock interface and ReentrantLock?**

**Q38: What is ReadWriteLock and when to use it?**

**Q39: What is CountDownLatch and its use cases?**

**Q40: What is CyclicBarrier and difference from CountDownLatch?**

**Q41: What is Semaphore in Java?**

**Q42: What is ForkJoinPool and work-stealing algorithm?**

**Q43: What is CompletableFuture?**

**Q44: What is Java Memory Model (JMM)?**

---

## ⚡ QUICK-FIRE Questions (Know the Answers)

**Q45: What is daemon thread in Java?**
*Answer: Background thread that doesn't prevent JVM from exiting*

**Q46: What is Thread.yield() method?**
*Answer: Hints scheduler to give other threads a chance to execute*

**Q47: What is Thread.join() method?**
*Answer: Waits for a thread to die/complete execution*

**Q48: Can we synchronize static methods?**
*Answer: Yes, it synchronizes on Class object*

**Q49: What happens when main thread finishes before child threads?**
*Answer: JVM waits for non-daemon threads to complete*

**Q50: What is the default priority of a thread?**
*Answer: NORM_PRIORITY (5)*

---

## 🎪 CODING Questions (Practice These)

**Q51: Write a program to print even-odd numbers using two threads**

**Q52: Implement Producer-Consumer using wait() and notify()**

**Q53: Create a thread-safe counter class**

**Q54: Implement a simple thread pool**

**Q55: Write a program demonstrating deadlock**

**Q56: Create a program using CountDownLatch**

**Q57: Implement thread-safe Singleton (double-checked locking)**

**Q58: Write a program using BlockingQueue for inter-thread communication**

---
