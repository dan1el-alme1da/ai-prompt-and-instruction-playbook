
-----

# 3\. MVC Architecture in Java

## MVC Architecture

The **MVC** (Model-View-Controller) architecture divides the system into three layers, each with specific responsibilities:

  * **Model**: This layer contains the **entities** and classes for object **persistence**.
  * **View**: This is the **presentation** layer—what the user sees.
  * **Controller**: Primarily acts as the **mediator** for actions, controlling business objects.

To learn more about the MVC architecture, watch the video below:

*(Visual Content - Video: "MVC Architecture")*

In this video, you will understand the MVC architecture in web development. This architecture separates the application into three parts (**model, view, and controller**), allowing for its transparent management.

### MVC Layers

Below are the main characteristics of each layer that makes up the MVC architecture:

#### Model

  * **Business entities**.
  * Contains **business logic**.
  * Connections and calls to the **database**.
  * May use value objects.
  * May use object-relational mapping.
  * **Does not depend** on the View.

#### Controller

  * Implements the system's **business rules**.
  * Defines the sequence of calls.
  * **Receives** user requests.
  * May use distributed objects.
  * **Does not depend** on the View or the Model.

#### View

  * Defines the system's **interface**.
  * Responsible for **presenting** information.
  * Contains only **formatting rules**.
  * **Interacts** with the system's Controller.
  * Cannot access the Model layer.

A fundamental rule for the MVC architecture is that elements in the **View** layer cannot access the **Model** (data) layer or the **Controller** layer. Elements in the **Controller** layer cannot access the **View** layer. Elements in the **Model** layer operate exclusively on business objects.

> The **MVC architecture is based on layers**, and each layer sees only the **immediate** layer (above or below) it.

In an MVC architecture, entities are the units of information that travel between the layers. All SQL commands are concentrated in the **DAO** (Data Access Object) classes. Only the **Controller** layer can access the **Model**, and the latter uses the **DAO** classes (which contain the SQL commands) to access the database.

Since **SQL** commands are highly standardized, it is possible to create tools for generating the commands and possibly the **DAO** classes. This way of thinking gave rise to the **DAO** development pattern.

The **Controller** layer needs to be defined independently of the specific environment, using **Interfaces (DAO)** or employing development **design patterns**, such as **Factory**. This way, distinct **Controller** classes (not the models that manage the DAO components), which define the complexity in the subordinate activities initiated in the **View**, are maintained.

Thus, the **Controller** layer is the best place to define **authorization** rules for using system functionalities. **Beans** can be implemented as *helpers* for the correct implementation of business logic. **Business rules** will be an attribution of the **Controller** layer. In the *models*, a *bean* will always be generated, maintaining persistence between the layers.

-----

## Java Components for MVC

Much of the MVC architecture is geared towards **web application development**. The goal is for the web application to be able to separate the client request processing from the presentation code (HTML, XML, etc.) and the database access code.

To learn more about Java components for MVC, watch the video below:

*(Visual Content - Video: "Java Components for MVC")*

The image below shows the relationship between the components and the MVC layers:

*(Visual Content - Diagram showing the relationship between **JSP/HTML/JSTL** in the **View**, **Servlet/Action** in the **Controller**, and **POJO/DAO/JDBC** in the **Model**.)*

The components in each MVC layer are:

  * **View**: **JSP**, **HTML**, **JSTL**
  * **Controller**: **Servlet**, **Action**
  * **Model**: **POJO**, **DAO**, **JDBC**

-----

# Development Patterns

Object orientation represents a major advance in system implementation. This concept has improved the development of complex systems, allowing problem-solving to be elaborated with a logic of reasoning.

To learn more about development patterns, watch the video below:

*(Visual Content - Video: "Delivering Solutions with Design Patterns")*

## Description of Development Patterns

Let's look at a formal description of the mentioned development patterns, plus a few others commonly found in systems created for the J2EE environment:

  * **Abstract Factory**: defines an abstract structure for creating families of classes or interfaces.
  * **Factory Method**: defines a structure for creating some type of request, widely used for handling requests made by the Controller.
  * **Data Access Object (DAO)**: uses specific classes to access data sources (databases).
  * **Facade**: simplifies calls to a complex system.
  * **Intercepting Filter**: creates filters between client calls and the system, preventing unusual requests from creating calls.
  * **Service Locator**: provides a central hub for locating and routing calls for each request.
  * **Value Object**: handles objects that represent a collection, which is naturally implemented in Java.
  * **Transfer Object**: also known as **DTO** (Data Transfer Object), these objects are used in services to optimize the transparent connection to the client.
  * **Session Facade**: encapsulates the business logic of components, based on name and directory services.
  * **View Helper**: assists the View layer in screen formatting, such as in access controls.
  * **Front Controller**: manages the single point of execution, based on a provided pattern.

## Architectural Patterns

Architectural patterns define the system's **basic structure**, such as components and interactions, with two goals: **economy** and the **exchange** of projects during system implementation. Choosing the architectural pattern is the first step in implementing the system.

To learn more about design patterns, including examples, watch the video below:

*(Visual Content - Video: "Introduction to Architectural Patterns")*

### Architectural Pattern Models

| Architectural Patterns | Models |
| :--- | :--- |
| Broker | Distributed Systems |
| Client-Server | Distributed Systems |
| Layers | Systems with internal layers |
| Structure and objects | Systems with internal layers |
| Master-Slave | Systems with internal layers |
| Pipes and Filters | Systems that process sequential data |
| Repository | Central data systems |
| Peer-to-Peer (P2P) | Systems with independent components |
| Interpreter | Systems that interpret commands |
| Virtual Machine | Systems that process internal commands |
| Event-Driven | Systems with events and handling |
| MVC | Interactive Systems |
| BlackBoard | Artificial Intelligence Systems |
| MicroKernel | Adaptive Systems |
| Reflection | Adaptive Systems |

**Distributed Objects Model**: The idea behind this architectural pattern for *software* development is to avoid wasting time reinventing the wheel. The distributed objects model is one of the most widely implemented and can be considered an architectural pattern, with all the conveniences integrated into the conflicts.

The distributed objects model allows *software* components created in the Java language to be called and processed by *software* created in another language, such as **C++**, for example.

-----

# Implementing MVC in the Java Environment

Developing a system using the MVC architecture in the Java environment is important. It defines how the components will be implemented in the database and model layers. However, this situation often involves a lot of code (boilerplate code) due to repetition.

In this section, you will find a good example of implementing a database system to store data, the business rule in the database layer, and the data presentation in the View layer.

To learn more about implementing MVC in Java, watch the video below:

*(Visual Content - Video: "Implementing MVC in the Java Environment")*

## Practice Script

Use the following product system example with the layers corresponding to the **"three-layer"** MVC architecture, which contains: **Model** (the data), **Controller** (the engine that defines what needs to be done), and **View** (the presentation of information to the user).

1.  Create the classes and insert sample data for persistence.
2.  Create a persistence connection object for the Model class.
3.  Create a **DAO** (*Data Access Object*) for managing the Product entity.
4.  Create a **Servlet** (*Controller*) for calling the business rules.
5.  Create a **JSP** (*View*) for data presentation.

-----

## Code Examples

### DAO.java

```java
package br.DAO;
import br.model.Produto;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.util.ArrayList;

public class DAO {
    private Connection con = null;

    public DAO() {
        con = Conexao.conectar();
    }
    
    // ... [code continues]
```

### Produto.java (Product)

```java
package br.model;

public class Produto {
    private int id;
    private String nome;
    private double preco;
    private int quantidade;
    
    // ... [constructors and getters/setters methods]
    
    @Override
    public String toString() {
        return "Produto [id=" + id + ", nome=" + nome + ", preco=" + preco + ", quantidade=" + quantidade + "]";
    }
}
```

### Entidade.java (Entity)

```java
package br.model;
import br.DAO.DAO;
import java.util.ArrayList;

public class Entidade {
    public ArrayList<Produto> getProdutos() {
        DAO dao = new DAO();
        return dao.listarProdutos();
    }
}
```

### ProdutoDAO.java (ProductDAO)

```java
package br.DAO;

public class ProdutoDAO extends DAO {
    // ... [listarProdutos() method and other BD access methods]
    
    public ArrayList<Produto> listarProdutos() {
        ArrayList<Produto> produtos = new ArrayList<>();
        // ... [logic to fetch products from the DB]
        return produtos;
    }
}
```

### ProdutoFactory.java (ProductFactory)

```java
package br.DAO;
import br.model.Produto;

public class ProdutoFactory {
    public static Produto getProduto() {
        return new Produto();
    }

    public static ProdutoDAO getProdutoDAO() {
        return new ProdutoDAO();
    }
}
```

### ViewServlet.java

```java
package br.Controller;
import br.model.Entidade;
import br.model.Produto;
import java.io.IOException;
import java.util.ArrayList;
import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class ViewServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Entidade entidade = new Entidade();
        ArrayList<Produto> produtos = entidade.getProdutos();
        request.setAttribute("listaProdutos", produtos);
        request.getRequestDispatcher("Produtos.jsp").forward(request, response);
    }
}
```

### Produtos.jsp (Products.jsp)

```jsp
<%@page import="java.util.ArrayList"%>
<%@page import="br.model.Produto"%>
<%@page contentType="text/html" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>Produtos</title>
    </head>
    <body>
        <h1>Produtos</h1>
        <ul>
            <% 
                ArrayList<Produto> produtos = (ArrayList<Produto>) request.getAttribute("listaProdutos");
                for (Produto produto : produtos) {
            %>
            <li><%= produto.getNome() %></li>
            <% 
                }
            %>
        </ul>
    </body>
</html>
```

### Simple Servlet Output Example

*(Visual Content - Screenshot showing the output of a Simple Servlet, displaying a list of products.)*

**Simple Servlet Example**

  * Strawberry
  * Banana
  * Mango
  * Avocado