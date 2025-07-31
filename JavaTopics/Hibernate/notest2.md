# Hibernate Interview Questions & Answers

## Core Hibernate Concepts

### 1. What is Hibernate? How does it differ from JDBC?

**Hibernate:**
- It's an ORM (Object-Relational Mapping) framework for Java
- Maps Java objects to database tables automatically
- Handles database operations through objects, not SQL

**🔍 JDBC vs Hibernate - The Key Differences:**

| Aspect | JDBC | Hibernate |
|--------|------|-----------|
| **Code Style** | Write SQL manually | Work with objects |
| **Mapping** | Manual ResultSet to Object | Automatic Object-Table mapping |
| **Database Dependency** | Database-specific SQL | Database independent |
| **Boilerplate Code** | Lots of repetitive code | Minimal code |

**📝 Simple Example:**
```java
// JDBC Way (Manual & More Code)
String sql = "SELECT * FROM student WHERE id = ?";
PreparedStatement ps = connection.prepareStatement(sql);
ps.setInt(1, studentId);
ResultSet rs = ps.executeQuery();
// Manual mapping to object...

// Hibernate Way (Automatic & Less Code)
Student student = session.get(Student.class, studentId);
```

### 2. Explain the Hibernate architecture

**Key Components:**
- **Configuration:**
- Reads config files and creates SessionFactory
- Reads settings from hibernate.cfg.xml or hibernate.properties.
- Contains database connection info, dialect, mappings, and other properties.
- Responsible for bootstrapping Hibernate.
  
- **SessionFactory:**
- A heavyweight object created once during application startup.
- Responsible for creating Session objects.
- Immutable and thread-safe.
- Holds metadata about mappings and configurations.

- **Session:**
- Interface between Java app and database (not thread-safe)
- A lightweight, non-thread-safe object.
- Used to interact with the database (CRUD operations).
- Wraps a JDBC connection.
  
- **Transaction:**
- Manages database transactions
- Helps manage commit/rollback behavior.
- Ensures ACID properties.
  
- **Query:**
- For HQL/SQL queries
- Used to fetch and manipulate data.

**Flow:** Configuration → SessionFactory → Session → Transaction → Database

**🔄 Flow Process:**
```
Configuration → SessionFactory → Session → Transaction → Database
    ↓              ↓               ↓          ↓           ↓
 Read Config   Create Factory   Get Session  Start Work  Execute SQL
```

### 3. What is ORM? What problems does it solve?

**ORM (Object-Relational Mapping):**
- Maps database tables to Java classes
- Maps table rows to object instances
- Maps table columns to object properties

**Problems it solves:**
- No manual SQL writing
- Automatic object-to-table mapping
- Database independence
- Reduces boilerplate code
- Handles relationships automatically

### 4. Advantages of Hibernate over plain SQL

- **Database Independence:** Same code works with different databases
- **Less Code:** No need to write repetitive CRUD operations
- **Object-Oriented:** Work with objects instead of tables
- **Automatic Mapping:** No manual result set handling
- **Caching:** Built-in first and second level caching
- **Lazy Loading:** Load data only when needed

## Hibernate Configuration & Mapping

### 1. What is hibernate.cfg.xml?

**Purpose:** 
-hibernate.cfg.xml is the primary configuration file used by Hibernate to set up and initialize the core infrastructure. 
-It tells Hibernate how to connect to the database and provides necessary settings like dialect, driver class, connection URL, credentials, and mapping files or annotated classes.

**Example:**
```xml
<hibernate-configuration>
    <session-factory>
        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/mydb</property>
        <property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
    </session-factory>
</hibernate-configuration>
```

### 2. What is .hbm.xml mapping file?

**Purpose:** 
-Maps Java class to database table (XML-based mapping)
-.hbm.xml is a Hibernate mapping file used to map Java classes (POJOs) to database tables without using annotations.

**Example:**
```xml
<hibernate-mapping>
    <class name="Student" table="student">
        <id name="id" column="student_id">
            <generator class="identity"/>
        </id>
        <property name="name" column="student_name"/>
    </class>
</hibernate-mapping>
```

### 3. Common Hibernate Annotations

- **@Entity:** Marks class as Hibernate entity
- **@Table:** Specifies table name
- **@Id:** Marks primary key field
- **@Column:** Maps field to specific column
- **@GeneratedValue:** Auto-generates primary key values

**Example:**
```java
@Entity
@Table(name = "student")
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int id;
    
    @Column(name = "student_name")
    private String name;
}
```

### 4. What is hibernate.dialect?

**Purpose:** Tells Hibernate which database you're using

**Why Important:**
- Different databases have different SQL syntax
- Hibernate generates database-specific SQL
- Examples: MySQLDialect, OracleDialect, PostgreSQLDialect

## Session & Transaction Management

### 1. What is Hibernate Session?

**🔄 HTTP Session vs Hibernate Session:**

| HTTP Session | Hibernate Session |
|--------------|------------------|
| Stores user data across web requests | Manages database operations |
| Lives across multiple page visits | Lives for single database operation |
| Web layer concept | Persistence layer concept |
| Stores shopping cart, login info | Tracks entity changes, executes SQL |


### 2. Entity Lifecycle

**Four States:**
1. **Transient:** New object, not associated with session
2. **Persistent:** Object associated with session, changes tracked
3. **Detached:** Was persistent, but session closed
4. **Removed:** Marked for deletion

**Example:**
```java
Student s = new Student(); // Transient
session.save(s); // Now Persistent
session.close(); // Now Detached
```

### 3. save() vs persist() vs saveOrUpdate()

- **save():** Saves object, returns generated ID immediately
- **persist():** Saves object, ID generated at flush/commit time
- **saveOrUpdate():** Saves if new, updates if existing

**When to use:**
- Use `persist()` in most cases (JPA standard)
- Use `save()` when you need ID immediately
- Use `saveOrUpdate()` when unsure if object exists

### 4. Transaction Management

**Transaction Interface:**
- Ensures ACID properties
- Groups multiple operations
- Can rollback on failure

**Example:**
```java
Transaction tx = session.beginTransaction();
try {
    session.save(student);
    tx.commit();
} catch (Exception e) {
    tx.rollback();
}
```

## HQL & Criteria API

### 1. What is HQL?

**HQL (Hibernate Query Language):**
- Object-oriented query language
- Similar to SQL but works with objects/properties instead of tables/columns
- Database independent

**HQL vs SQL:**
- HQL: `FROM Student WHERE name = 'John'`
- SQL: `SELECT * FROM student WHERE student_name = 'John'`

### 2. Basic HQL Query

```java
// Get all students
Query query = session.createQuery("FROM Student");
List<Student> students = query.list();

// Get student by name
Query query = session.createQuery("FROM Student WHERE name = :name");
query.setParameter("name", "John");
Student student = (Student) query.uniqueResult();
```

### 3. Criteria API

**Purpose:** Build queries programmatically without writing HQL/SQL

**When to use:**
- Dynamic queries (conditions change at runtime)
- Complex search forms
- When you want type safety

**Example:**
```java
Criteria criteria = session.createCriteria(Student.class);
criteria.add(Restrictions.eq("name", "John"));
List<Student> students = criteria.list();
```

### 4. Named Queries (@NamedQuery)

**Purpose:** Pre-defined, reusable queries

**Benefits:**
- Better performance (parsed once)
- Centralized query management
- Easy to maintain

**Example:**
```java
@Entity
@NamedQuery(name = "Student.findByName", 
           query = "FROM Student WHERE name = :name")
public class Student { ... }

// Usage
Query query = session.getNamedQuery("Student.findByName");
query.setParameter("name", "John");
```

## Performance & Caching

### 1. N+1 Problem

**Problem:** Loading 1 parent + N children results in N+1 database queries

**Example:**
```java
// 1 query to get students
List<Student> students = session.createQuery("FROM Student").list();

// N queries (one for each student's address)
for(Student s : students) {
    s.getAddress().getCity(); // Triggers separate query
}
```

**Solutions:**
- Use `JOIN FETCH` in HQL
- Set `@OneToMany(fetch = FetchType.EAGER)`
- Use `@BatchSize` annotation

### 2. Hibernate Caching

**First-Level Cache:**
- Session-level cache (enabled by default)
- Stores objects within single session
- Automatic cache management

**Second-Level Cache:**
- SessionFactory-level cache
- Shared across sessions
- Must be enabled and configured
- Use with `@Cache` annotation

**Query Cache:**
- Caches query results
- Works with second-level cache
- Enable with `hibernate.cache.use_query_cache=true`

### 3. Lazy vs Eager Loading

**Lazy Loading (Default):**
- Data loaded only when accessed
- Better performance for large datasets
- May cause LazyInitializationException

**Eager Loading:**
- All data loaded immediately
- No lazy exceptions
- May load unnecessary data

**Configuration:**
```java
@OneToMany(fetch = FetchType.LAZY) // Default
private List<Address> addresses;

@OneToMany(fetch = FetchType.EAGER)
private List<Address> addresses;
```

### 4. Performance Optimization

**Strategies:**
- Use appropriate fetch strategies
- Enable second-level cache for read-only data
- Use batch fetching (`@BatchSize`)
- Optimize HQL queries with indexes
- Use projections to fetch only needed columns
- Monitor SQL with `show_sql=true`

## Relationships & Associations

### 1. Mapping Relationships

**@OneToOne:**
```java
@OneToOne
@JoinColumn(name = "address_id")
private Address address;
```

**@OneToMany:**
```java
@OneToMany(mappedBy = "student")
private List<Course> courses;
```

**@ManyToOne:**
```java
@ManyToOne
@JoinColumn(name = "student_id")
private Student student;
```

**@ManyToMany:**
```java
@ManyToMany
@JoinTable(name = "student_course")
private List<Course> courses;
```

### 2. Cascading (CascadeType)

**Purpose:** Propagate operations from parent to child

**Types:**
- **PERSIST:** Save parent → save children
- **MERGE:** Update parent → update children  
- **REMOVE:** Delete parent → delete children
- **ALL:** All above operations
- **DETACH:** Detach parent → detach children

**Example:**
```java
@OneToMany(cascade = CascadeType.ALL)
private List<Address> addresses;
```

### 3. mappedBy vs @JoinColumn

**mappedBy:**
- Used in bidirectional relationships
- Indicates the relationship is owned by other side
- No foreign key column created

**@JoinColumn:**
- Specifies foreign key column
- Used on the owning side of relationship

**Example:**
```java
// Parent side
@OneToMany(mappedBy = "student") // No FK here
private List<Course> courses;

// Child side  
@ManyToOne
@JoinColumn(name = "student_id") // FK column
private Student student;
```

### 4. Bidirectional Relationships

**Best Practices:**
- Use `mappedBy` on one side (usually parent)
- Use `@JoinColumn` on other side (usually child)
- Keep both sides synchronized

**Example:**
```java
// Student.java
@OneToMany(mappedBy = "student", cascade = CascadeType.ALL)
private List<Course> courses = new ArrayList<>();

public void addCourse(Course course) {
    courses.add(course);
    course.setStudent(this); // Keep both sides in sync
}

// Course.java
@ManyToOne
@JoinColumn(name = "student_id")
private Student student;
```

## Quick Tips for Interview:

1. **Always mention** that Session is not thread-safe
2. **Remember** to close sessions to avoid memory leaks
3. **Know** the difference between save() and persist()
4. **Understand** lazy loading can cause exceptions after session closes
5. **Be able to explain** N+1 problem with a simple example
6. **Know** when to use HQL vs Criteria API vs Native SQL
