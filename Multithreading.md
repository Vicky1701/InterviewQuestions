# Java Multithreading Interview Questions

## Basic Level Questions

1. What is multithreading in Java?
2. What is the difference between process and thread?
3. How many ways can you create a thread in Java?
4. What is the difference between extending Thread class and implementing Runnable interface?
5. What is the main thread in Java?
6. What are the different states of a thread in Java?
7. What is the difference between start() and run() method?
8. What happens if we call start() method twice on the same thread?
9. What is Thread.sleep() method?
10. What is Thread.yield() method?
11. What is Thread.join() method?
12. What is daemon thread in Java?
13. How do you set a thread as daemon thread?
14. What happens when main thread finishes before child threads?

## Intermediate Level Questions

15. What is synchronization in Java?
16. Why do we need synchronization?
17. What is synchronized keyword in Java?
18. What is the difference between synchronized method and synchronized block?
19. What is monitor or intrinsic lock in Java?
20. Can we use synchronized keyword with variables?
21. What is deadlock in multithreading?
22. How can you avoid deadlock?
23. What is race condition?
24. What are wait(), notify(), and notifyAll() methods?
25. Why wait(), notify(), and notifyAll() methods are defined in Object class?
26. What is the difference between notify() and notifyAll()?
27. What is the difference between sleep() and wait() methods?
28. Can we call wait() method without synchronization?
29. What is thread safety?
30. Which collection classes are thread-safe in Java?
31. What is volatile keyword in Java?
32. What is the difference between synchronized and volatile?
33. What is happens-before relationship?
34. What is the producer-consumer problem?

## Advanced Level Questions

35. What is ExecutorService in Java?
36. What are the advantages of using ExecutorService over creating threads manually?
37. What are different types of thread pools?
38. What is FixedThreadPool?
39. What is CachedThreadPool?
40. What is SingleThreadExecutor?
41. What is ScheduledThreadPool?
42. What is Future interface in Java?
43. What is Callable interface?
44. What is the difference between Runnable and Callable?
45. How do you handle exceptions in ExecutorService?
46. What is CompletableFuture?
47. What is ForkJoinPool?
48. What is ThreadLocal in Java?
49. When would you use ThreadLocal?
50. What are atomic variables in Java?
51. What is AtomicInteger and how does it work?
52. What is Lock interface in Java?
53. What is ReentrantLock?
54. What is the difference between synchronized and ReentrantLock?
55. What is ReadWriteLock?
56. What is Semaphore in Java?
57. What is CountDownLatch?
58. What is CyclicBarrier?
59. What is the difference between CountDownLatch and CyclicBarrier?
60. What is Exchanger in Java?

## Concurrent Collections Questions

61. What is ConcurrentHashMap?
62. How does ConcurrentHashMap work internally?
63. What is the difference between HashMap, Hashtable, and ConcurrentHashMap?
64. What is BlockingQueue?
65. What are different implementations of BlockingQueue?
66. What is ArrayBlockingQueue?
67. What is LinkedBlockingQueue?
68. What is PriorityBlockingQueue?
69. What is SynchronousQueue?
70. What is CopyOnWriteArrayList?
71. When would you use CopyOnWriteArrayList?
72. What is ConcurrentLinkedQueue?

## Practical Coding Questions

73. Write a program to print numbers 1 to 10 using two threads alternately
74. Implement Producer-Consumer problem using wait() and notify()
75. Write a program to print even and odd numbers using two threads
76. Implement a thread-safe Singleton class
77. Write a program to demonstrate deadlock
78. Create a simple thread pool implementation
79. Implement a thread-safe counter
80. Write a program using CountDownLatch
81. Implement inter-thread communication using BlockingQueue
82. Create a program to demonstrate ThreadLocal usage

## Performance and Best Practices Questions

83. What are the best practices for multithreading in Java?
84. How do you measure thread performance?
85. What is thread starvation?
86. What is livelock?
87. How do you debug multithreading issues?
88. What tools can you use for analyzing thread dumps?
89. What is context switching?
90. How many threads should you create in an application?
91. What is thread pool sizing strategy?
92. What are the common multithreading mistakes?
93. How do you handle exceptions in multithreaded applications?
94. What is thread confinement?
95. What is immutability and how does it help in multithreading?

## Memory Model Questions

96. What is Java Memory Model (JMM)?
97. What is visibility problem in multithreading?
98. What is happens-before guarantee?
99. What is memory consistency error?
100. How does volatile keyword ensure visibility?
