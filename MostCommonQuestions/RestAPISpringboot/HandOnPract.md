# Spring Boot REST API Interview Questions

## Basic REST API Questions

### Question 1: Hello World Endpoint
Create a GET endpoint at `/api/hello` that returns:
```json
{"message": "Hello World"}
```

### Question 2: Query Parameter Handling
Create a GET endpoint at `/api/greet` that:
- Accepts a query parameter `name`
- Returns `{"message": "Hello, {name}!"}`
- Defaults to "Guest" if no name is provided

### Question 3: POST Request with Validation
Create a POST endpoint at `/api/greet` that:
- Accepts JSON: `{"name": "Alice"}`
- Returns `{"message": "Hello, Alice!"}` with status 201 (CREATED)
- Returns 400 (BAD_REQUEST) with `{"error": "Name cannot be empty"}` for empty input

### Question 4: Path Variables & DTOs
Create a GET endpoint at `/api/user/{id}` that:
- Returns a user DTO: `{"id": 1, "name": "Alice", "email": "alice@example.com"}`
- Returns 404 (NOT_FOUND) if the user doesn't exist

### Question 5: Advanced POST with DTO Validation
Create a POST endpoint at `/api/users` that:
- Accepts JSON: `{"name": "Alice", "email": "alice@example.com"}`
- Validates:
  - name: Not blank
  - email: Valid format
- Returns 201 (CREATED) for success
- Returns 400 (BAD_REQUEST) with detailed errors for invalid input

### Question 6: Error Handling
Extend Question 5 to:
- Handle MethodArgumentNotValidException globally
- Return consistent error format:
```json
{
  "timestamp": "2025-07-26T00:00:00",
  "status": 400,
  "errors": {
    "email": "must be a valid email address"
  }
}
```

### Question 7: PUT Endpoint
Create a PUT endpoint at `/api/users/{id}` that:
- Updates a user with the given ID
- Returns 200 (OK) with updated user data
- Returns 404 (NOT_FOUND) if the user doesn't exist

### Question 8: DELETE Endpoint
Create a DELETE endpoint at `/api/users/{id}` that:
- Deletes the user with the given ID
- Returns 204 (NO_CONTENT) on success
- Returns 404 (NOT_FOUND) if the user doesn't exist

## Exception Handling Questions

### Question 9: Custom Exception Handling
Create a custom exception `DuplicateEmailException` and:
- Throw it when trying to create a user with an email that already exists
- Handle it globally to return 409 (CONFLICT) with a meaningful message
- Include error details with timestamp and error code

### Question 10: Multiple Exception Types
Create a global exception handler that handles:
- `UserNotFoundException` → 404 with custom message
- `IllegalArgumentException` → 400 with validation message
- `DataIntegrityViolationException` → 409 for database constraint violations
- Generic `Exception` → 500 with generic error message

### Question 11: Exception Hierarchy
Design an exception hierarchy for your application:
- Create a base `BusinessException` class
- Extend it for `UserServiceException`, `ValidationException`, and `ResourceNotFoundException`
- Implement appropriate HTTP status codes for each exception type

### Question 12: Validation Exception Details
Enhance your exception handler to:
- Capture all field validation errors from `@Valid` annotations
- Return a structured response with field-specific error messages
- Include error codes for internationalization support

## JPA Repository Questions

### Question 13: Custom Query Methods
Create a UserRepository with custom query methods:
- Find users by email domain (e.g., all users with @gmail.com)
- Find users created between two dates
- Find users whose names contain a specific substring (case-insensitive)

### Question 14: Native Queries
Implement repository methods using `@Query` with native SQL:
- Count users by email domain
- Find top 5 most recently created users
- Update user's last login timestamp

### Question 15: Pagination and Sorting
Create an endpoint `/api/users/search` that:
- Accepts query parameters for pagination (page, size)
- Accepts sorting parameters (sort by name, email, createdDate)
- Returns paginated results with metadata (total elements, pages, etc.)

### Question 16: Specifications and Criteria API
Implement dynamic search functionality:
- Create specifications for filtering users by name, email, and date range
- Combine multiple criteria using AND/OR operations
- Create an endpoint that accepts multiple optional search parameters

### Question 17: Custom Repository Implementation
Create a custom repository interface and implementation:
- Add a method to perform bulk operations on users
- Implement complex search logic that cannot be achieved with derived queries
- Use EntityManager for complex queries

### Question 18: Transaction Management
Implement a service method that:
- Creates a user and sends a welcome email
- Ensures both operations are within the same transaction
- Handles rollback scenarios when email sending fails
- Demonstrates `@Transactional` propagation levels

## Advanced Topics

### Question 19: DTO Mapping and Conversion
Implement proper DTO mapping:
- Create separate DTOs for create, update, and response operations
- Use MapStruct or ModelMapper for automatic mapping
- Handle nested objects and collections in DTOs

### Question 20: API Versioning
Implement API versioning:
- Create v1 and v2 of the user endpoint
- Use header-based versioning (`Accept-Version: v2`)
- Maintain backward compatibility while adding new fields



### Question 21: Rate Limiting
Implement rate limiting:
- Limit API calls per user per minute
- Use Redis or in-memory store for rate limit tracking
- Return appropriate HTTP status codes (429 Too Many Requests)
- Include rate limit headers in responses



### Question 22: Integration Testing
Write comprehensive integration tests:
- Test complete user CRUD operations
- Mock external dependencies
- Test exception scenarios
- Use TestContainers for database testing



