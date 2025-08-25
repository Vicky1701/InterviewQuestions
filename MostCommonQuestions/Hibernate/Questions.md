🔑 Core Hibernate Basics

Q: What is Hibernate and why is it used?
👉 Hibernate is an ORM (Object-Relational Mapping) framework that maps Java objects to database tables. It eliminates boilerplate JDBC code, handles SQL generation, caching, and transaction management, making persistence easier and database-independent.

Q: Advantages of Hibernate over JDBC.

No need to write SQL manually for CRUD.

Database independent (dialects).

Built-in caching improves performance.

Manages relationships easily (1–1, 1–M, etc.).

Lazy loading, dirty checking, batch processing support.

Q: Difference between Session and SessionFactory.

SessionFactory → heavyweight, thread-safe, created once per DB. Manages Sessions.

Session → lightweight, not thread-safe, represents a single unit of work with DB.

Q: What is hibernate.cfg.xml vs hibernate.properties?

hibernate.cfg.xml → XML-based configuration (connection, mappings, dialect).

hibernate.properties → Property-file alternative, key-value style.
👉 Both serve the same purpose, but hibernate.cfg.xml is more common in enterprise apps.

Q: How do you map a Java class to a DB table?

Using annotations: @Entity, @Table, @Column, etc.

Or using XML mapping files (.hbm.xml).
👉 Modern apps use annotations (lighter & easier to maintain).

Q: Difference between @Entity and @Table.

@Entity → Marks a class as a persistent entity.

@Table → Specifies DB table name (optional, defaults to class name).

🧩 Mappings

Q: Types of inheritance mapping.

Single Table: One table for all classes (fast, but sparse columns).

Joined Table: Parent + child tables (normalized, flexible, but joins).

Table Per Class: Each class has its own table (redundant, rarely used).

Q: Association mappings.

One-to-One → e.g., User ↔ Profile.

One-to-Many → e.g., Department ↔ Employees.

Many-to-One → e.g., Employee → Department.

Many-to-Many → e.g., Students ↔ Courses (via join table).

Q: FetchType: LAZY vs EAGER.

LAZY (default for collections) → data loaded only when accessed.

EAGER → data loaded immediately with parent entity.

Q: Cascading in Hibernate.
Cascade defines operations to propagate from parent to child:

PERSIST, REMOVE, MERGE, REFRESH, DETACH, ALL.
👉 Example: Deleting a Department deletes all Employees if cascade is enabled.

⚙️ Querying & Transactions

Q: What is HQL? How is it different from SQL?

HQL → object-oriented query language (works with entity names, not table names).


Independent of DB, while SQL is DB-specific.

Q: Criteria API vs HQL.

HQL → String-based, readable but prone to errors.

Criteria API → Type-safe, dynamic query construction (preferred in complex queries).

Q: Difference between get() and load().

get() → Hits DB immediately, returns null if not found.

load() → Returns proxy, DB hit only when accessed, throws ObjectNotFoundException if missing.

Q: Pagination in Hibernate.
Use setFirstResult() and setMaxResults() with Query/Criteria API.

Q: Named Queries.
Predefined queries using @NamedQuery annotation or XML, improves reusability and readability.

Q: How does Hibernate handle transactions?
Hibernate integrates with JTA/JDBC, manages begin, commit, rollback. Transactions are handled at Session/EntityManager level.

Q: Why is @Transactional required?

Ensures atomicity & consistency.

Without it, Hibernate may throw LazyInitializationException or incomplete commits.

🧵 Caching & Performance

Q: First-level vs Second-level cache.

First-level → Session scope, mandatory, default.

Second-level → SessionFactory scope, optional, pluggable providers (EhCache, Infinispan).

Q: What is query cache?
Stores query results, used with second-level cache.

Q: Common performance issues.

N+1 problem → too many selects → solve with fetch joins/batch fetch.

LazyInitializationException → accessing lazy data outside session.

Overfetching → loading unnecessary data → fix with proper fetch strategy.

Q: What is lazy loading?
Defers fetching of associations until accessed. Helps performance, but must manage session properly.

🔄 Persistence Context & Object States

Q: Transient, Persistent, Detached states.

Transient → object not in DB or session.

Persistent → object attached to session, tracked.

Detached → was persistent, session closed, no longer tracked.

Q: Difference between merge() and update().

update() → reattaches a detached object, fails if same entity already in session.

merge() → copies state into managed entity, safer.

Q: Dirty checking mechanism.
Hibernate automatically detects changes in persistent objects and updates DB on commit.

⚠️ Exception Handling & Locking

Q: LazyInitializationException?
Happens when accessing lazy data outside session.
👉 Fix: Open Session in View, use JOIN FETCH, or initialize before session closes.

Q: Optimistic vs Pessimistic locking.

Optimistic → versioning, assumes no conflict.

Pessimistic → DB-level locks, prevents concurrent updates.

Q: NonUniqueObjectException, StaleObjectStateException.

NonUniqueObjectException → multiple objects with same ID in session.

StaleObjectStateException → concurrent update conflict, usually with versioning.

🛠️ Advanced / Real-World

Q: How do you integrate Hibernate with Spring?
Using Spring’s LocalSessionFactoryBean, HibernateTemplate, or JPA with Spring Data. Transactions managed via @Transactional.

Q: Batch insert/update in Hibernate.
Use hibernate.jdbc.batch_size + flush & clear periodically in loop.

Q: Hibernate Validator basics.
Implements JSR-380 (Bean Validation). Annotations like @NotNull, @Size, @Email.

Q: Can Hibernate work without XML mapping?
Yes, with annotations (preferred).

Q: @Embeddable vs @Embedded.

@Embeddable → defines reusable value type.

@Embedded → used inside entity to include that value type.

🌐 Miscellaneous

Q: @Id and @GeneratedValue strategies.

Strategies: AUTO, IDENTITY, SEQUENCE, TABLE.

Used for primary key generation.

Q: Hibernate vs JPA.

JPA → specification (interfaces).

Hibernate → implementation of JPA + extra features.

Q: Logging SQL queries.
Set hibernate.show_sql=true or use logging frameworks.

Q: Best practices in Hibernate.

Always prefer LAZY fetch.

Use batch fetch / fetch joins.

Avoid overusing cascades.

Use second-level cache carefully.

Keep transactions short.

⭐ Practical / Behavioral

Q: Tell me about a project where you used Hibernate.
👉 Example: “In my e-commerce project, we used Hibernate with Spring Boot to handle product catalog and orders. We optimized queries using HQL joins and second-level cache (EhCache).”

Q: Biggest challenge you faced?
👉 Example: “We faced the N+1 problem with lazy-loaded collections. Solved it using JOIN FETCH and batch fetching.”

Q: How did you optimize queries/performance?
👉 Example: “Enabled second-level cache, used projections for large queries, and replaced nested loops with HQL joins. Also tuned batch size for inserts.”