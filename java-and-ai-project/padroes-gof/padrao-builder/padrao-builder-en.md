
-----

## 1\. Builder Design Pattern

### Intent and Problem of the Builder Pattern

In many situations, the construction of a complex object can lead to the definition of multiple constructors in the code. The problem intensifies as new parameters are added, requiring the redefinition of all these constructors.

The **Builder** pattern allows for dissociating the methods that generate the construction steps so that these steps can be reused in the construction of objects that may have different representations.
Note that the Builder is increasingly present in various libraries. **Javalin**, for example, allows the user to define a Builder interface to compose the API **handlers**. **Lombok**, when present, allows data integration in formats such as **JSON** and **XML**.

The pattern aims to mitigate the limitation and problem of the **Builder** pattern.

#### Intent of the Builder Pattern

To isolate the part that knows how to expose the construction of a complex object from its representation, so that the same construction process can build different representations of that object.

#### Problem of the Builder Pattern

Imagine you need a mechanism for a mediating system conversation, and this system allows the client to export its negotiation notes in different formats, such as **JSON** and **XML**.

Imagine that the process of constructing any representation of a negotiation note is defined by using the following operations:

  * **Query**:
      * Query information about the notes.
  * **Identify**:
      * Identify and catalog the negotiation notes.
  * **Use**:
      * Document the note operations.
  * **Generate**:
      * Generate a summary with the totals and tickets of all daily operations.

A solution frequently used for this problem is to apply all the discussed operations in an export method, as per the following code:

```java
public class ExportaNota {
    // ... other attributes
    public String exportaNota(List<Negociacao> negociacoes, String formato, Exportador exportador) {
        String resultado = "";
        if (formato.equals("JSON")) {
            resultado += "{\n";
            resultado += " \"negociacoes\": [\n";
            // ... conversion code for JSON
            resultado += "]\n";
            resultado += "}";
        } else if (formato.equals("XML")) {
            resultado += "<negociacoes>\n";
            // ... conversion code for XML
            resultado += "</negociacoes>\n";
        }
        return resultado;
    }
}
```

The **exportaNota** operation receives the list of negotiations and will export to the export format defined in **formato** (format).

This approach has three (3) inadequacies, because, in addition to concentrating all the steps to export the negotiations in formats such as **JSON** or **XML** into a single method, the method is also concerned with the return for each specific format. Furthermore, the method must be modified with each new form of representation that the client wants. In such situations, the **Builder** pattern is the best alternative.

-----

## Builder Pattern Solution

The solution consists of having a common **Builder** that manages the creation of complex objects by the caller. The **Builder** is the central piece that hides all the complexity involved in creating the object, exposing only the necessary methods.

The **Director** manages the **Builder**, which constructs the parts of the **Product** and returns it.

### Class Diagram

The **Builder** architecture defines the operations that create the different parts of a product (a way to export a negotiation note in **JSON**, for example). The **ConcreteBuilder** implements the construction steps of the **Builder** and defines the product to be returned, containing all the different forms of representation (negotiation note in **JSON**, **XML**, **CSV**, **TEXT**, and others).

The **Director** manages the **Builder**, which constructs the parts of the **Product** and returns it.

### Pattern

The **Builder** pattern is a creational design pattern that allows complex objects to be constructed step by step. The **Builder** pattern isolates the product construction code so that the creation of the complex object can have different representations (formats for exporting negotiation notes: **JSON**, **XML**, **CSV**, **TEXT**, etc.), being controlled by a **Director**.

The concrete methods defined in the **Builder** should not be platform-dependent, but should determine the architecture and the order of operations for constructing complex objects.

The responsibilities of each class are:

  * **Builder**:
      * Defines an interface to create the parts of the **Product** object.
  * **ConcreteBuilder**:
      * Constructs and assembles the parts of the **Product** through the **Builder** interface.
      * Defines the internal representation of the **Product** and provides access.
  * **Director**:
      * Constructs an object using the **Builder** interface.
  * **Product**:
      * Represents the complex object that is being constructed.

### Relationship with the Abstract Factory Pattern

The **Builder** pattern is similar to the **Abstract Factory** pattern solution, as both construct complex objects. The main difference between the two patterns is:

| Pattern | Objective |
| :--- | :--- |
| **Builder** | Focuses on the manner of constructing a complex object in steps. |
| **Abstract Factory** | Focuses on the creation of families of products, with one product being returned and the pattern allowing the client only to use one operation call. |

### Applications of the Builder Design Pattern

A typical application of the **Builder** pattern is the replacement of multiple conditions, allowing the configuration only of the concrete classes that implement the builder.

-----

## Consequences and Patterns Related to the Builder

### Consequences of the Builder Pattern

The **Builder** pattern provides good control over the object construction process. This property makes it a powerful pattern, as it allows each construction step to be defined in interfaces and implemented in concrete classes, facilitating code alteration and extension.

Some consequences and patterns related to the **Builder**:

  * **Allows varying the product's internal representation:** It's possible to vary the product's internal representation by using the **ConcreteBuilder** in each variation.
  * **Encapsulates construction and representation code:** The **Builder** pattern hides the internal representation of the product.
  * **Provides fine control over the construction process:** The **Director** manages the **Builder**, defining the order of the **Builder**'s operations.

### Patterns Related to the Builder

The **Builder** pattern is frequently used in conjunction with other patterns.

  * The **Composite** pattern is used to represent objects composed of others in a tree representation hierarchy. The **Builder** can be used to construct complex **Composite**-type trees.
  * The **Abstract Factory** pattern, along with the **Builder**, can construct complex objects.
  * The **Singleton** pattern can be used to ensure that only one instance of the **Director** exists.