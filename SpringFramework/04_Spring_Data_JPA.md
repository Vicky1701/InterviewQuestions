# Spring Data JPA & Hibernate - Interview Questions & Answers

## 🎯 **Essential Questions for 2+ Years Experience**

---

## **1. What is Spring Data JPA? How is it different from plain JPA and Hibernate?**

**Answer:**

**Spring Data JPA** is a Spring module that provides repository abstraction over JPA, eliminating boilerplate data access code.

**Comparison Table:**

| Aspect | JPA | Hibernate | Spring Data JPA |
|--------|-----|-----------|-----------------|
| **Type** | Specification | Implementation | Abstraction Layer |
| **Provider** | Interface only | Concrete implementation | Repository abstraction |
| **Boilerplate Code** | High | Medium | Minimal |
| **Query Generation** | Manual | Manual | Automatic |
| **Configuration** | Complex | Medium | Simple |

**Example Comparison:**

**Plain JPA:**
```java
@Repository
public class UserRepositoryImpl {
    
    @PersistenceContext
    private EntityManager entityManager;
    
    public User findById(Long id) {
        return entityManager.find(User.class, id);
    }
    
    public List<User> findByEmail(String email) {
        TypedQuery<User> query = entityManager.createQuery(
            "SELECT u FROM User u WHERE u.email = :email", User.class);
        query.setParameter("email", email);
        return query.getResultList();
    }
    
    public User save(User user) {
        if (user.getId() == null) {
            entityManager.persist(user);
            return user;
        } else {
            return entityManager.merge(user);
        }
    }
}
```

**Spring Data JPA:**
```java
public interface UserRepository extends JpaRepository<User, Long> {
    
    // No implementation needed - Spring generates automatically
    Optional<User> findByEmail(String email);
    List<User> findByFirstNameAndLastName(String firstName, String lastName);
    List<User> findByAgeGreaterThan(int age);
    
    @Query("SELECT u FROM User u WHERE u.department = :dept")
    List<User> findByDepartment(@Param("dept") String department);
}
```

**Benefits of Spring Data JPA:**
1. **Reduced Boilerplate**: No need to write basic CRUD operations
2. **Query Derivation**: Automatic query generation from method names
3. **Pagination Support**: Built-in pagination and sorting
4. **Audit Support**: Automatic auditing capabilities
5. **Custom Queries**: Support for JPQL and native SQL

---

## **2. Explain the Spring Data JPA Repository hierarchy.**

**Answer:**

**Repository Hierarchy:**

```
Repository (Marker Interface)
    ↓
CrudRepository<T, ID>
    ↓
PagingAndSortingRepository<T, ID>
    ↓
JpaRepository<T, ID>
```

**Detailed Explanation:**

### **1. Repository Interface**
```java
// Marker interface - no methods
public interface Repository<T, ID> {
}
```

### **2. CrudRepository Interface**
```java
public interface CrudRepository<T, ID> extends Repository<T, ID> {
    <S extends T> S save(S entity);
    <S extends T> Iterable<S> saveAll(Iterable<S> entities);
    Optional<T> findById(ID id);
    boolean existsById(ID id);
    Iterable<T> findAll();
    Iterable<T> findAllById(Iterable<ID> ids);
    long count();
    void deleteById(ID id);
    void delete(T entity);
    void deleteAllById(Iterable<? extends ID> ids);
    void deleteAll(Iterable<? extends T> entities);
    void deleteAll();
}
```

### **3. PagingAndSortingRepository Interface**
```java
public interface PagingAndSortingRepository<T, ID> extends CrudRepository<T, ID> {
    Iterable<T> findAll(Sort sort);
    Page<T> findAll(Pageable pageable);
}
```

### **4. JpaRepository Interface**
```java
public interface JpaRepository<T, ID> extends PagingAndSortingRepository<T, ID>, QueryByExampleExecutor<T> {
    void flush();
    <S extends T> S saveAndFlush(S entity);
    <S extends T> List<S> saveAllAndFlush(Iterable<S> entities);
    void deleteAllInBatch(Iterable<T> entities);
    void deleteAllByIdInBatch(Iterable<ID> ids);
    void deleteAllInBatch();
    T getById(ID id);  // Returns reference (lazy)
    T getReferenceById(ID id);
    <S extends T> List<S> findAll(Example<S> example);
    <S extends T> List<S> findAll(Example<S> example, Sort sort);
}
```

**Usage Examples:**

```java
// Basic repository
public interface ProductRepository extends JpaRepository<Product, Long> {
    
    // Inherits all CRUD operations
    // save, findById, findAll, delete, etc.
    
    // Custom query methods
    List<Product> findByCategory(String category);
    List<Product> findByPriceBetween(BigDecimal minPrice, BigDecimal maxPrice);
    
    // Pagination example
    Page<Product> findByCategory(String category, Pageable pageable);
    
    // Sorting example
    List<Product> findByCategory(String category, Sort sort);
}

@Service
public class ProductService {
    
    @Autowired
    private ProductRepository productRepository;
    
    public Page<Product> getProducts(int page, int size, String sortBy) {
        Pageable pageable = PageRequest.of(page, size, Sort.by(sortBy));
        return productRepository.findAll(pageable);
    }
    
    public List<Product> getProductsByCategory(String category) {
        Sort sort = Sort.by(Sort.Direction.ASC, "name");
        return productRepository.findByCategory(category, sort);
    }
}
```

---

## **3. How do query methods work in Spring Data JPA? Explain naming conventions.**

**Answer:**

Spring Data JPA automatically generates queries based on method names using specific keywords and patterns.

**Query Method Keywords:**

| Keyword | Example | Generated JPQL |
|---------|---------|---------------|
| **find...By** | findByEmail | SELECT ... WHERE email = ? |
| **count...By** | countByStatus | SELECT count(...) WHERE status = ? |
| **delete...By** | deleteByEmail | DELETE WHERE email = ? |
| **...And...** | findByFirstNameAndLastName | WHERE firstName = ? AND lastName = ? |
| **...Or...** | findByEmailOrPhone | WHERE email = ? OR phone = ? |
| **...GreaterThan** | findByAgeGreaterThan | WHERE age > ? |
| **...LessThan** | findByAgeLessThan | WHERE age < ? |
| **...Between** | findByAgeBetween | WHERE age BETWEEN ? AND ? |
| **...Like** | findByNameLike | WHERE name LIKE ? |
| **...In** | findByStatusIn | WHERE status IN (?) |
| **...IsNull** | findByPhoneIsNull | WHERE phone IS NULL |
| **...IsNotNull** | findByPhoneIsNotNull | WHERE phone IS NOT NULL |
| **...OrderBy** | findByAgeOrderByNameAsc | WHERE age = ? ORDER BY name ASC |

**Comprehensive Examples:**

```java
public interface UserRepository extends JpaRepository<User, Long> {
    
    // Basic queries
    Optional<User> findByEmail(String email);
    List<User> findByFirstName(String firstName);
    
    // Multiple conditions
    List<User> findByFirstNameAndLastName(String firstName, String lastName);
    List<User> findByEmailOrPhone(String email, String phone);
    
    // Comparison operations
    List<User> findByAgeGreaterThan(int age);
    List<User> findByAgeGreaterThanEqual(int age);
    List<User> findByAgeLessThan(int age);
    List<User> findByAgeBetween(int minAge, int maxAge);
    
    // String operations
    List<User> findByFirstNameContaining(String name);
    List<User> findByFirstNameStartingWith(String prefix);
    List<User> findByFirstNameEndingWith(String suffix);
    List<User> findByEmailIgnoreCase(String email);
    
    // Collection operations
    List<User> findByAgeIn(Collection<Integer> ages);
    List<User> findByAgeNotIn(Collection<Integer> ages);
    
    // Null checks
    List<User> findByPhoneIsNull();
    List<User> findByPhoneIsNotNull();
    
    // Boolean operations
    List<User> findByActiveTrue();
    List<User> findByActiveFalse();
    List<User> findByActive(boolean active);
    
    // Ordering
    List<User> findByDepartmentOrderByLastNameAsc(String department);
    List<User> findByActiveOrderByCreatedDateDesc(boolean active);
    
    // Counting
    long countByActive(boolean active);
    long countByDepartment(String department);
    
    // Existence check
    boolean existsByEmail(String email);
    
    // Deletion
    void deleteByEmail(String email);
    long deleteByActiveIsFalse();
    
    // First/Top results
    Optional<User> findFirstByOrderByCreatedDateDesc();
    List<User> findTop10ByActiveOrderByCreatedDateDesc(boolean active);
    
    // Distinct results
    List<User> findDistinctByDepartment(String department);
}
```

**Complex Method Examples:**
```java
public interface OrderRepository extends JpaRepository<Order, Long> {
    
    // Complex query with multiple conditions and ordering
    List<Order> findByStatusAndCustomerEmailAndTotalGreaterThanOrderByCreatedDateDesc(
        OrderStatus status, String email, BigDecimal total);
    
    // Date range queries
    List<Order> findByCreatedDateBetween(LocalDateTime start, LocalDateTime end);
    
    // Nested property queries
    List<Order> findByCustomerAddressCity(String city);
    List<Order> findByCustomerAddressCityAndCustomerAddressState(String city, String state);
    
    // Collection size queries
    List<Order> findByOrderItemsIsEmpty();
    
    // Case insensitive queries
    List<Order> findByCustomerFirstNameContainingIgnoreCase(String name);
}
```

**Using with Pagination and Sorting:**
```java
public interface ProductRepository extends JpaRepository<Product, Long> {
    
    Page<Product> findByCategory(String category, Pageable pageable);
    Slice<Product> findByPriceGreaterThan(BigDecimal price, Pageable pageable);
    List<Product> findByActive(boolean active, Sort sort);
    
    // Custom pagination method
    @Query("SELECT p FROM Product p WHERE p.category = ?1")
    Page<Product> findProductsByCategory(String category, Pageable pageable);
}
```

---

## **4. What is the difference between save() and saveAndFlush()? When to use each?**

**Answer:**

**Key Differences:**

| Aspect | save() | saveAndFlush() |
|--------|--------|----------------|
| **Persistence Context** | Manages entity in memory | Immediately syncs to database |
| **Database Sync** | Deferred (at transaction commit) | Immediate |
| **Performance** | Better (batching possible) | Slower (immediate DB call) |
| **ID Generation** | May not be available immediately | ID available immediately |
| **Use Case** | Normal operations | Need immediate DB sync |

**Detailed Examples:**

### **save() Method:**
```java
@Service
@Transactional
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    public void createMultipleUsers(List<UserRequest> requests) {
        List<User> users = new ArrayList<>();
        
        for (UserRequest request : requests) {
            User user = new User(request.getName(), request.getEmail());
            User savedUser = userRepository.save(user); // Managed in persistence context
            
            // ID might not be available yet for sequence-based generation
            // Entity is in persistence context but not necessarily in database
            users.add(savedUser);
        }
        
        // All saves are batched and executed at transaction commit
        // Better performance for bulk operations
    }
}
```

### **saveAndFlush() Method:**
```java
@Service
@Transactional
public class OrderService {
    
    @Autowired
    private OrderRepository orderRepository;
    
    @Autowired
    private InventoryService inventoryService;
    
    public Order createOrder(OrderRequest request) {
        Order order = new Order(request.getCustomerId(), request.getItems());
        
        // Need immediate database sync for ID generation
        Order savedOrder = orderRepository.saveAndFlush(order);
        
        // ID is guaranteed to be available
        Long orderId = savedOrder.getId(); // This will have the generated ID
        
        // Use the ID for dependent operations
        inventoryService.reserveItems(orderId, request.getItems());
        
        return savedOrder;
    }
    
    public void processOrderWithAudit(OrderRequest request) {
        Order order = new Order(request.getCustomerId(), request.getItems());
        
        // Immediate flush needed for audit log
        Order savedOrder = orderRepository.saveAndFlush(order);
        
        // Create audit log entry with actual order ID
        auditService.logOrderCreation(savedOrder.getId(), savedOrder);
    }
}
```

**When to Use Each:**

### **Use save() when:**
```java
@Service
@Transactional
public class BulkUserService {
    
    public void importUsers(List<UserDto> userDtos) {
        List<User> users = userDtos.stream()
            .map(dto -> new User(dto.getName(), dto.getEmail()))
            .collect(Collectors.toList());
        
        // Efficient bulk save - all batched together
        userRepository.saveAll(users);
        
        // Or individually
        for (User user : users) {
            userRepository.save(user); // Batched at transaction end
        }
    }
}
```

### **Use saveAndFlush() when:**
```java
@Service
public class OrderProcessingService {
    
    @Transactional
    public void processOrderWithExternalSystem(OrderRequest request) {
        Order order = new Order(request.getCustomerId(), request.getItems());
        
        // Need ID immediately for external API call
        Order savedOrder = orderRepository.saveAndFlush(order);
        
        // External system requires the actual database ID
        PaymentResult result = paymentService.processPayment(
            savedOrder.getId(), 
            request.getPaymentDetails()
        );
        
        if (result.isSuccess()) {
            savedOrder.setPaymentConfirmed(true);
            orderRepository.save(savedOrder);
        }
    }
    
    @Transactional
    public void createOrderWithSequentialNumber() {
        Order order = new Order();
        
        // Need immediate flush to get the database-generated ID
        Order savedOrder = orderRepository.saveAndFlush(order);
        
        // Generate sequential number based on ID
        String orderNumber = "ORD-" + String.format("%06d", savedOrder.getId());
        savedOrder.setOrderNumber(orderNumber);
        
        orderRepository.save(savedOrder);
    }
}
```

**Performance Considerations:**
```java
@Service
public class PerformanceExampleService {
    
    // BAD - Multiple flushes
    @Transactional
    public void badBulkInsert(List<UserRequest> requests) {
        for (UserRequest request : requests) {
            User user = new User(request.getName(), request.getEmail());
            userRepository.saveAndFlush(user); // Each call hits database
        }
        // Results in N database calls
    }
    
    // GOOD - Batch operations
    @Transactional
    public void goodBulkInsert(List<UserRequest> requests) {
        List<User> users = requests.stream()
            .map(req -> new User(req.getName(), req.getEmail()))
            .collect(Collectors.toList());
        
        userRepository.saveAll(users); // Single batch operation
        // Results in optimized batch insert
    }
}
```

---

## **5. What is the N+1 query problem? How do you solve it?**

**Answer:**

**N+1 Problem** occurs when you execute 1 query to fetch N records, then N additional queries to fetch related data for each record.

**Example of N+1 Problem:**

### **Entity Setup:**
```java
@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    
    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
    private List<Order> orders;
    
    // getters and setters
}

@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private BigDecimal total;
    
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "user_id")
    private User user;
    
    // getters and setters
}
```

### **Problematic Code:**
```java
@Service
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    // This causes N+1 problem
    public List<UserOrderSummary> getUserOrderSummaries() {
        List<User> users = userRepository.findAll(); // 1 query
        
        List<UserOrderSummary> summaries = new ArrayList<>();
        for (User user : users) { // N iterations
            int orderCount = user.getOrders().size(); // +N queries (one per user)
            summaries.add(new UserOrderSummary(user.getName(), orderCount));
        }
        
        return summaries;
    }
}
```

**Generated SQL:**
```sql
-- Initial query (1)
SELECT * FROM users;

-- Then for each user (N queries)
SELECT * FROM orders WHERE user_id = 1;
SELECT * FROM orders WHERE user_id = 2;
SELECT * FROM orders WHERE user_id = 3;
-- ... and so on
```

---

## **Solutions to N+1 Problem:**

### **Solution 1: JOIN FETCH in JPQL**
```java
public interface UserRepository extends JpaRepository<User, Long> {
    
    @Query("SELECT u FROM User u JOIN FETCH u.orders")
    List<User> findAllWithOrders();
    
    @Query("SELECT DISTINCT u FROM User u LEFT JOIN FETCH u.orders WHERE u.active = true")
    List<User> findActiveUsersWithOrders();
}

@Service
public class UserService {
    
    public List<UserOrderSummary> getUserOrderSummaries() {
        List<User> users = userRepository.findAllWithOrders(); // Single query
        
        return users.stream()
            .map(user -> new UserOrderSummary(user.getName(), user.getOrders().size()))
            .collect(Collectors.toList());
    }
}
```

### **Solution 2: @EntityGraph**
```java
public interface UserRepository extends JpaRepository<User, Long> {
    
    @EntityGraph(attributePaths = {"orders"})
    List<User> findAll();
    
    @EntityGraph(attributePaths = {"orders", "orders.items"})
    @Query("SELECT u FROM User u WHERE u.department = ?1")
    List<User> findByDepartmentWithOrdersAndItems(String department);
}

// Named Entity Graph
@Entity
@NamedEntityGraph(
    name = "User.withOrders",
    attributeNodes = @NamedAttributeNode("orders")
)
public class User {
    // ... entity definition
}

public interface UserRepository extends JpaRepository<User, Long> {
    
    @EntityGraph("User.withOrders")
    List<User> findByActive(boolean active);
}
```

### **Solution 3: Projection with DTO**
```java
// DTO for projection
public class UserOrderCount {
    private String userName;
    private Long orderCount;
    
    public UserOrderCount(String userName, Long orderCount) {
        this.userName = userName;
        this.orderCount = orderCount;
    }
    
    // getters and setters
}

public interface UserRepository extends JpaRepository<User, Long> {
    
    @Query("SELECT new com.example.dto.UserOrderCount(u.name, COUNT(o)) " +
           "FROM User u LEFT JOIN u.orders o GROUP BY u.id, u.name")
    List<UserOrderCount> findUserOrderCounts();
}
```

### **Solution 4: Batch Fetching**
```java
@Entity
public class User {
    @Id
    private Long id;
    private String name;
    
    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
    @BatchSize(size = 25) // Fetches in batches of 25
    private List<Order> orders;
}
```

### **Solution 5: Custom Repository with Batch Loading**
```java
@Repository
public class CustomUserRepository {
    
    @PersistenceContext
    private EntityManager entityManager;
    
    public List<User> findUsersWithOrdersBatched() {
        // First, get all users
        List<User> users = entityManager
            .createQuery("SELECT u FROM User u", User.class)
            .getResultList();
        
        // Extract user IDs
        List<Long> userIds = users.stream()
            .map(User::getId)
            .collect(Collectors.toList());
        
        // Batch load all orders
        Map<Long, List<Order>> ordersByUserId = entityManager
            .createQuery("SELECT o FROM Order o WHERE o.user.id IN :userIds", Order.class)
            .setParameter("userIds", userIds)
            .getResultStream()
            .collect(Collectors.groupingBy(order -> order.getUser().getId()));
        
        // Set orders for each user
        users.forEach(user -> 
            user.setOrders(ordersByUserId.getOrDefault(user.getId(), new ArrayList<>()))
        );
        
        return users;
    }
}
```

---

## **6. What are the different fetch strategies in JPA? When to use each?**

**Answer:**

**Fetch Strategies:**

| Strategy | When Loaded | Use Case | Performance Impact |
|----------|-------------|----------|-------------------|
| **EAGER** | Immediately with parent | Always need related data | Higher memory, fewer queries |
| **LAZY** | On first access | Sometimes need related data | Lower memory, potential N+1 |

### **EAGER Loading:**
```java
@Entity
public class User {
    @Id
    private Long id;
    
    @OneToOne(fetch = FetchType.EAGER)
    private UserProfile profile; // Always loaded
    
    @ManyToOne(fetch = FetchType.EAGER)
    private Department department; // Always loaded
}

// Usage
User user = userRepository.findById(1L).get();
// profile and department are already loaded - no additional queries
String profileBio = user.getProfile().getBio();
```

### **LAZY Loading:**
```java
@Entity
public class User {
    @Id
    private Long id;
    
    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
    private List<Order> orders; // Loaded only when accessed
    
    @OneToMany(mappedBy = "user", fetch = FetchType.LAZY)
    private List<Address> addresses; // Loaded only when accessed
}

// Usage
@Transactional
public void processUser(Long userId) {
    User user = userRepository.findById(userId).get(); // Only user data loaded
    
    if (needsOrderProcessing) {
        List<Order> orders = user.getOrders(); // Additional query executed here
    }
}
```

### **Default Fetch Types:**
```java
@Entity
public class User {
    // ToOne relationships are EAGER by default
    @ManyToOne // Default: FetchType.EAGER
    private Department department;
    
    @OneToOne // Default: FetchType.EAGER
    private UserProfile profile;
    
    // ToMany relationships are LAZY by default
    @OneToMany(mappedBy = "user") // Default: FetchType.LAZY
    private List<Order> orders;
    
    @ManyToMany // Default: FetchType.LAZY
    private List<Role> roles;
}
```

### **Best Practices and Advanced Scenarios:**

```java
@Entity
public class Order {
    @Id
    private Long id;
    
    // EAGER for critical data always needed
    @ManyToOne(fetch = FetchType.EAGER)
    private Customer customer;
    
    // LAZY for optional data
    @OneToMany(mappedBy = "order", fetch = FetchType.LAZY)
    private List<OrderItem> items;
    
    // LAZY for audit data rarely accessed
    @OneToMany(mappedBy = "order", fetch = FetchType.LAZY)
    private List<OrderStatusHistory> statusHistory;
}

@Service
public class OrderService {
    
    // Method 1: Load order with specific related data
    @Transactional(readOnly = true)
    public OrderDTO getOrderSummary(Long orderId) {
        Order order = orderRepository.findById(orderId).get();
        // Customer is already loaded (EAGER)
        // Items will be loaded only if accessed
        
        return new OrderDTO(
            order.getId(),
            order.getCustomer().getName(), // No additional query
            order.getItems().size() // Additional query executed here
        );
    }
    
    // Method 2: Use specific fetch strategy for this query
    @Transactional(readOnly = true)
    public OrderDTO getOrderWithItems(Long orderId) {
        Order order = orderRepository.findByIdWithItems(orderId);
        // Both customer and items loaded in single query
        
        return new OrderDTO(
            order.getId(),
            order.getCustomer().getName(),
            order.getItems().size() // No additional query
        );
    }
}

public interface OrderRepository extends JpaRepository<Order, Long> {
    
    @Query("SELECT o FROM Order o JOIN FETCH o.items WHERE o.id = ?1")
    Optional<Order> findByIdWithItems(Long id);
    
    @EntityGraph(attributePaths = {"customer", "items", "items.product"})
    Optional<Order> findDetailedById(Long id);
}
```

### **Performance Considerations:**
```java
// BAD: EAGER loading everything
@Entity
public class User {
    @OneToMany(fetch = FetchType.EAGER) // Loads all orders always
    private List<Order> orders;
    
    @OneToMany(fetch = FetchType.EAGER) // Loads all addresses always
    private List<Address> addresses;
    
    @OneToMany(fetch = FetchType.EAGER) // Loads all preferences always
    private List<UserPreference> preferences;
}

// GOOD: LAZY by default, EAGER only when needed
@Entity
public class User {
    @OneToMany(fetch = FetchType.LAZY) // Load only when needed
    private List<Order> orders;
    
    @OneToMany(fetch = FetchType.LAZY)
    private List<Address> addresses;
    
    @OneToMany(fetch = FetchType.LAZY)
    private List<UserPreference> preferences;
}

// Use specific queries when you need related data
public interface UserRepository extends JpaRepository<User, Long> {
    
    @EntityGraph(attributePaths = {"orders"})
    Optional<User> findByIdWithOrders(Long id);
    
    @EntityGraph(attributePaths = {"addresses"})
    Optional<User> findByIdWithAddresses(Long id);
    
    @EntityGraph(attributePaths = {"orders", "addresses", "preferences"})
    Optional<User> findByIdWithAllRelations(Long id);
}
```

---

## **🎯 Quick Review Questions**

1. **What is the difference between findById() and getById()?**
   - `findById()` returns Optional, loads eagerly
   - `getById()` returns proxy/reference, loads lazily

2. **How do you handle transactions in Spring Data JPA?**
   - Use `@Transactional` annotation on service methods

3. **What is the difference between @Modifying and regular @Query?**
   - `@Modifying` is used for UPDATE/DELETE queries

4. **How do you implement soft delete in Spring Data JPA?**
   - Use `@Where` annotation or custom repository methods

5. **What is the purpose of @Version annotation?**
   - Implements optimistic locking for concurrent updates

---

**🚀 Pro Tips for Interview Success:**
- Understand the N+1 problem and its solutions deeply
- Know when to use different fetch strategies
- Practice writing complex queries with JOIN FETCH
- Understand the repository hierarchy
- Be familiar with performance optimization techniques
