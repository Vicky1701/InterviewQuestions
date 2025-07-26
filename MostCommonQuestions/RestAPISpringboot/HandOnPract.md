Question 1: Hello World Endpoint
Create a GET endpoint at /api/hello that returns:

json
{"message": "Hello World"}
Question 2: Query Parameter Handling
Create a GET endpoint at /api/greet that:

Accepts a query parameter name

Returns {"message": "Hello, {name}!"}

Defaults to "Guest" if no name is provided

Question 3: POST Request with Validation
Create a POST endpoint at /api/greet that:

Accepts JSON: {"name": "Alice"}

Returns {"message": "Hello, Alice!"} with status 201 (CREATED)

Returns 400 (BAD_REQUEST) with {"error": "Name cannot be empty"} for empty input

Question 4: Path Variables & DTOs
Create a GET endpoint at /api/user/{id} that:

Returns a user DTO: {"id": 1, "name": "Alice", "email": "alice@example.com"}

Returns 404 (NOT_FOUND) if the user doesn't exist

Question 5: Advanced POST with DTO Validation
Create a POST endpoint at /api/users that:

Accepts JSON: {"name": "Alice", "email": "alice@example.com"}

Validates:

name: Not blank

email: Valid format

Returns 201 (CREATED) for success

Returns 400 (BAD_REQUEST) with detailed errors for invalid input

Question 6: Error Handling
Extend Question 5 to:

Handle MethodArgumentNotValidException globally

Return consistent error format:

json
{
  "timestamp": "2025-07-26T00:00:00",
  "status": 400,
  "errors": {
    "email": "must be a valid email address"
  }
}
Question 7: PUT Endpoint
Create a PUT endpoint at /api/users/{id} that:

Updates a user with the given ID

Returns 200 (OK) with updated user data

Returns 404 (NOT_FOUND) if the user doesn't exist

Question 8: DELETE Endpoint
Create a DELETE endpoint at /api/users/{id} that:

Deletes the user with the given ID

Returns 204 (NO_CONTENT) on success

Returns 404 (NOT_FOUND) if the user doesn't exist
