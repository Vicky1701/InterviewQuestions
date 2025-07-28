# JPA (Java Persistence API) Questions

## Core Concepts
1. **What is JPA and how is it different from Hibernate?**
2. **Explain JPA as specification vs Hibernate as implementation.**
3. **Explain JPA entity lifecycle states.**
   - Cover New, Managed, Detached, and Removed states with transitions
4. **What are JPA annotations? Explain commonly used ones.**
   - Discuss `@Entity`, `@Id`, `@GeneratedValue`, `@Column`, `@Table`, `@OneToMany`, `@ManyToOne`, `@JoinColumn`

## Relationships
5. **What is the difference between @OneToMany and @ManyToOne?**
   - Include bidirectional relationships and cascade operations
6. **Explain fetch types in JPA - LAZY vs EAGER.**
   - Discuss when to use each and performance implications

## Querying
7. **What is JPQL? How is it different from SQL?**
   - Explain object-oriented queries vs table-based queries

---

# JDBC Questions

## Core Concepts
1. **What is JDBC? Explain JDBC architecture.**
   - Cover Driver Manager, Connection, Statement, ResultSet components
2. **What are different types of JDBC drivers?**
   - Explain Type 1, 2, 3, and 4 drivers with examples

## Advanced Topics
3. **Explain JDBC connection pooling. Why is it important?**
   - Discuss performance benefits and connection lifecycle management
4. **What is the difference between Statement, PreparedStatement, and CallableStatement?**
   - Cover SQL injection prevention and performance aspects
5. **How do you handle transactions in JDBC?**
   - Discuss `setAutoCommit(false)`, `commit()`, `rollback()`, and savepoints
6. **What are JDBC batch operations?**
   - Explain `addBatch()` and `executeBatch()` methods

---

# Hibernate Questions

## Core Concepts
1. **What is Hibernate? What are its advantages?**
   - Explain ORM benefits, caching, lazy loading, and HQL
2. **Explain Hibernate architecture.**
   - Cover SessionFactory, Session, Transaction, and Configuration
3. **What is the difference between Session and SessionFactory?**
   - Discuss thread-safety and lifecycle differences

## Performance
4. **Explain Hibernate caching mechanisms.**
   - Cover first-level cache (Session) and second-level cache (SessionFactory)
5. **What are different ways to map entities in Hibernate?**
   - Discuss XML configuration vs annotation-based mapping
6. **Explain cascade operations in Hibernate associations.**
   - Cover `CascadeType.ALL`, `PERSIST`, `MERGE`, `REMOVE`, `REFRESH`, `DETACH`
7. **What is N+1 problem in Hibernate? How to solve it?**
   - Explain the issue and solutions like `JOIN FETCH`, `@BatchSize`, `@Fetch`

---

# Advanced/Integration Questions

1. **How does SpringBoot integrate with JPA/Hibernate?**
   - Explain `spring-boot-starter-data-jpa` and auto-configuration
2. **What is Spring Data JPA? How is it different from plain JPA?**
   - Discuss repository pattern, query methods, and custom implementations
3. **Explain `@Transactional` annotation in Spring.**
   - Cover propagation levels, isolation levels, and rollback scenarios
4. **How do you handle database migrations in SpringBoot?**
   - Discuss Flyway and Liquibase integration
5. **What are JPA repositories? Explain different types.**
   - Cover `CrudRepository`, `JpaRepository`, `PagingAndSortingRepository`
6. **How do you implement custom queries in Spring Data JPA?**
   - Explain `@Query` annotation, named queries, and method naming conventions
7. **What is the difference between save() and saveAndFlush() in JPA?**
   - Discuss when changes are persisted to database

---

# Practical Coding Questions

Be prepared to write code for:
- Simple entity classes with JPA annotations
- Repository interfaces with custom query methods
- Service classes with `@Transactional` methods
- JDBC connection and basic CRUD operations
- Hibernate configuration and session management

---

# Performance & Best Practices

1. **How do you optimize JPA/Hibernate performance?**
   - Discuss proper fetch strategies, query optimization, connection pooling
2. **What are common JPA/Hibernate pitfalls to avoid?**
   - Cover N+1 queries, improper session management, and inefficient mappings
