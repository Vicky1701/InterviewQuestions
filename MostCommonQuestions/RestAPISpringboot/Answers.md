# REST API Java Interview Questions - Complete Answers

## 🔥 MUST-KNOW Questions (High Priority)

### REST Fundamentals

**Q1: What is REST? What are its principles?**

**Answer:** REST (Representational State Transfer) is an architectural style for designing web services. It's not a protocol but a set of guidelines.

**6 Key Principles:**
1. **Client-Server Architecture** - Separation of concerns
2. **Stateless** - Each request contains all information needed
3. **Cacheable** - Responses can be cached to improve performance
4. **Uniform Interface** - Consistent way to interact with resources
5. **Layered System** - Architecture can have multiple layers
6. **Code on Demand** (Optional) - Server can send executable code

**Q2: What are the HTTP methods used in REST? Explain each.**

**Answer:**
- **GET** - Retrieve data (Read operation) - Safe & Idempotent
- **POST** - Create new resource - Not idempotent
- **PUT** - Update/Replace entire resource - Idempotent
- **PATCH** - Partial update of resource - Not necessarily idempotent
- **DELETE** - Remove resource - Idempotent
- **HEAD** - Like GET but returns only headers
- **OPTIONS** - Get allowed methods for a resource

**Q3: What is the difference between REST and SOAP?**

**Answer:**
| REST | SOAP |
|------|------|
| Architectural style | Protocol |
| Uses HTTP methods | Uses XML messaging |
| Lightweight (JSON/XML) | Heavy (XML only) |
| Stateless | Can be stateful |
| Better performance | More overhead |
| Easy to implement | Complex implementation |

**Q4: What are HTTP status codes? Name important ones.**

**Answer:**
- **200 OK** - Successful GET, PUT, PATCH
- **201 Created** - Successful POST (resource created)
- **204 No Content** - Successful DELETE
- **400 Bad Request** - Invalid request syntax
- **401 Unauthorized** - Authentication required
- **403 Forbidden** - Access denied
- **404 Not Found** - Resource doesn't exist
- **500 Internal Server Error** - Server error

**Q5: What is JSON? Why is it preferred over XML in REST APIs?**

**Answer:** JSON (JavaScript Object Notation) is a lightweight data-interchange format.

**Why preferred over XML:**
- **Lightweight** - Less verbose, smaller payload
- **Faster parsing** - Native JavaScript support
- **Human readable** - Easy to read and write
- **Better performance** - Less bandwidth usage
- **Simple structure** - Key-value pairs, arrays, objects

**Q6: What is the difference between PUT and POST?**

**Answer:**
| PUT | POST |
|-----|------|
| Update/Replace resource | Create new resource |
| Idempotent (same result) | Not idempotent |
| Client knows resource ID | Server generates ID |
| `/users/123` | `/users` |
| Full resource update | Can be partial creation |

**Q7: What does it mean for REST to be stateless?**

**Answer:** Each HTTP request must contain all information needed to process it. The server doesn't store any client context between requests.

**Benefits:**
- **Scalability** - No server memory of previous requests
- **Reliability** - No session state to lose
- **Simplicity** - Each request is independent

**Example:** Every request must include authentication token, not stored in server session.

**Q8: What is a resource in REST? How do you design RESTful URLs?**

**Answer:** A resource is any information that can be named and accessed via URL.

**RESTful URL Design:**
- Use nouns, not verbs: `/users` not `/getUsers`
- Use plural nouns: `/users` not `/user`
- Hierarchical structure: `/users/123/orders/456`
- Use hyphens for readability: `/user-profiles`
- Keep URLs simple and predictable

**Examples:**
```
GET /users          - Get all users
GET /users/123      - Get user with ID 123
POST /users         - Create new user
PUT /users/123      - Update user 123
DELETE /users/123   - Delete user 123
```

## Spring Boot REST Basics

**Q9: How do you create a REST API in Spring Boot?**

**Answer:**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping
    public List<User> getAllUsers() {
        return userService.findAll();
    }
    
    @GetMapping("/{id}")
    public User getUserById(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
}
```

**Q10: What are @RestController and @Controller annotations?**

**Answer:**
- **@Controller** - Traditional MVC controller, returns view names
- **@RestController** - Combines @Controller + @ResponseBody, returns data directly

```java
@Controller
public class WebController {
    @RequestMapping("/home")
    public String home() {
        return "home"; // Returns view name
    }
}

@RestController
public class ApiController {
    @RequestMapping("/users")
    public List<User> users() {
        return userList; // Returns JSON data
    }
}
```

**Q11: Explain @RequestMapping, @GetMapping, @PostMapping annotations.**

**Answer:**
- **@RequestMapping** - Generic mapping for any HTTP method
- **@GetMapping** - Shortcut for @RequestMapping(method = GET)
- **@PostMapping** - Shortcut for @RequestMapping(method = POST)

```java
@RequestMapping(value = "/users", method = RequestMethod.GET)
// Same as
@GetMapping("/users")

@RequestMapping(value = "/users", method = RequestMethod.POST)
// Same as
@PostMapping("/users")
```

**Q12: What is @PathVariable and @RequestParam?**

**Answer:**
- **@PathVariable** - Extract values from URL path
- **@RequestParam** - Extract values from query parameters

```java
// PathVariable: /users/123
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) { }

// RequestParam: /users?page=1&size=10
@GetMapping("/users")
public List<User> getUsers(
    @RequestParam int page,
    @RequestParam int size) { }

// Optional RequestParam
@GetMapping("/users")
public List<User> getUsers(
    @RequestParam(defaultValue = "0") int page) { }
```

## 💡 LIKELY Questions (Medium Priority)

### Spring Boot Annotations

**Q13: What is @RequestBody and @ResponseBody?**

**Answer:**
- **@RequestBody** - Converts HTTP request body to Java object
- **@ResponseBody** - Converts Java object to HTTP response body

```java
@PostMapping("/users")
public User createUser(@RequestBody User user) {
    // user object created from JSON in request body
    return userService.save(user);
}

// @RestController automatically adds @ResponseBody
// But in @Controller, you need it explicitly
@Controller
public class UserController {
    @PostMapping("/users")
    @ResponseBody
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
}
```

**Q14: Explain @Valid annotation and how validation works.**

**Answer:** The @Valid annotation is part of the Java Bean Validation API (JSR-380, also known as Jakarta Bean Validation) that allows you to declaratively validate Java objects.

When this endpoint receives a request:

Spring binds the request body to the User object

The @Valid annotation triggers validation

If validation fails, a MethodArgumentNotValidException is thrown (which typically results in a 400 Bad Request response)


```java
public class User {
    @NotNull(message = "Name is required")
    @Size(min = 2, max = 50)
    private String name;
    
    @Email
    private String email;
    
    @Min(18)
    private int age;
}

@PostMapping("/users")
public User createUser(@Valid @RequestBody User user) {
    return userService.save(user);
}

// Handle validation errors
@ExceptionHandler(MethodArgumentNotValidException.class)
public ResponseEntity<String> handleValidation(
    MethodArgumentNotValidException ex) {
    return ResponseEntity.badRequest()
        .body("Validation failed: " + ex.getMessage());
}
```

**Q15: What is @CrossOrigin annotation? When do you use it?**

**Answer:**The @CrossOrigin annotation is a Spring Framework annotation that enables Cross-Origin Resource Sharing (CORS) for specific controllers or handler methods in a Spring application.

What is CORS?
CORS is a security mechanism that allows or restricts web pages from making requests to a different domain than the one that served the web page. Browsers enforce this "same-origin policy" by default for security reasons.

When to Use @CrossOrigin
You need @CrossOrigin when:

Your frontend (running on http://localhost:3000) needs to call your backend API (running on http://localhost:8080)

Your web application is hosted on a different domain than your API server

You're developing a public API that needs to be consumed by clients from various domains

You're integrating with third-party services that require cross-origin requests


```java
@RestController
@CrossOrigin(origins = "http://localhost:3000") // Allow React app
public class UserController {
    
    @CrossOrigin(origins = "*") // Allow all origins (not recommended for production)
    @GetMapping("/users")
    public List<User> getUsers() { }
}

// Global CORS configuration
@Configuration
public class CorsConfig {
    @Bean
    public WebMvcConfigurer corsConfigurer() {
        return new WebMvcConfigurer() {
            @Override
            public void addCorsMappings(CorsRegistry registry) {
                registry.addMapping("/api/**")
                    .allowedOrigins("http://localhost:3000")
                    .allowedMethods("GET", "POST", "PUT", "DELETE");
            }
        };
    }
}
```

**Q16: What are @RequestHeader and @ResponseStatus annotations?**

**Answer:**
- **@RequestHeader** - Extract HTTP header values
- **@ResponseStatus** - Set HTTP response status

```java
@GetMapping("/users")
public List<User> getUsers(
    @RequestHeader("Authorization") String authToken,
    @RequestHeader(value = "User-Agent", required = false) String userAgent) {
    // Use headers
}

@PostMapping("/users")
@ResponseStatus(HttpStatus.CREATED) // Returns 201
public User createUser(@RequestBody User user) {
    return userService.save(user);
}

@DeleteMapping("/users/{id}")
@ResponseStatus(HttpStatus.NO_CONTENT) // Returns 204
public void deleteUser(@PathVariable Long id) {
    userService.delete(id);
}
```

### Request/Response Handling

**Q18: How do you handle request and response in Spring Boot REST?**

**Answer:**
```java
@RestController
public class UserController {
    
    // Handle different content types
    @PostMapping(value = "/users", 
                consumes = "application/json",
                produces = "application/json")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        User savedUser = userService.save(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
    }
    
    // Handle optional parameters
    @GetMapping("/users")
    public ResponseEntity<List<User>> getUsers(
        @RequestParam(required = false) String name,
        @RequestParam(defaultValue = "0") int page) {
        
        List<User> users = userService.findUsers(name, page);
        return ResponseEntity.ok(users);
    }
}
```

**Q19: How do you return different HTTP status codes from REST endpoints?**

**Answer:**
```java
@RestController
public class UserController {
    
    // Using ResponseEntity
    @GetMapping("/users/{id}")
    public ResponseEntity<User> getUser(@PathVariable Long id) {
        User user = userService.findById(id);
        if (user != null) {
            return ResponseEntity.ok(user); // 200
        } else {
            return ResponseEntity.notFound().build(); // 404
        }
    }
    
    // Using @ResponseStatus annotation
    @PostMapping("/users")
    @ResponseStatus(HttpStatus.CREATED) // 201
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
    
    // Custom status with body
    @PostMapping("/users/bulk")
    public ResponseEntity<String> createUsers(@RequestBody List<User> users) {
        userService.saveAll(users);
        return ResponseEntity.status(HttpStatus.CREATED)
            .body("Users created successfully");
    }
}
```

**Q20: How do you handle file uploads in REST APIs?**

**Answer:**
```java
@RestController
public class FileController {
    
    @PostMapping("/upload")
    public ResponseEntity<String> uploadFile(
        @RequestParam("file") MultipartFile file) {
        
        if (file.isEmpty()) {
            return ResponseEntity.badRequest()
                .body("Please select a file");
        }
        
        try {
            String fileName = file.getOriginalFilename();
            Path path = Paths.get("uploads/" + fileName);
            Files.copy(file.getInputStream(), path);
            
            return ResponseEntity.ok("File uploaded: " + fileName);
        } catch (IOException e) {
            return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body("Upload failed");
        }
    }
    
    // Multiple file upload
    @PostMapping("/upload/multiple")
    public ResponseEntity<String> uploadFiles(
        @RequestParam("files") MultipartFile[] files) {
        
        List<String> uploadedFiles = new ArrayList<>();
        
        for (MultipartFile file : files) {
            // Process each file
        }
        
        return ResponseEntity.ok("Files uploaded: " + uploadedFiles.size());
    }
}
```

**Q21: How do you implement pagination in REST APIs?**

**Answer:**
```java
@RestController
public class UserController {
    
    @GetMapping("/users")
    public ResponseEntity<Page<User>> getUsers(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size,
        @RequestParam(defaultValue = "id") String sortBy) {
        
        Pageable pageable = PageRequest.of(page, size, 
            Sort.by(sortBy));
        Page<User> users = userService.findAll(pageable);
        
        return ResponseEntity.ok(users);
    }
    
    // Custom pagination response
    @GetMapping("/users/custom")
    public ResponseEntity<Map<String, Object>> getUsersCustom(
        @RequestParam(defaultValue = "0") int page,
        @RequestParam(defaultValue = "10") int size) {
        
        Page<User> pageUsers = userService.findAll(
            PageRequest.of(page, size));
        
        Map<String, Object> response = new HashMap<>();
        response.put("users", pageUsers.getContent());
        response.put("currentPage", pageUsers.getNumber());
        response.put("totalItems", pageUsers.getTotalElements());
        response.put("totalPages", pageUsers.getTotalPages());
        
        return ResponseEntity.ok(response);
    }
}
```

### Error Handling

**Q23: How do you handle exceptions in REST APIs?**

**Answer:**
```java
// Method level exception handling
@RestController
public class UserController {
    
    @GetMapping("/users/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id); // May throw UserNotFoundException
    }
    
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<String> handleUserNotFound(UserNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
            .body("User not found: " + ex.getMessage());
    }
}
```

**Q24: What is @ControllerAdvice and @ExceptionHandler?**

**Answer:** @ControllerAdvice provides global exception handling for all controllers.

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleUserNotFound(
        UserNotFoundException ex) {
        
        ErrorResponse error = new ErrorResponse(
            "USER_NOT_FOUND", 
            ex.getMessage(),
            System.currentTimeMillis()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }
    
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidation(
        MethodArgumentNotValidException ex) {
        
        String message = ex.getBindingResult().getFieldErrors()
            .stream()
            .map(error -> error.getField() + ": " + error.getDefaultMessage())
            .collect(Collectors.joining(", "));
            
        ErrorResponse error = new ErrorResponse(
            "VALIDATION_ERROR", 
            message,
            System.currentTimeMillis()
        );
        return ResponseEntity.badRequest().body(error);
    }
    
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGeneral(Exception ex) {
        ErrorResponse error = new ErrorResponse(
            "INTERNAL_ERROR", 
            "Something went wrong",
            System.currentTimeMillis()
        );
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR)
            .body(error);
    }
}

// Error response class
public class ErrorResponse {
    private String code;
    private String message;
    private long timestamp;
    
    // constructors, getters, setters
}
```

## 🎯 SCENARIO-BASED Questions

**Q27: How would you design REST APIs for CRUD operations?**

**Answer:**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    // CREATE
    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public User createUser(@Valid @RequestBody User user) {
        return userService.save(user);
    }
    
    // READ - Get all
    @GetMapping
    public ResponseEntity<Page<User>> getAllUsers(Pageable pageable) {
        Page<User> users = userService.findAll(pageable);
        return ResponseEntity.ok(users);
    }
    
    // READ - Get by ID
    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        return userService.findById(id)
            .map(user -> ResponseEntity.ok(user))
            .orElse(ResponseEntity.notFound().build());
    }
    
    // UPDATE - Full update
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(
        @PathVariable Long id, 
        @Valid @RequestBody User user) {
        
        return userService.update(id, user)
            .map(updatedUser -> ResponseEntity.ok(updatedUser))
            .orElse(ResponseEntity.notFound().build());
    }
    
    // UPDATE - Partial update
    @PatchMapping("/{id}")
    public ResponseEntity<User> partialUpdate(
        @PathVariable Long id,
        @RequestBody Map<String, Object> updates) {
        
        return userService.partialUpdate(id, updates)
            .map(user -> ResponseEntity.ok(user))
            .orElse(ResponseEntity.notFound().build());
    }
    
    // DELETE
    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void deleteUser(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

**Q28: How do you version REST APIs?**

**Answer:** There are several approaches:

**1. URL Versioning (Most Common):**
```java
@RestController
@RequestMapping("/api/v1/users")
public class UserV1Controller {
    @GetMapping
    public List<UserV1> getUsers() { }
}

@RestController
@RequestMapping("/api/v2/users")
public class UserV2Controller {
    @GetMapping
    public List<UserV2> getUsers() { }
}
```

**2. Header Versioning:**
```java
@RestController
@RequestMapping("/api/users")
public class UserController {
    
    @GetMapping(headers = "API-Version=1")
    public List<UserV1> getUsersV1() { }
    
    @GetMapping(headers = "API-Version=2")
    public List<UserV2> getUsersV2() { }
}
```

**3. Parameter Versioning:**
```java
@GetMapping(params = "version=1")
public List<UserV1> getUsersV1() { }

@GetMapping(params = "version=2")
public List<UserV2> getUsersV2() { }
```

**Q31: What are the best practices for REST API design?**

**Answer:**
1. **Use meaningful resource names** - `/users` not `/getAllUsers`
2. **Use HTTP methods correctly** - GET for read, POST for create, etc.
3. **Use proper HTTP status codes** - 200, 201, 400, 404, 500
4. **Implement proper error handling** - Consistent error format
5. **Version your APIs** - `/api/v1/users`
6. **Use pagination** - For large datasets
7. **Implement security** - Authentication and authorization
8. **Document your APIs** - Use Swagger/OpenAPI
9. **Use HTTPS** - Always encrypt in production
10. **Follow naming conventions** - Consistent and predictable URLs

**Example of well-designed API:**
```java
@RestController
@RequestMapping("/api/v1")
@Validated
public class ProductController {
    
    @GetMapping("/products")
    public ResponseEntity<Page<Product>> getProducts(
        @RequestParam(required = false) String category,
        @RequestParam(required = false) String search,
        @PageableDefault(size = 20) Pageable pageable) {
        
        Page<Product> products = productService.findProducts(
            category, search, pageable);
        return ResponseEntity.ok(products);
    }
    
    @PostMapping("/products")
    public ResponseEntity<Product> createProduct(
        @Valid @RequestBody CreateProductRequest request) {
        
        Product product = productService.create(request);
        return ResponseEntity.status(HttpStatus.CREATED).body(product);
    }
}
```
