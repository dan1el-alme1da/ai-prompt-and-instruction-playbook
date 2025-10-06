
-----

## What does the Object class represent in Java?

The **Object** class is the **root of the class hierarchy in Java**, meaning all classes inherit from it directly or indirectly. It defines fundamental methods such as **equals()**, **hashCode()**, **toString()**, **clone()**, and **getClass()**, which serve as the basic behavior contract for any object.

These methods are very important in comparison operations, in collections like HashMap and HashSet, and even in debugging.

Another point is that frameworks like Spring or Quarkus rely on these contracts to manage objects internally.

-----

## What is the difference between $==$ and $equals()$ in String objects?

The **$==$** compares **references in memory**, while **equals()** compares **content**. This difference is crucial for avoiding bugs.

```java
String ninja1 = "Naruto";
String ninja2 = new String("Naruto");
System.out.println(ninja1 == ninja2); // false
System.out.println(ninja1.equals(ninja2)); // true
```

-----

## How does the Math class handle random numbers?

**Math.random()** returns a **double** number between **0.0 and 1.0**. For integers, you just need to multiply and cast (convert).

```java
int hokage = (int)(Math.random() * 7); // Hokage draw from 0 to 6
System.out.println("The chosen Hokage was no: " + hokage);
```

-----

## What is the Enum class in Java used for?

The **Enum in Java** is used when we need to represent a **fixed set of constants**, such as days of the week, months, or ninja villages in the Naruto universe.

Unlike String or int, it guarantees **compile-time safety** and prevents invalid values. Furthermore, we can associate attributes and methods with each constant, making the code more readable and easier to maintain.

```java
enum Vila { KONOHA, SUNA, KIRI }
Vila vila = Vila.KONOHA;
System.out.println("Naruto is from the village: " + vila);
```

-----

## How does autoboxing work with the Integer class?

**Autoboxing** automatically converts **primitive types to their wrappers**, such as `int` to `Integer`. **Unboxing** does the reverse.

This feature is helpful when using generic collections, which do not accept primitive types. However, it is important to be careful in large loops, because autoboxing can create many unnecessary objects.

```java
Integer chakra = 100; // autoboxing
int gasto = chakra - 40; // unboxing
System.out.println("Remaining Chakra: " + gasto);
```

-----

## What is the difference between Throwable, Exception, and Error?

Every exception or error in Java inherits from **Throwable**. The difference is that **Exception** represents **problems that the program can anticipate and handle**, such as I/O or connection failure.

**Error**, on the other hand, represents **serious environment problems**, such as lack of memory, and generally **should not be caught**.

```java
try {
  throw new Exception("B rank mission failure!");
} catch (Exception e) {
  System.out.println("Handling: " + e.getMessage());
}
```

-----

## Are Strings in Java mutable or immutable?

Strings in Java are **immutable**. This means that any modification creates a **new object in memory**, and the original remains intact.

This characteristic provides safety in multi-threaded environments and allows caching, but it can impact performance in intense concatenations. In these cases, it is more appropriate to use **StringBuilder** or **StringBuffer**.

```java
String ninja = "Naruto";
ninja.concat(" Uzumaki");
System.out.println(ninja); // still "Naruto"
```