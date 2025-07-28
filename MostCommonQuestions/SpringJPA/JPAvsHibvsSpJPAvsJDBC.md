JPA (Java Persistence API) Questions
What is JPA and how is it different from Hibernate?
Explain JPA as specification vs Hibernate as implementation.
Explain JPA entity lifecycle states.
Cover New, Managed, Detached, and Removed states with transitions.
What are JPA annotations? Explain commonly used ones.
Discuss @Entity, @Id, @GeneratedValue, @Column, @Table, @OneToMany, @ManyToOne, @JoinColumn.
What is the difference between @OneToMany and @ManyToOne?
Include bidirectional relationships and cascade operations.
Explain fetch types in JPA - LAZY vs EAGER.
Discuss when to use each and performance implications.
What is JPQL? How is it different from SQL?
Explain object-oriented queries vs table-based queries.
JDBC Questions
What is JDBC? Explain JDBC architecture.
Cover Driver Manager, Connection, Statement, ResultSet components.
What are different types of JDBC drivers?
Explain Type 1, 2, 3, and 4 drivers with examples.
Explain JDBC connection pooling. Why is it important?
Discuss performance benefits and connection lifecycle management.
What is the difference between Statement, PreparedStatement, and CallableStatement?
Cover SQL injection prevention and performance aspects.
How do you handle transactions in JDBC?
Discuss setAutoCommit(false), commit(), rollback(), and savepoints.
What are JDBC batch operations?
Explain addBatch() and executeBatch() methods.
Hibernate Questions
What is Hibernate? What are its advantages?
Explain ORM benefits, caching, lazy loading, and HQL.
Explain Hibernate architecture.
Cover SessionFactory, Session, Transaction, and Configuration.
What is the difference between Session and SessionFactory?
Discuss thread-safety and lifecycle differences.
Explain Hibernate caching mechanisms.
Cover first-level cache (Session) and second-level cache (SessionFactory).
What are different ways to map entities in Hibernate?
Discuss XML configuration vs annotation-based mapping.
Explain cascade operations in Hibernate associations.
Cover CascadeType.ALL, PERSIST, MERGE, REMOVE, REFRESH, DETACH.
What is N+1 problem in Hibernate? How to solve it?
Explain the issue and solutions like JOIN FETCH, @BatchSize, @Fetch.
Advanced/Integration Questions
How does SpringBoot integrate with JPA/Hibernate?
Explain spring-boot-starter-data-jpa and auto-configuration.
What is Spring Data JPA? How is it different from plain JPA?
Discuss repository pattern, query methods, and custom implementations.
Explain @Transactional annotation in Spring.
Cover propagation levels, isolation levels, and rollback scenarios.
How do you handle database migrations in SpringBoot?
Discuss Flyway and Liquibase integration.
What are JPA repositories? Explain different types.
Cover CrudRepository, JpaRepository, PagingAndSortingRepository.
How do you implement custom queries in Spring Data JPA?
Explain @Query annotation, named queries, and method naming conventions.
What is the difference between save() and saveAndFlush() in JPA?
Discuss when changes are persisted to database.
Practical Coding Questions
Be prepared to write code for:

Simple entity classes with JPA annotations
Repository interfaces with custom query methods
Service classes with @Transactional methods
JDBC connection and basic CRUD operations
Hibernate configuration and session management

Performance & Best Practices
How do you optimize JPA/Hibernate performance?
Discuss proper fetch strategies, query optimization, connection pooling.
What are common JPA/Hibernate pitfalls to avoid?
Cover N+1 queries, improper session management, and inefficient mappings.
