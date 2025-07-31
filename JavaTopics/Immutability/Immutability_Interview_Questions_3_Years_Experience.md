# Immutability - Interview Questions (3 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is Immutability and why is it important?

**Answer:** Immutability means an object's state cannot be changed after creation. Once created, the object remains constant throughout its lifetime.

**Benefits:**
- **Thread Safety**: No synchronization needed
- **Caching**: Safe to cache immutable objects
- **Security**: Cannot be modified accidentally
- **Simplified reasoning**: Predictable behavior
- **Side-effect free**: Safe to pass around without defensive copying

### Q2: How to create an Immutable class in Java?

**Answer:** Follow these rules to create immutable classes:

1. **Declare class as final** (prevent inheritance)
2. **Make all fields private and final**
3. **No setter methods**
4. **Initialize all fields via constructor**
5. **Defensive copying for mutable objects**
6. **Return defensive copies from getters**

```java
public final class ImmutablePerson {
    private final String name;
    private final int age;
    private final List<String> hobbies;
    
    public ImmutablePerson(String name, int age, List<String> hobbies) {
        this.name = name;
        this.age = age;
        // Defensive copying
        this.hobbies = hobbies != null ? new ArrayList<>(hobbies) : new ArrayList<>();
    }
    
    public String getName() { return name; }
    public int getAge() { return age; }
    
    // Return defensive copy
    public List<String> getHobbies() {
        return new ArrayList<>(hobbies);
    }
}
```

### Q3: Why declare immutable class as final?

**Answer:** Declaring class as `final` prevents inheritance, which could break immutability.

```java
// ❌ Without final - can be broken
public class ImmutableClass {
    private final String value;
    
    public ImmutableClass(String value) { this.value = value; }
    public String getValue() { return value; }
}

// Subclass can break immutability
class MutableSubclass extends ImmutableClass {
    private String mutableField;
    
    public void setMutableField(String value) {
        this.mutableField = value; // Breaks immutability concept
    }
}

// ✅ With final - prevents inheritance
public final class TrulyImmutableClass {
    private final String value;
    // Implementation...
}
```

### Q4: What is defensive copying and why is it needed?

**Answer:** Defensive copying creates copies of mutable objects to prevent external modification of internal state.

```java
public final class ImmutableStudent {
    private final List<String> subjects;
    private final Date enrollmentDate;
    
    public ImmutableStudent(List<String> subjects, Date enrollmentDate) {
        // Defensive copying in constructor
        this.subjects = new ArrayList<>(subjects);
        this.enrollmentDate = new Date(enrollmentDate.getTime());
    }
    
    public List<String> getSubjects() {
        return new ArrayList<>(subjects); // Defensive copy in getter
    }
    
    public Date getEnrollmentDate() {
        return new Date(enrollmentDate.getTime()); // Defensive copy
    }
}
```

### Q5: Are all fields in immutable class required to be final?

**Answer:** YES, all fields should be final to ensure immutability. However, there are some edge cases:

```java
public final class ImmutableExample {
    private final String name;           // ✅ Final field
    private final int[] numbers;         // ✅ Final reference, but array content mutable
    
    // ❌ Non-final field breaks immutability
    // private String mutableField;
    
    public ImmutableExample(String name, int[] numbers) {
        this.name = name;
        this.numbers = numbers.clone(); // Defensive copy
    }
    
    public int[] getNumbers() {
        return numbers.clone(); // Return copy to prevent modification
    }
}
```

### Q6: Can immutable objects contain mutable objects?

**Answer:** YES, but with proper defensive copying. The immutable object should not expose references to internal mutable objects.

```java
public final class ImmutableOrder {
    private final String orderId;
    private final List<String> items;        // Mutable object
    private final Map<String, String> metadata; // Mutable object
    
    public ImmutableOrder(String orderId, List<String> items, Map<String, String> metadata) {
        this.orderId = orderId;
        // Deep defensive copying
        this.items = items != null ? new ArrayList<>(items) : new ArrayList<>();
        this.metadata = metadata != null ? new HashMap<>(metadata) : new HashMap<>();
    }
    
    public List<String> getItems() {
        return new ArrayList<>(items); // Return copy
    }
    
    public Map<String, String> getMetadata() {
        return new HashMap<>(metadata); // Return copy
    }
}
```

### Q7: What are the built-in immutable classes in Java?

**Answer:** Java provides several built-in immutable classes:

**Primitive Wrappers:**
- String, Integer, Long, Double, Float, Boolean, Character, Byte, Short

**Other Classes:**
- BigInteger, BigDecimal
- LocalDate, LocalTime, LocalDateTime (Java 8+)
- UUID
- File (mostly immutable)

```java
// Examples of immutable classes
String str = "Hello";           // Immutable
Integer num = 42;               // Immutable
LocalDate date = LocalDate.now(); // Immutable
BigDecimal amount = new BigDecimal("100.50"); // Immutable
```

### Q8: Why is String immutable in Java?

**Answer:** String is immutable for several important reasons:

1. **Security**: Used in network connections, file paths, database URLs
2. **Synchronization**: Thread-safe without synchronization
3. **Caching**: String pool optimization
4. **Hash code caching**: Consistent hash codes for HashMap keys
5. **Class loading**: Class names are strings

```java
// String immutability example
String original = "Hello";
String modified = original.concat(" World"); // Creates new String
System.out.println(original); // Still "Hello" - unchanged
System.out.println(modified); // "Hello World" - new object
```

### Q9: Immutable objects and collections?

**Answer:** Collections can be made immutable using various approaches:

```java
// Using Collections utility methods
List<String> mutableList = new ArrayList<>();
List<String> immutableList = Collections.unmodifiableList(mutableList);

// Using Google Guava
List<String> guavaImmutable = ImmutableList.of("A", "B", "C");

// Using Java 9+ factory methods
List<String> java9Immutable = List.of("A", "B", "C");
Set<String> java9Set = Set.of("X", "Y", "Z");
Map<String, String> java9Map = Map.of("key1", "value1", "key2", "value2");

// Custom immutable collection wrapper
public final class ImmutableStringList {
    private final List<String> items;
    
    public ImmutableStringList(List<String> items) {
        this.items = new ArrayList<>(items);
    }
    
    public List<String> getItems() {
        return Collections.unmodifiableList(items);
    }
}
```

### Q10: Builder pattern for immutable objects?

**Answer:** Builder pattern is excellent for creating immutable objects with many parameters:

```java
public final class ImmutableUser {
    private final String firstName;
    private final String lastName;
    private final String email;
    private final int age;
    private final List<String> roles;
    
    private ImmutableUser(Builder builder) {
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.email = builder.email;
        this.age = builder.age;
        this.roles = Collections.unmodifiableList(new ArrayList<>(builder.roles));
    }
    
    // Getters...
    public String getFirstName() { return firstName; }
    public String getLastName() { return lastName; }
    
    public static class Builder {
        private String firstName;
        private String lastName;
        private String email;
        private int age;
        private List<String> roles = new ArrayList<>();
        
        public Builder setFirstName(String firstName) {
            this.firstName = firstName;
            return this;
        }
        
        public Builder setLastName(String lastName) {
            this.lastName = lastName;
            return this;
        }
        
        public Builder addRole(String role) {
            this.roles.add(role);
            return this;
        }
        
        public ImmutableUser build() {
            validateBuilder(); // Validation before creation
            return new ImmutableUser(this);
        }
        
        private void validateBuilder() {
            if (firstName == null || lastName == null) {
                throw new IllegalStateException("Name fields are required");
            }
        }
    }
}

// Usage
ImmutableUser user = new ImmutableUser.Builder()
    .setFirstName("John")
    .setLastName("Doe")
    .addRole("ADMIN")
    .build();
```

---

## 🔥 Tricky Questions (High Probability)

### Q11: Can you modify this immutable class?

```java
public final class ImmutableClass {
    private final List<String> items;
    
    public ImmutableClass(List<String> items) {
        this.items = items; // Problem here?
    }
    
    public List<String> getItems() {
        return items; // Problem here?
    }
}

// Usage
List<String> list = new ArrayList<>();
list.add("Item1");
ImmutableClass obj = new ImmutableClass(list);
list.add("Item2"); // What happens?
obj.getItems().add("Item3"); // What happens?
```

**Answer:** YES, this class is NOT truly immutable because:
1. Constructor doesn't create defensive copy - external list modification affects object
2. Getter returns direct reference - caller can modify internal state

**Fix:**
```java
public ImmutableClass(List<String> items) {
    this.items = new ArrayList<>(items); // Defensive copy
}

public List<String> getItems() {
    return new ArrayList<>(items); // Return defensive copy
}
```

### Q12: Immutable class with Date field?

```java
public final class ImmutableEvent {
    private final Date eventDate;
    
    public ImmutableEvent(Date eventDate) {
        this.eventDate = eventDate;
    }
    
    public Date getEventDate() {
        return eventDate;
    }
}

// Usage
Date date = new Date();
ImmutableEvent event = new ImmutableEvent(date);
date.setTime(System.currentTimeMillis()); // What happens to event?
event.getEventDate().setTime(0); // What happens?
```

**Answer:** Both operations BREAK immutability because Date is mutable. 

**Fix:**
```java
public ImmutableEvent(Date eventDate) {
    this.eventDate = new Date(eventDate.getTime()); // Defensive copy
}

public Date getEventDate() {
    return new Date(eventDate.getTime()); // Return copy
}

// Better: Use immutable LocalDateTime
private final LocalDateTime eventDateTime;
```

### Q13: Immutable inheritance problem?

```java
public class BaseImmutable {
    private final String value;
    
    public BaseImmutable(String value) { this.value = value; }
    public String getValue() { return value; }
}

public final class ChildImmutable extends BaseImmutable {
    private final String childValue;
    
    public ChildImmutable(String value, String childValue) {
        super(value);
        this.childValue = childValue;
    }
}

// Is this design correct for immutability?
```

**Answer:** RISKY design. Better to avoid inheritance for immutable classes:

1. **Base class should be final** to prevent unwanted inheritance
2. **Prefer composition over inheritance** for immutable objects
3. **Use interfaces** if you need polymorphism

**Better approach:**
```java
public interface Valuable {
    String getValue();
}

public final class ImmutableValue implements Valuable {
    private final String value;
    // Implementation...
}
```

### Q14: Shallow vs Deep immutability?

```java
public final class ShallowImmutable {
    private final List<Person> persons;
    
    public ShallowImmutable(List<Person> persons) {
        this.persons = new ArrayList<>(persons); // Shallow copy
    }
    
    public List<Person> getPersons() {
        return new ArrayList<>(persons);
    }
}

class Person {
    private String name; // Mutable field
    public void setName(String name) { this.name = name; }
}

// Usage
Person person = new Person();
person.setName("John");
ShallowImmutable obj = new ShallowImmutable(List.of(person));
person.setName("Jane"); // Does this affect obj?
```

**Answer:** YES, this affects the object because only the list is copied, not the Person objects inside (shallow copy).

**Deep immutability solution:**
```java
public ShallowImmutable(List<Person> persons) {
    this.persons = persons.stream()
        .map(Person::copy) // Deep copy each person
        .collect(Collectors.toList());
}

// Or ensure Person is also immutable
public final class ImmutablePerson {
    private final String name;
    // Immutable implementation
}
```

### Q15: Immutable objects and serialization?

```java
public final class ImmutableSerializable implements Serializable {
    private final String name;
    private final transient String password; // transient field
    
    public ImmutableSerializable(String name, String password) {
        this.name = name;
        this.password = password;
    }
    
    // What happens during deserialization?
}
```

**Answer:** **Potential issues:**
1. **Final fields**: Deserialization can bypass final field initialization
2. **Validation**: Constructor validation is bypassed
3. **Transient fields**: Will be null after deserialization

**Solution:**
```java
private void readObject(ObjectInputStream in) throws IOException, ClassNotFoundException {
    in.defaultReadObject();
    // Validation after deserialization
    if (name == null) {
        throw new InvalidObjectException("Name cannot be null");
    }
}

// Or implement readResolve() for singleton-like behavior
private Object readResolve() {
    return new ImmutableSerializable(name, "default-password");
}
```

### Q16: String pool and immutability?

```java
String s1 = "Hello";
String s2 = "Hello";
String s3 = new String("Hello");
String s4 = s3.intern();

System.out.println(s1 == s2); // ?
System.out.println(s1 == s3); // ?
System.out.println(s1 == s4); // ?
```

**Answer:**
- `s1 == s2`: **true** (both reference string pool)
- `s1 == s3`: **false** (s3 is new object in heap)
- `s1 == s4`: **true** (intern() returns pool reference)

String immutability enables string pool optimization.

### Q17: Immutable objects in HashMap?

```java
public final class ImmutableKey {
    private final String value;
    
    public ImmutableKey(String value) { this.value = value; }
    
    @Override
    public int hashCode() { return value.hashCode(); }
    
    @Override
    public boolean equals(Object obj) {
        return obj instanceof ImmutableKey && 
               Objects.equals(value, ((ImmutableKey) obj).value);
    }
}

// Usage in HashMap
Map<ImmutableKey, String> map = new HashMap<>();
ImmutableKey key = new ImmutableKey("test");
map.put(key, "value");

// Why is immutability important for HashMap keys?
```

**Answer:** Immutability is crucial for HashMap keys because:
1. **Consistent hash code**: Hash code won't change after insertion
2. **Reliable retrieval**: Keys can be found consistently
3. **No corruption**: HashMap internal structure remains intact

**Mutable key problem:**
```java
// ❌ Mutable key
class MutableKey {
    private String value;
    public void setValue(String value) { this.value = value; }
    // hashCode() and equals() based on value
}

MutableKey key = new MutableKey("original");
map.put(key, "value");
key.setValue("modified"); // Changes hash code!
// map.get(key) might return null - key "lost" in wrong bucket
```

### Q18: Performance implications of immutability?

```java
// String concatenation performance
String result = "";
for (int i = 0; i < 1000; i++) {
    result += i; // Creates new String object each time
}

// vs StringBuilder (mutable)
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append(i); // Modifies existing buffer
}
String result = sb.toString();
```

**Answer:** 
- **Immutable approach**: O(n²) time complexity due to object creation
- **Mutable approach**: O(n) time complexity with StringBuilder
- **Memory**: Immutable creates many temporary objects
- **Trade-off**: Safety vs Performance

### Q19: Immutable collections pitfall?

```java
List<String> original = new ArrayList<>();
original.add("A");

List<String> unmodifiable = Collections.unmodifiableList(original);
List<String> immutable = List.of("A"); // Java 9+

original.add("B"); // What happens to unmodifiable and immutable lists?

System.out.println(unmodifiable.size()); // ?
System.out.println(immutable.size());    // ?
```

**Answer:**
- `unmodifiable.size()`: **2** (unmodifiable is a view, reflects original changes)
- `immutable.size()`: **1** (true immutable copy, independent of original)

**Key difference:**
- `Collections.unmodifiableList()`: Creates view, not copy
- `List.of()`: Creates true immutable copy

### Q20: Reflection and immutability?

```java
public final class ImmutableClass {
    private final String value;
    
    public ImmutableClass(String value) { this.value = value; }
    public String getValue() { return value; }
}

// Using reflection to break immutability
ImmutableClass obj = new ImmutableClass("original");
Field field = ImmutableClass.class.getDeclaredField("value");
field.setAccessible(true);
field.set(obj, "modified"); // Can this work?
```

**Answer:** **Depends on JVM and security settings:**
- **Normal JVM**: May work and break immutability
- **With Security Manager**: May throw SecurityException
- **Module system**: May be restricted

**Protection strategies:**
1. Use Security Manager
2. Module system restrictions
3. Defensive programming (validate in methods)
4. Consider this an edge case in normal applications

---

## 💡 Expert Level Concepts (3+ Years)

### Q21: Immutability and functional programming?

**Answer:** Immutability is fundamental to functional programming principles:

```java
// Functional style with immutable objects
public final class ImmutableCalculator {
    private final double value;
    
    public ImmutableCalculator(double value) { this.value = value; }
    
    public ImmutableCalculator add(double operand) {
        return new ImmutableCalculator(value + operand); // Returns new object
    }
    
    public ImmutableCalculator multiply(double operand) {
        return new ImmutableCalculator(value * operand);
    }
    
    // Method chaining with immutable objects
    public double getValue() { return value; }
}

// Usage
double result = new ImmutableCalculator(10)
    .add(5)        // Returns new object
    .multiply(2)   // Returns new object
    .getValue();   // 30
```

### Q22: Copy-on-Write pattern?

**Answer:** Copy-on-Write optimizes immutable collections by sharing memory until modification:

```java
public class CopyOnWriteList<T> {
    private volatile Object[] array;
    
    public CopyOnWriteList() {
        array = new Object[0];
    }
    
    public void add(T element) {
        synchronized (this) {
            Object[] oldArray = array;
            int len = oldArray.length;
            Object[] newArray = Arrays.copyOf(oldArray, len + 1);
            newArray[len] = element;
            array = newArray; // Atomic update
        }
    }
    
    public T get(int index) {
        return (T) array[index]; // No synchronization needed for reads
    }
}

// Java's CopyOnWriteArrayList uses this pattern
List<String> cowList = new CopyOnWriteArrayList<>();
```

### Q23: Immutable objects in concurrent programming?

**Answer:** Immutable objects provide excellent concurrent programming benefits:

```java
public final class ImmutableCounter {
    private final AtomicInteger count;
    
    public ImmutableCounter(int initialValue) {
        this.count = new AtomicInteger(initialValue);
    }
    
    public ImmutableCounter increment() {
        return new ImmutableCounter(count.get() + 1);
    }
    
    public int getValue() { return count.get(); }
}

// Thread-safe without synchronization
public class ConcurrentProcessor {
    private volatile ImmutableCounter counter = new ImmutableCounter(0);
    
    public void increment() {
        // Atomic update using compare-and-swap
        ImmutableCounter current, updated;
        do {
            current = counter;
            updated = current.increment();
        } while (!compareAndSet(current, updated));
    }
    
    private boolean compareAndSet(ImmutableCounter expect, ImmutableCounter update) {
        // Simplified CAS implementation
        synchronized (this) {
            if (counter == expect) {
                counter = update;
                return true;
            }
            return false;
        }
    }
}
```

### Q24: Memory optimization for immutable objects?

**Answer:** Several techniques optimize memory usage:

**Flyweight Pattern:**
```java
public final class ImmutableColor {
    private final int red, green, blue;
    private static final Map<String, ImmutableColor> CACHE = new ConcurrentHashMap<>();
    
    private ImmutableColor(int red, int green, int blue) {
        this.red = red;
        this.green = green;
        this.blue = blue;
    }
    
    public static ImmutableColor of(int red, int green, int blue) {
        String key = red + "," + green + "," + blue;
        return CACHE.computeIfAbsent(key, k -> new ImmutableColor(red, green, blue));
    }
}
```

**Interning:**
```java
public final class ImmutableStringWrapper {
    private final String value;
    
    public ImmutableStringWrapper(String value) {
        this.value = value.intern(); // Use string pool
    }
}
```

### Q25: Testing immutable objects?

**Answer:** Specific testing strategies for immutable objects:

```java
@Test
public void testImmutability() {
    // Test defensive copying
    List<String> originalList = new ArrayList<>();
    originalList.add("item1");
    
    ImmutableContainer container = new ImmutableContainer(originalList);
    
    // Modify original - should not affect container
    originalList.add("item2");
    assertEquals(1, container.getItems().size());
    
    // Modify returned list - should not affect container
    List<String> returned = container.getItems();
    returned.add("item3");
    assertEquals(1, container.getItems().size());
}

@Test
public void testBuilderValidation() {
    // Test builder validation
    assertThrows(IllegalStateException.class, () -> {
        new ImmutableUser.Builder().build(); // Missing required fields
    });
}

@Test
public void testEqualsAndHashCode() {
    ImmutablePerson person1 = new ImmutablePerson("John", 30);
    ImmutablePerson person2 = new ImmutablePerson("John", 30);
    
    assertEquals(person1, person2);
    assertEquals(person1.hashCode(), person2.hashCode());
    
    // Test as HashMap key
    Map<ImmutablePerson, String> map = new HashMap<>();
    map.put(person1, "value");
    assertEquals("value", map.get(person2));
}
```

---

## 🎭 Common Pitfalls & Best Practices

### 1. **Forgetting Defensive Copying:**
```java
// ❌ Bad - exposes internal mutable state
public List<String> getItems() {
    return items; // Direct reference
}

// ✅ Good - defensive copy
public List<String> getItems() {
    return new ArrayList<>(items);
}

// ✅ Better - unmodifiable view
public List<String> getItems() {
    return Collections.unmodifiableList(items);
}
```

### 2. **Mutable Fields in Immutable Class:**
```java
// ❌ Bad - non-final field
public final class BadImmutable {
    private String value; // Not final!
    
    public void setValue(String value) { // Setter breaks immutability
        this.value = value;
    }
}

// ✅ Good - all fields final
public final class GoodImmutable {
    private final String value;
    
    public GoodImmutable(String value) { this.value = value; }
    // No setters
}
```

### 3. **Inheritance Issues:**
```java
// ❌ Bad - not final class
public class BaseImmutable {
    private final String value;
    // Implementation...
}

// Subclass can break immutability
class MutableChild extends BaseImmutable {
    private String mutableField;
    public void setMutableField(String value) { this.mutableField = value; }
}

// ✅ Good - final class
public final class TrulyImmutable {
    private final String value;
    // Implementation...
}
```

### 4. **Performance Considerations:**
```java
// ❌ Bad for frequent operations
String result = "";
for (String item : items) {
    result += item; // Creates new String each time
}

// ✅ Good - use mutable builder when needed
StringBuilder builder = new StringBuilder();
for (String item : items) {
    builder.append(item);
}
return builder.toString(); // Final immutable result
```

---

## 🏗️ Real-world Examples

### 1. **Configuration Objects:**
```java
public final class DatabaseConfig {
    private final String url;
    private final String username;
    private final String password;
    private final int maxConnections;
    private final Duration timeout;
    
    private DatabaseConfig(Builder builder) {
        this.url = builder.url;
        this.username = builder.username;
        this.password = builder.password;
        this.maxConnections = builder.maxConnections;
        this.timeout = builder.timeout;
    }
    
    // Builder pattern implementation...
    
    public static class Builder {
        private String url = "localhost:5432";
        private String username = "admin";
        private String password;
        private int maxConnections = 10;
        private Duration timeout = Duration.ofSeconds(30);
        
        public Builder url(String url) { this.url = url; return this; }
        public Builder username(String username) { this.username = username; return this; }
        public Builder password(String password) { this.password = password; return this; }
        
        public DatabaseConfig build() {
            validateConfig();
            return new DatabaseConfig(this);
        }
    }
}
```

### 2. **Value Objects:**
```java
public final class Money {
    private final BigDecimal amount;
    private final Currency currency;
    
    public Money(BigDecimal amount, Currency currency) {
        this.amount = amount;
        this.currency = currency;
    }
    
    public Money add(Money other) {
        if (!currency.equals(other.currency)) {
            throw new IllegalArgumentException("Currency mismatch");
        }
        return new Money(amount.add(other.amount), currency);
    }
    
    public Money multiply(BigDecimal multiplier) {
        return new Money(amount.multiply(multiplier), currency);
    }
    
    // equals(), hashCode(), toString()...
}
```

### 3. **Event Objects:**
```java
public final class UserRegisteredEvent {
    private final String userId;
    private final String email;
    private final LocalDateTime timestamp;
    private final Map<String, String> metadata;
    
    public UserRegisteredEvent(String userId, String email, 
                              LocalDateTime timestamp, Map<String, String> metadata) {
        this.userId = userId;
        this.email = email;
        this.timestamp = timestamp;
        this.metadata = metadata != null ? Map.copyOf(metadata) : Map.of();
    }
    
    // Getters return immutable views
    public Map<String, String> getMetadata() {
        return metadata; // Already immutable from Map.copyOf()
    }
}
```

---

## 📝 Quick Reference Card

```java
// Immutable Class Checklist
public final class ImmutableExample {        // ✅ Final class
    private final String name;               // ✅ Final fields
    private final List<String> items;        // ✅ Final reference
    
    public ImmutableExample(String name, List<String> items) {
        this.name = name;
        this.items = new ArrayList<>(items); // ✅ Defensive copy
    }
    
    public String getName() { return name; } // ✅ Simple getter
    
    public List<String> getItems() {         // ✅ Defensive copy in getter
        return new ArrayList<>(items);
    }
    
    // ❌ What NOT to include:
    // public void setName(String name) { }  // No setters
    // private String mutableField;          // No non-final fields
    // public List<String> getItems() { return items; } // No direct references
}
```

---

## 🎯 Interview Success Strategy

### **Key Points to Emphasize:**

1. **Thread safety** - Primary benefit of immutability
2. **Defensive copying** - Critical for proper implementation
3. **Final fields and class** - Essential requirements
4. **Builder pattern** - Best practice for complex objects
5. **Real-world examples** - String, LocalDateTime, BigDecimal

### **Common Interview Scenarios:**
- **Code review**: "Is this class truly immutable?"
- **Design questions**: "Design an immutable configuration class"
- **Performance**: "When would you choose mutable vs immutable?"
- **Threading**: "How do immutable objects help in concurrent programming?"

### **Buzzwords to Use:**
- Thread Safety
- Defensive Copying
- Value Objects
- Builder Pattern
- Side-effect Free
- Functional Programming
- Copy-on-Write

Remember: Immutability is about **state that never changes** - demonstrate with practical examples showing thread safety and defensive programming!
