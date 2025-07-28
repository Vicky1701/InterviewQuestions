# Spring Data JPA Interview Questions

## Core Concepts

### 1 What is Spring Data JPA? How is it different from plain JPA?
Spring Data JPA is a part of Spring Data framework that provides repository abstraction over JPA. It eliminates boilerplate code by providing pre-built repository interfaces and automatic query generation from method names, while plain JPA requires manual EntityManager handling.

### 2 Explain the repository hierarchy in Spring Data JPA.
- **Repository** (marker interface)
- **CrudRepository** (basic CRUD operations)
- **PagingAndSortingRepository** (adds pagination and sorting)
- **JpaRepository** (JPA specific methods like flush, saveAndFlush)

### What is the difference between CrudRepository, JpaRepository, and PagingAndSortingRepository?
- **CrudRepository**: Basic CRUD (save, findById, findAll, delete)
- **PagingAndSortingRepository**: Extends CrudRepository, adds findAll(Pageable) and findAll(Sort)
- **JpaRepository**: Extends PagingAndSortingRepository, adds JPA-specific methods like flush(), saveAndFlush(), deleteInBatch()

## Repository Methods

### 3 How does query method generation work in Spring Data JPA?
Spring Data JPA generates queries automatically based on method names using keywords like findBy, countBy, deleteBy, etc. It parses method names and creates corresponding JPQL queries.

### 4 What are the naming conventions for query methods?
```java
// Find operations
findByFirstName(String firstName)
findByFirstNameAndLastName(String first, String last)
findByFirstNameOrLastName(String first, String last)
findByFirstNameIgnoreCase(String firstName)
findByFirstNameContaining(String firstName)
findByAgeGreaterThan(int age)
findByAgeBetween(int start, int end)

// Count operations
countByFirstName(String firstName)

// Delete operations
deleteByFirstName(String firstName)
```

### 5 Explain query method keywords with examples.
- **And/Or**: `findByFirstNameAndLastName`
- **GreaterThan/LessThan**: `findByAgeGreaterThan`
- **Between**: `findByAgeBetween`
- **Like/Containing**: `findByFirstNameContaining`
- **In**: `findByAgeIn(List<Integer> ages)`
- **IsNull/IsNotNull**: `findByFirstNameIsNull`
- **IgnoreCase**: `findByFirstNameIgnoreCase`
- **OrderBy**: `findByLastNameOrderByFirstNameAsc`

## Custom Queries

### How do you write custom queries using @Query annotation?
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    @Query("SELECT u FROM User u WHERE u.email = ?1")
    User findByEmail(String email);
    
    @Query("SELECT u FROM User u WHERE u.firstName = :firstName AND u.lastName = :lastName")
    List<User> findByFullName(@Param("firstName") String first, @Param("lastName") String last);
    
    // Native SQL query
    @Query(value = "SELECT * FROM users WHERE email = ?1", nativeQuery = true)
    User findByEmailNative(String email);
}
```

### 6 What is the difference between JPQL and native queries in @Query?
- **JPQL**: Object-oriented queries using entity names and properties
- **Native queries**: Raw SQL queries using table and column names, set nativeQuery = true

### 7 How do you implement custom repository methods?
Create a custom interface and implementation:
```java
// Custom interface
public interface UserRepositoryCustom {
    List<User> findUsersWithCustomLogic(String criteria);
}

// Implementation
@Component
public class UserRepositoryCustomImpl implements UserRepositoryCustom {
    @PersistenceContext
    private EntityManager entityManager;
    
    @Override
    public List<User> findUsersWithCustomLogic(String criteria) {
        // Custom implementation using EntityManager
    }
}

// Main repository extends both
public interface UserRepository extends JpaRepository<User, Long>, UserRepositoryCustom {
}
```

## Pagination and Sorting

### 8 How do you implement pagination in Spring Data JPA?
```java
@Repository
public interface UserRepository extends PagingAndSortingRepository<User, Long> {
    Page<User> findByLastName(String lastName, Pageable pageable);
}

// Usage in service
public Page<User> getUsers(int page, int size) {
    Pageable pageable = PageRequest.of(page, size);
    return userRepository.findAll(pageable);
}
```

### 9 How do you implement sorting?
```java
// Using Sort object
Sort sort = Sort.by(Sort.Direction.ASC, "firstName");
List<User> users = userRepository.findAll(sort);

// With pagination
Pageable pageable = PageRequest.of(0, 10, Sort.by("firstName"));
Page<User> userPage = userRepository.findAll(pageable);
```

### 10 What is the difference between Page and Slice?
- **Page**: Contains total count of elements and total pages (requires additional count query)
- **Slice**: Only knows if there's next slice available (more efficient, no count query)

## Modifying Queries

### 11 How do you perform update/delete operations using @Query?
```java
@Modifying
@Query("UPDATE User u SET u.firstName = :firstName WHERE u.id = :id")
int updateUserFirstName(@Param("id") Long id, @Param("firstName") String firstName);

@Modifying
@Query("DELETE FROM User u WHERE u.lastLoginDate < :date")
int deleteInactiveUsers(@Param("date") LocalDateTime date);
```

### 12 What is @Modifying annotation and when to use it?
@Modifying is required for update/delete queries. It indicates that the query modifies the database. Should be used with @Transactional for proper transaction management.

## Projections

###  13 What are projections in Spring Data JPA?
Projections allow you to retrieve only specific fields from entities instead of full objects.

### Types of projections:
```java
// Interface-based projection
public interface UserSummary {
    String getFirstName();
    String getLastName();
    String getEmail();
}

// Class-based projection (DTO)
public class UserDto {
    private String firstName;
    private String lastName;
    // constructors, getters, setters
}

// Repository methods
List<UserSummary> findByDepartment(String department);
List<UserDto> findDtoByDepartment(String department);
```

### 14 What is @Value annotation in projections?
```java
public interface UserInfo {
    @Value("#{target.firstName + ' ' + target.lastName}")
    String getFullName();
    
    String getEmail();
}
```

## Advanced Features

### 15 What are Specifications in Spring Data JPA?
Specifications provide a programmatic way to build dynamic queries using Criteria API:
```java
public interface UserRepository extends JpaRepository<User, Long>, JpaSpecificationExecutor<User> {
}

// Specification example
public static Specification<User> hasFirstName(String firstName) {
    return (root, query, criteriaBuilder) -> 
        criteriaBuilder.equal(root.get("firstName"), firstName);
}
```

###  16 How do you handle auditing in Spring Data JPA?
```java
@Entity
@EntityListeners(AuditingEntityListener.class)
public class User {
    @CreatedDate
    private LocalDateTime createdDate;
    
    @LastModifiedDate
    private LocalDateTime lastModifiedDate;
    
    @CreatedBy
    private String createdBy;
    
    @LastModifiedBy
    private String lastModifiedBy;
}

// Enable auditing
@EnableJpaAuditing
@Configuration
public class JpaConfig {
}
```

### 17 What is @EnableJpaRepositories annotation?
Enables Spring Data JPA repositories. Auto-configures repository beans and can specify base packages:
```java
@EnableJpaRepositories(basePackages = "com.example.repository")
@SpringBootApplication
public class Application {
}
```

## Performance and Best Practices

### 18 How do you optimize Spring Data JPA performance?
- Use appropriate fetch strategies (LAZY vs EAGER)
- Implement pagination for large datasets  
- Use projections for selective field retrieval
- Leverage query method caching
- Use batch operations for bulk updates
- Optimize N+1 queries with @EntityGraph

### 19 What is @EntityGraph and how to use it?
```java
@EntityGraph(attributePaths = {"orders", "orders.items"})
List<User> findAll();

// Or define named entity graph on entity
@NamedEntityGraph(name = "User.orders", attributeNodes = @NamedAttributeNode("orders"))
@Entity
public class User {
}
```

### 20 Common pitfalls to avoid in Spring Data JPA:
- Using findAll() without pagination on large datasets
- Not using @Transactional with @Modifying queries
- Creating too many custom repository methods instead of using Specifications
- Ignoring N+1 query problems
- Not optimizing fetch strategies

## Practice Tips
These questions cover the essential Spring Data JPA concepts you should master for your interviews. Practice implementing these features in small projects to gain hands-on experience.
