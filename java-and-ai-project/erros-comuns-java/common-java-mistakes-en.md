
# 04 Common Java Mistakes Every Developer Makes

This summary covers four frequent errors committed by Java developers, along with the correct solution for each one.

-----

## Error 1: Using `==` to Compare Strings

### The Mistake

Using the equality operator (`==`) to compare the content of two strings.

```java
if(name == "Bruno") {
    // ...
}
```

**The problem:** This does not compare the **content** of the `String`; it compares its **memory reference**. In many cases, this will result in an incorrect comparison.

### The Fix

Use the `equals()` method to compare the string values.

```java
if(name.equals("Bruno")) {
    // ...
}
```

**Tip:** Whenever you need to compare **String** values, use `equals()`.

-----

## Error 2: Not Handling Exceptions Correctly

### The Mistake

Throwing generic exceptions like `RuntimeException` without customization and without centralized control.

```java
throw new RuntimeException("Error fetching user");
```

**The problem:** This makes it difficult to understand the error and handle it properly on the front-end or in other parts of the system.

### The Fix

Create **custom exceptions** (e.g., `UserNotFoundException`) and **centralize** their handling using `@RestControllerAdvice` and `@ExceptionHandler` (common in the Spring Boot ecosystem).

-----

## Error 3: Exposing the User's Password in the API Response

### The Mistake

Directly returning the `User` entity (or similar) with all its fields, including the password, in an API response.

```java
return userRepository.findAll();
```

**The problem:** This can cause the password or other sensitive data to be included in the response JSON. Exposing sensitive data is a **severe security flaw**.

### The Fix

Create a **DTO (Data Transfer Object)**, such as `UserResponseDTO`, containing only the data that **needs** to be returned.

**Tip:** Use **DTOs** to protect data and keep API responses clean and secure.

-----

## Error 4: Returning `null` Instead of an Empty List

### The Mistake

Returning `null` from a method that should return a list when there are no results.

```java
public List<Product> listProducts() {
    if(isStockEmpty()) return null; // The Mistake!
    return productRepository.findAll();
}
// ...
productList.size(); // Can cause NullPointerException
```

**The problem:** Returning `null` forces the consumer of the method to always perform a check `if (list != null)` before using the list, such as calling `size()` or iterating. If the check is missed, a `NullPointerException` is thrown.

### The Fix

Return an **empty list** when there are no results.

```java
return new ArrayList<>(); // The Fix!
```

**Tip:** Return **empty lists** instead of `null` to avoid exceptions (like `NullPointerException`) and make the code cleaner and safer.