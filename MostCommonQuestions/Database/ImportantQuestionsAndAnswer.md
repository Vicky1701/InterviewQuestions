# Database & DBMS Interview Questions - Complete Answer Guide

## 🔥 MUST-KNOW Questions (High Priority)

### Fundamental Concepts

**Q1: What is a Database and DBMS? What's the difference?**

**Database**: An organized collection of structured, interrelated data stored electronically in a computer system. It's the actual data itself.

**DBMS (Database Management System)**: Software that provides an interface to interact with databases. It manages data storage, retrieval, and organization.

**Key Differences**:
- Database = Data itself
- DBMS = Software to manage the data
- Example: MySQL is a DBMS, while your company's customer data stored in MySQL is the database

**Q2: What are the types of databases? (Relational vs Non-Relational)**

**Relational Databases (SQL)**:
- Data stored in tables with rows and columns
- Follow ACID properties
- Use structured query language (SQL)
- Examples: MySQL, PostgreSQL, Oracle, SQL Server

**Non-Relational Databases (NoSQL)**:
- Flexible data models (document, key-value, graph, column-family)
- Better scalability for large datasets
- Examples: MongoDB, Cassandra, Redis, Neo4j

**Q3: What is RDBMS? Name some popular RDBMS systems.**

**RDBMS (Relational Database Management System)**: A DBMS based on the relational model where data is stored in tables and relationships are established using keys.

**Popular RDBMS**:
- MySQL (open-source, web applications)
- PostgreSQL (advanced open-source)
- Oracle Database (enterprise-level)
- Microsoft SQL Server (Windows environment)
- SQLite (lightweight, embedded)

**Q4: What is a Primary Key? What are its properties?**

**Primary Key**: A column or combination of columns that uniquely identifies each row in a table.

**Properties**:
- **Uniqueness**: No two rows can have the same primary key value
- **Non-null**: Cannot contain NULL values
- **Immutable**: Should not change once assigned
- **Single**: Only one primary key per table
- **Minimal**: Should use minimum number of columns

**Q5: What is a Foreign Key and how does it work?**

**Foreign Key**: A column or combination of columns that establishes a link between data in two tables by referencing the primary key of another table.

**How it works**:
- Maintains referential integrity
- Prevents invalid data entry
- Ensures relationships between tables remain consistent
- Example: `customer_id` in Orders table references `id` in Customers table

**Q6: What are the different types of keys in DBMS?**

- **Primary Key**: Uniquely identifies each row
- **Foreign Key**: References primary key of another table
- **Candidate Key**: Columns that can serve as primary key
- **Super Key**: Any combination of columns that uniquely identifies rows
- **Alternate Key**: Candidate keys that are not chosen as primary key
- **Composite Key**: Primary key made up of multiple columns
- **Unique Key**: Ensures uniqueness but can have one NULL value

**Q7: What is normalization? Explain 1NF, 2NF, 3NF.**

**Normalization**: Process of organizing data to reduce redundancy and dependency.

**1NF (First Normal Form)**:
- Each cell contains only one value (atomic values)
- No repeating groups or arrays
- Each row must be unique

**2NF (Second Normal Form)**:
- Must be in 1NF
- All non-key attributes must be fully dependent on the primary key
- Eliminates partial dependencies

**3NF (Third Normal Form)**:
- Must be in 2NF
- No transitive dependencies (non-key attributes should not depend on other non-key attributes)
- Every non-key attribute depends only on the primary key

**Q8: What is denormalization and when would you use it?**

**Denormalization**: Intentionally introducing redundancy by combining tables or adding redundant data to improve query performance.

**When to use**:
- When read performance is more critical than storage space
- In data warehousing and reporting systems
- When complex joins are causing performance issues
- For frequently accessed data that doesn't change often

### SQL Fundamentals

**Q9: What are the different types of SQL commands?**

**DDL (Data Definition Language)**:
- CREATE, ALTER, DROP, TRUNCATE
- Define database structure

**DML (Data Manipulation Language)**:
- SELECT, INSERT, UPDATE, DELETE
- Manipulate data within tables

**DCL (Data Control Language)**:
- GRANT, REVOKE
- Control access permissions

**TCL (Transaction Control Language)**:
- COMMIT, ROLLBACK, SAVEPOINT
- Manage transactions

**Q10: Explain different types of JOINs with examples.**

**INNER JOIN**: Returns records that have matching values in both tables
```sql
SELECT * FROM Orders o 
INNER JOIN Customers c ON o.customer_id = c.id;
```

**LEFT JOIN**: Returns all records from left table and matched records from right
```sql
SELECT * FROM Customers c 
LEFT JOIN Orders o ON c.id = o.customer_id;
```

**RIGHT JOIN**: Returns all records from right table and matched records from left
```sql
SELECT * FROM Orders o 
RIGHT JOIN Customers c ON o.customer_id = c.id;
```

**FULL OUTER JOIN**: Returns all records when there's a match in either table
```sql
SELECT * FROM Customers c 
FULL OUTER JOIN Orders o ON c.id = o.customer_id;
```

**Q11: What's the difference between WHERE and HAVING clauses?**

**WHERE**:
- Filters rows before grouping
- Cannot use aggregate functions
- Applied to individual rows
- Used with SELECT, UPDATE, DELETE

**HAVING**:
- Filters groups after GROUP BY
- Can use aggregate functions
- Applied to grouped data
- Used only with GROUP BY

```sql
-- WHERE example
SELECT * FROM employees WHERE salary > 50000;

-- HAVING example
SELECT department, AVG(salary) 
FROM employees 
GROUP BY department 
HAVING AVG(salary) > 60000;
```

**Q12: What are indexes? How do they improve query performance?**

**Indexes**: Data structures that improve query performance by creating shortcuts to find data quickly.

**How they improve performance**:
- Reduce the number of rows scanned
- Enable faster searching, sorting, and joining
- Use B-tree or hash structures for quick lookups
- Trade-off: Faster reads but slower writes

**Types**:
- **Clustered**: Physical order of data matches index order
- **Non-clustered**: Separate structure pointing to data rows
- **Unique**: Ensures uniqueness
- **Composite**: Multiple columns

## 💡 LIKELY Questions (Medium Priority)

### ACID Properties & Transactions

**Q13: What are ACID properties? Explain each.**

**Atomicity**: Transaction is all-or-nothing. Either all operations complete successfully or none do.

**Consistency**: Database must remain in valid state before and after transaction. All constraints must be satisfied.

**Isolation**: Concurrent transactions don't interfere with each other. Each transaction appears to run in isolation.

**Durability**: Once transaction is committed, changes are permanent even if system fails.

**Q14: What is a transaction? What are transaction states?**

**Transaction**: A logical unit of work that consists of one or more database operations that should be executed as a single unit.

**Transaction States**:
- **Active**: Transaction is being executed
- **Partially Committed**: Final statement executed but not yet committed
- **Committed**: Transaction completed successfully
- **Aborted**: Transaction cancelled, database rolled back
- **Failed**: Transaction cannot proceed due to error

**Q15: What's the difference between COMMIT and ROLLBACK?**

**COMMIT**:
- Makes all changes permanent
- Ends the current transaction
- Releases locks and resources
- Cannot be undone

**ROLLBACK**:
- Undoes all changes made in current transaction
- Returns database to state before transaction began
- Releases locks and resources
- Can be partial (to savepoint) or complete

**Q16: What are isolation levels in databases?**

**READ UNCOMMITTED**: Lowest isolation, allows dirty reads
**READ COMMITTED**: Prevents dirty reads, allows non-repeatable reads
**REPEATABLE READ**: Prevents dirty and non-repeatable reads, allows phantom reads
**SERIALIZABLE**: Highest isolation, prevents all phenomena

**Q17: What are deadlocks? How can you prevent them?**

**Deadlock**: Situation where two or more transactions permanently block each other by holding locks that the other transactions need.

**Prevention strategies**:
- Always acquire locks in same order
- Use timeouts
- Keep transactions short
- Use lower isolation levels when appropriate
- Implement deadlock detection and resolution

### Advanced SQL Concepts

**Q18: What's the difference between UNION and UNION ALL?**

**UNION**:
- Combines results from multiple SELECT statements
- Removes duplicate rows
- Slower due to duplicate elimination
- Columns must match in number and data type

**UNION ALL**:
- Combines results from multiple SELECT statements
- Includes all rows (including duplicates)
- Faster performance
- More commonly used when duplicates are not a concern

**Q19: What are subqueries? Types of subqueries?**

**Subquery**: A query nested inside another query.

**Types**:
- **Scalar**: Returns single value
- **Column**: Returns single column
- **Row**: Returns single row
- **Table**: Returns multiple rows and columns

**By execution**:
- **Correlated**: References outer query
- **Non-correlated**: Independent of outer query

**Q20: What's the difference between correlated and non-correlated subqueries?**

**Non-correlated subquery**:
- Executes independently of outer query
- Executes once
- Can be run separately

```sql
SELECT * FROM employees 
WHERE salary > (SELECT AVG(salary) FROM employees);
```

**Correlated subquery**:
- References columns from outer query
- Executes once for each row of outer query
- Cannot be run independently

```sql
SELECT * FROM employees e1 
WHERE salary > (SELECT AVG(salary) FROM employees e2 
                WHERE e2.department = e1.department);
```

**Q21: What are views? What are materialized views?**

**Views**:
- Virtual tables based on SQL queries
- Don't store data physically
- Always show current data
- Provide security and simplification

**Materialized Views**:
- Store query results physically
- Need periodic refresh
- Faster query performance
- Use more storage space

**Q22: What are stored procedures and functions? What's the difference?**

**Stored Procedures**:
- Precompiled SQL code stored in database
- Can modify data
- Don't return values directly
- Called using CALL or EXEC

**Functions**:
- Return a single value
- Cannot modify data (in most databases)
- Can be used in SELECT statements
- Must return a value

**Q23: What are triggers? Types of triggers?**

**Triggers**: Special stored procedures that execute automatically in response to database events.

**Types by timing**:
- **BEFORE**: Execute before the triggering event
- **AFTER**: Execute after the triggering event
- **INSTEAD OF**: Replace the triggering event (views only)

**Types by event**:
- **INSERT triggers**
- **UPDATE triggers**
- **DELETE triggers**

### Database Design

**Q24: What is Entity-Relationship (ER) model?**

**ER Model**: Conceptual framework for designing databases using entities, attributes, and relationships.

**Components**:
- **Entities**: Objects or things (Customer, Order)
- **Attributes**: Properties of entities (Name, Email)
- **Relationships**: Associations between entities (Customer places Order)

**Q25: What's the difference between strong and weak entities?**

**Strong Entity**:
- Has primary key
- Exists independently
- Represented by rectangle

**Weak Entity**:
- Depends on strong entity for existence
- No primary key of its own
- Uses discriminator + strong entity's key
- Represented by double rectangle

**Q26: What are cardinality and participation constraints?**

**Cardinality**: Number of entity instances that can participate in a relationship
- One-to-One (1:1)
- One-to-Many (1:M)
- Many-to-Many (M:N)

**Participation**:
- **Total**: Every entity must participate (double line)
- **Partial**: Entity may or may not participate (single line)

**Q27: How do you convert ER diagram to relational schema?**

**Steps**:
1. Convert entities to tables
2. Convert attributes to columns
3. Convert relationships:
   - 1:1 - Add foreign key to either table
   - 1:M - Add foreign key to "many" side
   - M:N - Create junction table
4. Handle weak entities by including strong entity's key

## 🎯 SCENARIO-BASED Questions

### Performance & Optimization

**Q28: How would you optimize a slow-running query?**

**Steps**:
1. **Analyze execution plan** to identify bottlenecks
2. **Add appropriate indexes** on WHERE, JOIN, and ORDER BY columns
3. **Rewrite query** to use more efficient logic
4. **Avoid SELECT *** - select only needed columns
5. **Use LIMIT** to reduce result set
6. **Consider query hints** or database-specific optimizations
7. **Update table statistics** for better query planning
8. **Partition large tables** if appropriate

**Q29: What causes poor database performance?**

**Common causes**:
- Missing or inappropriate indexes
- Poorly written queries (N+1 problems, unnecessary JOINs)
- Large result sets without LIMIT
- Lock contention and blocking
- Outdated statistics
- Hardware limitations (CPU, memory, disk I/O)
- Network latency
- Inefficient database design

**Q30: When would you use clustered vs non-clustered indexes?**

**Clustered Index**:
- Use for frequently accessed range queries
- Primary key is good candidate
- Only one per table
- Data pages stored in order of index

**Non-clustered Index**:
- Use for specific column lookups
- Multiple allowed per table
- Good for covering queries
- Separate structure from data

**Q31: How do you handle large datasets efficiently?**

**Strategies**:
- **Partitioning**: Horizontal or vertical splitting
- **Indexing**: Appropriate index strategy
- **Pagination**: LIMIT/OFFSET for large result sets
- **Caching**: Query result caching
- **Data archiving**: Move old data to separate storage
- **Read replicas**: Distribute read load
- **Compression**: Reduce storage requirements

**Q32: What is query execution plan and how do you analyze it?**

**Query Execution Plan**: Shows how database engine executes a query, including operations, order, and resource usage.

**Analysis steps**:
1. Look for **table scans** instead of index seeks
2. Identify **expensive operations** (high cost percentage)
3. Check for **missing indexes** warnings
4. Look for **unnecessary sorts** or joins
5. Analyze **estimated vs actual rows**
6. Identify **blocking operations**

### Design Scenarios

**Q33: How would you design a database for an e-commerce application?**

**Key entities**:
- **Users**: id, email, password, name, address
- **Products**: id, name, price, description, inventory
- **Categories**: id, name, parent_category_id
- **Orders**: id, user_id, total, status, created_date
- **Order_Items**: order_id, product_id, quantity, price
- **Reviews**: id, user_id, product_id, rating, comment

**Key relationships**:
- User 1:M Orders
- Order 1:M Order_Items
- Product 1:M Order_Items
- Category 1:M Products (with hierarchy)

**Q34: How would you handle many-to-many relationships?**

**Solution**: Create a junction/bridge table

**Example**: Students and Courses
```sql
-- Instead of storing courses in student table
CREATE TABLE Students (
    student_id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE Courses (
    course_id INT PRIMARY KEY,
    course_name VARCHAR(100)
);

-- Create junction table
CREATE TABLE Student_Courses (
    student_id INT,
    course_id INT,
    enrollment_date DATE,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);
```

**Q35: How would you store hierarchical data in a relational database?**

**Methods**:

1. **Adjacency List**: Store parent_id in each record
```sql
CREATE TABLE Categories (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    parent_id INT REFERENCES Categories(id)
);
```

2. **Nested Set Model**: Store left and right values
3. **Path Enumeration**: Store full path as string
4. **Closure Table**: Separate table for all ancestor-descendant relationships

**Q36: How would you implement audit trails in database?**

**Approaches**:

1. **Audit Table**: Separate table for each audited table
```sql
CREATE TABLE User_Audit (
    audit_id INT PRIMARY KEY,
    user_id INT,
    action VARCHAR(10), -- INSERT, UPDATE, DELETE
    old_values JSON,
    new_values JSON,
    changed_by INT,
    changed_at TIMESTAMP
);
```

2. **Triggers**: Automatically capture changes
3. **Application-level**: Handle in application code
4. **Database-specific**: Use built-in auditing features

**Q37: How would you handle soft deletes vs hard deletes?**

**Soft Delete**:
- Add `deleted_at` or `is_deleted` column
- Don't actually remove data
- Filter out deleted records in queries
- Can recover deleted data
- Requires more storage

**Hard Delete**:
- Permanently remove data
- Cannot recover
- Less storage usage
- Simpler queries

**Implementation**:
```sql
-- Soft delete
UPDATE users SET deleted_at = NOW() WHERE id = 1;
SELECT * FROM users WHERE deleted_at IS NULL;

-- Hard delete
DELETE FROM users WHERE id = 1;
```

## 📝 SQL CODING Questions

**Q38: Write a query to find the second highest salary.**

```sql
-- Method 1: Using subquery
SELECT MAX(salary) as second_highest
FROM employees 
WHERE salary < (SELECT MAX(salary) FROM employees);

-- Method 2: Using LIMIT with ORDER BY
SELECT salary as second_highest
FROM employees 
ORDER BY salary DESC 
LIMIT 1 OFFSET 1;

-- Method 3: Using ROW_NUMBER()
SELECT salary as second_highest
FROM (
    SELECT salary, ROW_NUMBER() OVER (ORDER BY salary DESC) as rn
    FROM employees
) ranked
WHERE rn = 2;
```

**Q39: Write a query to find employees with salary greater than their manager.**

```sql
SELECT e.name as employee_name, e.salary as employee_salary,
       m.name as manager_name, m.salary as manager_salary
FROM employees e
JOIN employees m ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

**Q40: Write a query to find duplicate records in a table.**

```sql
-- Find duplicate emails
SELECT email, COUNT(*) as count
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- Get all duplicate records
SELECT *
FROM users u1
WHERE EXISTS (
    SELECT 1 FROM users u2 
    WHERE u2.email = u1.email 
    AND u2.id != u1.id
);
```

**Q41: Write a query using window functions (ROW_NUMBER, RANK, DENSE_RANK).**

```sql
SELECT 
    name,
    salary,
    department,
    ROW_NUMBER() OVER (PARTITION BY department ORDER BY salary DESC) as row_num,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) as rank_pos,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) as dense_rank_pos
FROM employees
ORDER BY department, salary DESC;
```

**Q42: Write a query to calculate running totals.**

```sql
SELECT 
    order_date,
    daily_sales,
    SUM(daily_sales) OVER (ORDER BY order_date) as running_total
FROM (
    SELECT 
        order_date,
        SUM(amount) as daily_sales
    FROM orders
    GROUP BY order_date
) daily_totals
ORDER BY order_date;
```

## 🔧 NoSQL & Modern Databases

**Q43: What are NoSQL databases? Types of NoSQL databases?**

**NoSQL**: "Not Only SQL" - databases that don't use traditional relational model.

**Types**:
1. **Document**: Store data as documents (MongoDB, CouchDB)
2. **Key-Value**: Simple key-value pairs (Redis, DynamoDB)
3. **Column-Family**: Store data in column families (Cassandra, HBase)
4. **Graph**: Store data as nodes and relationships (Neo4j, ArangoDB)

**Q44: When would you choose NoSQL over SQL?**

**Choose NoSQL when**:
- Need horizontal scalability
- Handling unstructured or semi-structured data
- Rapid development with changing requirements
- Need high availability and partition tolerance
- Working with big data and real-time analytics

**Choose SQL when**:
- Need ACID compliance
- Complex queries and transactions
- Well-defined schema
- Strong consistency requirements
- Mature ecosystem and tools

**Q45: What is CAP theorem?**

**CAP Theorem**: In distributed systems, you can only guarantee 2 out of 3:

- **Consistency**: All nodes see same data simultaneously
- **Availability**: System remains operational
- **Partition Tolerance**: System continues despite network failures

**Examples**:
- **CP**: Traditional RDBMS (sacrifice availability)
- **AP**: NoSQL databases like Cassandra (sacrifice consistency)
- **CA**: Single-node systems (not distributed)

**Q46: What are document databases? (MongoDB example)**

**Document Databases**: Store data as documents (usually JSON-like format).

**MongoDB Example**:
```javascript
// Document structure
{
  "_id": ObjectId("..."),
  "name": "John Doe",
  "email": "john@example.com",
  "address": {
    "street": "123 Main St",
    "city": "New York"
  },
  "orders": [
    {"product": "laptop", "price": 999},
    {"product": "mouse", "price": 29}
  ]
}
```

**Advantages**: Flexible schema, natural object mapping, horizontal scaling

**Q47: What are key-value stores? (Redis example)**

**Key-Value Stores**: Simplest NoSQL model with unique key mapped to value.

**Redis Example**:
```
SET user:1001:name "John Doe"
SET user:1001:email "john@example.com"
GET user:1001:name  // Returns "John Doe"

// Can store complex data structures
HSET user:1001 name "John Doe" email "john@example.com"
LPUSH user:1001:orders "laptop" "mouse"
```

**Use cases**: Caching, session storage, real-time analytics, queuing

**Q48: What is eventual consistency?**

**Eventual Consistency**: Distributed system model where system will become consistent over time, but not immediately.

**Characteristics**:
- Updates propagate to all nodes eventually
- No guarantee of immediate consistency
- Allows for higher availability and performance
- Common in NoSQL databases

**Example**: When you post on social media, all users don't see it immediately, but eventually everyone will see the same content.

## ⚡ QUICK-FIRE Questions

**Q49: What's the difference between DELETE, DROP, and TRUNCATE?**

- **DELETE**: Removes rows, can use WHERE clause, slower, can rollback, triggers fire
- **DROP**: Removes entire table structure and data, cannot rollback
- **TRUNCATE**: Removes all rows, faster than DELETE, cannot use WHERE, limited rollback

**Q50: What's the difference between CHAR and VARCHAR?**

- **CHAR**: Fixed length, pads with spaces, faster for fixed-size data
- **VARCHAR**: Variable length, uses only needed space, more flexible

**Q51: What is the default isolation level in most databases?**
**Answer**: READ COMMITTED

**Q52: Can a table have multiple primary keys?**
**Answer**: No, only one primary key per table (but can be composite)

**Q53: What's the maximum number of columns in a table?**
**Answer**: Varies by database (MySQL: 4096, PostgreSQL: 1600, SQL Server: 1024)

**Q54: What is referential integrity?**
**Answer**: Ensures relationships between tables remain valid. Foreign key values must either be NULL or match a primary key in referenced table.

**Q55: What's the difference between clustered and non-clustered indexes?**

**Clustered**: 
- Data pages stored in order of index
- One per table
- Faster for range queries

**Non-clustered**: 
- Separate structure pointing to data
- Multiple per table
- Good for specific lookups

## 🎪 ADVANCED Questions

### Database Administration

**Q56: How do you backup and restore databases?**

**Backup Types**:
- **Full Backup**: Complete database copy
- **Incremental**: Changes since last backup
- **Differential**: Changes since last full backup
- **Transaction Log**: Continuous log of changes

**Methods**:
```sql
-- SQL Server
BACKUP DATABASE MyDB TO DISK = 'C:\backup\MyDB.bak';
RESTORE DATABASE MyDB FROM DISK = 'C:\backup\MyDB.bak';

-- MySQL
mysqldump -u user -p database_name > backup.sql
mysql -u user -p database_name < backup.sql
```

**Q57: What are database partitioning strategies?**

**Horizontal Partitioning (Sharding)**:
- Split rows across multiple tables/databases
- Based on partition key (date, region, ID range)

**Vertical Partitioning**:
- Split columns across tables
- Separate frequently and rarely accessed columns

**Methods**:
- **Range**: Partition by value ranges
- **Hash**: Use hash function on partition key
- **List**: Explicit list of values per partition

**Q58: How do you handle database replication?**

**Types**:
- **Master-Slave**: One write node, multiple read replicas
- **Master-Master**: Multiple write nodes
- **Synchronous**: Wait for replica confirmation
- **Asynchronous**: Don't wait for replica confirmation

**Benefits**: Load distribution, high availability, geographic distribution

**Q59: What is database sharding?**

**Sharding**: Horizontal partitioning across multiple database instances.

**Strategies**:
- **Range-based**: Partition by ID or date ranges
- **Hash-based**: Use hash function to determine shard
- **Directory-based**: Lookup service determines shard

**Challenges**: Cross-shard queries, rebalancing, complexity

**Q60: How do you monitor database performance?**

**Key Metrics**:
- Query response time
- Throughput (queries per second)
- Connection count
- Lock waits and deadlocks
- Buffer cache hit ratio
- Disk I/O utilization

**Tools**: Database-specific monitoring tools, APM solutions, custom dashboards

### Concurrency Control

**Q61: What are locking mechanisms in databases?**

**Lock Types**:
- **Shared (S)**: Multiple transactions can read
- **Exclusive (X)**: Only one transaction can write
- **Intent**: Indicates intention to acquire locks at lower level

**Lock Granularity**:
- **Row-level**: Lock individual rows
- **Page-level**: Lock data pages
- **Table-level**: Lock entire tables

**Q62: What's the difference between optimistic and pessimistic locking?**

**Pessimistic Locking**:
- Assume conflicts will occur
- Lock resources before using
- Better for high-conflict scenarios
- May cause blocking

**Optimistic Locking**:
- Assume conflicts are rare
- Check for conflicts before committing
- Better for low-conflict scenarios
- May require retry logic

**Q63: What is two-phase locking protocol?**

**Two-Phase Locking (2PL)**: Protocol ensuring serializability.

**Phases**:
1. **Growing Phase**: Can acquire locks, cannot release
2. **Shrinking Phase**: Can release locks, cannot acquire

**Ensures**: Conflict serializable schedules, prevents some anomalies

**Q64: How do you handle concurrent transactions?**

**Strategies**:
- **Proper isolation levels** based on requirements
- **Appropriate locking strategy** (optimistic vs pessimistic)
- **Deadlock detection and resolution**
- **Connection pooling** to manage resources
- **Queue management** for high-concurrency scenarios
- **Retry logic** for transient failures

---

## 🚀 Key Takeaways for Interviews

### Always Remember:
1. **ACID properties** in order: Atomicity, Consistency, Isolation, Durability
2. **Primary key** cannot be NULL or duplicate
3. **Foreign key** maintains referential integrity
4. **Normalization** reduces redundancy and dependency
5. **Indexes** speed up reads but slow down writes
6. **HAVING** is used with GROUP BY, **WHERE** is not
7. **INNER JOIN** excludes non-matching rows

### When Writing SQL:
- Start with basic SELECT structure
- Build step by step: FROM → WHERE → GROUP BY → HAVING → ORDER BY
- Use table aliases for readability
- Explain your approach as you write

### For System Design Questions:
- Always ask clarifying questions
- Consider scalability, performance, and data consistency
- Think about trade-offs between different approaches
- Consider both SQL and NoSQL solutions based on requirements
