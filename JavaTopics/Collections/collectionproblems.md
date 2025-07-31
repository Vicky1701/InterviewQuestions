# Java Collections Interview Questions - Organized Study Guide

## 📋 Overview
This comprehensive guide contains 45 Java Collections interview questions organized by topic and difficulty level. Each question includes problem statements, expected outputs, time duration, and interview tips.

---

## 🗂️ Categories Index

1. [ArrayList & List Operations](#arraylist--list-operations) (4 questions)
2. [HashMap & Map Operations](#hashmap--map-operations) (4 questions)  
3. [HashSet & Set Operations](#hashset--set-operations) (3 questions)
4. [LinkedList & Queue Operations](#linkedlist--queue-operations) (3 questions)
5. [TreeMap & TreeSet Operations](#treemap--treeset-operations) (3 questions)
6. [Iterator & Enhanced For-Loop](#iterator--enhanced-for-loop) (3 questions)
7. [Collections Utility Methods](#collections-utility-methods) (3 questions)
8. [Real-World Scenarios](#real-world-scenarios) (4 questions)
9. [Stream API with Collections](#stream-api-with-collections) (3 questions)
10. [Performance & Memory](#performance--memory) (3 questions)
11. [Common Pitfalls & Debugging](#common-pitfalls--debugging) (3 questions)
12. [Algorithm Implementation](#algorithm-implementation) (4 questions)
13. [Data Structure Design](#data-structure-design) (3 questions)
14. [Custom Collections](#custom-collections) (2 questions)

---

## ArrayList & List Operations

### Q1: Basic List Operations ⭐ Easy (5-10 min)
**Problem:** Given a list of integers [1, 2, 3, 4, 5, 2, 3]:
- Remove all duplicates
- Find the second largest number
- Reverse the list without using Collections.reverse()

**Key Concepts:** ArrayList, HashSet, Manual reversal
**Expected Output:** 
- Duplicates removed: [1, 2, 3, 4, 5]
- Second largest: 4
- Reversed: [5, 4, 3, 2, 1]

**Interview Tips:** Focus on multiple approaches, explain time complexity

### Q2: List Manipulation ⭐ Easy (5-10 min)
**Problem:** Create a method that takes List<String> names = ["John", "Jane", "Bob", "Alice"]:
- Returns names starting with 'J'
- Sorts them alphabetically
- Converts to uppercase

**Key Concepts:** Stream API, Filtering, Sorting
**Expected Output:** ["JANE", "JOHN"]
**Interview Tips:** Show both traditional and stream approaches

### Q3: List Merging ⭐ Easy (5-10 min)
**Problem:** Given two sorted lists:
- List1: [1, 3, 5, 7]
- List2: [2, 4, 6, 8, 10]
- Merge them into one sorted list without using Collections.sort()

**Key Concepts:** Two-pointer technique, Merge sort logic
**Expected Output:** [1, 2, 3, 4, 5, 6, 7, 8, 10]
**Interview Tips:** Explain merge sort algorithm, handle different list sizes

### Q4: List Filtering ⭐ Easy (5-10 min)
**Problem:** Given List<Integer> numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
- Filter even numbers
- Find numbers greater than 5
- Calculate sum of filtered numbers

**Key Concepts:** Filtering, Stream operations, Reduce
**Expected Output:** 
- Even: [2, 4, 6, 8, 10]
- >5: [6, 7, 8, 9, 10]
- Sum: 40

**Interview Tips:** Compare imperative vs functional programming approaches

---

## HashMap & Map Operations

### Q5: Character Frequency Count ⭐ Easy (5-10 min)
**Problem:** Given string "hello world":
- Count frequency of each character
- Return Map<Character, Integer>
- Ignore spaces and case-insensitive

**Key Concepts:** HashMap, Character manipulation, Case handling
**Expected Output:** {'h': 1, 'e': 1, 'l': 3, 'o': 2, 'w': 1, 'r': 1, 'd': 1}
**Interview Tips:** Handle edge cases like empty strings, special characters

### Q6: Word Frequency Counter ⭐ Easy (5-10 min)
**Problem:** Given string "java is great java is powerful":
- Count frequency of each word
- Return words with frequency > 1
- Sort by frequency (descending)

**Key Concepts:** HashMap, String splitting, Sorting by value
**Expected Output:** java: 2, is: 2 (sorted by frequency)
**Interview Tips:** Discuss different sorting approaches for maps

### Q7: Map Manipulation ⭐ Easy (5-10 min)
**Problem:** Given Map<String, Integer> scores: {"Alice": 85, "Bob": 92, "Charlie": 78, "Diana": 96}:
- Find student with highest score
- Get all students with score > 80
- Calculate average score

**Key Concepts:** Map iteration, Max finding, Filtering
**Expected Output:** 
- Highest: Diana (96)
- >80: [Alice, Bob, Diana]
- Average: 87.75

**Interview Tips:** Show different iteration methods for maps

### Q8: Two Sum Problem ⭐ Easy (5-10 min)
**Problem:** Given array [2, 7, 11, 15] and target = 9:
- Find two numbers that add up to target
- Return their indices
- Use HashMap for O(n) solution

**Key Concepts:** HashMap, Two-pointer alternative, Time complexity
**Expected Output:** [0, 1] (indices of 2 and 7)
**Interview Tips:** Explain why HashMap approach is better than nested loops

---

## HashSet & Set Operations

### Q9: Remove Duplicates ⭐ Easy (5-10 min)
**Problem:** Given List<String> = ["apple", "banana", "apple", "orange", "banana"]:
- Remove duplicates using Set
- Maintain original order
- Return as List

**Key Concepts:** LinkedHashSet, Order preservation
**Expected Output:** ["apple", "banana", "orange"]
**Interview Tips:** Compare HashSet vs LinkedHashSet for order preservation

### Q10: Set Operations ⭐ Easy (5-10 min)
**Problem:** Given two sets:
- Set1: {1, 2, 3, 4, 5}
- Set2: {4, 5, 6, 7, 8}
- Find intersection, union, and difference (Set1 - Set2)

**Key Concepts:** Set operations, retainAll, removeAll
**Expected Output:** 
- Intersection: {4, 5}
- Union: {1, 2, 3, 4, 5, 6, 7, 8}
- Difference: {1, 2, 3}

**Interview Tips:** Demonstrate built-in set methods vs manual implementation

### Q11: Unique Elements ⭐ Easy (5-10 min)
**Problem:** Given array [1, 2, 2, 3, 4, 4, 5]:
- Find elements that appear only once
- Use Set to track duplicates
- Return List of unique elements

**Key Concepts:** Set for duplicate tracking, Frequency counting
**Expected Output:** [1, 3, 5]
**Interview Tips:** Show multiple approaches using different data structures

---

## LinkedList & Queue Operations

### Q12: LinkedList vs ArrayList ⭐ Easy (5-10 min)
**Problem:** Compare performance:
- Insert 1000 elements at beginning
- Insert 1000 elements at middle
- Measure and explain time difference

**Key Concepts:** Time complexity, Performance comparison
**Expected Output:** LinkedList faster for beginning insertions, ArrayList faster for middle
**Interview Tips:** Focus on explaining O(1) vs O(n) operations

### Q13: Queue Implementation ⭐ Easy (5-10 min)
**Problem:** Using LinkedList as Queue:
- Add elements [1, 2, 3, 4, 5]
- Remove and print in FIFO order
- Implement using offer() and poll()

**Key Concepts:** Queue interface, FIFO operations
**Expected Output:** 1, 2, 3, 4, 5 (in FIFO order)
**Interview Tips:** Explain difference between add/offer and remove/poll

### Q14: Stack Implementation ⭐ Easy (5-10 min)
**Problem:** Using LinkedList as Stack:
- Push elements [1, 2, 3, 4, 5]
- Pop and print in LIFO order
- Check if parentheses are balanced: "((()))"

**Key Concepts:** Stack operations, LIFO, Parentheses matching
**Expected Output:** 5, 4, 3, 2, 1 (LIFO), Balanced: true
**Interview Tips:** Show practical application of stack in parentheses checking

---

## TreeMap & TreeSet Operations

### Q15: Sorted Map Operations ⭐ Easy (5-10 min)
**Problem:** Create TreeMap<String, Integer> for student grades:
- Add: {"John": 85, "Alice": 92, "Bob": 78}
- Print in alphabetical order of names
- Find student with lowest grade

**Key Concepts:** TreeMap, Natural ordering, Min finding
**Expected Output:** 
- Alphabetical: Alice, Bob, John
- Lowest: Bob (78)

**Interview Tips:** Explain automatic sorting in TreeMap

### Q16: Range Queries ⭐ Easy (5-10 min)
**Problem:** Given TreeMap<Integer, String> timeline: {2020: "Start", 2021: "Growth", 2022: "Peak", 2023: "Stable"}:
- Get entries between 2021-2022
- Use subMap() method

**Key Concepts:** TreeMap range operations, subMap
**Expected Output:** {2021: "Growth", 2022: "Peak"}
**Interview Tips:** Show NavigableMap methods for range queries

### Q17: TreeSet Operations ⭐ Easy (5-10 min)
**Problem:** Given TreeSet<Integer> = {1, 3, 5, 7, 9}:
- Find elements greater than 5
- Find elements in range [3, 7]
- Use headSet(), tailSet(), subSet()

**Key Concepts:** TreeSet navigation methods
**Expected Output:** 
- >5: {7, 9}
- [3,7]: {3, 5, 7}

**Interview Tips:** Demonstrate NavigableSet methods

---

## Iterator & Enhanced For-Loop

### Q18: Safe Removal During Iteration ⭐ Easy (5-10 min)
**Problem:** Given List<Integer> = [1, 2, 3, 4, 5, 6]:
- Remove all even numbers safely
- Use Iterator to avoid ConcurrentModificationException
- Compare with enhanced for-loop approach

**Key Concepts:** Iterator, ConcurrentModificationException
**Expected Output:** [1, 3, 5]
**Interview Tips:** Show what happens with enhanced for-loop vs Iterator

### Q19: Custom Iterator ⭐ Easy (5-10 min)
**Problem:** Create a class that implements Iterable<Integer>:
- Iterate over even numbers from 0 to n
- Implement iterator() method
- Use in enhanced for-loop

**Key Concepts:** Iterable interface, Custom iterator
**Expected Output:** 0, 2, 4, 6, 8, 10 (for n = 10)
**Interview Tips:** Focus on proper Iterator implementation

### Q20: ListIterator Usage ⭐ Easy (5-10 min)
**Problem:** Given List<String> = ["a", "b", "c", "d"]:
- Insert "X" after each element using ListIterator
- Result should be: ["a", "X", "b", "X", "c", "X", "d", "X"]

**Key Concepts:** ListIterator, Bidirectional iteration
**Expected Output:** ["a", "X", "b", "X", "c", "X", "d", "X"]
**Interview Tips:** Show ListIterator advantages over regular Iterator

---

## Collections Utility Methods

### Q21: Collections.sort() Variations ⭐ Easy (5-10 min)
**Problem:** Given List<String> names = ["John", "alice", "Bob", "CHARLIE"]:
- Sort case-insensitive
- Sort by length
- Sort in reverse order
- Use Comparator

**Key Concepts:** Comparator, Custom sorting
**Expected Output:** Case-insensitive: [alice, Bob, CHARLIE, John]
**Interview Tips:** Show lambda expressions vs anonymous classes

### Q22: Collections.binarySearch() ⭐ Easy (5-10 min)
**Problem:** Given sorted List<Integer> = [1, 3, 5, 7, 9, 11]:
- Search for element 7
- Search for element 6 (not present)
- Explain return values

**Key Concepts:** Binary search, Return value interpretation
**Expected Output:** 
- 7 found at index 3
- 6 not found (return negative)

**Interview Tips:** Explain negative return values for missing elements

### Q23: Collections.frequency() ⭐ Easy (5-10 min)
**Problem:** Given List<String> = ["apple", "banana", "apple", "orange", "apple"]:
- Count frequency of "apple"
- Find most frequent element
- Use Collections.frequency()

**Key Concepts:** Collections utility methods
**Expected Output:** 
- apple frequency: 3
- Most frequent: apple

**Interview Tips:** Compare with manual counting approach

---

## Real-World Scenarios

### Q24: Employee Management ⭐ Easy (10-15 min)
**Problem:** Create Employee class with name, id, salary. Given List<Employee>:
- Group employees by department (Map<String, List<Employee>>)
- Find highest paid employee
- Calculate average salary per department

**Key Concepts:** Custom objects in collections, Grouping
**Expected Output:** Grouped map, highest paid employee, averages per dept
**Interview Tips:** Focus on object design and collection operations

### Q25: Shopping Cart ⭐ Easy (10-15 min)
**Problem:** Implement shopping cart using Map<Product, Integer>:
- Add/remove products
- Update quantities
- Calculate total price
- Handle duplicate products

**Key Concepts:** Map as data store, Business logic
**Expected Output:** Updated cart with totals
**Interview Tips:** Show real-world application of Map operations

### Q26: Library System ⭐ Easy (10-15 min)
**Problem:** Design book management system:
- Use Map<String, Book> for ISBN lookup
- Use Set<String> for unique authors
- Implement search by author functionality

**Key Concepts:** Multiple collections working together
**Expected Output:** Search results by author
**Interview Tips:** Demonstrate choosing right collection for each use case

### Q27: Cache Implementation ⭐ Easy (10-15 min)
**Problem:** Implement simple LRU cache using LinkedHashMap:
- Fixed capacity (e.g., 3 items)
- Remove least recently used item when full
- Override removeEldestEntry() method

**Key Concepts:** LinkedHashMap, LRU logic
**Expected Output:** Working LRU cache
**Interview Tips:** Explain LinkedHashMap access order feature

---

## Stream API with Collections

### Q28: Stream Operations ⭐ Easy (10-15 min)
**Problem:** Given List<Integer> = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]:
- Filter even numbers
- Map to their squares
- Collect to List
- Find sum using reduce()

**Key Concepts:** Stream operations, Functional programming
**Expected Output:** 
- Even squares: [4, 16, 36, 64, 100]
- Sum: 220

**Interview Tips:** Compare stream vs traditional approach

### Q29: Grouping with Streams ⭐ Easy (10-15 min)
**Problem:** Given List<String> words = ["apple", "banana", "apricot", "blueberry"]:
- Group by first letter
- Return Map<Character, List<String>>
- Use Collectors.groupingBy()

**Key Concepts:** Stream grouping, Collectors
**Expected Output:** {a: [apple, apricot], b: [banana, blueberry]}
**Interview Tips:** Show power of Collectors.groupingBy()

### Q30: Complex Stream Operations ⭐ Easy (10-15 min)
**Problem:** Given List<Person> with age and city:
- Find oldest person in each city
- Use Collectors.toMap() with merge function
- Handle duplicate keys

**Key Concepts:** Advanced collectors, Merge functions
**Expected Output:** Map<City, OldestPerson>
**Interview Tips:** Focus on handling key collisions in toMap()

---

## Performance & Memory

### Q31: ArrayList vs LinkedList Performance ⭐ Easy (5-10 min)
**Problem:** Compare time complexity:
- Random access by index
- Insertion at middle
- Deletion from beginning
- Memory usage comparison

**Key Concepts:** Time complexity analysis, Memory overhead
**Expected Output:** Detailed analysis of O(1) vs O(n) operations
**Interview Tips:** Provide concrete examples and use cases

### Q32: HashMap vs TreeMap Performance ⭐ Easy (5-10 min)
**Problem:** Test performance with 1000 elements:
- Insertion time
- Lookup time
- Memory usage
- When to use which?

**Key Concepts:** Performance comparison, Use case analysis
**Expected Output:** Performance metrics and recommendations
**Interview Tips:** Explain trade-offs between hash-based and tree-based structures

### Q33: HashSet vs TreeSet vs LinkedHashSet ⭐ Easy (5-10 min)
**Problem:** Compare three Set implementations:
- Insertion order preservation
- Sorting behavior
- Performance characteristics
- Use cases for each

**Key Concepts:** Set implementations comparison
**Expected Output:** Comparison table with use cases
**Interview Tips:** Create clear decision matrix for choosing right Set

---

## Common Pitfalls & Debugging

### Q34: ConcurrentModificationException ⭐ Easy (5-10 min)
**Problem:** Demonstrate the problem with modifying collection during iteration:
- Provide correct solutions using Iterator

**Key Concepts:** Exception handling, Safe iteration
**Expected Output:** Working solution without exception
**Interview Tips:** Show both the problem and multiple solutions

### Q35: equals() and hashCode() Issues ⭐ Easy (5-10 min)
**Problem:** Create Person class without proper equals/hashCode:
- Add to HashSet
- Demonstrate duplicate addition problem
- Fix with proper implementation

**Key Concepts:** Object contract, HashSet behavior
**Expected Output:** Proper equals/hashCode implementation
**Interview Tips:** Explain why both methods must be overridden together

### Q36: Null Handling ⭐ Easy (5-10 min)
**Problem:** Handle null values in collections:
- List with null elements
- Map with null keys/values
- TreeMap null key issue
- Best practices for null handling

**Key Concepts:** Null handling, Collection behavior
**Expected Output:** Safe null handling practices
**Interview Tips:** Show which collections allow nulls and which don't

---

## Algorithm Implementation

### Q37: Find Missing Number ⭐ Easy (10-15 min)
**Problem:** Given array [1, 2, 4, 5, 6] (missing 3):
- Find missing number using Set
- O(n) time complexity
- Handle edge cases

**Key Concepts:** Set operations, Algorithm design
**Expected Output:** Missing number: 3
**Interview Tips:** Show multiple approaches and their complexities

### Q38: Intersection of Arrays ⭐ Easy (10-15 min)
**Problem:** Given two arrays: arr1 = [1, 2, 2, 1], arr2 = [2, 2]:
- Find intersection elements
- Return result as array
- Use HashMap to track frequencies

**Key Concepts:** Array intersection, Frequency counting
**Expected Output:** [2, 2]
**Interview Tips:** Handle duplicates correctly in intersection

### Q39: Top K Frequent Elements ⭐ Easy (10-15 min)
**Problem:** Given array [1,1,1,2,2,3] and k=2:
- Find k most frequent elements
- Use Map for counting + PriorityQueue
- Return [1, 2]

**Key Concepts:** Frequency counting, PriorityQueue
**Expected Output:** [1, 2]
**Interview Tips:** Explain heap-based approach for top-k problems

### Q40: Valid Anagram ⭐ Easy (10-15 min)
**Problem:** Given strings "listen" and "silent":
- Check if they are anagrams
- Use Map to count character frequencies
- Handle case sensitivity

**Key Concepts:** String manipulation, Character counting
**Expected Output:** true (are anagrams)
**Interview Tips:** Show multiple approaches: sorting vs frequency counting

---

## Data Structure Design

### Q41: Design Phone Directory ⭐ Easy (15-20 min)
**Problem:** Implement phone book:
- Add contact (name, number)
- Search by name
- Search by number
- Update contact
- Use appropriate data structures

**Key Concepts:** System design, Multiple data structures
**Expected Output:** Working phone directory
**Interview Tips:** Design for both name and number lookups

### Q42: Design Word Dictionary ⭐ Easy (15-20 min)
**Problem:** Implement word dictionary:
- Add word with definition
- Search word (case-insensitive)
- Get all words starting with prefix
- Use TreeMap for sorted storage

**Key Concepts:** Dictionary design, Prefix matching
**Expected Output:** Working dictionary with prefix search
**Interview Tips:** Consider case-insensitive operations

### Q43: Design Simple Database ⭐ Easy (15-20 min)
**Problem:** Create in-memory database:
- Table as Map<Integer, Map<String, Object>>
- Insert, update, delete, select operations
- Handle multiple columns

**Key Concepts:** Database concepts, Nested collections
**Expected Output:** Working in-memory database
**Interview Tips:** Focus on data modeling with collections

---

## Custom Collections

### Q44: Custom ArrayList ⭐ Easy (10-15 min)
**Problem:** Implement basic dynamic array:
- add(), get(), size() methods
- Handle array resizing
- Implement Iterable interface

**Key Concepts:** Dynamic arrays, Resizing logic
**Expected Output:** Working dynamic array implementation
**Interview Tips:** Explain resizing strategy and growth factor

### Q45: Custom HashMap ⭐ Easy (10-15 min)
**Problem:** Implement basic hash table:
- put(), get(), remove() methods
- Handle collisions with chaining
- Basic hash function implementation

**Key Concepts:** Hash tables, Collision handling
**Expected Output:** Working hash table with chaining
**Interview Tips:** Focus on hash function and collision resolution

---

## 📊 Quick Reference Summary

### Time Allocations by Category
- **5-10 minutes:** Basic operations, utility methods, performance comparisons
- **10-15 minutes:** Algorithm implementations, stream operations, custom collections
- **15-20 minutes:** System design questions

### Key Concepts Distribution
- **Core Collections:** ArrayList, HashMap, HashSet, LinkedList, TreeMap, TreeSet
- **Advanced Topics:** Stream API, Iterators, Performance analysis
- **Practical Applications:** Real-world scenarios, system design
- **Best Practices:** Error handling, null safety, proper object contracts

### Success Tips
1. **Understand time complexities** for all operations
2. **Practice both traditional and Stream API approaches**
3. **Know when to use which collection type**
4. **Handle edge cases** (nulls, empty collections, etc.)
5. **Explain your reasoning** for choosing specific data structures
6. **Write clean, readable code** with proper variable names