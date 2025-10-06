
-----

## Java Record Classes

-----

## What are Records in Java?

  * **Records** are special classes in Java 14+ used to represent **immutable data** in a simple and concise way.
  * No more gigantic POJOs or DTOs\! Records eliminate **verbosity**.

-----

## What are they for?

  * They avoid the **verbosity** of traditional classes (POJOs/DTOs) that only store data.
  * Goodbye manual getters, equals, hashCode, and toString\!

-----

## Magic in one line\!

```java
public record Pessoa(String nome, int idade) {}
```

This single line automatically generates:

  * Fields `nome` and `idade` (private and `final`)
  * Constructor
  * Methods `nome()`, `idade()`, `toString()`, `equals()`, and `hashCode()`

-----

## Essential Characteristics

  * **Immutable**: Values do not change after creation.
  * **No inheritance**: They cannot extend other classes (but they can implement interfaces\!).
  * **Ideal**: Perfect for simple and secure data.

-----

## How to use?

```java
Pessoa p = new Pessoa("João", 30);
System.out.println(p.nome()); // João
System.out.println(p.idade()); // 30
```

  * Create instances easily.
  * Access data directly through the generated methods.

-----

## When to Use Records?

Use **Records** when you need a lightweight structure **ONLY** for storing data, such as:

  * API Parameters
  * Data Transfer Objects (**DTOs**)
  * **Immutable** Configurations

-----

## Record vs. Traditional Class: The Battle\!

  * **Traditional Class (POJO):**
      * **VERBAL**: Lots of boilerplate methods.
      * Requires manual code for constructor, getters, `equals()`, `hashCode()`, `toString()`.
  * **Record:**
      * **CONCISE**: One line solves everything.
      * Generates all the boilerplate code automatically.

-----

## Final Decision: When to choose?

  * **Choose RECORD:**

      * When you need classes to carry **immutable data**.
      * If there is **no complex internal logic**.

  * **Choose TRADITIONAL CLASS:**

      * When you need **mutability**.
      * To add **complex logic** to methods.
      * If there is a need for **inheritance**.