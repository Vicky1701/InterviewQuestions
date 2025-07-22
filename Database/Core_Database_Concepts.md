# Core Database Concepts - Study Notes

## ACID Properties

**ACID** ensures database transactions are processed reliably and maintain data integrity.

### Atomicity
- **All or nothing principle** - A transaction either completes entirely or fails completely
- If any part of a transaction fails, the entire transaction is rolled back
- Example: Bank transfer must debit one account AND credit another, or neither happens

### Consistency
- Database remains in a **valid state** before and after transactions
- All data integrity rules, constraints, and triggers are enforced
- Example: Account balance cannot go negative if business rules prohibit it

### Isolation
- **Concurrent transactions don't interfere** with each other
- Each transaction appears to execute in isolation, even when running simultaneously
- Prevents issues like dirty reads, phantom reads, and non-repeatable reads
- Controlled through isolation levels (Read Uncommitted, Read Committed, Repeatable Read, Serializable)

### Durability
- **Committed changes are permanent** and survive system failures
- Data is stored in non-volatile storage and can be recovered after crashes
- Achieved through transaction logs and backup mechanisms

---

## Database Normalization

**Normalization** eliminates data redundancy and reduces storage space while preventing update anomalies.

### First Normal Form (1NF)
- **Eliminate repeating groups** and ensure atomic values
- Each cell contains only single values (no lists or comma-separated data)
- Each row must be unique
- Example: Split "Phone: 123-456, 789-012" into separate rows

### Second Normal Form (2NF)
- Must be in **1NF first**
- **Remove partial dependencies** - non-key attributes must depend on the entire primary key
- Applies to tables with composite primary keys
- Example: In (StudentID, CourseID, StudentName, CourseName), move StudentName to separate Students table

### Third Normal Form (3NF)
- Must be in **2NF first**
- **Eliminate transitive dependencies** - non-key attributes shouldn't depend on other non-key attributes
- Example: If (StudentID, DeptID, DeptName), move DeptName to separate Departments table since it depends on DeptID, not StudentID

### Boyce-Codd Normal Form (BCNF)
- **Stricter version of 3NF**
- Every determinant must be a candidate key
- Eliminates anomalies that 3NF might miss in certain edge cases
- Example: Professor teaches Course in Room - if Professor determines Room, but (Professor, Course) is the key, violates BCNF

---

## Entity-Relationship (ER) Modeling

**ER modeling** provides a conceptual framework for designing databases by representing real-world entities and their relationships.

### Entities
- **Objects or concepts** that can be distinctly identified
- Represented as rectangles in ER diagrams
- Examples: Customer, Order, Product, Employee

### Attributes
- **Properties or characteristics** of entities
- Represented as ovals connected to entities
- Types: Simple, Composite, Derived, Multi-valued
- Example: Customer has Name, Address, Phone, Email

### Relationships
- **Associations between entities**
- Represented as diamonds in ER diagrams
- Cardinality: One-to-One (1:1), One-to-Many (1:M), Many-to-Many (M:N)
- Example: Customer "places" Orders (1:M relationship)

### Weak Entities
- **Cannot exist independently** without a strong entity
- Represented with double rectangles
- Example: Order Line Items depend on Orders

---

## Keys and Constraints

### Primary Keys
- **Unique identifier** for each row in a table
- Cannot contain NULL values
- Only one primary key per table
- Can be single column or composite (multiple columns)
- Automatically creates a unique index

### Foreign Keys
- **References primary key** of another table (or same table)
- Establishes and enforces links between tables
- Maintains referential integrity
- Can contain NULL values (unless specified otherwise)
- Example: OrderID in Order_Items table references Orders.OrderID

### Constraints
- **Rules that enforce data integrity**

#### Common Constraint Types:
- **NOT NULL**: Prevents empty values
- **UNIQUE**: Ensures no duplicate values (allows one NULL)
- **CHECK**: Validates data against specified conditions
- **DEFAULT**: Provides default value when none specified
- **FOREIGN KEY**: Maintains referential integrity between tables

---

## Data Types and Storage Considerations

### Numeric Types
- **INTEGER**: Whole numbers (-2^31 to 2^31-1)
- **BIGINT**: Large whole numbers (-2^63 to 2^63-1)
- **DECIMAL/NUMERIC**: Fixed-point numbers for precise calculations
- **FLOAT/REAL**: Floating-point numbers for approximate values
- **Storage tip**: Use smallest appropriate type to save space

### String Types
- **CHAR(n)**: Fixed-length strings, padded with spaces
- **VARCHAR(n)**: Variable-length strings up to n characters
- **TEXT**: Large variable-length strings
- **Storage tip**: VARCHAR more efficient for variable content, CHAR better for fixed-length data

### Date/Time Types
- **DATE**: Calendar dates (YYYY-MM-DD)
- **TIME**: Time of day (HH:MM:SS)
- **DATETIME/TIMESTAMP**: Combined date and time
- **Storage tip**: Use appropriate precision to avoid wasting space

### Boolean Type
- **BOOLEAN**: True/False values
- Often stored as single bit or tiny integer

### Binary Types
- **BLOB**: Binary Large Objects for files, images, etc.
- **VARBINARY**: Variable-length binary data
- **Storage tip**: Consider storing file paths instead of actual files for better performance

### Storage Considerations
- **Choose appropriate data types** to minimize storage overhead
- **Index frequently queried columns** but avoid over-indexing
- **Consider data compression** for large text fields
- **Plan for growth** - estimate future data volume
- **Denormalization trade-offs** - sometimes breaking normalization rules improves performance
