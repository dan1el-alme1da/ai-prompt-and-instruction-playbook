
-----

## 1\. Composite and Facade Design Patterns

### Intention and Problem Solved by the Composite Pattern

The **Composite** pattern's intention is to allow the creation of tree structures to represent **part-whole hierarchies**. It solves the problem of treating **individual objects and compositions of objects uniformly**, meaning the client code can use the same interface to manipulate both simple objects and collections of objects.

**Composite** is ideal for representing hierarchical structures where parts can be treated equally. It simplifies the usage and restructuring of code, as well as the handling of complex structures within a system.

### Intention of the Composite Pattern

The **Composite** pattern allows for **representing composition hierarchies of objects in a tree structure**, where each object (individual) or a group of objects (composition) responds to the same set of operations.

### Problem Solved by the Composite Pattern

Suppose you are developing a message sending and receiving program. Initially, you have classes for sending and receiving messages for a single user (simple objects).

$$\text{Diagram representing a single user for sending and receiving messages.}$$

You need to apply operations to both objects (simple and aggregate): pay a message, encrypt a message, among others.

These same operations apply to the **pattern**, where **delegating a message** to multiple users, **encrypting messages** from multiple users, and **deleting messages** from multiple users.

The **Composite** pattern is particularly useful for problems involving the representation of element hierarchies where we observe redundancy in the execution of operations applied to aggregates.

The following code illustrates the structure of the implementation for the problem without using the Composite pattern:

```java
public interface ServicosMensagem {
	void enviarMensagem(Mensagem mensagem, Cliente cliente);
	void apagarMensagem(Mensagem mensagem, Cliente cliente);
	void criptografar(Mensagem mensagem, Cliente cliente);
}

public class Cliente implements ServicosMensagem {
	@Override
	public void enviarMensagem(Mensagem mensagem, Cliente cliente) {
		// Implementation of sending
	}
	@Override
	public void apagarMensagem(Mensagem mensagem, Cliente cliente) {
		// Implementation of deletion
	}
	@Override
	public void criptografar(Mensagem mensagem, Cliente cliente) {
		// Implementation of encryption
	}
}

public class GrupoClientes implements ServicosMensagem {
	@Override
	public void enviarMensagem(Mensagem mensagem, Cliente cliente) {
		// Implementation for all clients
	}
	@Override
	public void apagarMensagem(Mensagem mensagem, Cliente cliente) {
		// Implementation for all clients
	}
	@Override
	public void criptografar(Mensagem mensagem, Cliente cliente) {
		// Implementation for all clients
	}
}
```

This code presents problems because, in addition to the distinct implementations of operations for different elements, the **GrupoClientes** (Client Group) needs to have more operations, such as add (message or posts), which identifies the operation to be executed (send message or delete).

The addition of new operations and new object types will certainly increase the complexity of this implementation.

-----

## 1\. Composite and Facade Design Patterns

### Solution, Consequences, and Related Patterns for the Composite Pattern

The proposed solution for the problem solved by the **Composite** pattern is based on the idea of using a single interface that will serve both for simple and composite elements. This interface is called **Component**.

The **Composite** pattern proposes the creation of an object structure hierarchically, through three key elements: **Component**, **Leaf**, and **Composite**.

  * The **Component** (or Component, in the UML diagram) is a module (interface) that defines the operations that will be implemented in the elements of the hierarchy. It can define a default implementation for these operations, which can be overridden by the elements (Leaves or other aggregates).
  * The **Leaf** represents the elements that do not contain other objects. They are the simplest objects. They represent the primitive elements of the pattern.
  * The **Client Participant** is any module (client) that uses the elements of the hierarchy.
  * The **Composite** represents the elements that can have other Components (Leaves or other Composites).
  * Furthermore, the **Composite** defines the implementation for the operations of managing aggregate objects, allowing the addition and removal of child elements, in addition to having the operations defined in the **Component**.

The structure of the solution proposed by the **Composite** pattern is represented in the diagram below:

$$\text{UML Diagram of the Composite Pattern Solution}$$

$$
\begin{array}{c}
\text{Client} \\
\uparrow \\
\begin{array}{|c|} \hline \text{Component} \\ \hline \text{Operation()} \\ \text{Add(Component)} \\ \text{Remove(Component)} \\ \hline \end{array} \\
\swarrow \quad \downarrow \quad \searrow \\
\begin{array}{|c|} \hline \text{Leaf} \\ \hline \text{Operation()} \\ \hline \end{array} \quad
\begin{array}{|c|} \hline \text{Composite} \\ \hline \text{Child components} \\ \hline \text{Operation()} \\ \text{Add(Component)} \\ \text{Remove(Component)} \\ \hline \end{array}
\end{array}
$$The central idea of the pattern is to represent the hierarchy so that all elements have a generic superclass interface (the **Component** interface), which allows the client code to uniformly manipulate the **Leaf** (simple objects) and the **Composite** (aggregates).

The **Component** class defines the operations that must be implemented by all elements in the hierarchy. The interface defines methods to access and manipulate descendant elements, while the **Component** participant represents the aggregators of elements (Leaves or other aggregates).

Aggregates are represented by the **Composite** participant, which contains and manipulates the list of child elements.

The **Client** participant represents any module (client) that uses the elements of the hierarchy.

The **Leaf** class is the simplest class, representing elements without aggregates.

The following code structure illustrates the application of the pattern in the message program example:

```java
public interface ServicosMensagem {
void enviarMensagem(Mensagem mensagem, Cliente cliente);
void apagarMensagem(Mensagem mensagem, Cliente cliente);
void criptografar(Mensagem mensagem, Cliente cliente);
void adicionar(ServicosMensagem servico);
void remover(ServicosMensagem servico);
}

public class Cliente implements ServicosMensagem {
// Implementations of operations...
// ...

@Override
public void adicionar(ServicosMensagem servico) {
// Does nothing, Client is a Leaf element.
}

@Override
public void remover(ServicosMensagem servico) {
// Does nothing, Client is a Leaf element.
}
}

public class GrupoClientes implements ServicosMensagem {
private List<ServicosMensagem> lista = new ArrayList<>();

// Implementations of operations (delegating to list elements)...
// ...

@Override
public void adicionar(ServicosMensagem servico) {
lista.add(servico);
}

@Override
public void remover(ServicosMensagem servico) {
lista.remove(servico);
}
}
```

Note that the following key elements of the pattern are understood:

* The **ServicosMensagem** interface is the **Component**.
* The **GrupoClientes** participant is the **Composite**, which stores the list of aggregation elements (objects), in addition to the add and remove operations. It applies/executes a message to multiple elements as if they were only one.
* The **Cliente** participant is the **Leaf**, which does not have the capacity to aggregate other objects; it only implements the operations to manipulate a message.
* The code is now more robust and concise because the **ServicosMensagem** interface enforces the implementation of operations by all elements in the hierarchy. This is in addition to the specific behavior for the **apagar** (delete) and **criptografar** (encrypt) operations.

**Consequences and Related Patterns for the Composite Pattern**

The **Composite** pattern has a set of consequences, both positive and negative.

| Advantage | Disadvantage |
|---|---|
| Client code becomes simpler because it does not need to know whether it is manipulating a simple object (**Leaf**) or a composition (**Composite**), simplifying programming and avoiding code repetition. | The common interface for all elements can become too generic, including child management methods (such as `add` and `remove`) that do not apply to **Leaves**. |
| Simplifies the addition of new types of components (**Leaves** or **Composites**), as they only need to implement the **Component** interface. | |

The **Factory** pattern is often used to parameterize the elements of a hierarchy implemented with the **Composite** pattern.

The **Decorator** pattern is also frequently used with the **Composite** pattern. In this case, the **Decorator** adds new responsibilities to the elements, and the **Composite** organizes the hierarchy into a tree structure, allowing the new responsibilities to be applied to all **Component** participants.

-----

## 1\. Composite and Facade Design Patterns

### Solution, Consequences, and Related Patterns for the Facade Pattern

The **Facade** pattern provides a solution by creating a **simplified and unified interface** for a complex set of interfaces in a subsystem. The core idea is to define a **high-level interface** that simplifies the use of the subsystem for the client. However, a more specific interface that might still be needed can also be chosen.

The **Facade** pattern is generally used to work with existing structures. However, it can be used in more complex structures. It solves the problem of treating individual objects and compositions of objects uniformly, meaning the client code can use the same interface to manipulate both simple objects and collections of objects.

The **Facade** pattern aims to offer the solution to the problem solved by the **Facade** pattern, which allows for providing a simplified interface for a complex subsystem, providing a lighter and simpler interface, allowing communication between objects, and avoiding excessive coupling between the client and the subsystem.

### Intention of the Facade Pattern

The **Facade** pattern's purpose is to **provide a high-level interface for a component or subsystem**, which makes the subsystem easier to use.

### Problem Solved by the Facade Pattern

Imagine you are implementing an **e-commerce purchase system** with various complex functionalities, such as stock control, cart validation, shipping calculation, business rules, and data persistence. The idea is to create a common point for these functionalities and simplify the components in layers.

In this separation, one layer would contain the elements that make up the system, such as **Estoque** (Stock, responsible for stock control), **LogicaNegocio** (Business Logic, responsible for business rules), **Persistencia** (Persistence, responsible for data retrieval), **Integracao** (Integration, with external systems and devices), among others.

Suppose that to implement a part of the purchase page, the code needs to manipulate:

* a **CarrinhoRepository** (Cart Repository) object to retrieve the client's cart;
* a **Carrinho** (Cart) object to add products to the client's cart;
* a **ProdutoRepository** (Product Repository) object to retrieve client information and movements;
* a **ValidacaoCarrinho** (Cart Validation) object to check if the cart meets business rules;
* a **Frete** (Shipping) object to calculate the shipping value and due dates;
* a **LogicaNegocio** (Business Logic) object for business rules.

The dependency structure of these subsystems is presented in the class diagrams below:

#### UML Diagram: Facade (High-Level View)

$$\\begin{array}{c}
\\text{Client} \\
\\downarrow \\
\\begin{array}{|c|} \\hline \\text{Facade} \\ \\hline \\end{array} \\
\\downarrow \\
\\text{Element\_1} \\quad \\text{Element\_2} \\quad \\text{Element\_n}
\\end{array}
$$\#\#\#\# UML Diagram: Facade with Subsystems and Dependent Classes (Dependency Structure)

The dependency structure for implementing new subsystems:

$$
\begin{array}{c}
\text{FacadeCompra} \\
\uparrow \\
\begin{array}{|c|} \hline \text{CarrinhoRepository} \\ \hline \end{array} \quad
\begin{array}{|c|} \hline \text{Carrinho} \\ \hline \end{array} \quad
\begin{array}{|c|} \hline \text{ProdutoRepository} \\ \hline \end{array} \quad
\begin{array}{|c|} \hline \text{ValidacaoCarrinho} \\ \hline \end{array} \quad
\begin{array}{|c|} \hline \text{Frete} \\ \hline \end{array} \quad
\begin{array}{|c|} \hline \text{LogicaNegocio} \\ \hline \end{array}
\end{array}
$$The **Facade** pattern solves the problem because it **simplifies communication** and the coupling requirements of the client with a subsystem. With the **Facade**, the client only needs to interact with the **Facade**, which delegates the call to the subsystems and classes that actually perform the operation.

Thus, the problem solved by the **Facade** pattern is that a module uses a complex subsystem or layer of a system without needing to know or manipulate various elements.

**Consequences and Related Patterns for the Facade Pattern**

The **Facade** pattern has a set of consequences, both positive and negative.

* **Advantage:** The client is coupled to a simplified and unified interface, reducing the complexity of using the subsystem. It allows for the organization of the subsystem's layers. The subsystem can be modified without affecting the clients (modules).
* **Disadvantage:** The **Facade** pattern can become a **"God Object"** if implemented incorrectly, concentrating too many responsibilities in a single class.

Some design patterns are applied in conjunction with the **Facade** pattern:

* **Abstract Factory Pattern:** The **Abstract Factory** (or **Toolkit**) pattern for creating dependencies can be used by the **Facade** to **encapsulate** the creation of the elements that the **Facade** uses.
* **Mediator Pattern:** The **Mediator** pattern has a structure similar to the **Facade**, but its purpose is to **eliminate communication** between objects in the same layer, **automating** functionalities that are not of the **command** type.
* **Adapter Pattern:** The **Facade** pattern's purpose is to **provide a high-level interface** for clients with low coupling, and the **Adapter** pattern's purpose is to **convert one interface to another** that the client expects.
$$