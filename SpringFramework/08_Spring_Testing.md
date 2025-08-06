# Spring Testing - Interview Questions & Answers

## 🎯 **Essential Questions for 2+ Years Experience**

---

## **1. What are the different types of testing in Spring Boot? Explain each.**

**Answer:**

Spring Boot provides specialized test annotations for different layers of testing:

### **Testing Pyramid:**

| Test Type | Scope | Speed | Cost | Tools |
|-----------|-------|-------|------|-------|
| **Unit Tests** | Individual components | Fast | Low | JUnit, Mockito |
| **Integration Tests** | Multiple components | Medium | Medium | @SpringBootTest |
| **End-to-End Tests** | Full application | Slow | High | TestContainers, REST Assured |

### **Spring Boot Test Annotations:**

#### **1. @SpringBootTest - Full Integration Test**
```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@TestPropertySource(locations = "classpath:test.properties")
class FullIntegrationTest {
    
    @Autowired
    private TestRestTemplate restTemplate;
    
    @LocalServerPort
    private int port;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldCreateUserEndToEnd() {
        // Given
        UserCreateRequest request = UserCreateRequest.builder()
            .username("john.doe")
            .email("john@example.com")
            .build();
        
        // When
        ResponseEntity<UserResponse> response = restTemplate.postForEntity(
            "http://localhost:" + port + "/api/users",
            request,
            UserResponse.class
        );
        
        // Then
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.CREATED);
        assertThat(response.getBody().getUsername()).isEqualTo("john.doe");
        
        // Verify in database
        Optional<User> savedUser = userRepository.findByUsername("john.doe");
        assertThat(savedUser).isPresent();
    }
}
```

#### **2. @WebMvcTest - Web Layer Test**
```java
@WebMvcTest(UserController.class)
class UserControllerTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Autowired
    private ObjectMapper objectMapper;
    
    @Test
    void shouldCreateUser() throws Exception {
        // Given
        UserCreateRequest request = new UserCreateRequest("john.doe", "john@example.com");
        User user = User.builder().id(1L).username("john.doe").email("john@example.com").build();
        
        when(userService.createUser(any(UserCreateRequest.class))).thenReturn(user);
        
        // When & Then
        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(request)))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.username").value("john.doe"))
            .andExpect(jsonPath("$.email").value("john@example.com"))
            .andDo(print());
        
        verify(userService).createUser(any(UserCreateRequest.class));
    }
    
    @Test
    void shouldReturnValidationError() throws Exception {
        // Given - invalid request
        UserCreateRequest request = new UserCreateRequest("", "invalid-email");
        
        // When & Then
        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content(objectMapper.writeValueAsString(request)))
            .andExpect(status().isBadRequest())
            .andExpect(jsonPath("$.fieldErrors.username").exists())
            .andExpect(jsonPath("$.fieldErrors.email").exists());
    }
}
```

#### **3. @DataJpaTest - Repository Layer Test**
```java
@DataJpaTest
@TestPropertySource(properties = {
    "spring.jpa.hibernate.ddl-auto=create-drop"
})
class UserRepositoryTest {
    
    @Autowired
    private TestEntityManager entityManager;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldFindUserByEmail() {
        // Given
        User user = User.builder()
            .username("john.doe")
            .email("john@example.com")
            .password("password")
            .build();
        entityManager.persistAndFlush(user);
        
        // When
        Optional<User> found = userRepository.findByEmail("john@example.com");
        
        // Then
        assertThat(found).isPresent();
        assertThat(found.get().getUsername()).isEqualTo("john.doe");
    }
    
    @Test
    void shouldReturnEmptyWhenUserNotFound() {
        // When
        Optional<User> found = userRepository.findByEmail("nonexistent@example.com");
        
        // Then
        assertThat(found).isEmpty();
    }
    
    @Test
    void shouldSaveUserWithGeneratedId() {
        // Given
        User user = User.builder()
            .username("jane.doe")
            .email("jane@example.com")
            .password("password")
            .build();
        
        // When
        User saved = userRepository.save(user);
        
        // Then
        assertThat(saved.getId()).isNotNull();
        assertThat(saved.getUsername()).isEqualTo("jane.doe");
    }
}
```

#### **4. @JsonTest - JSON Serialization Test**
```java
@JsonTest
class UserJsonTest {
    
    @Autowired
    private JacksonTester<User> userJson;
    
    @Autowired
    private JacksonTester<UserResponse> userResponseJson;
    
    @Test
    void shouldSerializeUser() throws Exception {
        // Given
        User user = User.builder()
            .id(1L)
            .username("john.doe")
            .email("john@example.com")
            .createdAt(LocalDateTime.of(2023, 1, 1, 10, 0))
            .build();
        
        // When & Then
        assertThat(userJson.write(user))
            .hasJsonPathNumberValue("@.id")
            .extractingJsonPathNumberValue("@.id").isEqualTo(1);
        
        assertThat(userJson.write(user))
            .hasJsonPathStringValue("@.username")
            .extractingJsonPathStringValue("@.username").isEqualTo("john.doe");
        
        assertThat(userJson.write(user))
            .hasJsonPathStringValue("@.createdAt")
            .extractingJsonPathStringValue("@.createdAt").isEqualTo("2023-01-01T10:00:00");
    }
    
    @Test
    void shouldDeserializeUser() throws Exception {
        // Given
        String json = """
            {
                "id": 1,
                "username": "john.doe",
                "email": "john@example.com",
                "createdAt": "2023-01-01T10:00:00"
            }
            """;
        
        // When
        User user = userJson.parseObject(json);
        
        // Then
        assertThat(user.getId()).isEqualTo(1L);
        assertThat(user.getUsername()).isEqualTo("john.doe");
        assertThat(user.getEmail()).isEqualTo("john@example.com");
    }
}
```

---

## **2. How do you mock dependencies in Spring Boot tests?**

**Answer:**

Spring Boot provides several mechanisms for mocking dependencies:

### **1. @MockBean - Spring Context Mock**
```java
@SpringBootTest
class OrderServiceIntegrationTest {
    
    @MockBean
    private UserService userService;  // Replaces bean in Spring context
    
    @MockBean
    private PaymentService paymentService;
    
    @Autowired
    private OrderService orderService;  // Real bean with mocked dependencies
    
    @Test
    void shouldCreateOrderWithMockedDependencies() {
        // Given
        User user = User.builder().id(1L).username("john").build();
        PaymentResult paymentResult = PaymentResult.builder().status("SUCCESS").build();
        
        when(userService.findById(1L)).thenReturn(user);
        when(paymentService.processPayment(any())).thenReturn(paymentResult);
        
        OrderRequest request = new OrderRequest(1L, BigDecimal.valueOf(100));
        
        // When
        Order order = orderService.createOrder(request);
        
        // Then
        assertThat(order.getUserId()).isEqualTo(1L);
        assertThat(order.getStatus()).isEqualTo("CONFIRMED");
        
        verify(userService).findById(1L);
        verify(paymentService).processPayment(any());
    }
}
```

### **2. @Mock - Mockito Mock**
```java
@ExtendWith(MockitoExtension.class)
class UserServiceUnitTest {
    
    @Mock
    private UserRepository userRepository;
    
    @Mock
    private PasswordEncoder passwordEncoder;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    void shouldCreateUser() {
        // Given
        UserCreateRequest request = new UserCreateRequest("john", "john@example.com", "password");
        User savedUser = User.builder().id(1L).username("john").email("john@example.com").build();
        
        when(passwordEncoder.encode("password")).thenReturn("encoded-password");
        when(userRepository.save(any(User.class))).thenReturn(savedUser);
        
        // When
        User result = userService.createUser(request);
        
        // Then
        assertThat(result.getId()).isEqualTo(1L);
        assertThat(result.getUsername()).isEqualTo("john");
        
        ArgumentCaptor<User> userCaptor = ArgumentCaptor.forClass(User.class);
        verify(userRepository).save(userCaptor.capture());
        assertThat(userCaptor.getValue().getPassword()).isEqualTo("encoded-password");
    }
}
```

### **3. @Spy - Partial Mock**
```java
@ExtendWith(MockitoExtension.class)
class NotificationServiceTest {
    
    @Spy
    private EmailService emailService;  // Real object with some methods mocked
    
    @InjectMocks
    private NotificationService notificationService;
    
    @Test
    void shouldSendNotificationWithSpiedService() {
        // Given
        User user = User.builder().email("john@example.com").build();
        
        // Mock only specific method
        doReturn(true).when(emailService).isEmailValid(anyString());
        
        // When
        notificationService.sendWelcomeEmail(user);
        
        // Then
        verify(emailService).isEmailValid("john@example.com");
        verify(emailService).sendEmail(eq("john@example.com"), anyString(), anyString());
    }
}
```

### **4. Custom Test Configuration**
```java
@TestConfiguration
public class TestConfig {
    
    @Bean
    @Primary
    public UserService mockUserService() {
        return Mockito.mock(UserService.class);
    }
    
    @Bean
    @Primary
    public PaymentService mockPaymentService() {
        PaymentService mock = Mockito.mock(PaymentService.class);
        // Setup default behavior
        when(mock.processPayment(any())).thenReturn(
            PaymentResult.builder().status("SUCCESS").build()
        );
        return mock;
    }
}

@SpringBootTest
@Import(TestConfig.class)
class OrderServiceWithCustomConfigTest {
    
    @Autowired
    private UserService userService;  // Will be the mock
    
    @Autowired
    private OrderService orderService;
    
    @Test
    void shouldUseCustomMocks() {
        // Given
        when(userService.findById(1L)).thenReturn(
            User.builder().id(1L).username("john").build()
        );
        
        // When
        Order order = orderService.createOrder(new OrderRequest(1L, BigDecimal.valueOf(100)));
        
        // Then
        assertThat(order).isNotNull();
        verify(userService).findById(1L);
    }
}
```

---

## **3. How do you test REST APIs in Spring Boot?**

**Answer:**

### **1. Using MockMvc for Unit Testing**
```java
@WebMvcTest(UserController.class)
class UserControllerMockMvcTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @MockBean
    private UserService userService;
    
    @Test
    void shouldGetUser() throws Exception {
        // Given
        User user = User.builder().id(1L).username("john").email("john@example.com").build();
        when(userService.findById(1L)).thenReturn(user);
        
        // When & Then
        mockMvc.perform(get("/api/users/1")
                .accept(MediaType.APPLICATION_JSON))
            .andExpect(status().isOk())
            .andExpect(content().contentType(MediaType.APPLICATION_JSON))
            .andExpect(jsonPath("$.id").value(1))
            .andExpect(jsonPath("$.username").value("john"))
            .andExpect(jsonPath("$.email").value("john@example.com"))
            .andDo(print());
    }
    
    @Test
    void shouldCreateUser() throws Exception {
        // Given
        UserCreateRequest request = new UserCreateRequest("john", "john@example.com");
        User createdUser = User.builder().id(1L).username("john").email("john@example.com").build();
        
        when(userService.createUser(any(UserCreateRequest.class))).thenReturn(createdUser);
        
        // When & Then
        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("""
                    {
                        "username": "john",
                        "email": "john@example.com"
                    }
                    """))
            .andExpect(status().isCreated())
            .andExpect(header().exists("Location"))
            .andExpect(jsonPath("$.id").value(1))
            .andExpect(jsonPath("$.username").value("john"));
    }
    
    @Test
    void shouldHandleValidationErrors() throws Exception {
        mockMvc.perform(post("/api/users")
                .contentType(MediaType.APPLICATION_JSON)
                .content("""
                    {
                        "username": "",
                        "email": "invalid-email"
                    }
                    """))
            .andExpect(status().isBadRequest())
            .andExpected(jsonPath("$.message").value("Validation failed"))
            .andExpect(jsonPath("$.fieldErrors.username").exists())
            .andExpect(jsonPath("$.fieldErrors.email").exists());
    }
}
```

### **2. Using TestRestTemplate for Integration Testing**
```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@Transactional
class UserControllerIntegrationTest {
    
    @Autowired
    private TestRestTemplate restTemplate;
    
    @LocalServerPort
    private int port;
    
    @Autowired
    private UserRepository userRepository;
    
    private String getBaseUrl() {
        return "http://localhost:" + port + "/api/users";
    }
    
    @Test
    void shouldCreateAndRetrieveUser() {
        // Given
        UserCreateRequest request = new UserCreateRequest("john", "john@example.com");
        
        // When - Create user
        ResponseEntity<UserResponse> createResponse = restTemplate.postForEntity(
            getBaseUrl(),
            request,
            UserResponse.class
        );
        
        // Then - Verify creation
        assertThat(createResponse.getStatusCode()).isEqualTo(HttpStatus.CREATED);
        Long userId = createResponse.getBody().getId();
        
        // When - Retrieve user
        ResponseEntity<UserResponse> getResponse = restTemplate.getForEntity(
            getBaseUrl() + "/" + userId,
            UserResponse.class
        );
        
        // Then - Verify retrieval
        assertThat(getResponse.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(getResponse.getBody().getUsername()).isEqualTo("john");
        assertThat(getResponse.getBody().getEmail()).isEqualTo("john@example.com");
    }
    
    @Test
    void shouldUpdateUser() {
        // Given - Create user first
        User existingUser = userRepository.save(
            User.builder().username("john").email("john@example.com").build()
        );
        
        UserUpdateRequest updateRequest = new UserUpdateRequest("john.doe", "john.doe@example.com");
        
        // When
        restTemplate.put(getBaseUrl() + "/" + existingUser.getId(), updateRequest);
        
        // Then
        ResponseEntity<UserResponse> response = restTemplate.getForEntity(
            getBaseUrl() + "/" + existingUser.getId(),
            UserResponse.class
        );
        
        assertThat(response.getBody().getUsername()).isEqualTo("john.doe");
        assertThat(response.getBody().getEmail()).isEqualTo("john.doe@example.com");
    }
    
    @Test
    void shouldDeleteUser() {
        // Given
        User existingUser = userRepository.save(
            User.builder().username("john").email("john@example.com").build()
        );
        
        // When
        restTemplate.delete(getBaseUrl() + "/" + existingUser.getId());
        
        // Then
        ResponseEntity<String> response = restTemplate.getForEntity(
            getBaseUrl() + "/" + existingUser.getId(),
            String.class
        );
        
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.NOT_FOUND);
    }
}
```

### **3. Using WebTestClient (Reactive)**
```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
class UserControllerWebTestClientTest {
    
    @Autowired
    private WebTestClient webTestClient;
    
    @Test
    void shouldCreateUser() {
        UserCreateRequest request = new UserCreateRequest("john", "john@example.com");
        
        webTestClient.post()
            .uri("/api/users")
            .contentType(MediaType.APPLICATION_JSON)
            .bodyValue(request)
            .exchange()
            .expectStatus().isCreated()
            .expectHeader().exists("Location")
            .expectBody()
            .jsonPath("$.username").isEqualTo("john")
            .jsonPath("$.email").isEqualTo("john@example.com");
    }
    
    @Test
    void shouldGetAllUsers() {
        webTestClient.get()
            .uri("/api/users")
            .accept(MediaType.APPLICATION_JSON)
            .exchange()
            .expectStatus().isOk()
            .expectHeader().contentType(MediaType.APPLICATION_JSON)
            .expectBodyList(UserResponse.class)
            .hasSize(0);  // Assuming empty database
    }
}
```

---

## **4. How do you test database interactions in Spring Boot?**

**Answer:**

### **1. @DataJpaTest with H2 Database**
```java
@DataJpaTest
class UserRepositoryDataJpaTest {
    
    @Autowired
    private TestEntityManager entityManager;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldFindUsersByStatus() {
        // Given
        User activeUser = User.builder().username("active").status(UserStatus.ACTIVE).build();
        User inactiveUser = User.builder().username("inactive").status(UserStatus.INACTIVE).build();
        
        entityManager.persist(activeUser);
        entityManager.persist(inactiveUser);
        entityManager.flush();
        
        // When
        List<User> activeUsers = userRepository.findByStatus(UserStatus.ACTIVE);
        
        // Then
        assertThat(activeUsers).hasSize(1);
        assertThat(activeUsers.get(0).getUsername()).isEqualTo("active");
    }
    
    @Test
    void shouldExecuteCustomQuery() {
        // Given
        User user1 = User.builder().username("john").email("john@example.com").build();
        User user2 = User.builder().username("jane").email("jane@example.com").build();
        
        entityManager.persist(user1);
        entityManager.persist(user2);
        entityManager.flush();
        
        // When
        List<User> users = userRepository.findUsersWithEmailDomain("example.com");
        
        // Then
        assertThat(users).hasSize(2);
    }
}
```

### **2. Using @Sql for Test Data**
```java
@DataJpaTest
@Sql("/test-data.sql")  // Load test data before each test
class UserRepositoryWithSqlTest {
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldFindUsersFromSqlFile() {
        // Given - data loaded from test-data.sql
        
        // When
        List<User> users = userRepository.findAll();
        
        // Then
        assertThat(users).hasSizeGreaterThan(0);
    }
    
    @Test
    @Sql(scripts = "/additional-data.sql", executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD)
    @Sql(scripts = "/cleanup.sql", executionPhase = Sql.ExecutionPhase.AFTER_TEST_METHOD)
    void shouldExecuteScriptsBeforeAndAfter() {
        // Test logic here
    }
}
```

### **3. Using TestContainers for Real Database Testing**
```java
@SpringBootTest
@Testcontainers
@Transactional
class UserRepositoryTestContainersTest {
    
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:13")
        .withDatabaseName("testdb")
        .withUsername("test")
        .withPassword("test");
    
    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldWorkWithRealPostgresDatabase() {
        // Given
        User user = User.builder()
            .username("john")
            .email("john@example.com")
            .build();
        
        // When
        User saved = userRepository.save(user);
        
        // Then
        assertThat(saved.getId()).isNotNull();
        
        Optional<User> found = userRepository.findById(saved.getId());
        assertThat(found).isPresent();
    }
}
```

### **4. Service Layer Testing with Database**
```java
@SpringBootTest
@Transactional
class UserServiceIntegrationTest {
    
    @Autowired
    private UserService userService;
    
    @Autowired
    private UserRepository userRepository;
    
    @Test
    void shouldCreateUserWithGeneratedId() {
        // Given
        UserCreateRequest request = new UserCreateRequest("john", "john@example.com");
        
        // When
        User created = userService.createUser(request);
        
        // Then
        assertThat(created.getId()).isNotNull();
        assertThat(created.getUsername()).isEqualTo("john");
        
        // Verify in database
        Optional<User> found = userRepository.findById(created.getId());
        assertThat(found).isPresent();
    }
    
    @Test
    void shouldThrowExceptionForDuplicateEmail() {
        // Given
        userRepository.save(User.builder().username("existing").email("john@example.com").build());
        UserCreateRequest request = new UserCreateRequest("john", "john@example.com");
        
        // When & Then
        assertThatThrownBy(() -> userService.createUser(request))
            .isInstanceOf(DuplicateEmailException.class)
            .hasMessage("Email already exists: john@example.com");
    }
}
```

---

## **5. How do you test security in Spring Boot applications?**

**Answer:**

### **1. Testing with @WithMockUser**
```java
@SpringBootTest
@AutoConfigureTestDatabase
class SecurityIntegrationTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    void shouldDenyAccessWithoutAuthentication() throws Exception {
        mockMvc.perform(get("/api/admin/users"))
            .andExpect(status().isUnauthorized());
    }
    
    @Test
    @WithMockUser(username = "admin", roles = {"ADMIN"})
    void shouldAllowAccessWithAdminRole() throws Exception {
        mockMvc.perform(get("/api/admin/users"))
            .andExpect(status().isOk());
    }
    
    @Test
    @WithMockUser(username = "user", roles = {"USER"})
    void shouldDenyAccessWithUserRole() throws Exception {
        mockMvc.perform(get("/api/admin/users"))
            .andExpect(status().isForbidden());
    }
}
```

### **2. Custom Security Test Annotations**
```java
@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@WithMockUser(username = "admin", roles = {"ADMIN"})
public @interface WithMockAdmin {
}

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@WithMockUser(username = "user", roles = {"USER"})
public @interface WithMockRegularUser {
}

@SpringBootTest
class SecurityTestWithCustomAnnotations {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    @WithMockAdmin
    void shouldAllowAdminAccess() throws Exception {
        mockMvc.perform(get("/api/admin/users"))
            .andExpect(status().isOk());
    }
    
    @Test
    @WithMockRegularUser
    void shouldAllowUserAccess() throws Exception {
        mockMvc.perform(get("/api/users/profile"))
            .andExpected(status().isOk());
    }
}
```

### **3. Testing JWT Authentication**
```java
@SpringBootTest
class JwtAuthenticationTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Autowired
    private JwtTokenUtil jwtTokenUtil;
    
    @Test
    void shouldAuthenticateWithValidJwt() throws Exception {
        // Given
        UserDetails userDetails = User.withUsername("john")
            .password("password")
            .roles("USER")
            .build();
        String token = jwtTokenUtil.generateToken(userDetails);
        
        // When & Then
        mockMvc.perform(get("/api/users/profile")
                .header("Authorization", "Bearer " + token))
            .andExpected(status().isOk());
    }
    
    @Test
    void shouldRejectInvalidJwt() throws Exception {
        mockMvc.perform(get("/api/users/profile")
                .header("Authorization", "Bearer invalid-token"))
            .andExpect(status().isUnauthorized());
    }
}
```

---

## **🎯 Quick Review Questions**

1. **What is the difference between @Mock and @MockBean?**
   - `@Mock`: Mockito annotation for unit tests
   - `@MockBean`: Spring Boot annotation that replaces beans in Spring context

2. **When should you use @Transactional in tests?**
   - Use when you want automatic rollback after each test method

3. **How do you test asynchronous methods?**
   - Use `@Async` with `CompletableFuture` and `verify` with `timeout()`

4. **What is TestContainers and when to use it?**
   - Library for running real databases in Docker containers during tests

5. **How do you test exception handling in controllers?**
   - Use `andExpect(status().isBadRequest())` and verify error response structure

---

**🚀 Pro Tips for Interview Success:**
- Understand the testing pyramid and when to use each type
- Know the difference between unit and integration tests
- Practice writing tests for different layers
- Understand how to mock external dependencies
- Be familiar with test data management strategies
