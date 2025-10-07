
## 1\. Abstract Factory Design Pattern

### Consequences and related patterns to Abstract Factory

The **Abstract Factory** design pattern is a creational pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes. By doing this, it promotes **low coupling** and **encapsulation** by centralizing the creation and use of abstract objects, while concrete instances are resolved at runtime.

This pattern allows for **centralized configuration** and system **extensibility**, as adding new products or families does not require modifying the client code.

There are some creational patterns that resemble Abstract Factory or are commonly used with it:

  * **Factory Method:** This is a creational pattern that aims to define an interface for creating an object, but lets subclasses decide which class to instantiate. It is a method that is implemented in the subclasses.
  * **Singleton:** This pattern ensures that a class has only one instance and provides a global access point to it.

Explore the notes below on the consequences and patterns related to Abstract Factory.

The Abstract Factory pattern promotes the **encapsulation** of the object creation process, isolating clients from the specific details of how objects are created. This simplifies maintenance and modification in implementations. Furthermore, this pattern promotes **cohesion** between related products, meaning it guarantees that all objects created by a concrete factory belong to the same family, preventing incompatible objects from being mixed. It allows the system to be **configurable** at runtime by using a new naming convention for creation in each structure factory.

It is important for **centralized configuration**, as applications that use Abstract Factory can have coupling implemented using the Factory Method pattern. It is possible to configure several rules, using the configuration file, allowing the substitution of concrete classes of the structure. For example, a class can receive a Factory object to instantiate other objects.

In the previous example, we could also add the application of similar code for implementation, which allows for the simple substitution of objects. For example, we can have a generic **Decorator** pattern (a specific encapsulation method) and transform these operational components into generic objects so that clients do not need to know the specific details of the encapsulated objects.

$${\text{Decoration Example}}$$

Study the following notes about the structure of the example, applying the Decoration criteria.

To improve the initial application, the first step is the definition of the interfaces (**Decorator**) and the concrete products. Thus, concrete classes extend the interfaces (Decorator) and the return organizes the Singleton pattern.

$${\text{Java (UBL Decoder)}}$$

```java
public class UBLDecoder implements Decoder {
    private final String origin;

    public UBLDecoder(String origin) {
        this.origin = origin;
    }

    @Override
    public String decode(String msg) {
        // UBL decoder implementation
        return "Decoded by UBL: " + msg.toUpperCase() + " (origin: " + origin + ")";
    }
}
```

As many common patterns exist, the **Template Method** pattern can be used, defining the core of the algorithm in a superclass, leaving subclasses to implement the rest.

$${\text{Java (Generic Decoder)}}$$

```java
public abstract class GenericDecoder implements Decoder {
    private final String origin;

    public GenericDecoder(String origin) {
        this.origin = origin;
    }

    @Override
    public final String decode(String msg) {
        // Common decoding logic
        String processedMsg = preprocess(msg);
        String decoded = doDecode(processedMsg);
        return postprocess(decoded);
    }

    protected abstract String doDecode(String msg);

    protected String preprocess(String msg) {
        return msg.trim();
    }

    protected String postprocess(String decodedMsg) {
        return decodedMsg + " (Processed - " + this.getClass().getSimpleName() + " from " + origin + ")";
    }
}
```

Finally, the service can be implemented simply, with a structure similar to a Factory Method with Decoration.

$${\text{Java (Message Service)}}$$

```java
public class MessageService {
    private final DecoderFactory decoderFactory;

    public MessageService(DecoderFactory decoderFactory) {
        this.decoderFactory = decoderFactory;
    }

    public String processMessage(String msg, String format) {
        Decoder decoder = decoderFactory.createDecoder(format);
        return decoder.decode(msg);
    }
}
```

-----

## 1\. Abstract Factory Design Pattern

### Intention and problem of the Abstract Factory pattern

According to the **Abstract Factory** pattern, implementations are not allowed to vary on the format, the client creates them. The main intention of the pattern is to provide an interface for creating families of related or dependent objects without specifying their concrete classes. By doing so, it solves the problem of coupling and dependency on concrete implementations at runtime, allowing the application to be configured to use different product families without changing the client code.

Observe in the following video the intention and problem of the Abstract Factory pattern.

The **Abstract Factory** is a pattern that provides an interface for the creation of families of related or dependent objects, without specifying their concrete classes, allowing the client code to handle objects through their abstract interfaces.

For example, consider a messaging system that needs to send data in different formats (XML, EDI, Fixed Text, etc.) and to different organizations. Consider that the clients or areas of each organization can only use the format assigned to them. Furthermore, the rules of each organization differ in how messages are stored (database, files, JMS, etc.).

Below, we present an example table that represents the need for different registration and destination formats for each organization.

| Organization | Internal Registration Format | External Destination Format |
| :---: | :---: | :---: |
| Organization 1 | Internal Registration | XML |
| Organization 2 | Short Registration | EDI |
| Organization 3 | Internal Registration | Fixed Text |
| Organization 4 | Short Registration | XML |
| Organization 5 | Short Registration | Fixed-size field |

We can define a set of classes responsible for decoding messages of a specific format into objects independent of that format. Thus, the Abstract Factory can be structured as follows:

$${\text{Abstract Factory Structure for Registrations}}$$
$${\text{Class Diagram: Abstract Factory Structure}}$$

The **RegistroAbstractFactory** interface represents the generic service that creates a client registration message, respecting the defined rules. The pattern structure defines an interface to create registration objects, but leaves the decision of which object to instantiate to the concrete implementations (subclasses). Thus, the client code does not need to know the concrete implementations.

The implementations of this abstract interface for each specific format are:

  * XML
  * EDI
  * Fixed Text
  * Fixed-size field

To integrate this pattern into our system (Client Registration, Short Registration), a class structure that the client must use must be created.

The class diagram shows the interfaces and concrete classes for different registration types and decoding formats. The abstract classes create the (received) messages from different organizations, and the concrete implementations create the concrete objects (such as **XMLFormatado** or **EDIFormatado**). The client interacts only with the abstract interface **RegistroAbstractFactory** to create its products.

$${\text{Java (Interfaces and Abstract Classes)}}$$

```java
// Interfaces for abstract products
interface Formatado {
    String formatar(String registro);
}

// Interfaces for the abstract factory
interface RegistroAbstractFactory {
    Formatado criarFormatoMensagem();
}

// Concrete factory implementation (example for XML)
class RegistroInternoAbstractFactory implements RegistroAbstractFactory {
    @Override
    public Formatado criarFormatoMensagem() {
        return new XMLFormatado(); // Concrete product
    }
}
```

This pattern is present in all possible types of decoding, convincing the entire complexity of interaction between the client and the abstract factory. Concrete factories encapsulate the details of how objects are created, ensuring that related object classes are created cohesively. Thus, each organization will have to be modified, which configures a centralization of object creation. The client uses one of the SOLID principles.

The **concrete factory classes** implement the abstract factory interface. They are responsible for creating and returning the concrete instances of the products. For example, the **RegistroInternoAbstractFactory** is the concrete factory that creates the **XMLFormatado** instance for Organization 1 and the **RegistroCurtoAbstractFactory** for Organization 2.

The application of this pattern offers the following advantages:

  * **Isolates the client of a family of related products from its specific implementations.**
  * **Makes the system configurable**, allowing it to work with other decoding formats without the client's code needing to be changed.
  * **Ensures cohesion** between the created products.

-----

## 1\. Abstract Factory Design Pattern

### Solution for the Abstract Factory pattern

According to the examples presented, the **Abstract Factory** pattern defines an abstract interface (**RegistroAbstractFactory**) to create families of products (**Formatado**). Concrete factories (**RegistroInternoAbstractFactory**, **RegistroCurtoAbstractFactory**, etc.) implement this interface to produce concrete products (**XMLFormatado**, **EDIFormatado**, etc.) that belong to the same family. By doing this, we isolate the client from the concrete classes and ensure that the products created by a concrete factory are compatible with each other. The client interacts only with the abstract interfaces and classes, maintaining the characteristics of **low coupling** and **high cohesion**.

Observe in the following video the solution for the Abstract Factory pattern.

The solution structure proposed by the Abstract Factory pattern is represented in the class diagram below.

$${\text{Class Diagram: Abstract Factory Solution}}$$

On the **client side**, objects are created by the factory classes. Each product type is defined by an abstract interface, for example, **AbstractProductA** and **AbstractProductB**. These interfaces define the methods that all products in this category must implement.

Concrete classes, such as **ProductA1** and **ProductA2**, are specific implementations of **AbstractProductA**. They provide the real logic for the methods defined in the interface. They represent the concrete products **ProductA1** and **ProductA2**.

Concrete factory classes, such as **ConcreteFactory1** and **ConcreteFactory2**, implement the **AbstractFactory** interface. They are responsible for creating and returning the concrete instances of the products. For example, **ConcreteFactory1** creates **ProductA1** and **ProductB1**, and **ConcreteFactory2** creates **ProductA2** and **ProductB2**.

The concrete factory classes (ConcreteFactory1 and ConcreteFactory2) implement the abstract factory interface. They are responsible for creating and returning the concrete instances of the products. For example, **ConcreteFactory1** is the concrete factory that creates the instance of **ProductA1** and **ProductB1** of family 1. Similarly, **ConcreteFactory2** is the concrete factory that creates the instance of **ProductA2** and **ProductB2** of family 2.

Observe below a class diagram that illustrates the integration with the application of the structure proposed by the Abstract Factory pattern.

$${\text{Class Diagram: Integration of Abstract Factory with the Message Service}}$$

  * **Decoder Families:**

In the presented example, the abstract classes **AbstractDecoderFactory** and **DecoderFactory** define an abstract interface to create families of **Decoder** objects (**UBLDecoder**, **EDIDecoder**), responsible for decoding messages in the **UBL** or **EDI** format.

The client **MessageService**, which uses the **DecoderFactory** interface, can be configured to use any concrete factory, such as **UBLDecoderFactory** or **EDIDecoderFactory**, without knowing the concrete classes that are being used. This ensures that the application is configurable for different decoding formats without changing the client code.

Only the concrete decoder classes (**UBLDecoder** and **EDIDecoder**) implement the **Decoder** interface. For example, **UBLDecoder** is the decoding implementation for the UBL format, while **EDIDecoder** is the decoding implementation for the EDI format.

$${\text{Java (Interfaces and Abstract Factories)}}$$

```java
// Interfaces for abstract products
interface Decoder {
    String decode(String msg);
}

// Interfaces for abstract factories
interface DecoderFactory {
    Decoder createDecoder();
}

// Concrete factory implementation (example for UBL)
class UBLDecoderFactory implements DecoderFactory {
    @Override
    public Decoder createDecoder() {
        return new UBLDecoder(); // Concrete product
    }
}
```

Now let's use the techniques to simplify the organization of the specialization of the **DecoderAbstractFactory** class and use the Decorator pattern. The Decorator class (**DecoratorDecoder**) will be used to encapsulate the real decoder and add extra functionalities, such as logging or other operations before/after decoding.

$${\text{Java (Decorator Decoder)}}$$

```java
class DecoratorDecoder implements Decoder {
    private final Decoder wrappedDecoder;

    public DecoratorDecoder(Decoder wrappedDecoder) {
        this.wrappedDecoder = wrappedDecoder;
    }

    @Override
    public String decode(String msg) {
        System.out.println("LOG: Pre-decoding message...");
        String result = wrappedDecoder.decode(msg);
        System.out.println("LOG: Post-decoding finished.");
        return result;
    }
}
```

$${\text{Java (Message Service with Decorator)}}$$

```java
class MessageService {
    private final DecoderFactory decoderFactory;

    public MessageService(DecoderFactory decoderFactory) {
        this.decoderFactory = decoderFactory;
    }

    public String processMessage(String msg) {
        Decoder decoder = new DecoratorDecoder(decoderFactory.createDecoder());
        return decoder.decode(msg);
    }
}
```

Note that the new code will not need to be modified for new message formats, as it will be enough to create a new concrete factory and its decoders.

## This pattern is useful, for example, in the implementation of the **EJB framework** or the user interface **GUI**, where it is necessary to create different families of *widgets* (buttons, *text fields*, etc.) that are consistent in their appearance. Thus, the **Abstract Factory** is a technique that allows the creation of related objects in different product families (such as *Motif* and *Open Look*), and it is a technique that offers an interface to create families of related or dependent objects without specifying their concrete classes.

## 2\. Abstract Factory Design Pattern

### Abstract Factory X Dependency Injection

**Dependency Injection** and **Abstract Factory** are common concepts in **object-oriented programming** and often work together. When injecting a dependency, such as in the use of persistence components in the **Spring Framework**, the **DAO** class to be injected, instead of being a concrete class, is an interface that is replaced by the **Abstract Factory** pattern, offering a simpler approach to decoupling. This happens when concrete classes are resolved at runtime, differing only in the writing model, without changing the functionality.

Watch the following video to see how the dependency injection *framework* can replace a manual implementation of the Abstract Factory pattern.

A classic example of using the **Abstract Factory** pattern is the **implementation of persistence** in **databases**. Instead of the client directly manipulating the **DAO** (Data Access Object) object, an interface is created for generating an **abstract DAO**, which foresees methods for including, changing, deleting, and querying records. By directing to a specific database, we have the **concrete factory** and the **concrete DAO**, which is the one that actually implements the data manipulation.

In an implementation, using **SQL, ANSI**, much of the access code, more for the search and data maintenance part, could be outsourced to the layer. In this case, it is leveraged by *frameworks*, such as **Hibernate** or **Spring**, which centralize the business rules in their locations, promoting **dependency injection**, and leaving the **factory** with the responsibility of executing the necessary **SQL commands**.

Internally, we have an **abstract factory** and **Spring** uses it for **connection configuration**, that is, using the connection information, in addition to the *driver* used, to generate each **SQL command** correctly, based on the specific database. The factory uses **JDBC** and the **specific API** to return an **Entity** object. The entire mechanism is activated in a **service** when a **DAO** instance is annotated with **@Autowired**, which configures **dependency injection**, where the control of the invoked processes starts to consume the **contextualization** of the **class** that always **manages** the **factory**.