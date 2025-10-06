
-----

## Essential Java Concepts (`java.lang`)

### What is the difference between `==` and `equals()` for String objects?

The **`==` compares memory references**, while **`equals()` compares content**. This distinction is crucial to avoid bugs.

```java
String ninja1 = "Naruto";
String ninja2 = new String("Naruto");

System.out.println(ninja1 == ninja2);       // false
System.out.println(ninja1.equals(ninja2)); // true
```

-----

### Are Strings in Java mutable or immutable?

**Strings in Java are immutable.**

This means that any alteration creates a new object in memory, and the original remains intact.

This characteristic provides **thread-safety** in multi-threaded environments and enables caching, but it can impact performance during intense concatenations. In these cases, the most appropriate choice is to use **`StringBuilder`** or **`StringBuffer`**.

```java
String ninja = "Naruto";
ninja.concat(" Uzumaki"); 
System.out.println(ninja); // still "Naruto"
```

-----

### What does the Object class represent in Java?

The **`Object` class is the root of the class hierarchy in Java**, meaning all other classes inherit from it, directly or indirectly. It defines fundamental methods such as **`equals()`**, **`hashCode()`**, **`toString()`**, **`clone()`**, and **`getClass()`**, which serve as the **basic behavioral contract for any object**.

These methods are very important in comparison operations, in collections like **`HashMap`** and **`HashSet`**, and even for debugging.

Another point is that frameworks like Spring or Quarkus often rely on these contracts to manage objects internally.

-----

### What is the difference between Throwable, Exception, and Error?

Every exception or error in Java inherits from **`Throwable`**. The fundamental difference is:

**`Exception`** represents problems that the program **can anticipate and handle**, such as I/O failure or connection issues.

**`Error`** represents **serious environment problems**, such as lack of memory (`OutOfMemoryError`), and **should generally not be caught** by the application code.

```java
try {
    throw new Exception("Failure in rank B mission!");
} catch (Exception e) {
    System.out.println("Handling: " + e.getMessage());
}
```

-----

### What is the Enum class used for in Java?

The **`Enum`** (Enumeration) in Java is used when we need to represent a **fixed set of constants**, such as days of the week, months, or ninja villages in the Naruto universe.

Unlike `String` or `int`, it ensures **compile-time safety** and prevents invalid values. Furthermore, we can associate attributes and methods with each constant, making the code more readable and easier to maintain.

```java
enum Village { KONOHA, SUNA, KIRI }

Village village = Village.KONOHA;
System.out.println("Naruto is from village: " + village);
```

-----

### How does autoboxing work with the Integer class?

**Autoboxing automatically converts primitive types into their wrappers**, like `int` to `Integer`. **Unboxing performs the inverse** operation.

This feature is helpful when using generic collections, which do not accept primitive types. However, it's important to be cautious in large loops because **autoboxing can create many unnecessary objects**.

```java
Integer chakra = 100;     // autoboxing
int spent = chakra - 40;  // unboxing
System.out.println("Remaining chakra: " + spent);
```

-----

### How does the Math class handle random numbers?

**`Math.random()` returns a `double` number between $0.0$ and $1.0$**. For integers, you simply multiply and cast.

```java
int hokage = (int)(Math.random() * 7); // Hokage draw 0 to 6
System.out.println("The chosen Hokage was No: " + hokage);
```