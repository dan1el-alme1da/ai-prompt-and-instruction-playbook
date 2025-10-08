
-----

# GoF Structural Patterns

The GoF (Gang of Four) **structural patterns** deal with the **composition of classes and objects** into larger, more flexible, and efficient structures, maintaining flexibility and efficiency. The structural patterns presented in this content are: **Adapter**, **Bridge**, **Composite**, **Decorator**, **Facade**, **Flyweight**, and **Proxy**.

Alexandre Luis Correa and Prof. Marcelo Pavan Antoni

## Initial Items

**Preparation**

  * **Concept of Design Patterns**: It is recommended to start or review this content in a program that allows the reader to assemble their slides and the form of UML 2 diagrams, Engagement (Message structure). In this section, you should focus attention on the patterns that will be discussed in chapter 5 of the book **Head First Design Patterns**.
  * **Research**: Search for the ways to formulate the three sets of patterns (creational, structural, and behavioral) and the connection in their use, as well as the class structure, with the creation methods and their implementation characteristics. We suggest studying and representing the **Adapter, Bridge, Decorator, Composite, Flyweight**, and **Proxy** patterns.
  * **Development**: We suggest using free tools for modeling systems in UML (UML\_SAK, Trac), where the focus should be on the application of the patterns.
  * **Reading**: Therefore, we recommend setting up a programming environment in Java. The programming environment should have the latest version of Eclipse and also the JRE (Java Runtime Environment) and JDK (Java Development Kit) referring to the Java SE (Standard Edition) edition, which can be found for free on the technology's website (Oracle, for Java).

**Objectives**

  * Reconstruct the purpose, structure, and application situations of the **Adapter**, **Bridge**, and **Decorator** design patterns.
  * Reconstruct the purpose, structure, and application situations of the **Bridge** and **Decorator** design patterns.
  * Reconstruct the purpose, structure, and application situations of the **Composite** and **Flyweight** design patterns.
  * Reconstruct the purpose, structure, and application situations of the **Flyweight** and **Proxy** design patterns.

**Introduction**
The **GoF Patterns**, from the English Gang of Four, are object-oriented design patterns divided into three groups (Creational, Structural, and Behavioral), developed by Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides in the book **Design Patterns: Elements of Reusable Object-Oriented Software** in 1994. These **structural** patterns are those that allow modularizing the solution to a problem through object composition where inheritance is not used, but object composition is. In this unit, you will learn about the seven patterns in this category, which are: **Adapter, Bridge, Composite, Decorator, Facade, Flyweight, and Proxy**.

The **Adapter** pattern allows system modules to interact with different implementations of external libraries (with incompatible interfaces), or to adapt one interface to another, allowing the reuse of an object of its implementation. So that both can vary independently. The **Bridge** pattern separates responsibilities between classes or modules in order to have independent hierarchies of abstractions and implementations. The **Composite** pattern shows how we can structure an object hierarchy built from two types of elements: primitives and composites, so that clients can treat the objects uniformly. The **Decorator** pattern enables the extension of an object's behavior dynamically at runtime. The **Flyweight** pattern presents a solution to the problem of sharing a large number of objects, with the aim of saving memory resources. The **Facade** pattern, in turn, simplifies a complex object interface for clients. Finally, the **Proxy** pattern offers a substitute to control access to a real object.

-----

# Adapter Design Pattern

## Intent and problem solved by the Adapter pattern

The Adapter design pattern's intent is to allow two classes with incompatible interfaces to collaborate. It is one of the GoF (Gang of Four) structural patterns, which deals with the composition of classes and objects into larger structures, keeping them flexible and efficient. Structural patterns explain how to assemble objects and classes into larger structures, maintaining these structures flexible and efficient.

**Intent of the Adapter pattern**
The Adapter is a pattern that allows adapting responsibilities or the interface of an existing class to a new interface required by the client, at runtime.

**Problem solved by the Adapter pattern**
**Classes and objects** that have incompatible interfaces or cannot communicate directly due to the difference in interface. The Adapter acts as a translator, transforming the interface of one class (called the *Adaptee*) to the interface that the client (called the *Client*) expects, allowing them to work together.

What happens in practice is that the code expected to be used is implemented in an interface that cannot be used by the client code.

**What must be done to adapt the code in a way that the client can use it?**

We must adapt the existing code to an interface that the client expects to use the code without changing it.

The **Adapter** pattern is useful in projects where the system is evolving or needs to integrate with an external system that has an incompatible interface.

*Imagine that you are developing a system for sending two different types of cards, with each card type using a specific payment system, with different APIs. How do you make your code usable for all payment methods without changing the client code?*

In this example, you have the interfaces `APIPagamentos` and `APIPagamentos2` and an interface for your program, called `CartaoAdapter`.

-----

## Solution, consequences, and patterns related to the Adapter pattern

The Adapter pattern allows classes with incompatible interfaces to work together. It is a structural object pattern that uses **object composition** to allow a class (called *Adaptee* or Adaptation) to work with another class (called *Target* or Alvo) that expects a different interface.

**Structure of the Adapter pattern**
The diagram below represents the structure of the Adapter pattern:

*Class Diagram: `Target` and `Adaptee` Interfaces. `Client` Class relates to `Target`. `Adaptee` Class has a method `SpecificRequest()`. `Adapter` Class implements `Target` and relates to `Adaptee`. The `Request()` method of `Adapter` calls `SpecificRequest()` of `Adaptee`.*

In the diagram, the **Target** interface represents the interface that the client expects. The **Adaptee** interface (Adaptation) represents the existing incompatible interface. The **Adapter** class implements the **Target** interface and wraps the **Adaptee** class, acting as a bridge to make the interfaces compatible.

### Interface Adaptation

In some scenarios, adaptation is crucial, either due to the need for integration with legacy systems or the search for flexibility in design.

**The Adaptee interface (adaptation) represents the generalization of the target services inappropriately.**

The solution consists of redefining the existing interface through an adapter class, which implements the interface required by the client, and wraps the object that needs to be adapted (Adaptee).

The Adapter solution fits perfectly into the scenario of needing to integrate legacy systems, where there is a set of data or legacy code that the client system cannot use.

*Imagine a scenario where a client needs a specific service, but the available service has a different interface than the one the client expects.*

**Payment Solution 1**

A card needs to perform two operations: `Pagamento` (Payment) and `Estorno` (Chargeback). The payment system (Adaptee) offers `Debit` and `Credit`.

**Payment Solution 2**

A card needs to perform two operations: `Pagamento` (Payment) and `Estorno` (Chargeback). The payment system (Adaptee) offers `FazerPagamento` (MakePayment) and `FazerEstorno` (MakeChargeback).

Notice that, in addition to different names, the operation patterns are also different.

**Code (Java example)**

```java
// Client interface that the code expects
public interface CartaoAdapter {
    void processarPagamento(String numeroCartao, double valor);
    void processarEstorno(String numeroCartao, double valor);
}

// Adapted class 1 (Adaptee)
public class AdaptadorPagamento implements CartaoAdapter {
    private final APIPagamentos apiPagamentos;

    public AdaptadorPagamento(APIPagamentos apiPagamentos) {
        this.apiPagamentos = apiPagamentos;
    }

    @Override
    public void processarPagamento(String numeroCartao, double valor) {
        apiPagamentos.debitar(numeroCartao, valor);
    }

    @Override
    public void processarEstorno(String numeroCartao, double valor) {
        apiPagamentos.creditar(numeroCartao, valor);
    }
}
//... (code for APIPagamentos and others)
```

**Code (Java example with another API)**

```java
// Adapted class 2 (Another Adaptee)
public class AdaptadorPagamento2 implements CartaoAdapter {
    private final APIPagamentos2 apiPagamentos2;

    public AdaptadorPagamento2(APIPagamentos2 apiPagamentos2) {
        this.apiPagamentos2 = apiPagamentos2;
    }

    @Override
    public void processarPagamento(String numeroCartao, double valor) {
        apiPagamentos2.fazerPagamento(numeroCartao, valor);
    }

    @Override
    public void processarEstorno(String numeroCartao, double valor) {
        apiPagamentos2.fazerEstorno(numeroCartao, valor);
    }
}
//... (code for APIPagamentos2 and others)
```

Thus, since each API has a specific interface and methods for credit card payment, one solution is to create an **Adapter** class for each API, allowing the application to use the same code to process each payment method.

**Consequences and patterns related to the Adapter pattern**

### Consequences

The **Adapter** pattern brings a series of benefits, the most relevant being the possibility for classes with incompatible interfaces to collaborate. This is important in large-scale projects that need to integrate with legacy or third-party systems, where the interface cannot be modified, and the client cannot change it to use the existing code.
The disadvantage is that for every new incompatible implementation, a new **Adapter** class must be created.

### Related Patterns

| Bridge | Adapter |
| :--- | :--- |
| It applies when we want to decouple the abstraction from its implementation, allowing both to vary independently. | It allows a class with an incompatible interface to work together, wrapping an object and providing it with a new interface. |
| Generally used at design time to allow the independent evolution of two class hierarchies. | Generally used to make an existing class work with a client class that expects a different interface. |

-----

# Bridge and Decorator Design Patterns

## Intent and problem solved by the Bridge pattern

The **Bridge** design pattern's intent is to decouple the abstraction from its implementation, so that the two can vary independently. It is one of the GoF (Gang of Four) structural patterns, which deals with the composition of classes and objects into larger structures, keeping them flexible and efficient. Structural patterns explain how to assemble objects and classes into larger structures, allowing objects to be built by one or more layers of *decorators*, facilitating code reuse and system maintenance.

**Intent of the Bridge pattern**
The Bridge pattern allows decoupling an **abstraction** from its **implementation**, allowing both to vary independently.

**Problem solved by the Bridge pattern**
In a large-scale project, when classes have different implementations in systems or platforms, using inheritance, the code can become inflexible, with excessive classes, making the code difficult to maintain and modify.
The **Bridge** pattern tries to solve this problem by swapping inheritance for object composition.

**What happens when working with different implementations in code at runtime, using inheritance?**
The class hierarchy will grow exponentially because adding a new component or supporting a new platform will require the creation of new classes.

These common problems occur when we combine different classification perspectives in an abstraction.

**What is the solution to the problem of exponential class growth due to the variation of abstractions and implementations?**

The solution consists of separating the hierarchies of abstractions and implementations, allowing them to vary independently through a bridge. The bridge is achieved through an instance of the implementation within the abstraction.

**Structure of the Bridge pattern**
The design solution of the Bridge pattern is represented in the class diagram below:

*Class Diagram: `Implementor` and `Abstraction` Interfaces. `ImplementationA` and `ImplementationB` Classes implement `Implementor`. `RefinedAbstraction` Class inherits from `Abstraction`. `Abstraction` Class contains a reference to `Implementor` and has an `operation()` method, which calls the `implementedOperation()` method of `Implementor`.*

The central idea is to separate the abstraction hierarchy from the implementation hierarchy, so that the client code interacts only with the abstraction hierarchy. The abstraction, in turn, must delegate execution to the specific implementation that is connected to the abstraction.

**Practical Example (Java)**
The Bridge pattern is applied in two scenarios:

1.  In projects where it is necessary to develop implementations for different platforms (Windows, Linux, macOS).
2.  In projects that have various abstractions and implementations (Remote Control and Device).

**Code (Java example)**

```java
public interface Component {
    void draw();
}

public class TextField implements Component {
    private String text;
    private int width;
    private int height;

    public TextField(String text, int width, int height) {
        this.text = text;
        this.width = width;
        this.height = height;
    }

    @Override
    public void draw() {
        System.out.println("Text: " + text + ", Size: " + width + "x" + height);
    }
}
//... (other classes and code)
```

Note that the code for the `TextField` class is platform-specific. To have different implementations, the Bridge solution is to create a hierarchy of abstractions (`Component` and its concretizations) and a hierarchy of implementations (`PlatformImpl` and its concretizations).

**Consequences and patterns related to the Bridge pattern**

### Consequences

The **Bridge** pattern allows the **decoupling** between the abstraction and the implementation, which is useful in large-scale projects, where there are various abstractions and implementations. Because it is a pattern that uses object composition instead of inheritance, it provides flexibility to create and configure objects at runtime. This is fundamental when the abstraction and implementation need to vary independently.

### Related Patterns

| Adapter | Bridge |
| :--- | :--- |
| It applies when we want a class with an incompatible interface to work with a client class that expects a different interface. | It allows creating a bridge to vary not only its implementation but also its abstractions. |
| Generally used to make an existing class work with a client class that expects a different interface, after the application is under development. | Generally used at design time to allow the independent evolution of two class hierarchies. |

-----

# Bridge and Decorator Design Patterns

## Intent and problem solved by the Decorator pattern

The **Decorator** design pattern's intent is to add responsibilities to objects dynamically and transparently, without changing their internal structure. It is one of the GoF (Gang of Four) structural patterns, which deals with the composition of classes and objects into larger structures, keeping them flexible and efficient. Structural patterns explain how to assemble objects and classes into larger structures, allowing objects to be built by one or more layers of *decorators*, facilitating code reuse and system maintenance.

**Intent of the Decorator pattern**
The Decorator is a pattern that allows adding responsibilities to an object **dynamically** and **transparently**, without changing its internal structure.

**Problem solved by the Decorator pattern**
A common way to extend class functionality is through subclass inheritance.
The problem with inheritance is that functionality is added at compile time, making the code inflexible and requiring new code to be written.

**What is the solution to the problem of extending a class's functionality at runtime without changing the code?**

We must, then, create a **wrapper** for an object instance. The *wrapper* implements the same interface as the wrapped object, adding responsibilities at runtime.

**Code (Java example)**

```java
public interface Component {
    void doOperation();
}

public class ConcreteComponent implements Component {
    @Override
    public void doOperation() {
        System.out.println("Executing the main operation.");
    }
}

public abstract class Decorator implements Component {
    protected Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    @Override
    public void doOperation() {
        component.doOperation(); // Delegates the call to the wrapped object
    }
}
//... (other classes and code)
```

To add the ability to load compressed text, we could use an **inheritance** based solution, but if there is a need to include new behaviors, such as encryption, the code will become complex.

In this solution, we suggest that compression occurs only at the time of saving and loading.

**Code (Java example with Decorators)**

```java
// Concrete Decorator for compression
public class CompressionDecorator extends Decorator {
    public CompressionDecorator(Component component) {
        super(component);
    }

    @Override
    public void doOperation() {
        System.out.println("Compressing the content.");
        super.doOperation();
        System.out.println("Content compressed.");
    }
}

// Concrete Decorator for encryption
public class EncryptionDecorator extends Decorator {
    public EncryptionDecorator(Component component) {
        super(component);
    }

    @Override
    public void doOperation() {
        System.out.println("Encrypting the content.");
        super.doOperation();
        System.out.println("Content encrypted.");
    }
}

// Usage of Decorators
public class Client {
    public static void main(String[] args) {
        Component component = new ConcreteComponent();
        Component decoratedComponent = new CompressionDecorator(new EncryptionDecorator(component));
        decoratedComponent.doOperation();
    }
}
```

Inheritance-based solutions for problems of this nature are not very flexible. Can you imagine what happens if we had several functionality options (encryption, compression, authentication, logging, etc.)?

Inheritance can generate inflexible and complex code. See below what could be done and the resulting situation:

| Compression/Invert | Problematic Situation |
| :--- | :--- |
| We could define the `Compression` class that inherits from `Invert`. And the same with the `Invert` class that inherits from `Compression`. What if we had other options like: `Encryption`, `Authentication`, `Logging`, etc. | The number of classes would explode (e.g., `CompressionEncryption`, `CompressionInversion`, `EncryptionInversion`, `EncryptionCompression`, `AuthEncryption`, `AuthLogging`, for example). |

Therefore, the problem solved by the **Decorator** pattern is to add new behaviors dynamically and transparently to an object, without changing its internal structure. The object is wrapped by *decorators* that implement the same interface as it. This allows these elements to be combined in various ways, offering **flexibility** and **extension**.

-----

## Solution, consequences, and patterns related to the Decorator pattern

The **Decorator** pattern allows adding responsibilities to objects dynamically and transparently, without changing their internal structure. It is a structural object pattern that uses **object composition** to extend or alter a class's functionality.

**Structure of the Decorator pattern**
The design solution of the Decorator pattern is represented in the class diagram below:

*Class Diagram: `Component` and `Decorator` Interfaces. `ConcreteComponent` Class implements `Component`. `Decorator` Class implements `Component` and contains a reference to `Component`. `ConcreteDecoratorA` and `ConcreteDecoratorB` Classes inherit from `Decorator`.*

Now, let's understand the role of the participants in the structure presented below:

1.  **Component**: Defines the interface for objects that can have responsibilities added dynamically.
2.  **ConcreteComponent**: Defines the object to which responsibilities are to be added.
3.  **Decorator**: Maintains a reference to the **Component** object and conforms to the **Component** interface.
4.  **ConcreteDecorator**: Adds responsibilities to the **Component**.

An important characteristic of the **Decorator** pattern is that the composition is **recursive**, meaning a **Decorator** can wrap another **Decorator** (which in turn wraps another **Decorator**, and so on), which is not possible with the **Adapter** pattern.

*Class Diagram: Instances of `ConcreteComponent`, `ConcreteDecoratorA`, and `ConcreteDecoratorB`. `ConcreteDecoratorB` points to `ConcreteDecoratorA`, which points to `ConcreteComponent`.*

**Now, let's understand the role of the classes presented below:**

1.  **Concrete Component**: Original class (unmodified).
2.  **Concrete Decorator A**: Adds a functionality.
3.  **Concrete Decorator B**: Adds another functionality, different from A.

What happens is that when invoking the `doOperation()` method on **Concrete Decorator B**, it delegates the call to **Concrete Decorator A**, which in turn delegates to the **Concrete Component**, adding the functionalities hierarchically (B, A, and Component).

The presented code uses the ability to create objects that implement the `Component` interface, through *decorators* that enhance the main class's functionality dynamically at runtime.

**Code (Java example)**

```java
public class ConcreteComponent implements Component {
    @Override
    public void doOperation() {
        System.out.println("Executing the main operation.");
    }
}
//... (other classes and code for Decorators)
```

The **`ConcreteComponent` class** is the class that contains the **main functionality**, without modifications.

**Code (Java usage example)**

```java
// Create the base object
Component component = new ConcreteComponent();

// Decorate with Encryption and then with Compression
Component decoratedComponent = new CompressionDecorator(new EncryptionDecorator(component));

System.out.println("Starting the decorated operation...");
decoratedComponent.doOperation();
System.out.println("Decorated operation finished.");

// Create another decorated object
Component anotherDecorated = new EncryptionDecorator(component);
System.out.println("\nStarting another decorated operation...");
anotherDecorated.doOperation();
System.out.println("Another decorated operation finished.");
```

### Consequences

The **Decorator** pattern is used to add or extend an object's behavior dynamically. It is very flexible, as the combination of *decorators* allows adding different functionalities at runtime without changing the internal structure of the classes.
The disadvantage is that an object wrapped by many *decorators* becomes difficult to debug and trace code execution.

### Related Patterns

| Adapter | Decorator |
| :--- | :--- |
| The Adapter is used to make an existing interface work with a client class that expects a different interface, changing the interface. | The Decorator is used to extend an object's behavior dynamically, keeping the same interface. |
| Does not support recursive composition. | Supports recursive composition, allowing one *decorator* to wrap another *decorator*. |