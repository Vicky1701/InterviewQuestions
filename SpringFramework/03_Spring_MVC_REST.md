# Spring MVC & REST API - Interview Questions & Answers

## 🎯 **Essential Questions for 2+ Years Experience**

---

## **1. What is Spring MVC? Explain its architecture and request flow.**

**Answer:**
Spring MVC is a web framework based on Model-View-Controller pattern that separates application logic, data, and presentation.

**MVC Architecture Components:**

1. **Model**: Data and business logic
2. **View**: Presentation layer (JSP, Thymeleaf, JSON)
3. **Controller**: Handles requests and coordinates between Model and View

**Detailed Request Flow:**

```
1. Client Request → DispatcherServlet
2. DispatcherServlet → HandlerMapping (find controller)
3. HandlerMapping → DispatcherServlet (return controller)
4. DispatcherServlet → Controller (execute business logic)
5. Controller → Model (process data)
6. Controller → DispatcherServlet (return ModelAndView)
7. DispatcherServlet → ViewResolver (resolve view name)
8. ViewResolver → DispatcherServlet (return View object)
9. DispatcherServlet → View (render response)
10. View → Client (HTTP response)
```

**Example Implementation:**
```java
@Controller
@RequestMapping("/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping
    public String listUsers(Model model) {
        List<User> users = userService.getAllUsers();
        model.addAttribute("users", users);
        return "users"; // View name
    }
    
    @GetMapping("/{id}")
    public String getUser(@PathVariable Long id, Model model) {
        User user = userService.findById(id);
        model.addAttribute("user", user);
        return "user-detail";
    }
}
```

**Key Components Explained:**

**DispatcherServlet Configuration:**
```java
@Configuration
@EnableWebMvc
@ComponentScan(basePackages = "com.example.controller")
public class WebConfig implements WebMvcConfigurer {
    
    @Bean
    public ViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        return resolver;
    }
}
```

---

## **2. What is the difference between @Controller and @RestController?**

**Answer:**

| Aspect | @Controller | @RestController |
|--------|-------------|-----------------|
| **Purpose** | Traditional MVC controller | RESTful web services |
| **Response** | Returns view name | Returns data directly |
| **Content Type** | HTML pages | JSON/XML data |
| **Annotation Combination** | @Controller only | @Controller + @ResponseBody |
| **Method Return** | String (view name) | Object (serialized to JSON) |

**@Controller Example:**
```java
@Controller
@RequestMapping("/web/users")
public class UserWebController {
    
    @GetMapping
    public String listUsers(Model model) {
        List<User> users = userService.getAllUsers();
        model.addAttribute("users", users);
        return "users"; // Returns view name
    }
    
    @PostMapping
    public String createUser(@ModelAttribute User user, RedirectAttributes redirectAttributes) {
        userService.save(user);
        redirectAttributes.addFlashAttribute("message", "User created successfully");
        return "redirect:/web/users";
    }
}
```

**@RestController Example:**
```java
@RestController
@RequestMapping("/api/users")
public class UserRestController {
    
    @GetMapping
    public List<User> listUsers() {
        return userService.getAllUsers(); // Returns JSON data
    }
    
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User savedUser = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
    }
}
```

**Mixed Approach:**
```java
@Controller
@RequestMapping("/users")
public class UserController {
    
    @GetMapping
    public String listUsersPage(Model model) {
        return "users"; // Returns view
    }
    
    @GetMapping("/api")
    @ResponseBody
    public List<User> listUsersApi() {
        return userService.getAllUsers(); // Returns JSON
    }
}
```

---

## **3. What are the different HTTP methods and when to use them?**

**Answer:**

**REST HTTP Methods:**

| Method | Purpose | Idempotent | Safe | Use Case |
|--------|---------|------------|------|----------|
| **GET** | Retrieve data | ✅ | ✅ | Fetch user details |
| **POST** | Create new resource | ❌ | ❌ | Create new user |
| **PUT** | Update/Replace resource | ✅ | ❌ | Update user completely |
| **PATCH** | Partial update | ❌ | ❌ | Update user email only |
| **DELETE** | Remove resource | ✅ | ❌ | Delete user |

**Implementation Examples:**

```java
@RestController
@RequestMapping("/api/users")
public class UserRestController {
    
    // GET - Retrieve all users
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "10") int size) {
        Page<User> users = userService.getUsers(page, size);
        return ResponseEntity.ok(users.getContent());
    }
    
    // GET - Retrieve specific user
    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        return ResponseEntity.ok(user);
    }
    
    // POST - Create new user
    @PostMapping
    public ResponseEntity<User> createUser(@Valid @RequestBody UserCreateRequest request) {
        User user = userService.createUser(request);
        URI location = ServletUriComponentsBuilder
            .fromCurrentRequest()
            .path("/{id}")
            .buildAndExpand(user.getId())
            .toUri();
        return ResponseEntity.created(location).body(user);
    }
    
    // PUT - Complete update
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, 
                                          @Valid @RequestBody UserUpdateRequest request) {
        User user = userService.updateUser(id, request);
        return ResponseEntity.ok(user);
    }
    
    // PATCH - Partial update
    @PatchMapping("/{id}")
    public ResponseEntity<User> patchUser(@PathVariable Long id, 
                                         @RequestBody Map<String, Object> updates) {
        User user = userService.patchUser(id, updates);
        return ResponseEntity.ok(user);
    }
    
    // DELETE - Remove user
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
        return ResponseEntity.noContent().build();
    }
}
```

**Idempotent vs Non-Idempotent:**
```java
// Idempotent - Multiple calls have same effect
@PutMapping("/{id}")
public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User user) {
    user.setId(id);
    User updated = userService.save(user); // Always same result
    return ResponseEntity.ok(updated);
}

// Non-Idempotent - Multiple calls have different effects
@PostMapping
public ResponseEntity<User> createUser(@RequestBody User user) {
    User created = userService.save(user); // Creates new user each time
    return ResponseEntity.status(HttpStatus.CREATED).body(created);
}
```

---

## **4. What is @PathVariable and @RequestParam? What's the difference?**

**Answer:**

**@PathVariable** - Extracts values from URI path
**@RequestParam** - Extracts values from query parameters

**Comparison:**

| Aspect | @PathVariable | @RequestParam |
|--------|---------------|---------------|
| **Location** | URI path | Query string |
| **Required** | Required by default | Optional by default |
| **Example URL** | `/users/123` | `/users?id=123` |
| **Use Case** | Resource identification | Filtering, pagination |

**Examples:**

```java
@RestController
@RequestMapping("/api")
public class ApiController {
    
    // @PathVariable Examples
    @GetMapping("/users/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @GetMapping("/users/{userId}/orders/{orderId}")
    public Order getUserOrder(@PathVariable Long userId, 
                             @PathVariable Long orderId) {
        return orderService.findByUserAndOrder(userId, orderId);
    }
    
    // Optional PathVariable
    @GetMapping({"/users", "/users/{id}"})
    public ResponseEntity<?> getUsers(@PathVariable(required = false) Long id) {
        if (id != null) {
            return ResponseEntity.ok(userService.findById(id));
        }
        return ResponseEntity.ok(userService.findAll());
    }
    
    // @RequestParam Examples
    @GetMapping("/users")
    public List<User> getUsers(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "10") int size,
            @RequestParam(required = false) String name,
            @RequestParam(required = false) String email) {
        
        return userService.findUsers(page, size, name, email);
    }
    
    // RequestParam with Map
    @GetMapping("/search")
    public List<User> searchUsers(@RequestParam Map<String, String> params) {
        return userService.searchUsers(params);
    }
    
    // Multiple values
    @GetMapping("/users/bulk")
    public List<User> getMultipleUsers(@RequestParam List<Long> ids) {
        return userService.findAllById(ids);
    }
    
    // Custom parameter name
    @GetMapping("/users/by-email")
    public User getUserByEmail(@RequestParam("email-address") String email) {
        return userService.findByEmail(email);
    }
}
```

**Advanced Usage:**

```java
@RestController
public class ProductController {
    
    // Regex validation for PathVariable
    @GetMapping("/products/{code:[A-Z]{3}\\d{3}}")
    public Product getProductByCode(@PathVariable String code) {
        return productService.findByCode(code);
    }
    
    // Date parsing
    @GetMapping("/orders/{date}")
    public List<Order> getOrdersByDate(
            @PathVariable @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate date) {
        return orderService.findByDate(date);
    }
    
    // Enum PathVariable
    @GetMapping("/products/category/{category}")
    public List<Product> getProductsByCategory(@PathVariable ProductCategory category) {
        return productService.findByCategory(category);
    }
}
```

---

## **5. How do you handle exceptions in Spring REST API?**

**Answer:**
Spring provides multiple ways to handle exceptions in REST APIs:

**1. @ExceptionHandler (Controller Level):**
```java
@RestController
public class UserController {
    
    @GetMapping("/users/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id); // May throw UserNotFoundException
    }
    
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleUserNotFound(UserNotFoundException ex) {
        ErrorResponse error = new ErrorResponse(
            "USER_NOT_FOUND",
            ex.getMessage(),
            System.currentTimeMillis()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
}
```

**2. @ControllerAdvice (Global Exception Handling):**
```java
@ControllerAdvice
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);
    
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleUserNotFound(UserNotFoundException ex) {
        logger.warn("User not found: {}", ex.getMessage());
        
        ErrorResponse error = ErrorResponse.builder()
            .errorCode("USER_NOT_FOUND")
            .message(ex.getMessage())
            .timestamp(Instant.now())
            .build();
            
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    @ExceptionHandler(ValidationException.class)
    public ResponseEntity<ErrorResponse> handleValidation(ValidationException ex) {
        ErrorResponse error = ErrorResponse.builder()
            .errorCode("VALIDATION_ERROR")
            .message(ex.getMessage())
            .timestamp(Instant.now())
            .build();
            
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
    }
    
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ValidationErrorResponse> handleValidationErrors(
            MethodArgumentNotValidException ex) {
        
        List<FieldError> fieldErrors = ex.getBindingResult().getFieldErrors();
        Map<String, String> errors = fieldErrors.stream()
            .collect(Collectors.toMap(
                FieldError::getField,
                FieldError::getDefaultMessage,
                (existing, replacement) -> existing
            ));
        
        ValidationErrorResponse errorResponse = ValidationErrorResponse.builder()
            .errorCode("VALIDATION_FAILED")
            .message("Request validation failed")
            .fieldErrors(errors)
            .timestamp(Instant.now())
            .build();
            
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(errorResponse);
    }
    
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGenericException(Exception ex) {
        logger.error("Unexpected error occurred", ex);
        
        ErrorResponse error = ErrorResponse.builder()
            .errorCode("INTERNAL_SERVER_ERROR")
            .message("An unexpected error occurred")
            .timestamp(Instant.now())
            .build();
            
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}
```

**3. Custom Error Response Classes:**
```java
@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class ErrorResponse {
    private String errorCode;
    private String message;
    private Instant timestamp;
}

@Data
@Builder
@AllArgsConstructor
@NoArgsConstructor
public class ValidationErrorResponse {
    private String errorCode;
    private String message;
    private Map<String, String> fieldErrors;
    private Instant timestamp;
}
```

**4. Custom Exceptions:**
```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(Long userId) {
        super("User not found with id: " + userId);
    }
}

@ResponseStatus(HttpStatus.CONFLICT)
public class UserAlreadyExistsException extends RuntimeException {
    public UserAlreadyExistsException(String email) {
        super("User already exists with email: " + email);
    }
}
```

**5. ResponseEntityExceptionHandler:**
```java
@RestControllerAdvice
public class CustomExceptionHandler extends ResponseEntityExceptionHandler {
    
    @Override
    protected ResponseEntity<Object> handleMethodArgumentNotValid(
            MethodArgumentNotValidException ex,
            HttpHeaders headers,
            HttpStatus status,
            WebRequest request) {
        
        // Custom validation error handling
        return super.handleMethodArgumentNotValid(ex, headers, status, request);
    }
    
    @Override
    protected ResponseEntity<Object> handleHttpMessageNotReadable(
            HttpMessageNotReadableException ex,
            HttpHeaders headers,
            HttpStatus status,
            WebRequest request) {
        
        ErrorResponse error = ErrorResponse.builder()
            .errorCode("MALFORMED_JSON")
            .message("Request body contains invalid JSON")
            .timestamp(Instant.now())
            .build();
            
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
    }
}
```

---

## **6. What are HTTP Status Codes? Explain important ones for REST APIs.**

**Answer:**

**HTTP Status Code Categories:**

| Category | Range | Meaning |
|----------|-------|---------|
| **1xx** | 100-199 | Informational |
| **2xx** | 200-299 | Success |
| **3xx** | 300-399 | Redirection |
| **4xx** | 400-499 | Client Error |
| **5xx** | 500-599 | Server Error |

**Important Status Codes for REST APIs:**

**2xx Success Codes:**
```java
@RestController
public class UserController {
    
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        return ResponseEntity.ok(user); // 200 OK
    }
    
    @PostMapping("/users")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User created = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(created); // 201 Created
    }
    
    @PutMapping("/users/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User user) {
        User updated = userService.update(id, user);
        return ResponseEntity.ok(updated); // 200 OK
    }
    
    @DeleteMapping("/users/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build(); // 204 No Content
    }
}
```

**4xx Client Error Codes:**
```java
@RestControllerAdvice
public class ErrorHandler {
    
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleNotFound(UserNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND) // 404 Not Found
            .body(new ErrorResponse("User not found"));
    }
    
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidation(MethodArgumentNotValidException ex) {
        return ResponseEntity.status(HttpStatus.BAD_REQUEST) // 400 Bad Request
            .body(new ErrorResponse("Validation failed"));
    }
    
    @ExceptionHandler(AccessDeniedException.class)
    public ResponseEntity<ErrorResponse> handleAccessDenied(AccessDeniedException ex) {
        return ResponseEntity.status(HttpStatus.FORBIDDEN) // 403 Forbidden
            .body(new ErrorResponse("Access denied"));
    }
    
    @ExceptionHandler(AuthenticationException.class)
    public ResponseEntity<ErrorResponse> handleAuth(AuthenticationException ex) {
        return ResponseEntity.status(HttpStatus.UNAUTHORIZED) // 401 Unauthorized
            .body(new ErrorResponse("Authentication required"));
    }
}
```

**Status Code Guide:**

| Code | Name | When to Use | Example |
|------|------|-------------|---------|
| **200** | OK | Successful GET, PUT | Retrieve user data |
| **201** | Created | Successful POST | User created |
| **204** | No Content | Successful DELETE | User deleted |
| **400** | Bad Request | Invalid request data | Missing required field |
| **401** | Unauthorized | Authentication required | Invalid token |
| **403** | Forbidden | Access denied | Insufficient permissions |
| **404** | Not Found | Resource doesn't exist | User ID not found |
| **409** | Conflict | Resource already exists | Email already registered |
| **422** | Unprocessable Entity | Validation error | Invalid email format |
| **500** | Internal Server Error | Server-side error | Database connection failed |

---

## **7. What is @RequestBody and @ResponseBody? How do they work?**

**Answer:**

**@RequestBody** - Converts HTTP request body to Java object
**@ResponseBody** - Converts Java object to HTTP response body

**How They Work:**
- Use **HttpMessageConverters** for serialization/deserialization
- Default converter: **MappingJackson2HttpMessageConverter** for JSON

**@RequestBody Examples:**
```java
@RestController
public class UserController {
    
    @PostMapping("/users")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        // HTTP JSON request body automatically converted to User object
        User savedUser = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
    }
    
    @PostMapping("/users/validate")
    public ResponseEntity<User> createUserWithValidation(@Valid @RequestBody UserCreateRequest request) {
        // Validation annotations are applied
        User user = userService.createUser(request);
        return ResponseEntity.ok(user);
    }
    
    @PutMapping("/users/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, 
                                          @RequestBody Map<String, Object> updates) {
        // Can bind to Map for partial updates
        User user = userService.updateUser(id, updates);
        return ResponseEntity.ok(user);
    }
}
```

**@ResponseBody Examples:**
```java
@Controller // Note: @Controller, not @RestController
public class UserController {
    
    @GetMapping("/users/{id}")
    @ResponseBody
    public User getUser(@PathVariable Long id) {
        return userService.findById(id); // Converted to JSON automatically
    }
    
    @GetMapping("/users")
    @ResponseBody
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    // Without @ResponseBody (returns view name)
    @GetMapping("/users/page")
    public String getUsersPage(Model model) {
        model.addAttribute("users", userService.findAll());
        return "users"; // Returns view name, not data
    }
}
```

**Custom Request/Response DTOs:**
```java
// Request DTO
@Data
@NoArgsConstructor
@AllArgsConstructor
public class UserCreateRequest {
    @NotBlank(message = "Name is required")
    private String name;
    
    @Email(message = "Invalid email format")
    @NotBlank(message = "Email is required")
    private String email;
    
    @Size(min = 8, message = "Password must be at least 8 characters")
    private String password;
}

// Response DTO
@Data
@Builder
public class UserResponse {
    private Long id;
    private String name;
    private String email;
    private LocalDateTime createdAt;
    private boolean active;
}

@RestController
public class UserController {
    
    @PostMapping("/users")
    public ResponseEntity<UserResponse> createUser(@Valid @RequestBody UserCreateRequest request) {
        User user = userService.createUser(request);
        
        UserResponse response = UserResponse.builder()
            .id(user.getId())
            .name(user.getName())
            .email(user.getEmail())
            .createdAt(user.getCreatedAt())
            .active(user.isActive())
            .build();
            
        return ResponseEntity.status(HttpStatus.CREATED).body(response);
    }
}
```

**Custom Message Converters:**
```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
    
    @Override
    public void configureMessageConverters(List<HttpMessageConverter<?>> converters) {
        // Custom JSON converter
        MappingJackson2HttpMessageConverter jsonConverter = new MappingJackson2HttpMessageConverter();
        ObjectMapper objectMapper = new ObjectMapper();
        objectMapper.configure(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES, false);
        objectMapper.registerModule(new JavaTimeModule());
        jsonConverter.setObjectMapper(objectMapper);
        
        converters.add(jsonConverter);
    }
}
```

---

## **8. How do you implement Content Negotiation in Spring MVC?**

**Answer:**
Content Negotiation allows the same endpoint to return different representations (JSON, XML, etc.) based on client preferences.

**Configuration:**
```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {
    
    @Override
    public void configureContentNegotiation(ContentNegotiationConfigurer configurer) {
        configurer
            .favorParameter(true)
            .parameterName("mediaType")
            .favorPathExtension(false)
            .ignoreAcceptHeader(false)
            .useRegisteredExtensionsOnly(false)
            .defaultContentType(MediaType.APPLICATION_JSON)
            .mediaType("json", MediaType.APPLICATION_JSON)
            .mediaType("xml", MediaType.APPLICATION_XML);
    }
}
```

**Controller Implementation:**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(value = "/{id}", 
                produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        return ResponseEntity.ok(user);
    }
    
    @PostMapping(consumes = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE},
                 produces = {MediaType.APPLICATION_JSON_VALUE, MediaType.APPLICATION_XML_VALUE})
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User created = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(created);
    }
}
```

**Client Usage:**
```bash
# Request JSON
curl -H "Accept: application/json" http://localhost:8080/api/users/1

# Request XML
curl -H "Accept: application/xml" http://localhost:8080/api/users/1

# Using parameter
curl http://localhost:8080/api/users/1?mediaType=xml
```

---

## **🎯 Quick Review Questions**

1. **What is the purpose of DispatcherServlet?**
   - Front controller that handles all HTTP requests and routes them to appropriate controllers

2. **How do you handle file uploads in Spring MVC?**
   - Use `@RequestParam MultipartFile` or `MultipartHttpServletRequest`

3. **What is @ModelAttribute used for?**
   - Binds request parameters to model objects, adds attributes to model

4. **How do you implement CORS in Spring?**
   - Use `@CrossOrigin` annotation or configure global CORS

5. **What is the difference between @RequestMapping and @GetMapping?**
   - `@GetMapping` is specialized version of `@RequestMapping(method = RequestMethod.GET)`

---

**🚀 Pro Tips for Interview Success:**
- Understand the complete request-response flow
- Know HTTP status codes and when to use them
- Practice exception handling patterns
- Understand content negotiation concepts
- Be familiar with validation and data binding
