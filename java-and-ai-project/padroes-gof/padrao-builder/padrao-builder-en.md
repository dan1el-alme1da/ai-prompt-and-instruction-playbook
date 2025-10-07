
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

  * **Query**
      * Query information about the notes.
  * **Identify**
      * Identify and catalog the negotiation notes.
  * **Use**
      * Document the note operations.
  * **Generate**
      * Generate a summary with the totals and tickets of all daily operations.

A solution frequently used for this problem is to apply all the discussed operations in an export method, as per the following code:

```java
public class ExportaNota {
    // ... outros atributos
    public String exportaNota(List<Negociacao> negociacoes, String formato, Exportador exportador) {
        String resultado = "";
        if (formato.equals("JSON")) {
            resultado += "{\n";
            resultado += " \"negociacoes\": [\n";
            // ... código de conversão para JSON
            resultado += "]\n";
            resultado += "}";
        } else if (formato.equals("XML")) {
            resultado += "<negociacoes>\n";
            // ... código de conversão para XML
            resultado += "</negociacoes>\n";
        }
        return resultado;
    }
}
```

The **exportaNota** operation receives the list of negotiations and will export to the export format defined in **formato** (format).

This approach presents three (3) inadequacies, because, in addition to concentrating all the steps to export the negotiations in formats such as **JSON** or **XML** into a single method, the method is also concerned with the return for each specific format. Furthermore, the method must be modified with each new form of representation that the client wants. In such situations, the **Builder** pattern is the best alternative.

-----

## Builder Pattern Solution

The solution consists of having a common **Builder** (Constructor) that manages the creation of complex objects by the caller. The **Builder** is the central piece that hides all the complexity involved in creating the object, exposing only the necessary methods.

The **Director** manages the **Builder**, which constructs the parts of the **Product** and returns it.

### Class Diagram

The **Builder** architecture defines the operations that create the different parts of a product (a way to export a negotiation note in **JSON**, for example). The **ConcreteBuilder** implements the construction steps of the **Builder** and defines the product to be returned, containing all the different forms of representation (negotiation note in **JSON**, **XML**, **CSV**, **TEXT**, and others).

The **Director** manages the **Builder**, which constructs the parts of the **Product** and returns it.

**[Diagram illustrating the interaction between Director, Builder, ConcreteBuilder, and Product classes]**

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

The **NotaNegociacaoExportavelBuilderImpl** class is an example of a client of the **ExportavelBuilder** interface. The **build()** method returns the instance of **NotaNegociacaoExportavelImpl**.

```java
// Código de exemplo para a classe NotaNegociacaoExportavelBuilderImpl (Example code for the NotaNegociacaoExportavelBuilderImpl class)
public class NotaNegociacaoExportavelBuilderImpl implements ExportavelBuilder<NotaNegociacaoExportavel> {
    private NotaNegociacaoExportavelImpl notaExportavel;

    public NotaNegociacaoExportavelBuilderImpl(NotaNegociacaoExportavelImpl notaExportavel) {
        this.notaExportavel = notaExportavel;
    }

    @Override
    public ExportavelBuilder<NotaNegociacaoExportavel> comTipo(String tipo) {
        this.notaExportavel.setTipo(tipo);
        return this;
    }

    // ... outros métodos builder (other builder methods)
    
    @Override
    public NotaNegociacaoExportavel build() {
        return notaExportavel;
    }
}
```

The **NotaNegociacaoExportavelImpl** class implements the operations defined in **Exportavel**.

```java
// Código de exemplo para a classe NotaNegociacaoExportavelImpl (Example code for the NotaNegociacaoExportavelImpl class)
public class NotaNegociacaoExportavelImpl implements NotaNegociacaoExportavel {
    private String tipo;
    private double valorTotal;
    // ... outros atributos (other attributes)

    // Getters e Setters

    @Override
    public void exportar() {
        System.out.println("Exportando nota de negociação do tipo: " + tipo + " com valor total: " + valorTotal);
        // Lógica de exportação (Export logic)
    }
}
```

There are some important considerations in the implementation of the **Builder** pattern:

The **Builder** object, to be accessed, only has one product; after the execution of the **build()** method, a new **Builder** instance must be created to construct a new product.

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

### Applications of the Builder Design Pattern

A typical application of the **Builder** pattern is the replacement of multiple conditions, allowing the configuration only of the concrete classes that implement the constructor.

#### Builder Pattern Code Example

```java
// Definição da interface ExportavelBuilder (Definition of the ExportavelBuilder interface)
public interface ExportavelBuilder<T extends Exportavel> {
    ExportavelBuilder<T> comTipo(String tipo);
    ExportavelBuilder<T> comValorTotal(double valorTotal);
    // ... outros métodos builder (other builder methods)
    T build();
}
```

```java
// Definição da interface Exportavel (Definition of the Exportavel interface)
public interface Exportavel {
    void exportar();
}
```

Based on common constants, it is possible to define our **Product** and the most appropriate **Builder** for object generation.

```java
// Implementação da Product NotaNegociacaoExportavelImpl (Implementation of the Product NotaNegociacaoExportavelImpl)
public class NotaNegociacaoExportavelImpl implements NotaNegociacaoExportavel {
    private String tipo;
    private double valorTotal;
    // ... outros atributos (other attributes)

    // Getters e Setters

    @Override
    public void exportar() {
        System.out.println("Exportando nota de negociação do tipo: " + tipo + " com valor total: " + valorTotal);
        // Lógica de exportação (Export logic)
    }
}
```

```java
// Implementação do Builder NotaNegociacaoExportavelBuilderImpl (Implementation of the Builder NotaNegociacaoExportavelBuilderImpl)
public class NotaNegociacaoExportavelBuilderImpl implements ExportavelBuilder<NotaNegociacaoExportavel> {
    private NotaNegociacaoExportavelImpl notaExportavel;

    public NotaNegociacaoExportavelBuilderImpl() {
        this.notaExportavel = new NotaNegociacaoExportavelImpl();
    }

    @Override
    public ExportavelBuilder<NotaNegociacaoExportavel> comTipo(String tipo) {
        this.notaExportavel.setTipo(tipo);
        return this;
    }

    @Override
    public ExportavelBuilder<NotaNegociacaoExportavel> comValorTotal(double valorTotal) {
        this.notaExportavel.setValorTotal(valorTotal);
        return this;
    }

    @Override
    public NotaNegociacaoExportavel build() {
        return notaExportavel;
    }
}
```

Finally, it is possible to test the construction of the product using the configurations defined in the **NotaNegociacaoExportavelBuilderImpl** class.

```java
// Exemplo de uso (Usage Example)
public class Cliente {
    public static void main(String[] args) {
        NotaNegociacaoExportavel nota = new NotaNegociacaoExportavelBuilderImpl()
            .comTipo("Compra")
            .comValorTotal(1500.75)
            .build();
        
        nota.exportar();
    }
}
```