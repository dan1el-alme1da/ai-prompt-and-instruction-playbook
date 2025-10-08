## 1\. Business Rules Implementation with EJBs

### Enterprise Java Beans (EJB)

In **Enterprise Java**, the **distributed object architecture** centrally manages and stores a system's **services** and **objects**, allowing us to access these services and objects in a distributed manner in an **enterprise application** architecture.

In this video, you will see the importance of **EJBs**, as well as their applications and advantages in an enterprise environment.

**EJBs** are **enterprise components** used remotely within a distributed object environment. They are objects that, once registered, become accessible to other applications through **service** interfaces. EJBs are located on the application server, usually a web server, in a distributed manner.

**Clients** access these components from the EJB pool via **local interfaces** (located in the same Java Virtual Machine - **JVM**) or **remote interfaces** (located in other Java Virtual Machines). For local access, **@EJBLocal** is used; for remote access, the client needs the **JNDI platform** to access the EJB.

**Step 1**
An Enterprise object is created.

**Step 2**
Creation of the access interface for the database.

**Step 3**
Interface delivery to the client, allowing the dialogue with the pool to begin.

This is the representation of this step in the following image:

**Server and JNDI Platform**

The client can access EJBs in a distributed object environment by registering the **DataSource** object, which is registered in **JNDI**. When we request a connection to the DataSource, it automatically **connects to the database** and delivers it to the client in a distributed object architecture. **JNDI** is an **object access platform** for resource access.

The client has access to the **application connection** to the connection pool. To obtain the connection via JNDI, using the `lookup` method of `InitialContext`, we call the **connection** for the correct EJB, in this case, the **EJB Bean**. If it is local access, the client only needs to request the **local interface**; if it is remote access, the client will need to request the **remote interface**.

```java
// Java Code
public void salvarCliente() throws Exception {
    Context context = new InitialContext();
    ConexaoEJBLocal conexaoEJB = (ConexaoEJBLocal) context.lookup("java:global/meuProjeto/ConexaoEJB!br.com.estacio.ConexaoEJBLocal");
    conexaoEJB.salvarCliente(); // calls the method on the EJB
}
```

The EJB has the **responsibility** of using the **pool**, which is in charge of the persistence **transaction**, of creating a connection to the database, and of delivering the object **interface** to the client. The client, in turn, only needs a **remote call** in **JNDI** to access the **business rules**.

This is the big difference starting from **EJB 3**: the **simple structure**. We only find two classes for implementation: the **EJB Bean** and the **local interface**. There is no code difference. The **JNDI platform** takes care of all the **necessary structure**. There is no difference from the standard **creation** process by J2EE either. The difference, starting from EJB 3, is only in the **annotation**: **@EJB** for local annotation; **@Remote** for remote annotation; **@WebService** for web service annotation; **@MessageDriven** for message annotation; following an **oriented** model.

-----

## 1\. Business Rules Implementation with EJBs

### Session Beans

**Session beans** are EJBs that are designed to encapsulate and implement the business rules of an application. They are responsible for managing the **logic** and providing **specific services** to clients. Session beans are generally used to implement the system's business rules, acting as the application's **service layer**.

In this video, you will see Session beans in **Java Enterprise Edition (Java EE)**, which are components used to implement the system's business rules in a distributed manner, as well as their characteristics, such as **transaction management**, **concurrency**, and **scalability**.

The EJB is used to implement our application's business rules based on the **Enterprise Java component** architecture. Session beans are used to implement the system's business logic, being the service layer. It is important to note that the business rules must be completely **independent of the system interfaces**.

There are three types of Session beans that can be used to implement logic and business rules in an **efficient** and **configurable** way, presenting three basic behaviors:

| Stateless | Stateful | Singleton |
| :---: | :---: | :---: |
| Does not store values. | Stores values like a shopping cart. | Allows only one instance per Java Virtual Machine. |

-----

**Stateless**

It does not store information about the server's previous processes, meaning it **does not store session values**, as it does not need to retain any previous information, defining the most aggressive behavior pattern in a Session bean.

They are used in operations where the session state is not relevant, such as in a virtual shopping account: processes with low concurrency in arithmetic calculations, where the client does not store any information in the session.

-----

**Stateful**

It stores access information so that the **access interface** is based on the **@Stateful** annotation, which stores the session **state**. This interface is what the client will access through a **session**. In terms of code, it will be enough for the developer to just use the **@Stateful annotation** on the service class.

```java
// Java Code
@Stateful
public class ConexaoEJB {
    // ...
}
```

When creating the EJB, it must implement the **access interface**, in addition to being designed as a **service class**, which will implement our application's business rules.

```java
// Java Code
@Remote
@Stateful
public class ConexaoEJB implements ConexaoEJBRemote {
    // ...
}
```

-----

**Singleton**

It is used when implementing the **Singleton** pattern and is utilized to **share data** among other classes. The Singleton Bean is mostly used as an object that is created only once and that will register the EJB and deliver it in **critical access** events, which require working with server **clusters**.

-----

### Session Bean with Servlet

In general, the **communication** and **use** of a **Session Bean** are done starting from a **Servlet**.

```java
// Java Code
@WebServlet("/ServletSoma")
public class ServletSoma extends HttpServlet {
    @EJB
    private ConexaoEJBLocal conexaoEJB; // EJB injection
    // ...
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        int resultado = conexaoEJB.soma(2, 3); // call the method on the EJB
        // ...
    }
    // ...
}
```

Every time we need to make and send an **event**, as a **local interface**, with **EJB**, there are examples: in the **client registration process**, if the registration was successful, it will notify the EJB, which, in turn, will notify other EJBs in other modules, such as the **accounts payable module**.

Almost all **business rules of an enterprise system** are implemented using a **Session Bean**. It is the layer that will implement all business rules: **registration, alteration, consultation, movement with messages**, following an **asynchronous model**.

-----

## 1\. Business Rules Implementation with EJBs

### Enterprise Applications

In this video, you will see what **enterprise applications** are and how they work, as well as their characteristics and advantages in a **distributed systems** environment.

An **EJB** that implements business rules with **EJBs** is based on applications in **Netbeans**, which is an **IDE** (integrated development environment) that uses the **GlassFish** application server.

#### Practice

**Step 1:** Create a new project in Netbeans.

**Step 2:** Configure the project.

**Step 3:** Configure Maven options.

**Step 4:** Configure the server and the Java EE version.

[Text excerpt about JBoss EAP and WildFly.]

> **JBoss EAP** and **WildFly** are application servers that implement the Java EE (now Jakarta EE) standard and are widely used in enterprise environments. You can choose to use them in your project.

**Step 5:** Create the **EJB Bean**.

**Step 6:** Implement the business logic in the EJB class (in the `soma` method).

**Step 7:** Create the EJB **local interface**.

**Step 8:** Create the **Servlet** in the WAR module.

**Step 9:** Implement the **call** to the EJB in the **Servlet**.

-----

**Result of the ServletSoma application: 5**

-----

### MDBs

A **Message-Driven Bean (MDB)** is a type of EJB that implements the **Java Message Service (JMS)** pattern to allow **asynchronous message processing**. This allows a component to develop two classes in the same project: one to **listen** and one to **send** the message. It is widely used in systems that communicate with the application's **messaging system**.

  * Create the **connection queue** and the **queue** in **glassfish**, using the **wizard**.
  * Create the EJB **Message-Driven Bean (MDB)** to **listen** and process the **messages**.
  * In the EJB module, develop a **MeuProduto** class, which contains a **Servlet**.
  * In the **web** project, develop the EJB.
  * In the **ear** project, use the EJB with a **formula** for **message** sending.
  * Access the application from the **Servlet**.

See the result of this practice:

*Minhaaplicacao.java*

-----

*QueueSender.java*

-----

## 1\. Business Rules Implementation with EJBs

### Message-driven Beans (MDB)

**Message-driven Beans (MDB)** are enterprise components that implement the **messaging** pattern. These processes promote application **decoupling** through a message sending and receiving architecture, which allows **asynchronous communication** and **fault tolerance** in a distributed systems environment.

In this video, you will see what a **Message-driven Bean (MDB)** is and how it is used to implement a system's business rules **asynchronously**. You will also see its main characteristics and advantages in a distributed **enterprise application** environment.

> [Video: Message-driven Beans (MDB)]

**Messages** act **asynchronously** and have six stages:

**Messages** are deposited in **topics** for **listeners** to **retrieve** them.

The **message pool** allows **asynchronous communication** with the EJB, being completely **decoupled**, meaning the client **does not wait** for the EJB's response. The messaging **platform** uses the **queue** and stores the **events** in the **queue** to be processed later.

To create a message, the messaging **platform** needs two objects: the **connection** to the server and the **destination** (queue/topic).

```java
// Java Code for connection and destination creation
@Resource(mappedName = "jms/myQueueFactory")
private static ConnectionFactory connectionFactory;
@Resource(mappedName = "jms/myQueue")
private static Queue queue;
```

-----

**MDB (Message-driven Bean)**

The EJB that implements the business rules is the **Message-driven Bean (MDB)**, which acts as a message **listener**, receiving events from the queue and processing the business rule. It is a **Stateless** type EJB (does not store state), which implements the **MessageListener** interface and has the **onMessage** method, which is the **entry point** for all messages.

The **@MessageDriven** **annotation** is used to configure the MDB and is applied to the service class. The MDB connects to the **messaging server** (such as Glassfish, JBoss, Weblogic), which provides the **messaging platform**. The messaging server is responsible for **storing**, **managing**, and **delivering** the messages.

```java
// Java Code for the MDB
@MessageDriven(mappedName = "jms/myQueue", activationConfig = {
    @ActivationConfigProperty(propertyName = "acknowledgeMode", propertyValue = "Auto-acknowledge"),
    @ActivationConfigProperty(propertyName = "destinationType", propertyValue = "javax.jms.Queue")
})
public class MeuMDB implements MessageListener {
    // ...
    @Override
    public void onMessage(Message message) {
        // Message processing logic
        // ...
    }
}
```

> **TIP:**
> The MDB is the **only type of EJB** that **does not** have an interface. The client **does not access** the MDB directly; it only **sends** the message to the queue and the MDB **processes** the message, **asynchronously**.

-----

#### Practice

The MDB practice will be implemented with the **Glassfish Server** using:

  * A **Session Bean** to send the message.
  * A **Message-Driven Bean** to process the message.
  * A **Servlet** to initiate the sending process.

**Step 1**
Create the **Message-Driven Bean (MDB)** in the EJB module.

**Step 2**
Create the **Session Bean** for message sending, containing the code to create the **message connection** and the MDB **destination queue**, using the **@Resource** annotation.

**Step 3**
Create the **Servlet** in the WAR module.

**Step 4**
Implement the **message sending** through the **Servlet**, where the sending must be **decoupled** from the receiving. The client **does not** wait for the MDB's response.

**Step 5**
View the message **receiving** in the Glassfish Server **log**.

-----

## 1\. Business Rules Implementation with EJBs

### Applying MDBs

In the web world, it is quite common to use **external resources**, whether they are from another project in your application, or even from another application. This communication is done **asynchronously** through **messages**, allowing **communication** between different parts. To facilitate communication, an excellent alternative is **message exchange**.

In this video, you will see how **Message Driven Beans (MDBs)** based on **topics** are used for **message** sending and **event** **notification** of a system **asynchronously** in an **MDB** implementation.

#### Practice Script

The MDB practice will be in the **MDB (Message-driven Bean) environment**. Therefore, we will learn how to **create** this type of component by developing two classes in the same project: one to **listen** and one to **send** the **message**. It is widely used in systems that communicate with the application's **messaging system**.

  * Create the **connection queue** and the **queue** in **glassfish**, using the **wizard**.
  * Create the EJB **Message-Driven Bean (MDB)** to **listen** and process the **messages**.
  * In the EJB module, develop a **MeuProduto** class, which contains a **Servlet**.
  * In the **web** project, develop the EJB.
  * In the **ear** project, use the EJB with a **formula** for **message** sending.
  * Access the application from the **Servlet**.

See the result of this practice:

*Minhaaplicacao.java*

```java
// Java Code for the Minhaaplicacao class
public static void main(String[] args) throws Exception {
    Properties props = new Properties();
    props.setProperty("java.naming.factory.initial", "com.sun.enterprise.naming.SerialObjectFactory");
    // ...
    Context context = new InitialContext(props);
    QueueSender queueSender = (QueueSender) context.lookup("java:global/meuprojeto/QueueSender!br.com.estacio.QueueSender");
    queueSender.sendMessages("Hello, MDB!"); // calls the send method
}
```

-----

*QueueSender.java*

```java
// Java Code for the QueueSender class
@Stateless
public class QueueSender {
    @Resource(mappedName = "jms/myQueueFactory")
    private ConnectionFactory connectionFactory;
    @Resource(mappedName = "jms/myQueue")
    private Queue queue;
    // ...
    public void sendMessage(String message) throws Exception {
        Connection connection = connectionFactory.createConnection();
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        MessageProducer messageProducer = session.createProducer(queue);
        TextMessage textMessage = session.createTextMessage(message);
        messageProducer.send(textMessage);
        // ...
    }
}
```

-----

*MDBListener.java*

```java
// Java Code for the MDBListener
@MessageDriven(mappedName = "jms/myQueue", activationConfig = {
    @ActivationConfigProperty(propertyName = "acknowledgeMode", propertyValue = "Auto-acknowledge"),
    @ActivationConfigProperty(propertyName = "destinationType", propertyValue = "javax.jms.Queue")
})
public class MDBListener implements MessageListener {
    // ...
    @Override
    public void onMessage(Message message) {
        try {
            TextMessage tm = (TextMessage) message;
            System.out.println("Mensagem recebida pelo MDB: " + tm.getText());
        } catch (JMSException e) {
            e.printStackTrace();
        }
    }
}
```