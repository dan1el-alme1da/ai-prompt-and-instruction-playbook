# JPA no desenvolvimento web

## Mapeamento objeto-relacional

O Mapeamento Objeto-Relacional (**ORM**) consiste em técnicas que permitem relacionar valores em vigor nas classes do sistema. Assim, o mapeamento objeto-relacional provê a comunicação entre a aplicação e o banco de dados. Essa comunicação consiste em traduzir os dados da linguagem de programação orientada a objetos (POO) para o formato do banco de dados relacional e vice-versa. Por meio do mapeamento, você não precisa mais se preocupar com os esquemas e comandos SQL. Essa tarefa será automatizada para você, para que o programador tenha apenas que se preocupar em trabalhar com uma estrutura orientada a objetos sem se preocupar com comandos de banco de dados.

### ORM no Java: JPA

O JPA (**Java Persistence API**) é uma especificação da linguagem **Java** que provê uma interface de comunicação entre o **ORM** e os dados no banco. Dessa forma, ao usar o **JPA**, você consegue realizar o mapeamento objeto-relacional, provendo a persistência de dados. A partir desse conceito, a estrutura de classes, atributos e valores da classe, serão mapeadas para **tabelas**, **colunas** e **registros** do banco de dados, respectivamente.

### Entity Manager

Ele porta a integração do **JPA** com o **Hibernate** (Editora) e o gerenciador de consultas e persistência de dados. Por meio do **EntityManager**, você será capaz de incluir, alterar, excluir e consultar registros no banco de dados. Você verá, a seguir, alguns exemplos de como o **EntityManager** pode ser utilizado para manipular os dados.

No trecho de código, temos a forma de definição de um **entity bean**, em que o objeto é gerado pela chamada do método **`createEntityManagerFactory()`** da classe **`Persistence`**. O método **`createEntityManager()`** da **`EntityManagerFactory`** gera o objeto **`EntityManager`** que será utilizado para manipular os dados.

```java
public class Principal {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.
            createEntityManagerFactory("topicos");
        EntityManager em = emf.createEntityManager();
        em.getTransaction().begin();
        em.close();
        emf.close();
    }
}
```

No trecho de código a seguir, temos um exemplo da classe **`Produto`**, com os comandos **`setter`** e **`getter`** e parte dos métodos do **`EntityManager`** para realizar a inclusão, alteração e exclusão de dados no banco de dados, que será apresentado nas próximas seções.

```java
public class Produto {
    private int id;
    private String nome;
    private String descricao;
    private double preco;

    // Métodos setter e getter

    @Override
    public String toString() {
        return "Produto [id=" + id + ", nome=" + nome + ", descricao=" + descricao +
            ", preco=" + preco + "]";
    }
}
```

### Hibernate

Para realizar o mapeamento, o padrão **JPA** é a interface, que exige uma implementação para funcionar. O **Hibernate** é um dos *frameworks* mais utilizados para implementação do **JPA**. Ele é um *framework* de persistência de código aberto que provê o gerenciamento e a persistência de dados por meio da integração com a interface **JPA**.

Para a **Hibernate**, as anotações são apenas classes comuns, sem métodos de negócios, também conhecidas como **Plain Old Java Objects** (**POJO**). Os objetos **POJO** são representações das tabelas do banco de dados, em que as colunas e os dados, ou seja, o mapeamento objeto-relacional, provê a persistência de dados.

```java
@Entity
@Table(name = "PRODUTO")
public class Produto {
    @Id
    @Column(name = "ID")
    private int id;
    @Column(name = "NOME")
    private String nome;
    @Column(name = "DESCRICAO")
    private String descricao;
    @Column(name = "PRECO")
    private double preco;

    // Métodos setter e getter
}
```

-----

## Java Persistence API

Para que você consiga usar o **ORM** no **Java**, você precisa utilizar o **JPA**. O **Java Persistence API** é uma especificação da linguagem **Java** que provê uma interface de comunicação entre o **ORM** e os dados no banco de dados. O **JPA** permite a persistência e a consulta de dados em ambientes **Java SE** e **Java EE**. Por ser apenas uma especificação, para funcionar, o **JPA** precisa de uma implementação, que pode ser o **Hibernate** ou o **EclipseLink**. A configuração dos dados de persistência deve ser feita por meio de um arquivo XML, geralmente configurado por meio do **JPA**.

Com o uso do **JPA**, você não precisa mais se preocupar em trabalhar diretamente com as tabelas, colunas e **SQL**. Tudo isso será gerado automaticamente pelo **ORM** do **JPA**. Também verá como o **JPA** provê a persistência e a consulta de dados em aplicações **Java**.

### Utilizando a anotação **`@Entity`** do **JPA**

Para definir uma entidade **JPA**, devemos utilizar uma classe sem métodos de negócios, também conhecida como **Plain Old Java Object** (**POJO**). Os objetos **POJO** são representações das tabelas do banco de dados, em que as colunas e os dados, ou seja, o mapeamento objeto-relacional, provê a persistência de dados.

```java
@Entity
@Table(name = "PRODUTO")
public class Produto implements Serializable {
    @Id
    @Column(name = "ID")
    private int id;
    @Column(name = "NOME")
    private String nome;
    @Column(name = "DESCRICAO")
    private String descricao;
    @Column(name = "PRECO")
    private double preco;

    // Métodos setter e getter
}
```

A anotação **`@Entity`** define a classe **`Produto`** como uma entidade para o **JPA**, enquanto **`@Table`** especifica a tabela do banco de dados. Os atributos são anotados com **`@Id`** para o campo chave primária e **`@Column`** para as colunas. A classe **`Produto`** implementa o **`Serializable`**, para que o objeto possa ser serializado. Veremos a seguir, de forma mais detalhada, as anotações principais de uma entidade gerenciada pelo **JPA**, para o mapeamento objeto-relacional.

#### Principais anotações de persistência de entidades do JPA:

  * **`@Entity`**: marca a classe como uma entidade para o **JPA**.
  * **`@Table`**: especifica a tabela que representa a entidade no banco.
  * **`@Id`**: especifica o atributo para a chave primária da tabela.
  * **`@Column`**: mapeia o atributo para as colunas da tabela.
  * **`@GeneratedValue`**: especifica o tipo de geração de valor de um atributo (como **`AUTO`** para incremento automático).
  * **`@Basic`**: especifica que um atributo é um campo da entidade.
  * **`@Transient`**: especifica que um atributo não é um campo da entidade.
  * **`@OneToOne`**: mapeia um relacionamento de um para um.
  * **`@OneToMany`**: mapeia um relacionamento de um para muitos.
  * **`@ManyToOne`**: mapeia um relacionamento de muitos para um.
  * **`@ManyToMany`**: mapeia um relacionamento de muitos para muitos.
  * **`@Embedded`**: mapeia a classe para ser incorporada em outra entidade.
  * **`@EmbeddedId`**: mapeia a classe como chave primária incorporada.
  * **`@JoinColumn`**: especifica a regra de relacionamento da chave.
  * **`@OrderBy`**: especifica a ordem de consulta.

As entidades **JPA** devem conter um construtor vazio e um atributo baseado na chave primária, como por exemplo, um atributo **`int`** para o **ID**, que será a chave primária. Além disso, a classe deve implementar a interface **`Serializable`**, para que a classe possa ser serializada.

A anotação de persistência do atributo **`@GeneratedValue`** retorna o modo de criação utilizado nos processos do **JPA**. Os tipos de criação podem ser **`AUTO`**, **`IDENTITY`**, **`SEQUENCE`** e **`TABLE`**. Por padrão, o tipo é **`AUTO`** para a geração de valor da entidade.

O arquivo **`persistence.xml`** é utilizado para configurar a unidade de persistência **JPA**, definindo as entidades a serem persistidas e as propriedades do **ORM** (como o **Hibernate** ou **EclipseLink**). As propriedades podem incluir elementos como a classe do *driver* **JDBC** (**Java Database Connectivity**) ou as regras de conexão, como o **URL** do banco, o nome do usuário e a senha.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence version="2.0"
    xmlns="http://java.sun.com/xml/ns/persistence" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd">
    <persistence-unit name="topicos" transaction-type="RESOURCE_LOCAL">
        <provider>org.eclipse.persistence.jpa.PersistenceProvider</provider>
        <class>produto.Produto</class>
        <properties>
            <property name="javax.persistence.jdbc.driver" value="org.apache.derby.jdbc.ClientDriver"/>
            <property name="javax.persistence.jdbc.url" value="jdbc:derby://localhost:1527/dbproduto"/>
            <property name="javax.persistence.jdbc.user" value="app"/>
            <property name="javax.persistence.jdbc.password" value="app"/>
        </properties>
    </persistence-unit>
</persistence>
```

A unidade de persistência referenciada é o nome da **unidade de persistência** (**`persistence-unit`**) com o valor **`topicos`**. O tipo de transação é **`RESOURCE_LOCAL`**, para que o **JPA** possa se integrar com o **JDBC** e o **Java Transactions API** (**JTA**).

O controle transacional pode ocorrer a partir de um *driver* próprio, para uso na interface **JSE** (**Java Standard Edition**) ou por meio da integração com **JTA** (**Java Transactions API**).

Os tipos são:

| Tipo | Descrição |
| :--- | :--- |
| **`RESOURCE_LOCAL`** | Utiliza o gestor de transações do **JPA**. Serve para a integração com **JDBC** e **Java Transactions API** (**JTA**). |
| **`JTA`** | Utiliza o gestor de transações do servidor de aplicação. Serve para a integração com **JTA** em ambientes **Java EE** (**Java Enterprise Edition**). |

Em seguida, definimos o *provider* de persistência no elemento **`<provider>`**. O elemento **`<class>`** define as classes de entidades e as propriedades de conexão são definidas no grupo **`<properties>`**.

-----

## Consulta e manipulação de dados

Com o uso do **EntityManager**, você poderá consultar e manipular os dados utilizando os comandos de **inclusão**, **alteração**, **exclusão** e **consulta**.

Dessa forma, você terá o mapeamento entre a aplicação e o banco de dados utilizando a tecnologia **JPA** e a implementação do **Hibernate** ou **EclipseLink**. Você verá a seguir como manipular os dados da aplicação.

### Consulta de dados

Para consultar os dados no banco, você precisa obter a instância do **EntityManager** por meio do **`EntityManagerFactory`** e utilizar a **JPQL** (**Java Persistence Query Language**), uma linguagem de consulta orientada a objetos que se baseia na estrutura **SQL** (Structured Query Language). Você verá a seguir como utilizar o **JPQL** para consultar dados no banco.

#### Passo 1

O primeiro passo é a obtenção do **EntityManagerFactory**, utilizando o nome da unidade de persistência (no exemplo, **`topicos`**) definida no **`persistence.xml`**. Em seguida, obtenha o **EntityManager** para manipular as operações de consulta.

```java
EntityManagerFactory emf = Persistence.createEntityManagerFactory("topicos");
EntityManager em = emf.createEntityManager();
```

#### Passo 2

Defina a consulta por meio do **JPQL**, que deve se basear nos atributos da classe e o nome da classe. A consulta **não** deve se basear no nome da tabela e sim no nome da classe.

```java
Query query = em.createQuery("SELECT p FROM Produto p");
```

#### Passo 3

Por último, execute a consulta e obtenha os resultados em uma **`List`** em **Java**. Em termos práticos, a consulta **JPQL** é traduzida pelo **ORM** (como **Hibernate** ou **EclipseLink**) em uma consulta **SQL** nativa, que é executada no banco de dados.

```java
List<Produto> lista = query.getResultList();
for (Produto produto : lista) {
    System.out.println(produto);
}
```

> **Atenção:**
>
> Você precisa utilizar o **JPQL** para realizar a consulta, que será convertida em **SQL** pelo **JPA**. Caso você utilize um comando **SQL** diretamente no **`em.createQuery()`**, o **JPA** não fará a conversão automática, utilizando apenas o comando **SQL** nativo.

### Inclusão de dados

Para incluir, você precisa criar uma instância do **Produto** e adicionar o objeto em uma **`List`** no banco de dados.

```java
Produto p1 = new Produto(1, "TV", "TV de LED", 700.0);
em.getTransaction().begin();
em.persist(p1);
em.getTransaction().commit();
```

-----

## Exclusão de dados

Para excluir um registro, devemos utilizar o método **`remove()`** para recuperar e realizar a exclusão. A exclusão, por ser uma alteração no banco de dados, precisa ocorrer entre **`em.getTransaction().begin()`** e **`em.getTransaction().commit()`**.

```java
Produto p = em.find(Produto.class, 1);
em.getTransaction().begin();
em.remove(p);
em.getTransaction().commit();
```

Podemos perceber que as classes **`Find`**, **`persist`**, **`merge`** e **`remove`** correspondem, respectivamente, aos comandos **`SELECT`**, **`INSERT`**, **`UPDATE`** e **`DELETE`** do **SQL**.

-----

## Execução do aplicativo

Para a execução do aplicativo, vamos adicionar o *driver* do **Derby** e o *framework* **EclipseLink** ao projeto. Para adicionar as bibliotecas, clique com o botão direito do mouse no nome do projeto e clique em **Build Path -\> Configure Build Path...** e adicione as bibliotecas **EclipseLink.jar** e o *driver* **Derby** na aba **Libraries**.

### Adicionando a biblioteca **JDBC** e o *framework* **EclipseLink**

As bibliotecas que serão adicionadas são:

  * **`EclipseLink.jar`**
  * **`derbyclient.jar`** (ou **`derby.jar`** se for utilizar o modo embarcado)

### Configurando o driver Derby

Clique com o botão direito do mouse no projeto, e clique em **Propriedades -\> Java Build Path** e adicione o *driver* **Derby** na aba **Libraries**.

### Definindo a conexão do banco **Derby**

Para definir a conexão do banco de dados, clique em **Window -\> Show View -\> Other...** e selecione a opção **Data Source Explorer**. Em seguida, clique em **Database Connections** e clique em **New**.

### Resultado de execução do aplicativo

A execução do aplicativo irá gerar a tabela **`PRODUTO`** no banco de dados e inserir o registro do **Produto** nela.

-----

## Manipulando dados com NamedQueries

Como já vimos, é extremamente necessário estabelecer a comunicação entre a aplicação e o banco de dados. Normalmente, precisamos realizar alguma consulta parametrizada, o que inclui receber os parâmetros da consulta diretamente na aplicação e passar para o banco de dados.

Neste vídeo, você verá na prática como utilizar parâmetros em **NamedQueries** para recuperar registros da tabela.

### Roteiro de prática

Avançaremos um passo a mais, propondo a você o seguinte desafio: alterar o programa apresentado anteriormente, de forma que a classe **`Principal`** utilize o **código** para selecionar um produto específico, através do uso de **NamedQueries**.

Para isso, você deverá:

  * Utilizar uma anotação para criar uma **NamedQuery** na classe **`Produto`**.
  * Alterar a **NamedQuery** utilizada em **`Principal`**.
  * Testar o resultado da prática.

**Resultado esperado dessa prática:** **`Classe "Modelo.Produto"`.**

### Classe **`Produto`**

```java
@Entity
@Table(name = "PRODUTO")
@NamedQueries({
    @NamedQuery(name = "Produto.findAll", query = "SELECT p FROM Produto p"),
    @NamedQuery(name = "Produto.findById", query = "SELECT p FROM Produto p WHERE p.id = :id"),
    @NamedQuery(name = "Produto.findByName", query = "SELECT p FROM Produto p WHERE p.nome LIKE :nome"),
    @NamedQuery(name = "Produto.findByPrice", query = "SELECT p FROM Produto p WHERE p.preco BETWEEN :precoMin AND :precoMax")
})
public class Produto implements Serializable {
    // ... atributos, construtor, getters e setters ...
}
```

### Classe **`Principal`**

```java
public class Principal {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.
            createEntityManagerFactory("topicos");
        EntityManager em = emf.createEntityManager();

        // Consulta usando NamedQuery com parâmetro
        Query query = em.createNamedQuery("Produto.findById");
        query.setParameter("id", 1); // Supondo que você queira buscar o produto com id=1

        Produto produto = (Produto) query.getSingleResult();

        if (produto != null) {
            System.out.println("Produto encontrado: " + produto.toString());
        } else {
            System.out.println("Produto não encontrado.");
        }

        em.close();
        emf.close();
    }
}
```