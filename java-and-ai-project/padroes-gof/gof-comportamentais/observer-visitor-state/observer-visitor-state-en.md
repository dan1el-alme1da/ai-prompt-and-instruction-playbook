
-----

# Translated Content

## Behavioral Design Patterns: Observer, Visitor, and State

### Visitor Pattern

The **Visitor** design pattern allows you to separate algorithms from the objects they operate on into different classes. This is particularly useful when you need to add new operations to a set of object classes without modifying those classes. Instead of adding a new method for each operation to every class, these operations are encapsulated in **Visitor** objects.

**Intention of the Visitor Pattern**

It represents an operation to be performed on the elements of an object structure. **Visitor** allows you to define a new operation without changing the classes of the elements on which it operates.

**Problem Solved by the Visitor Pattern**

Consider a system with different types of objects, such as **Elephant**, **Lion**, and **Monkey**, all derived from a base type **Animal**. If new operations were required, such as **Eat** or **Sleep**, each would require adding a new method to all classes, which would be costly and make the code confusing.

This class structure is common in languages for different formats, such as: checking if the instruction is valid, performing code optimization, etc.

Should this need arise, we can add a pattern object, called **Visitor**, that contains all the methods that the **Animal** can execute, and we pass the **Animal** object as a parameter.

The **Visitor pattern** allows us to **add new functionalities to existing classes** without needing to modify the original code of those classes. This is especially useful in projects with classes that cannot be modified (for example, libraries).

**Solution of the Visitor Pattern**

The class structure for the **Visitor** pattern is presented in the diagram and is as follows:

-----

**Code Observation:** The following source code shows an example of the **Visitor** interface implementation in Java, where each `visit()` method is overloaded for a specific object type.

```java
public interface Visitor {
    void visit(ElementA element);
    void visit(ElementB element);
    void visit(ElementC element);
}
```

And next, an example of the **Element** interface implementation, which has an `accept()` method to receive the **Visitor** and invoke the specific `visit()` method for it.

```java
public interface Element {
    void accept(Visitor visitor);
}
```

Below is the implementation of a **Concrete Element**, which implements the **Element** interface and provides the code for the `accept()` method.

```java
public class ConcreteElementA implements Element {
    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this); // Calls the specific visit method
    }
    // Other specific methods of ConcreteElementA
    public String operationA() {
        return "Operation A executed.";
    }
}
```

And finally, an example of the **Concrete Visitor** implementation, which implements the **Visitor** interface and provides the logic for each type of concrete element.

```java
public class ConcreteVisitor implements Visitor {
    @Override
    public void visit(ConcreteElementA element) {
        // Specific visitation logic for ConcreteElementA
        System.out.println("Visiting element A: " + element.operationA());
    }
    @Override
    public void visit(ConcreteElementB element) {
        // Specific visitation logic for ConcreteElementB
        System.out.println("Visiting element B.");
    }
    @Override
    public void visit(ConcreteElementC element) {
        // Specific visitation logic for ConcreteElementC
        System.out.println("Visiting element C.");
    }
}
```

-----

The **Visitor** pattern is useful in projects where the **set of object classes is expected to be stable**, but the **set of operations that can be performed** on these objects varies. This facilitates the introduction of new behaviors without modifying the object classes.

**Consequences and Patterns Related to Visitor**

The **Visitor** pattern aims for **interface separation** and promotes the **cohesion** of related behaviors into a single object.

It is useful when:

  * You have many different operations that need to be applied to objects of different classes that form a structure.
  * The classes that make up the structure rarely change, but you frequently need to define new operations on the structure.
  * You need to separate the algorithm from the object on which it operates.

**ATTENTION**
The **Visitor** pattern is very useful when used in conjunction with the **Composite** and **Iterator** patterns.

-----

-----

### Observer Pattern

The **Observer** design pattern allows an object, called the **Subject**, to notify other objects, called **Observers**, about any change in its state. Thus, it is possible to establish a **one-to-many dependency** relationship between objects, so that when one object (**Subject**) changes state, all its dependents (**Observers**) are automatically notified and updated.

**Intention of the Observer Pattern**

It defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

**Problem Solved by the Observer Pattern**

A classic example of applying the **Observer** pattern occurs in updating panels in a **Dashboard**, where various charts and tables (the Observers) need to be updated whenever the data from a data source (the Subject) is changed.

The first panel is presented in a **table**:

| Region 1 | Region 2 | Region 3 | Total |
| :--- | :--- | :--- | :--- |
| 40 | 30 | 100 | 170 |
| 120 | 80 | 70 | 270 |
| 80 | 50 | 90 | 220 |
| 240 | 160 | 260 | 660 |

**Table:** Panel 1.
Alexandre Luis Correa.

Panels 2 and 3 are presented in **charts**:

-----

The source code can become complex as the number of panels increases, making it difficult to maintain this synchronization manually. A mechanism is needed to manage this dependency relationship.

The **Observer pattern** allows the **Subject** to maintain a list of **Observer** objects and offer methods to **attach** (add), **detach** (remove), and **notify** all **Observers** when a state change occurs.

**Solution of the Observer Pattern**

The class structure for the **Observer** pattern is presented in the diagram and is as follows:

-----

**Code Observation:** The source code below shows the implementation of the **Observer** interface in Java, which has the `update()` method, which will be called by the **Subject** to notify the **Observer** about a state change.

```java
public interface Observer {
    void update(Subject subject);
}
```

The following code shows the **Subject** interface in Java, which defines the methods to add, remove, and notify **Observers**.

```java
public interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers();
    // Other Subject methods
}
```

Next, an example of the **Concrete Subject**, which implements the **Subject** interface and maintains the state of interest.

```java
import java.util.ArrayList;
import java.util.List;

public class ConcreteSubject implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private int state;

    public int getState() {
        return state;
    }

    public void setState(int state) {
        this.state = state;
        notifyObservers(); // Notifies observers when the state changes
    }

    @Override
    public void attach(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void detach(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(this);
        }
    }
}
```

And finally, an example of a **Concrete Observer**, which implements the **Observer** interface and contains the logic to update itself when notified.

```java
public class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    @Override
    public void update(Subject subject) {
        if (subject instanceof ConcreteSubject) {
            int state = ((ConcreteSubject) subject).getState();
            System.out.println("Observer " + name + " updated. New state: " + state);
            // Specific observer update logic, such as redrawing a chart.
        }
    }
}
```

-----

The **Observer** pattern promotes **low coupling** between the **Subject** and the **Observers**, allowing them to interact without having knowledge of each other. This facilitates system maintenance and extensibility, as new **Observers** can be added without modifying the **Subject**.

**Consequences and Patterns Related to Observer**

The **Observer** pattern is widely used in various areas, such as graphical user interface (GUI) frameworks, where interface elements (like buttons and text fields) act as **Subjects** and other parts of the system (the **Observers**) react to their actions.

It is important in event-driven systems and allows for efficient and flexible communication.

**ATTENTION**
The **Observer** pattern is very useful for projects that need to **maintain synchronization between multiple objects** when the state of a central object changes.

-----

-----

### State Pattern

The **State** design pattern allows an object to change its behavior when its internal state changes. Externally, the object appears to have changed its class. It encapsulates the behavior of an object in different states into separate classes. Instead of using conditional statements (like `if/else` or `switch`) to change behavior, the object delegates calls to the class that represents the current state.

**Intention of the State Pattern**

It allows an object to change its behavior when its internal state changes. The object will appear to have changed its class.

**Problem Solved by the State Pattern**

The **State** pattern is very useful for problems related to implementing entities with a **state machine**, meaning they have distinct behaviors in each of their states. Consider a **TrafficLight** object that changes its behavior (the light that is on) depending on its state (Red, Yellow, or Green). In a traditional implementation, the **TrafficLight** object would have a `ChangeSignal()` method with several conditional statements (`if/else` or `switch`) to determine the next state and what action to execute, which would make the code complex and difficult to maintain.

The source code follows a conventional implementation without using the **State** pattern, but this approach becomes complex and difficult to maintain as the number of states and transitions increases.

```java
public class SemaforoSemPadrao {
    private String estado; // "Vermelho", "Amarelo", "Verde" (Red, Yellow, Green)

    public SemaforoSemPadrao() {
        this.estado = "Vermelho";
    }

    public void MudarSinal() {
        if (this.estado.equals("Vermelho")) {
            this.estado = "Verde";
            System.out.println("Sinal Verde! Siga."); // Green Light! Go.
        } else if (this.estado.equals("Verde")) {
            this.estado = "Amarelo";
            System.out.println("Sinal Amarelo! Atenção."); // Yellow Light! Attention.
        } else if (this.estado.equals("Amarelo")) {
            this.estado = "Vermelho";
            System.out.println("Sinal Vermelho! Pare."); // Red Light! Stop.
        }
    }
}
```

**Solution of the State Pattern**

The class structure for the **State** pattern is presented in the diagram and is as follows:

-----

**Code Observation:** The following source code shows the implementation of the **State** interface in Java.

```java
public interface EstadoSemaforo {
    void handle(Semaforo context);
    String getNomeEstado();
}
```

The **Context** defines the interface and maintains a reference to an instance of a **State** subclass (the current state).

```java
public class Semaforo {
    private EstadoSemaforo estadoAtual;

    public Semaforo() {
        // Initial state
        this.estadoAtual = new EstadoVermelho();
    }

    public void setEstado(EstadoSemaforo novoEstado) {
        this.estadoAtual = novoEstado;
        System.out.println("Sinal mudou para: " + novoEstado.getNomeEstado()); // Signal changed to:
    }

    public void MudarSinal() {
        this.estadoAtual.handle(this);
    }

    public EstadoSemaforo getEstadoAtual() {
        return estadoAtual;
    }
}
```

Next, an example of a **Concrete State**, which implements the behavior associated with the "Red" state.

```java
public class EstadoVermelho implements EstadoSemaforo {
    @Override
    public void handle(Semaforo context) {
        System.out.println("Sinal Vermelho! Pare. Transicionando para Verde..."); // Red Light! Stop. Transitioning to Green...
        context.setEstado(new EstadoVerde()); // State transition
    }

    @Override
    public String getNomeEstado() {
        return "Vermelho"; // Red
    }
}
```

-----

The **State** pattern allows the **Context** object to delegate its behavior to the current **State** object. When the state changes, the **Context** changes the reference to another **State** subclass, modifying the object's behavior. This eliminates the conditional statements in the **Context**, making it cleaner and easier to extend.

**Consequences and Patterns Related to State**

The **State** pattern promotes **cohesion** by grouping all behavior related to a state into one class, and **low coupling** between the **Context** and the concrete states, which can be changed independently.

It facilitates the addition of new states and the modification of transitions without impacting the **Context** code. This makes the system more flexible and easier to maintain.

**ATTENTION**
This pattern is also known as the **Object-Based State Machine Pattern**.

The **State** pattern can be combined with the **Strategy** pattern, allowing the compartmentalization of different algorithms. The concrete classes, representing the state of an object, are useful for handling complex transitions.