
-----

## **What is an Exception?**

An **exception** is an abnormal event that occurs during program execution and **interrupts the normal flow of instructions**.

**For example:**

  * Trying to access an index outside the bounds of an array.
  * Trying to divide a number by zero.
  * Trying to open a file that does not exist.

-----

## **How to Handle Exceptions in Java?**

Exception handling is done with the **try**, **catch**, **finally** blocks and the **throw** keyword.

```java
try {
    // Code that might throw an exception
} catch (TypeOfException e) {
    // Code to handle the exception
} finally {
    // (Optional) Code that will always be executed
}
```

-----

## **Practical Example: Division by Zero**

```java
public class Example {
    public static void main(String[] args) {
        try {
            int result = 10 / 0; // ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Error: division by zero.");
        } finally {
            System.out.println("Finally block executed.");
        }
    }
}
```

**Output:** Error: division by zero. Finally block executed.

**Note how the catch handled the error and the finally block was always executed.**

-----

## **Multiple catch: Handling Variety**

You can have several **catch** blocks for different types of exceptions, making your handling more specific.

```java
try {
    int[] numbers = {1, 2, 3};
    System.out.println(numbers[5]); // ArrayIndexOutOfBoundsException
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Invalid index in array.");
} catch (Exception e) {
    System.out.println("Generic error.");
}
```

**Prioritize more specific exceptions before the generic ones\!**

-----

## **Throwing Exceptions with throw**

You can also **manually throw an exception** to indicate a problem in your code.

```java
public void checkAge(int age) {
    if (age < 18) {
        throw new IllegalArgumentException("Minor age not permitted.");
    }
}
```

**Perfect for validating inputs or business rules.**

-----

## **Checked vs. Unchecked Exceptions**

| Type | Example | Requires `try`/`catch` or `throws` |
| :--- | :--- | :--- |
| **Checked** | `IOException`, `SQLException` | Yes $\checkmark$ |
| **Unchecked** | `NullPointerException`, `ArithmeticException` | Not mandatory $\times$ |

**Observation: Checked exceptions are verified at compile time, unchecked at runtime.**

-----

## **Best Practices: Clean and Robust Code**

  * Only handle exceptions that you can genuinely **resolve**.
  * Never leave **`catch (Exception e)`** unnecessarily — it's too generic.
  * Use **finally** to close resources (like files, connections, etc.).
  * Prefer using **`try-with-resources`** when dealing with external resources.