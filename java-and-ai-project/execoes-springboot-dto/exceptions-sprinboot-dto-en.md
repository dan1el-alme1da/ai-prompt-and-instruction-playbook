
-----

### Image 1: The Problem

**The problem** ❌

This is an excerpt from a script I had in my first Spring Boot project

```java
return userRepository.findByUsername(username)
.orElseThrow(() -> new 
RuntimeException("Usuário não encontrado")); // User not found
```

Without custom handling, Spring might return something like this:

```json
{
  "timestamp": "2025-07-10T14:35:00.123",
  "status": 500,
  "error": "Internal Server Error",
  "message": "Usuário não encontrado", // User not found
  "path": "/users/login"
}
```

Okay, but what are the main problems with Spring returning it this way?

### Image 2: Main Problems

**Main problems** ❗

  * **Not standardized:** each error can come in a different format
  * **Wrong Status:** it always returns 500 even if the error should be, for example, 404
  * **Insecure:** internal exceptions can leak
  * **Inconsistent:** if the Spring version changes or if the whitelabel is enabled/disabled, the behavior can change

### Image 3: Creating a DTO for Error Response

**Creating DTO for Error Response** 💬

Create a simple DTO to format the error response

Instead of returning raw exceptions, you can create a clear and structured response class:

```java
public record ErrorResponseDTO(
    int status,
    String message,
    LocalDateTime timestamp
) {}
```

  * More control
  * More clarity for the front-end
  * More professionalism

### Image 4: Handling Errors with @RestControllerAdvice

**Handling Errors with @RestControllerAdvice** ✅

Now that the custom exception and the DTO have been created, capture and handle the exception globally with **`@RestControllerAdvice`**.

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponseDTO> 
    handleNotFound(ResourceNotFoundException ex) {
        return 
        buildErrorResponse(ex.getMessage(), 
        HttpStatus.NOT_FOUND);
    }

    private ResponseEntity<ErrorResponseDTO> 
    buildErrorResponse(String message, HttpStatus 
    status) {
        ErrorResponseDTO error = new 
        ErrorResponseDTO(
            status.value(),
            message,
            LocalDateTime.now()
        );
        return new ResponseEntity<>(error, 
        status);
    }
}
```

### Image 5: How the GlobalExceptionHandler Works

**How the GlobalExceptionHandler Works?**

  * **`@RestControllerAdvice`**: allows you to intercept errors globally, outside of the controllers.
  * **`@ExceptionHandler`**: captures a specific exception (like UserNotFoundException) whenever it is thrown.
  * **`buildErrorResponse(...)`**: centralizes the construction of the object that will be returned, guaranteeing pattern and organization.

**Result:** a clean JSON, with status, message, and timestamp.

```json
{
  "status": 404,
  "message": "Usuário não encontrado", // User not found
  "timestamp": "2025-07-10T14:42:00"
}
```

### Image 6: Exception Handling in Spring Boot

**Exception Handling in Spring Boot**

With custom exceptions and GlobalExceptionHandler

### Image 7: Custom Exception

**Custom Exception** ✔️

Create a custom exception for expected scenarios

Instead of throwing `RuntimeException` directly, you can create a class with a clear name and purpose:

```java
public class UserNotFoundException extends 
RuntimeException {
    public UserNotFoundException(String message) {
        super(message);
    }
}
```

**Advantages:**

  * Clear and specific message
  * More organized code
  * Facilitates global handling