
-----

## 1\. Implementação de regras de negócio com EJBs

### Enterprise Java Beans (EJB)

No enterprise Java, a **arquitetura de objetos distribuídos** gerencia e armazena centralmente os **serviços** e os **objetos** de um sistema, o que nos permite ter acesso a esses serviços e objetos de uma forma distribuída em uma arquitetura de **aplicações corporativas**.

Neste vídeo, você verá a importância dos **EJBs**, bem como suas aplicações e vantagens em um ambiente corporativo.


Os EJBs são **componentes corporativos** utilizados de forma remota dentro de um ambiente de objetos distribuídos. Eles são objetos que, após registrados, ficam acessíveis a outras aplicações por meio de interfaces de **serviços**. Os EJBs estão localizados no servidor de aplicações, geralmente um servidor web, de forma distribuída.

Os **clientes** acessam esses componentes pelo pool de EJB por meio de **interfaces local** (localizada na mesma máquina virtual Java - JVM) ou **remota** (localizada em outras máquinas virtuais Java). Para acesso local, se utiliza o **@EJBLocal**; para acesso remoto, o cliente precisa da plataforma de JNDI para poder acessar o EJB.

**Passo 1**
Cria-se um objeto Enterprise.

**Passo 2**
Criação da interface de acesso para o banco.

**Passo 3**
Entrega de interface ao cliente, permitindo iniciar o diálogo com o pool.

Esta é a representação desse passo na imagem a seguir:

[Imagem de um diagrama que mostra o fluxo: Cliente -\> JNDI -\> Servidor Enterprise (que contém o EJB Local) -\> EJB Bean -\> Servidor de Database. O EJB também se comunica com o Servidor de Enterprise.]

Servidor e plataforma JNDI

O cliente pode acessar os EJBs em um ambiente de objetos distribuídos, registrando o objeto do tipo **DataSource**, que é registrado no **JNDI**. Quando solicitamos uma conexão ao DataSource, ele, automaticamente, faz a **conexão ao banco de dados** e a entrega para o cliente em uma arquitetura de objetos distribuídos. O JNDI é uma **plataforma** de acesso de objetos para acesso aos recursos.

O cliente possui acesso à **conexão de aplicação** ao pool de conexões. Para obter a conexão via JNDI, por meio do método lookup da InitialContext, chamamos a **conexão** para o EJB correto, no caso, o **EJB Bean**. Se for um acesso local, o cliente precisa apenas solicitar a **interface local**; se for um acesso remoto, o cliente precisará solicitar a **interface remota**.

```java
// Código Java
public void salvarCliente() throws Exception {
    Context context = new InitialContext();
    ConexaoEJBLocal conexaoEJB = (ConexaoEJBLocal) context.lookup("java:global/meuProjeto/ConexaoEJB!br.com.estacio.ConexaoEJBLocal");
    conexaoEJB.salvarCliente(); // chama o método no EJB
}
```

O EJB possui a **responsabilidade** de utilizar o **pool**, que está a cargo da **transação** de persistência, de criar uma conexão com o banco de dados e a de entregar a **interface** do objeto para o cliente. O cliente, por sua vez, precisa apenas de uma **chamada remota** no **JNDI** para poder acessar as **regras de negócio**.

Essa é a grande diferença a partir do EJB3: a **estrutura simples**. Já encontramos apenas duas classes para implementação: o **EJB Bean** e a **interface local**. Não tem diferença de código. A plataforma **JNDI** cuida de toda a **estrutura necessária**. Não há nem diferença do processo de **criação** padrão pelo J2EE. A diferença, a partir do EJB 3, é apenas de **anotação**: **@EJB** para anotação local; **@Remote** para anotação remota; **@WebService** para anotação de web service; **@MessageDriven** para anotação de mensagens; seguindo um modelo **orientado**.


-----

## 1\. Implementação de regras de negócio com EJBs

### Session beans

Session beans são EJBs que são projetados para encapsular e implementar as regras de negócio de uma aplicação. Eles são responsáveis por gerenciar a **lógica** e fornecer **serviços específicos** para os clientes. Os Session beans são geralmente utilizados para implementar as regras de negócio do sistema, sendo a **camada de serviços** da aplicação.

Neste vídeo, você verá os Session beans no **Java Enterprise Edition (Java EE)s**, que são componentes utilizados para implementar as regras de negócio do sistema de forma distribuída, além de suas características, como **gerenciamento de transações**, **concorrência** e **escalabilidade**.


O EJB é utilizado para implementar as regras de negócio do nosso aplicativo com base na arquitetura de **componentes** Enterprise Java. Os Session beans são usados para implementar a lógica de negócio do sistema, sendo a camada de serviços. É bom observar que as regras de negócio devem ser totalmente **independentes das interfaces de sistema**.

Existem três tipos de Session beans que podem ser utilizados para implementar a lógica e as regras de negócio de forma **eficiente**, **configurável**, podendo apresentar três comportamentos básicos:

| Stateless | Stateful | Singleton |
| :---: | :---: | :---: |
| Não guarda valores. | Guarda valores como carrinho. | Permite apenas uma instância por máquina virtual Java. |

-----

**Stateless**

Não armazena informações sobre os processos anteriores do servidor, ou seja, **não guarda valores de sessão**, pois não precisa reter nenhuma informação anterior, definindo o padrão de comportamento mais agressivo em um Session bean.

São utilizados em operações nas quais o estado da sessão não é relevante, como em uma conta de compras virtual: os processos com pouca concorrência em cálculos aritméticos, em que o cliente não armazena nenhuma informação na sessão.

-----

**Stateful**

Armazena as informações de acesso para que a **interface de acesso** seja base na anotação **@Stateful**, que guarda o **estado** da sessão. Essa interface é que o cliente irá acessar por meio de uma **sessão**. Em termos de código, será suficiente que o desenvolvedor apenas utilize a **anotação @Stateful** na classe de serviço.

```java
// Código Java
@Stateful
public class ConexaoEJB {
    // ...
}
```

Ao criarmos o EJB, ele deverá implementar a **interface de acesso**, além de ser projetado como **classe de serviço**, que irá implementar as regras de negócio de nossa aplicação.

```java
// Código Java
@Remote
@Stateful
public class ConexaoEJB implements ConexaoEJBRemote {
    // ...
}
```

-----

**Singleton**

É utilizado quando se implementa o **Singleton** e é utilizado para **compartilhar dados** entre as outras classes. O Bean Singleton é mais utilizado como um objeto que é criado uma única vez e que irá registrar o EJB e a entregará em eventos de **acesso de crítica**, que necessitam trabalhar com **clusters** de servidores.

-----

### Session Bean com Servlet

Em geral, a **comunicação** e **utilização** de um **Session Bean** é feita a partir de um **Servlet**.

```java
// Código Java
@WebServlet("/ServletSoma")
public class ServletSoma extends HttpServlet {
    @EJB
    private ConexaoEJBLocal conexaoEJB; // injeção do EJB
    // ...
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        int resultado = conexaoEJB.soma(2, 3); // chamada do método no EJB
        // ...
    }
    // ...
}
```

Toda vez que precisamos fazer e enviar um **evento**, de forma de **interface local**, com **EJB**, há exemplos: no **processo de cadastro de cliente**, se o cadastro foi realizado com sucesso, ele irá notificar o EJB, que, por sua vez, irá notificar os outros EJBs de outros módulos, como o **módulo de contas a pagar**.

Quase todas as **regras de negócio de um sistema corporativo** são implementadas por meio de um **Session Bean**. É a camada que irá implementar todas as regras de negócio: **cadastro, alteração, consulta, movimentação com mensagens**, seguindo um **modelo assíncrono**.


-----

## 1\. Implementação de regras de negócio com EJBs

### Aplicativos corporativos

Neste vídeo, você verá o que são e como funcionam os **aplicativos corporativos**, bem como suas características e vantagens em um ambiente de **sistemas distribuídos**.


Um **EJB** que implementa regras de negócio com **EJBs** tem como base as aplicações no **Netbeans**, sendo ele uma **IDE** (ambiente de desenvolvimento integrado) que utiliza o servidor de aplicação **GlassFish**.

#### Prática

**Passo 1:** Criar um novo projeto no Netbeans.

[Imagem da tela de criação de novo projeto no Netbeans, com a opção "Maven" e "Enterprise Application" selecionadas.]

**Passo 2:** Configurar o projeto.

[Imagem da tela de configuração do projeto, mostrando campos como "Project Name", "Project Location", "Group Id", "Artifact Id" e "Version".]

**Passo 3:** Configurar as opções do Maven.

[Imagem da tela de configuração do Maven, mostrando as opções de "POM File" e "Packaging".]

**Passo 4:** Configurar o servidor e a versão do Java EE.

[Imagem da tela de configuração do servidor, mostrando as opções de "Server" (GlassFish Server), "Java EE Version" (Java EE 7), e "Create Modules" (EJB, WAR).]

[Trecho de texto sobre o JBoss EAP e WildFly.]

> O **JBoss EAP** e o **WildFly** são servidores de aplicações que implementam o padrão Java EE (agora Jakarta EE) e são amplamente utilizados em ambientes corporativos. Você pode optar por utilizá-los em seu projeto.

[Imagem da estrutura de projetos no Netbeans (Projetos, Services, Files) com as opções de servidor (GlassFish Server, WildFly Server, Tomcat) e a estrutura do projeto criado (ear, ejb, war).]

**Passo 5:** Criar o **EJB Bean**.

[Imagem da tela de criação de um EJB, mostrando a opção "Session Bean" e campos como "Class Name" e "Package".]

**Passo 6:** Implementar a lógica de negócio na classe EJB (no método `soma`).

[Imagem do código Java da classe `SomaEJB`, mostrando o método `soma(int a, int b)` que retorna `a + b` e a anotação `@Stateless`.]

**Passo 7:** Criar a **interface local** do EJB.

[Imagem da tela de criação de uma interface local para o EJB, mostrando o campo "Name" (SomaEJBLocal).]

**Passo 8:** Criar o **Servlet** no módulo WAR.

[Imagem da tela de criação de um Servlet, mostrando campos como "Class Name" e "Package".]

**Passo 9:** Implementar a **chamada** ao EJB no **Servlet**.

[Imagem do código Java da classe `ServletSoma`, mostrando a anotação `@WebServlet("/ServletSoma")`, a injeção do EJB (`@EJB private SomaEJBLocal somaEJB;`) e o uso do método `soma()` no método `doGet()`.]

-----

**Resultado da aplicação ServletSoma: 5**

-----

### MDBs

Um **Message-Driven Bean (MDB)** é um tipo de EJB que implementa o padrão **Java Message Service (JMS)** para permitir o **processamento assíncrono** de mensagens. Isso permite que um componente desenvolva duas classes no mesmo projeto: uma para **escutar** (realizar o *listen*) e outra para **enviar** (realizar o *send*) a mensagem. É muito utilizado em sistemas que se comunicam com o **sistema de mensagens** da aplicação.

  * Crie a **fila de conexão** e a **fila** no **glassfish**, utilizando o **assistente** (wizard).
  * Crie o EJB **Message-Driven Bean (MDB)** para fazer o **listen** e processar as **mensagens**.
  * No módulo EJB, desenvolva uma classe **MeuProduto**, que contenha um **Servlet**.
  * No projeto **web**, desenvolva o EJB.
  * No projeto **ear**, utilize o EJB com uma **fórmula** de envio de **mensagens**.
  * Acessar a aplicação a partir do **Servlet**.

Veja o resultado dessa prática:

*Minhaaplicacao.java*

-----

*QueueSender.java*

[Imagem do código Java da classe `QueueSender`, que configura a conexão JMS, busca a fila, cria um *session* e um *message producer*, e envia a mensagem de texto.]


-----

## 1\. Implementação de regras de negócio com EJBs

### Message-driven Beans (MDB)

**Message-driven Beans (MDB)** são componentes corporativos que implementam o padrão de **mensagens**. Esses processos promovem a **desacoplagem** das aplicações por meio de uma arquitetura de envio e recebimento de mensagens, o que permite a **comunicação assíncrona** e a **tolerância a falhas** em um ambiente de sistemas distribuídos.

Neste vídeo, você verá o que é um **Message-driven Bean (MDB)** e como ele é utilizado para implementar as regras de negócio de um sistema de forma **assíncrona**. Você também verá as suas principais características e vantagens em um ambiente de **aplicações corporativas** distribuídas.

> [Vídeo: Message-driven Beans (MDB)]

As **mensagens** atuam de forma **assíncrona** e possuem seis etapas:


As **mensagens** são depositadas em **tópicos** para que os **ouvintes** (listeners) as **recuperem**.

O pool de **mensagens** permite a **comunicação assíncrona** com o EJB, sendo totalmente **desacoplado**, ou seja, o cliente **não espera** pela resposta do EJB. A **plataforma** de mensagens utiliza a **fila** e armazena os **eventos** na **fila** para serem processados posteriormente.

Para criar uma mensagem, a **plataforma** de mensagens precisa de dois objetos: a **conexão** ao servidor e o **destino** (queue/topic).

```java
// Código Java para criação da conexão e destino
@Resource(mappedName = "jms/myQueueFactory")
private static ConnectionFactory connectionFactory;
@Resource(mappedName = "jms/myQueue")
private static Queue queue;
```

-----

**MDB (Message-driven Bean)**

O EJB que implementa as regras de negócio é o **Message-driven Bean (MDB)**, que atua como **ouvinte** (listener) de mensagens, recebendo os eventos da fila e processando a regra de negócio. Ele é um EJB do tipo **Stateless** (não armazena o estado), que implementa a interface **MessageListener** e tem o método **onMessage**, que é o **ponto de entrada** de todas as mensagens.

A **anotação** `@MessageDriven` é utilizada para configurar o MDB e é aplicada na classe de serviço. O MDB se conecta ao **servidor de mensagens** (como Glassfish, JBoss, Weblogic), que fornece a **plataforma de mensagens**. O servidor de mensagens é o responsável por **armazenar**, **gerenciar** e **entregar** as mensagens.

```java
// Código Java do MDB
@MessageDriven(mappedName = "jms/myQueue", activationConfig = {
    @ActivationConfigProperty(propertyName = "acknowledgeMode", propertyValue = "Auto-acknowledge"),
    @ActivationConfigProperty(propertyName = "destinationType", propertyValue = "javax.jms.Queue")
})
public class MeuMDB implements MessageListener {
    // ...
    @Override
    public void onMessage(Message message) {
        // Lógica de processamento da mensagem
        // ...
    }
}
```

> **DICA:**
> O MDB é o **único tipo de EJB** que **não** possui interface. O cliente **não acessa** o MDB de forma direta, ele apenas **envia** a mensagem para a fila e o MDB **processa** a mensagem, de forma **assíncrona**.

-----

#### Prática

A prática de MDB será implementada com o **Glassfish Server** utilizando:

  * Um **Session Bean** para enviar a mensagem.
  * Um **Message-Driven Bean** para processar a mensagem.
  * Um **Servlet** para iniciar o processo de envio.

**Passo 1**
Criar o **Message-Driven Bean (MDB)** no módulo EJB.

**Passo 2**
Criar o **Session Bean** para envio de mensagens, contendo o código para criar a **conexão de mensagens** e a **fila de destino** do MDB, por meio da anotação `@Resource`.

**Passo 3**
Criar o **Servlet** no módulo WAR.

**Passo 4**
Implementar o **envio de mensagem** por meio do **Servlet**, onde o envio deve ficar **desacoplado** do recebimento. O cliente **não** espera a resposta do MDB.

**Passo 5**
Ver o **recebimento** da mensagem no **log** do Glassfish Server.

-----

## 1\. Implementação de regras de negócio com EJBs

### Aplicando os MDBs

No mundo web é bastante comum a utilização de **recursos externos**, sejam eles de outro projeto de sua aplicação, ou até mesmo de outra aplicação. Essa comunicação é feita de forma **assíncrona** por meio de **mensagens**, permitindo a **comunicação** entre diferentes partes. Para facilitar a comunicação, uma excelente alternativa é a **troca de mensagens**.

Neste vídeo você verá como são utilizados **Message Driven Beans (MDBs)** baseados em **tópicos** para o envio de **mensagens** e a **notificação** de **eventos** de um sistema de forma **assíncrona** em uma implementação de **MDB**.


#### Roteiro de prática

A prática de MDB será no **ambiente de MDB (Message-driven Bean)**. Portanto, iremos aprender a **criação** desse tipo de componente desenvolvendo duas classes no mesmo projeto: uma para **escutar** (realizar o *listen*) e outra para **enviar** (realizar o *send*) a **mensagem**. É muito utilizado em sistemas que se comunicam com o **sistema de mensagens** da aplicação.

  * Crie a **fila de conexão** e a **fila** no **glassfish**, utilizando o **assistente** (wizard).
  * Crie o EJB **Message-Driven Bean (MDB)** para fazer o **listen** e processar as **mensagens**.
  * No módulo EJB, desenvolva uma classe **MeuProduto**, que contenha um **Servlet**.
  * No projeto **web**, desenvolva o EJB.
  * No projeto **ear**, utilize o EJB com uma **fórmula** de envio de **mensagens**.
  * Acessar a aplicação a partir do **Servlet**.

Veja o resultado dessa prática:

*Minhaaplicacao.java*

```java
// Código Java da classe Minhaaplicacao
public static void main(String[] args) throws Exception {
    Properties props = new Properties();
    props.setProperty("java.naming.factory.initial", "com.sun.enterprise.naming.SerialObjectFactory");
    // ...
    Context context = new InitialContext(props);
    QueueSender queueSender = (QueueSender) context.lookup("java:global/meuprojeto/QueueSender!br.com.estacio.QueueSender");
    queueSender.sendMessages("Hello, MDB!"); // chama o método de envio
}
```

-----

*QueueSender.java*

```java
// Código Java da classe QueueSender
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
// Código Java do MDBListener
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