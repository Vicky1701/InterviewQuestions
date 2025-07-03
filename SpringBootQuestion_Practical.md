# Spring Boot Interview Questions - Easy & Medium Level

## Basic Level Questions

### 1. Create a simple REST API with GET, POST, PUT, DELETE operations
- Build a controller with all CRUD endpoints
- Handle different HTTP methods appropriately
- Return proper HTTP status codes
- Use @RestController, @RequestMapping, @GetMapping, @PostMapping, @PutMapping, @DeleteMapping

### 2. Implement basic CRUD operations with JPA/Hibernate
- Create Entity classes with proper annotations
- Use @Entity, @Id, @GeneratedValue, @Column
- Implement Repository interfaces extending JpaRepository
- Write service layer methods for business logic

### 3. Configure application.properties for different environments
- Set up database configurations
- Configure server port and context path
- Add logging configurations
- Set up different property files for environments

### 4. Create custom configuration classes using @Configuration
- Use @Configuration annotation
- Create @Bean methods
- Implement conditional configurations
- Use @Value for property injection

### 5. Implement exception handling with @ControllerAdvice
- Create global exception handler
- Handle specific exceptions
- Return custom error responses
- Use @ExceptionHandler annotation

### 6. Add validation to REST endpoints using @Valid
- Use Bean Validation annotations (@NotNull, @NotBlank, @Size, etc.)
- Validate request bodies and parameters
- Handle validation errors properly
- Return meaningful error messages

### 7. Create a simple scheduler using @Scheduled
- Enable scheduling with @EnableScheduling
- Create scheduled methods with @Scheduled
- Implement fixed rate and fixed delay scheduling
- Handle cron expressions

### 8. Implement basic security with Spring Security
- Configure security settings
- Set up authentication and authorization
- Protect endpoints with roles
- Configure CORS if needed

### 9. Add logging with different log levels
- Configure logging in application.properties
- Use different log levels (DEBUG, INFO, WARN, ERROR)
- Implement structured logging
- Use logback or log4j2 configuration

### 10. Create profiles for dev, test, prod environments
- Use @Profile annotation
- Configure different application-{profile}.properties
- Set active profiles
- Environment-specific bean configurations

## Medium Level Questions

### 1. Implement JWT authentication and authorization
- Create JWT token generation and validation
- Implement login endpoint
- Add JWT filter for request authentication
- Handle token expiration and refresh

### 2. Create custom actuator endpoints for health checks
- Extend existing health indicators
- Create custom health check endpoints
- Implement custom metrics
- Configure actuator security

### 3. Implement database migrations with Flyway/Liquibase
- Set up migration scripts
- Configure migration tool in application
- Handle version control for database schema
- Implement rollback strategies

### 4. Add caching using @Cacheable
- Enable caching with @EnableCaching
- Use @Cacheable, @CacheEvict, @CachePut annotations
- Configure cache providers (Redis, Hazelcast, etc.)
- Implement cache key strategies

### 5. Create async methods using @Async
- Enable async processing with @EnableAsync
- Create asynchronous service methods
- Handle CompletableFuture return types
- Configure thread pool executors

### 6. Implement file upload/download functionality
- Handle multipart file uploads
- Store files in filesystem or cloud storage
- Implement download endpoints
- Add file validation and size limits

### 7. Add pagination and sorting to REST APIs
- Use Pageable parameter in controller methods
- Implement sorting with Sort parameter
- Return PagedModel responses
- Handle pagination metadata

### 8. Create custom validators for request validation
- Implement custom validation annotations
- Create validator classes
- Handle complex validation logic
- Combine multiple validation constraints

### 9. Implement event-driven architecture with @EventListener
- Create custom application events
- Implement event listeners
- Use @EventListener and @Async together
- Handle event publishing and consumption

### 10. Add API documentation with Swagger/OpenAPI
- Configure Swagger/OpenAPI 3
- Add API documentation annotations
- Generate interactive API documentation
- Configure security schemes in documentation

## Practice Tips


### Common Interview Scenarios:
- Build a complete REST API for a simple domain (User, Product, Order)
- Implement authentication and authorization
- Add proper error handling and validation
- Configure for different environments
- Add monitoring and health checks
