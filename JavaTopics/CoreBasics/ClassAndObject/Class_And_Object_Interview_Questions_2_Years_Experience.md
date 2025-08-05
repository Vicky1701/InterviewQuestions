# Class and Object - Interview Questions (2 Years Experience)

## 🎯 Must Know Questions (Your Level)

### Q1: What is a Class and Object in Java?
**Answer:** 
- **Class**: Blueprint/template that defines properties and behaviors. Doesn't consume memory.
- **Object**: Instance of a class that exists in memory with actual values.
```java
class Car {              // Class - Blueprint
    String brand;
    void start() { }
}

Car myCar = new Car();   // Object - Instance in memory
```

### Q2: Difference between Class and Object?
**Answer:**
| Class | Object |
|-------|--------|
| Blueprint/Template | Instance of class |
| No memory allocation | Memory allocated in heap |
| Logical entity | Physical entity |
| Declared once | Multiple objects can be created |
| Contains variables and methods | Contains actual values |

### Q3: How many ways can you create an object in Java?
**Answer:** 5 main ways:
1. **new keyword** - `Car car = new Car();`
2. **Class.forName()** - `Car car = (Car) Class.forName("Car").newInstance();`
3. **Clone()** - `Car car2 = (Car) car1.clone();`
4. **Deserialization** - Reading from file/stream
5. **Reflection** - `Constructor<Car> constructor = Car.class.getConstructor(); Car car = constructor.newInstance();`

### Q4: What happens when you create an object with 'new' keyword?
**Answer:** 4 steps:
1. **Memory allocation** in heap for object
2. **Instance variables initialized** with default values
3. **Constructor called** to initialize object
4. **Reference returned** to point to object location

```java
Car car = new Car("Toyota"); 
// 1. Memory allocated
// 2. brand = null (default)
// 3. Constructor sets brand = "Toyota"
// 4. Reference assigned to 'car'
```

### Q5: What are instance variables and methods?
**Answer:** 
- **Instance Variables**: Belong to specific object, unique copy per object
- **Instance Methods**: Operate on instance variables, need object to call

```java
class Student {
    String name;        // Instance variable
    int age;           // Instance variable
    
    void study() {     // Instance method
        System.out.println(name + " is studying");
    }
}

Student s1 = new Student(); // s1 has its own name, age
Student s2 = new Student(); // s2 has separate name, age
```

### Q6: Can a class exist without objects?
**Answer:** YES. Class is just a blueprint. Objects are optional.
```java
class Utility {
    static void helper() { }  // Can use static methods without objects
}

// No objects created, but class exists and can be used
Utility.helper();
```

### Q7: What is object reference vs object?
**Answer:**
- **Object**: Actual memory location in heap containing data
- **Reference**: Variable that holds memory address of object

```java
Car car1 = new Car();     // car1 is reference, new Car() is object
Car car2 = car1;          // car2 is another reference to same object
car1 = null;              // Reference is null, but object still exists if car2 points to it
```

### Q8: What happens to objects without references?
**Answer:** Objects become **eligible for Garbage Collection**. JVM automatically removes unreferenced objects to free memory.
```java
Car car = new Car();
car = null;              // Object becomes eligible for GC
// OR
car = new Car();         // Previous object becomes eligible for GC
```

### Q9: Can you have multiple references to same object?
**Answer:** YES. Multiple references can point to same object.
```java
Car car1 = new Car("Honda");
Car car2 = car1;         // Both point to same object
car1.brand = "Toyota";   
System.out.println(car2.brand); // Prints "Toyota"
```

### Q10: What is default constructor?
**Answer:** Constructor provided by Java when no constructor is defined. Initializes instance variables with default values.
```java
class Car {
    String brand;    // Will be null
    int year;        // Will be 0
    // Default constructor: Car() { }
}

Car car = new Car(); // Uses default constructor
```

---

## 🔥 Tricky Interview Questions (High Probability)

### Q11: Object creation without new keyword?
```java
class Test {
    static Test obj = new Test();  // Created during class loading
    
    public static void main(String[] args) {
        Test t1 = obj;              // No new keyword used
        Test t2 = getInstance();    // Factory method
    }
    
    static Test getInstance() {
        return new Test();
    }
}
```
**Answer:** Objects can be created without directly using `new` keyword through static variables, factory methods, cloning, deserialization, or reflection.

### Q12: What's the output?
```java
class Student {
    String name = "Default";
    
    Student() {
        this("Java");
        name = "Constructor";
    }
    
    Student(String name) {
        this.name = name;
    }
    
    public static void main(String[] args) {
        Student s = new Student();
        System.out.println(s.name);
    }
}
```
**Answer:** Output is "Constructor". 
- `this("Java")` calls parameterized constructor first
- Then remaining constructor code executes, setting name to "Constructor"

### Q13: Anonymous object behavior?
```java
class Car {
    void start() { System.out.println("Car started"); }
    
    public static void main(String[] args) {
        new Car().start();        // Anonymous object
        new Car().start();        // Another anonymous object
    }
}
```
**Answer:** Creates 2 separate objects. Each `new Car()` creates a new object, calls method, then becomes eligible for GC immediately.

### Q14: Reference assignment vs Object creation?
```java
class Test {
    public static void main(String[] args) {
        Test t1 = new Test();     // 1 object created
        Test t2 = new Test();     // 1 object created  
        Test t3 = t1;             // No object created
        t2 = t1;                  // No object created, original t2 object eligible for GC
    }
}
```
**Answer:** Only 2 objects created. Reference assignments don't create new objects.

### Q15: Object initialization order?
```java
class Parent {
    int a = getValue("Parent a");
    { System.out.println("Parent instance block"); }
    Parent() { System.out.println("Parent constructor"); }
    
    int getValue(String msg) {
        System.out.println("Initializing " + msg);
        return 10;
    }
}

class Child extends Parent {
    int b = getValue("Child b");
    { System.out.println("Child instance block"); }
    Child() { System.out.println("Child constructor"); }
}

Child c = new Child();
```
**Answer:**
```
Initializing Parent a
Parent instance block
Parent constructor
Initializing Child b
Child instance block
Child constructor
```

### Q16: Circular reference scenario?
```java
class A {
    B b;
    A() { b = new B(this); }
}

class B {
    A a;
    B(A a) { this.a = a; }
}

A objA = new A();  // What happens?
```
**Answer:** Works fine. Creates circular reference where A contains B and B contains A. Both objects remain in memory until both references are nullified.

### Q17: Object comparison confusion?
```java
class Person {
    String name;
    Person(String name) { this.name = name; }
    
    public static void main(String[] args) {
        Person p1 = new Person("John");
        Person p2 = new Person("John");
        Person p3 = p1;
        
        System.out.println(p1 == p2);        // ?
        System.out.println(p1 == p3);        // ?
        System.out.println(p1.name == p2.name); // ?
    }
}
```
**Answer:**
- `p1 == p2` → false (different objects, different references)
- `p1 == p3` → true (same object, same reference)
- `p1.name == p2.name` → true (String literals point to same object in string pool)

### Q18: Method chaining with objects?
```java
class Calculator {
    int result = 0;
    
    Calculator add(int x) {
        result += x;
        return this;
    }
    
    Calculator multiply(int x) {
        result *= x;
        return this;
    }
    
    public static void main(String[] args) {
        Calculator calc = new Calculator();
        calc.add(5).multiply(2).add(3);
        System.out.println(calc.result);  // ?
    }
}
```
**Answer:** Output is 13. 
- add(5): result = 5
- multiply(2): result = 10
- add(3): result = 13

### Q19: Object creation in static context?
```java
class Test {
    static Test obj = new Test();  // Line 1
    int count = 5;
    
    Test() {
        count++;
        System.out.println("Constructor: " + count);
    }
    
    public static void main(String[] args) {
        System.out.println("Main method");
        Test t = new Test();
    }
}
```
**Answer:**
```
Constructor: 6
Main method
Constructor: 6
```
Static variable initialization happens during class loading before main method.

### Q20: Object memory leak scenario?
```java
class Node {
    Node next;
    String data;
    
    Node(String data) { this.data = data; }
    
    public static void main(String[] args) {
        Node n1 = new Node("First");
        Node n2 = new Node("Second");
        n1.next = n2;
        n2.next = n1;        // Circular reference
        
        n1 = null;
        n2 = null;           // Memory leak?
    }
}
```
**Answer:** NOT a memory leak in modern Java. Even with circular references, if no external references exist, objects become eligible for GC.

---

## 🎯 Advanced Questions (Bonus Points)

### Q21: Object cloning - Shallow vs Deep?
**Answer:**
```java
class Address {
    String city;
    Address(String city) { this.city = city; }
}

class Person implements Cloneable {
    String name;
    Address address;
    
    // Shallow clone - default clone()
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();  // Address object is shared
    }
    
    // Deep clone - custom implementation
    protected Person deepClone() {
        Person cloned = new Person();
        cloned.name = this.name;
        cloned.address = new Address(this.address.city);  // New Address object
        return cloned;
    }
}
```

### Q22: Object serialization and class changes?
**Answer:** If class structure changes after serialization, deserialization may fail with `InvalidClassException` unless `serialVersionUID` is properly managed.

### Q23: Inner class object creation?
**Answer:**
```java
class Outer {
    class Inner { }
    static class StaticNested { }
    
    public static void main(String[] args) {
        Outer outer = new Outer();
        Inner inner = outer.new Inner();           // Need outer instance
        StaticNested nested = new StaticNested();  // No outer instance needed
    }
}
```

---

## 💡 Key Points for Your Experience Level

### Common Interview Mistakes (Avoid These):
1. **Confusing class and object** - Class is blueprint, object is instance
2. **Reference vs Object** - Reference points to object, not the object itself
3. **Object creation count** - Reference assignment doesn't create objects
4. **Memory management** - Understanding when objects become eligible for GC
5. **Constructor chaining** - Order of execution in inheritance

### What Interviewers Expect from 2-Year Experience:
- **Clear distinction** between class and object
- **Object creation lifecycle** understanding
- **Memory management** basics (heap allocation, GC)
- **Reference handling** and object comparison
- **Constructor and initialization** order

### Code Scenarios to Practice:
```java
// Object lifecycle
class Product {
    static int count = 0;
    int id;
    
    Product() {
        id = ++count;
        System.out.println("Product " + id + " created");
    }
    
    protected void finalize() {
        System.out.println("Product " + id + " garbage collected");
    }
}
```

---

## 🚀 Last Minute Quick Review

### One-Liner Answers:
1. **What is class?** → Blueprint/template that defines structure and behavior
2. **What is object?** → Instance of class that exists in memory with actual values
3. **Object creation ways?** → new, reflection, cloning, deserialization, Class.forName()
4. **Object without reference?** → Becomes eligible for garbage collection
5. **Reference vs Object?** → Reference points to object location in memory
6. **Multiple references?** → YES, multiple references can point to same object
7. **Class without objects?** → YES, class can exist independently
8. **Object comparison?** → == compares references, .equals() compares content
9. **Anonymous objects?** → Objects created without storing reference
10. **Default constructor?** → No-arg constructor provided by Java if none defined

### Memory Tips:
- **Class = Blueprint** (no memory until object created)
- **Object = Instance** (actual memory allocation in heap)
- **Reference = Address** (points to object location)
- **GC = Cleanup** (removes unreferenced objects)

### Common Patterns:
```java
// Singleton pattern
class Singleton {
    private static Singleton instance;
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

// Factory pattern
class CarFactory {
    public static Car createCar(String type) {
        if (type.equals("sedan")) {
            return new Sedan();
        } else if (type.equals("suv")) {
            return new SUV();
        }
        return null;
    }
}

// Builder pattern
class Person {
    private String name;
    private int age;
    
    private Person(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
    }
    
    static class Builder {
        private String name;
        private int age;
        
        Builder setName(String name) { this.name = name; return this; }
        Builder setAge(int age) { this.age = age; return this; }
        Person build() { return new Person(this); }
    }
}
```

---

## 🎯 Final Tips

### During Interview:
- **Always provide examples** when explaining class/object concepts
- **Draw memory diagrams** showing heap allocation
- **Trace object creation** step by step
- **Mention practical use cases** (Singleton, Factory, Builder patterns)

### Real-world Applications:
- **Entity classes** - User, Product, Order objects
- **Data Transfer Objects** - DTOs for API communication
- **Model classes** - Representing business entities
- **Utility objects** - Configuration, Helper objects

### Performance Considerations:
- **Object pooling** - Reusing expensive objects
- **Lazy initialization** - Creating objects only when needed
- **Memory optimization** - Avoiding unnecessary object creation
- **Garbage collection** - Understanding object lifecycle

---

*🎯 Perfect for 2-year experience class and object interviews!*
