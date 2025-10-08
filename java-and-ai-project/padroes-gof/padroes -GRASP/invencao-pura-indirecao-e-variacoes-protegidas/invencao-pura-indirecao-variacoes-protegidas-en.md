# Pure Fabrication, Indirection, and Protected Variations Patterns

## The Pure Fabrication Pattern

The **Pure Fabrication** pattern is a design pattern for creating artificial classes that do not represent domain concepts, but which are created to achieve design principles such as low coupling and high cohesion. These classes are used to assign complex responsibilities to an object without making already complex classes even more complex.

### Identifying the problem

When developing software, one of the first problems that can impact a system involves the assignment of responsibilities. The **Pure Fabrication** pattern is a pattern for assigning responsibilities, which suggests assigning the responsibility of performing a complex task to an artificial class, instead of assigning it to a class that already has high complexity.

**For example:** In a **Resource Allocation** system, it is a matter of deciding which class linked to a certain course can have the responsibility of saving a `Request`'s data in a relational database. To solve this problem, the **Pure Fabrication** pattern is applied.

Following the Pure Fabrication pattern, we create the class **PersistentRequest** which will be responsible for performing the data storage and retrieval tasks.

The code and figure show the implementation of the `Request` class with the 4 attributions and the responsibility to save the request. This responsibility, in the code without the pattern, is exercised by the `Request` class itself. With the application of the **Pure Fabrication** pattern, the responsibility is transferred to the `PersistentRequest` class, which is an artificial class that does not represent a concept in the domain, but which will be responsible for data persistence.

**Code (Without Pattern - Responsibility in the class itself):**

```java
class Request {
    private String id;
    private Date date;
    private Client client;

    // Request Attributes
    // Attributions:
    // 1 - Know the request total
    // 2 - Create a request item
    // 3 - Know the request status
    // 4 - Know the shipping value
    
    public void save() {
        // Logic to save the data in the database.
    }
    
    public void createItem() {
        // ...
    }
}
```

### Consequences of Pure Fabrication

Pure Fabrication allows the creation of artificial classes, which are classes with low cohesion and coupling, being very simple. The main consequence of Pure Fabrication is that it allows for greater flexibility and code reuse.

| Strategy | Description |
| :--- | :--- |
| **Low coupling** | The Pure Fabrication pattern ensures that the `Request` class is only coupled to abstractions of artificial classes, such as the `PersistentRequest` (pure fabrication). |
| **High cohesion** | The pattern ensures that the `Request` and `PersistentRequest` classes have high cohesion. |
| **Reuse** | The pattern promotes code reuse. |

**System Decomposition:**

| DECOMPOSITION BY REPRESENTATION | DECOMPOSITION BY BEHAVIOR |
| :--- | :--- |
| The decomposition of a module defines the way in which a subsystem can be divided into modules that contain its representations. | The GOF (Gang of Four) patterns, which are intended to organize objects in a system. Objects are organized by the concept of the target domain. |

A pure line must have different design policies for patterns depending on the target domain. Pure Fabrication is a responsibility assignment pattern. Other patterns based on Pure Fabrication, such as Adapter, Strategy, and Command, are examples of applying the Pure Fabrication pattern, as they are based on the decomposition of behavior.

## The Indirection Pattern

The **Indirection** pattern is a design pattern that assigns responsibilities to intermediate objects (intermediaries) to decouple components in a system.

**Indirection** is a pattern for assigning responsibilities, which suggests creating an intermediate object to mediate communication to reduce coupling between classes. The intermediate object is a *delegate* or *adapter*.

### Identifying the problem

The most evident problem is the use of one of the simplest and most used techniques in software projects, which can be perceived by the lack of design patterns. Most problems that occur in the system can be solved by the Indirection pattern.

When interacting with the system, for example, when creating an operation of a `Service A` object, such as talking to a `Service B` object, the simplest and most direct form of communication between objects is direct coupling.

### Indirection Solution

The pattern suggests creating an intermediate object, such as a *delegate* or *adapter*, to mediate communication indirectly between two classes or two subsystems.

**Design rules**

  * **Cohesion:** It is the measure of how strongly related and focused the responsibilities of an element are.
  * **Coupling:** It is the measure of how strongly an element is connected, has knowledge, or depends on other elements.

Example of how the Indirection pattern can be used in the situations described above:

  * **Communication between remote objects:**
    The Indirection pattern can be applied in systems that require communication between objects and/or distributed systems. The concept of **Stub** and **Skeleton** in RMI and **Web Services** are used to implement the pattern.

  * **Service API:**
    It is an intermediate object that serves as a facade for services provided by a subsystem. These objects act as intermediaries between clients and services.

With this, we can see how various design patterns (GOF) are specific applications of the **Indirection** pattern.

### Consequences of Indirection

The Indirection pattern can generate a series of benefits for the system, such as:

  * Reduce coupling between classes.
  * Increase flexibility and code reuse, as it allows for the reuse of classes and objects.
  * Increase cohesion.
  * Can improve performance, depending on the load and PAsS (Platform as a Service) used.

## The Protected Variations Pattern

The **Protected Variations** pattern identifies variation points and protects the system design. It is implemented using **Indirection** and **Polymorphism** to encapsulate what may vary.

The **Protected Variations** pattern is a design pattern that helps in assigning responsibilities. It involves the use of **Indirection** and **Polymorphism** to encapsulate elements that are prone to variation.

The pattern is applied to systems that need to adapt to different technologies (e.g., databases, communication, etc.).

### Identifying the problem

When maintenance operations become more difficult, most problems can impact the system, maintenance becomes difficult when changes in one platform affect other parts of the system. For example, changing the database.

### Variation Solution

The **Protected Variations** pattern suggests creating an interface to abstract the variation and avoid direct coupling.

**Aspects of interaction with the system:**

  * **Interfaces:** Define the contract of interaction between classes.
  * **Adapters:** Adapt the interface of one class to another that the client expects.
  * **Encapsulation:** Hides the complexity of a class.

**Design rules:**

  * **Coupling:** Reducing coupling is the objective.
  * **Cohesion:** Increasing the cohesion of classes.

The pattern suggests creating an **interface** to decouple the classes dependent on the class that is varying.

**Example:**
Consider a system that can store its data in different databases, such as SQL Server, MySQL, Oracle, or a NoSQL database.

**Code (Without Protected Variations - Direct Coupling):**

```java
class System {
    DB db = new DB();
    public void saveRequest() {
        db.save();
    }
}

class DB {
    public void save() {
        // Logic to save to the database.
    }
}
```

The code without the **Protected Variations** pattern directly couples the `System` class to the `DB` class. If the database type changes, the `System` class will have to be changed.

**Solution (With Protected Variations):**
Creation of an `IDataAccess` interface (abstraction) and concrete classes (`SqlServerDataAccess`, `NoSqlDataAccess`).

  * **Solution for MySQL, Oracle, or SQL Server:** Uses concrete classes for each relational database.
  * **Solution for NoSQL:** Uses concrete classes for non-relational databases.

**Code (With Protected Variations - Coupling to Interface):**

```java
class System {
    IDataAccess iDataAccess;

    public System() {
        this.iDataAccess = new SqlServerDataAccess(); // Or NoSqlDataAccess
    }

    public void saveRequest() {
        iDataAccess.save(); // Coupling to abstraction (Interface)
    }
}
```

**Design Rules:**

  * **Encapsulation:** Hiding the implementation details of the classes.
  * **Reuse:** Promoting code reuse.

### Consequences of Protected Variations

  * **Low coupling:** Between dependent classes and those that vary.
  * **High cohesion:** Classes are more cohesive, as they are focused on a single responsibility.
  * **Flexibility and reuse:** It is easier to change the variation.
  * **Maintainability:** The system becomes easier to maintain.
  * **Protected Variations:** The pattern protects against future variations.