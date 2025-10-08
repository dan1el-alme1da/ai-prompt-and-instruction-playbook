
-----

## Chain of Responsibility Pattern

The **Chain of Responsibility** (CoR) behavioral design pattern allows a request to be passed along a chain of potential handler objects. Each handler in the chain is given the opportunity to process the request or pass it to the next handler in the chain.

### Benefits of the Chain of Responsibility Pattern

  * **Decouples** the sender of a request from its receivers. The request can be handled by any object in the chain.
  * **Allows multiple objects to handle a request**, without the sender needing to know exactly which object will perform the handling.
  * **Increases flexibility** in assigning responsibilities to objects. The handler chain can be changed at runtime.

### Practical Example for the Chain of Responsibility Pattern

Imagine an expense approval system. An expense request (the `Request` object) might be:

1.  Approved by the immediate manager if it's less than R$ 100.00.
2.  Approved by the department manager if it's less than R$ 1,000.00.
3.  Approved by the director if it's over R$ 1,000.00.
4.  Rejected if it's over R$ 10,000.00.

The Chain of Responsibility allows the request to travel through the chain of approvers, with each one deciding whether to handle it or pass it on.

### Solution for the Chain of Responsibility Pattern

The Chain of Responsibility design pattern is composed of three main participants:

1.  **Handler:** Defines the interface for handling the request and, optionally, implements the chaining mechanism.
2.  **ConcreteHandler:** Handles the requests it's responsible for and, if not, forwards them to its successor in the chain.
3.  **Client:** Initiates the request to the first object in the chain.

#### Class Diagram (Textual Representation)

```
       +-----------------------+
       |        Handler        |
       +-----------------------+
       | + setNext(Handler)    |
       | + handleRequest(Request)|
       +-----------------------+
                ^
               / \
              /   \
             /     \
    +----------+----------+----------+
    |  ConcreteHandlerA  |  ConcreteHandlerB  |  ConcreteHandlerC  |
    +----------+----------+----------+
    | + handleRequest(Request) | + handleRequest(Request) | + handleRequest(Request) |
    +----------+----------+----------+
```

*The diagram shows the **Handler** interface with the `setNext` method (to define the successor) and `handleRequest` (to process the request). The **ConcreteHandlerA, ConcreteHandlerB**, and **ConcreteHandlerC** classes implement the Handler interface and contain the logic for handling and forwarding the request.*

-----

## Command Pattern

The **Command** behavioral design pattern encapsulates a request as an object, allowing you to **parameterize** clients with different requests, queue or log requests, and support operations that can be undone (undo).

### Introduction to the Command Pattern

The Command pattern turns a request into an object of its own, which allows you to:

  * Pass requests as method arguments.
  * Create request queues.
  * Define callbacks to be executed at specific times.
  * Support "undo" and "redo" functionality.

### Implementation of the Command Pattern

The Command pattern is composed of four main participants:

1.  **Command:** Declares an interface for executing an operation.
2.  **ConcreteCommand:** Defines the binding between a **Receiver** object and an action. It implements the **Execute** method by invoking the relevant operation on the Receiver.
3.  **Invoker:** Asks the command to carry out the request. It holds a reference to a Command object.
4.  **Receiver:** Knows how to perform the operations associated with the request.

#### Table: Command Pattern Participants

| Participant | Function |
| :--- | :--- |
| **Command** | Defines the interface to execute the operation. |
| **ConcreteCommand** | Implements the Command interface, stores the reference to the **Receiver**, and calls the Receiver's operations. |
| **Invoker** | Object that triggers the **Execute()** method of the Command. |
| **Receiver** | Object that executes the logic of the operation. |

### Solution for the Command Pattern

The `Invoker` stores the `Command`, but knows nothing about the `Receiver` or the operation logic. The `Command` is responsible for invoking the action on the `Receiver`.

#### Class Diagram (Textual Representation)

```
+----------------+      +-----------------+       +----------------+
|    Client      |----->|     Invoker     |------>|    Command     |
+----------------+      +-----------------+       +----------------+
                        | - Command       |       | + Execute()    |
                        +-----------------+       +----------------+
                                                         ^
                                                        / \
                                                       /   \
                                                      /     \
                                           +--------------+
                                           | ConcreteCommand |
                                           +--------------+
                                           | - Receiver     |
                                           | + Execute()     |----> +----------------+
                                           +--------------+      |    Receiver    |
                                                                 | + Action()     |
                                                                 +----------------+
```

*The diagram shows the **Client** creating a **ConcreteCommand** and a **Receiver**, and configuring the **Invoker** with the **ConcreteCommand**. When the **Invoker** calls `Execute()` on the **Command**, the **ConcreteCommand** calls the `Action()` method on the **Receiver** to perform the work.*

-----

## Iterator Pattern

The **Iterator** behavioral design pattern provides a way to access the elements of an underlying **aggregate** object sequentially without exposing its internal representation.

### Introduction to the Iterator Pattern

The Iterator allows you to traverse collections of objects in a uniform manner, regardless of the specific type of collection (e.g., `List`, `Array`, `Map`).

The pattern defines two main interfaces:

  * **Iterator:** Defines the interface for accessing and traversing the elements of a collection.
  * **Aggregate:** Defines the interface for creating the corresponding Iterator object.

### Solution for the Iterator Pattern

The Iterator pattern separates the responsibility of collection traversal from the data structure itself. The `Aggregate` object (Collection) is responsible for creating the `Iterator`.

#### Class Participants:

  * **Iterator:** Defines an interface for accessing and traversing elements. Typically contains methods like `hasNext()`, `next()`, and optionally `remove()`.
  * **ConcreteIterator:** Implements the **Iterator** interface and manages the current position in the aggregation traversal.
  * **Aggregate:** Defines an interface for creating an **Iterator** object.
  * **ConcreteAggregate:** Implements the **Aggregate** interface and returns an instance of the appropriate **ConcreteIterator**.

#### Class Diagram (Textual Representation)

```
      +----------------+               +----------------+
      |    Iterator    |<------------- | ConcreteIterator |
      +----------------+               +----------------+
      | + hasNext()    |               | - Aggregate     |
      | + next()       |               | + hasNext()     |
      | + remove() (optional)|        | + next()        |
      +----------------+               +----------------+
             ^
            / \
           /   \
          /     \
+--------------+               +-------------------+
|   Aggregate  |--------------->| ConcreteAggregate |
+--------------+               +-------------------+
| + createIterator() |           | + createIterator()|
+--------------+               +-------------------+
```

*The diagram shows the **Iterator** interface and its implementation, **ConcreteIterator**. The **ConcreteAggregate** implements the **Aggregate** interface and is responsible for instantiating the **ConcreteIterator**, which knows its internal structure and how to traverse it.*