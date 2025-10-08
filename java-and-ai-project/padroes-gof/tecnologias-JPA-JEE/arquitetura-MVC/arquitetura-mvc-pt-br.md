
-----

# 3\. Arquitetura MVC no Java

## Arquitetura MVC

A arquitetura **MVC** (Model-View-Controller) divide o sistema em três camadas, com responsabilidades específicas:

  * **Model**: Nesta camada, temos as **entidades** e as classes para a **persistência** dos objetos.
  * **View**: É a camada de **apresentação**, ou seja, o que o usuário vê.
  * **Controller**: Age, principalmente, como o **mediador** das ações, controlando os objetos de negócio.

Para saber um pouco mais sobre a arquitetura MVC, veja o vídeo a seguir:

*(Conteúdo visual - Vídeo: "Arquitetura MVC")*

Neste vídeo, você vai entender a arquitetura MVC no desenvolvimento web. Esta é a arquitetura consiste em separar a aplicação em três partes (model, visualização e controlador), permitindo o seu gerenciamento de forma transparente.

### Camadas da MVC

A seguir, vamos ver as principais características de cada camada que compõem a arquitetura MVC:

#### Model (modelo)

  * **Entidades** de negócio.
  * Contém a **lógica** de negócio.
  * Conexões e chamadas ao **banco de dados**.
  * Pode usar objetos de valor.
  * Pode utilizar mapeamento objeto-relacional.
  * **Não depende** da View.

#### Controller (controlador)

  * Implementa as **regras de negócio** do sistema.
  * Define a sequência de chamadas.
  * **Recebe** requisições de usuário.
  * Pode utilizar objetos distribuídos.
  * **Não depende** da View e do Model.

#### View (visualização)

  * Define a **interface** do sistema.
  * Responsável pela **apresentação** da informação.
  * Contém apenas **regras de formatação**.
  * **Interage** com o Controller do sistema.
  * Não pode acessar a camada Model.

Uma regra fundamental para a arquitetura MVC é a de que os elementos da camada **View** não podem acessar a camada **Model** (dados), nem a camada **Controller**. Os elementos da camada **Controller** não podem acessar a camada **View**. Os elementos da camada **Model** agem com restrições exclusivamente para os objetos de negócio.

> A arquitetura **MVC é baseada em camadas**, e cada camada enxerga apenas a camada **imediata** (superior ou inferior).

Em uma arquitetura MVC, as entidades são as unidades de informação para o trânsito entre as camadas. Todos os comandos SQL ficam concentrados nas classes **DAO** (Data Access Object). Apenas a camada **Controller** pode acessar a **Model** e esta atua as classes **DAO** (que contém os comandos SQL) para acessar o banco de dados.

Como os comandos **SQL** são bastante padronizados, é possível criar ferramentas para a geração dos comandos e possivelmente das classes **DAO**. Esta forma de pensar deu origem ao padrão de projeto de desenvolvimento **DAO**.

A camada **Controller** precisa ser definida de maneira independente do ambiente específico, como **Interfaces (DAO)** ou utilizando **padrões de projeto** de desenvolvimento, como **Factory**. Desta forma, classes **Controller** distintas (que não os *models* que gerenciam os componentes DAO), que define a complexidade nas atividades subordinadas iniciais na **View**.

Desta forma, a camada **Controller** é o melhor local para definir as regras de **autorização** para o uso de funcionalidades do sistema. Os **beans** podem ser implementados como *helpers* (auxiliares) para a correta implementação da lógica de negócio. A **regra de negócio** será a atribuição da camada **Controller**. Nos *models* atuará, sempre, a geração de um *bean*, mantendo a persistência entre as camadas.

## Componentes Java para MVC

Grande parte da arquitetura MVC é voltada para o **desenvolvimento de aplicações web**. O objetivo é que a aplicação web possa separar o processamento da requisição do cliente do código de apresentação (HTML, XML, etc.) do código de acesso ao banco de dados.

Para saber um pouco mais sobre os componentes Java para MVC, assista ao vídeo a seguir:

*(Conteúdo visual - Vídeo: "Componentes Java para MVC")*

A imagem a seguir mostra a relação entre os componentes e as camadas do MVC:

*(Conteúdo visual - Diagrama mostrando a relação entre **JSP/HTML/JSTL** na **View**, **Servlet/Action** no **Controller**, e **POJO/DAO/JDBC** no **Model**.)*

Os componentes em cada camada do MVC:

  * **View**: **JSP**, **HTML**, **JSTL**
  * **Controller**: **Servlet**, **Action**
  * **Model**: **POJO**, **DAO**, **JDBC**

-----

# Padrões de Desenvolvimento

A orientação a objetos representa um grande avanço na implementação de sistemas. Este conceito aprimorou o desenvolvimento de sistemas complexos, permitindo que a solução de problemas possa ser elaborada com uma lógica de raciocínio.

Para saber um pouco mais sobre padrões de desenvolvimento, assista ao vídeo a seguir:

*(Conteúdo visual - Vídeo: "Entregando Soluções com Design Patterns")*

## Descrição dos Padrões de Desenvolvimento

Vejamos uma descrição formal dos padrões de desenvolvimento citados, além de alguns outros, comumente em sistemas criados para o ambiente J2EE:

  * **Abstract Factory**: define uma estrutura abstrata para a criação de famílias de classes ou interfaces.
  * **Factory Method**: define uma estrutura para a criação de algum tipo de requisição, muito utilizado para o tratamento de solicitações feitas pelo Controller.
  * **Data Access Object (DAO)**: utiliza classes específicas para o acesso às fontes de dados (bancos de dados).
  * **Facade**: simplifica as chamadas para um sistema complexo.
  * **Intercepting Filter**: cria filtros entre as chamadas do cliente e o sistema, impedindo que requisições estranhas possam criar chamadas.
  * **Service Locator**: fornece uma central para a localização e o direcionamento de chamadas para cada chamada.
  * **Value Object**: trata objetos que representam uma coleção, o que é implementado naturalmente em Java.
  * **Transfer Object**: também conhecido como **DTO** (Data Transfer Object), são objetos usados nos serviços para otimizar a conexão transparente para o cliente.
  * **Session Facade**: encapsula a lógica de negócio de componentes, com base em serviços de nomes e diretórios.
  * **View Helper**: auxilia a camada de View na formatação para a tela, como em controles de acesso.
  * **Front Controller**: gerencia o ponto único de execução, com base em algum padrão fornecido.

## Padrões Arquiteturais

Padrões arquiteturais definem a **estrutura básica** do sistema, como os componentes e interações, com dois objetivos: a **economia** e a **troca** de projetos na implementação do sistema. A escolha do padrão arquitetural é o primeiro passo para a implementação do sistema.

Para saber um pouco mais sobre padrões de projeto, bem como exemplos, assista ao vídeo a seguir:

*(Conteúdo visual - Vídeo: "Introdução aos Padrões Arquiturais")*

### Modelos de Padrões Arquiteturais

| Padrões Arquiteturais | Modelos |
| :--- | :--- |
| Broker | Sistemas Distribuídos |
| Cliente-Servidor | Sistemas Distribuídos |
| Camadas | Sistemas com camadas internas |
| Estrutura e objetos | Sistemas com camadas internas |
| Mestre-Escravo | Sistemas com camadas internas |
| Pipes e Filtros | Sistemas que tratam dados sequenciais |
| Repositório | Sistemas de dados centrais |
| Peer-to-Peer (P2P) | Sistemas com componentes independentes |
| Intérprete | Sistemas que interpretam comandos |
| Máquina Virtual | Sistemas que processam comandos internos |
| Event-Driven | Sistemas com eventos e tratamento |
| MVC | Sistemas Interativos |
| BlackBoard | Sistemas de Inteligência Artificial |
| MicroKernel | Sistemas Adaptativos |
| Reflection | Sistemas Adaptativos |

**Modelo de Objetos Distribuídos**: a ideia deste padrão arquitetural para o desenvolvimento de *software* é que não se perca tempo em reinventar a roda. O modelo de objetos distribuídos é um dos mais implementados e que pode ser considerado um padrão arquitetural, com todas as conveniências integradas aos conflitos.

O modelo de objetos distribuídos permite que componentes de *software* criados na linguagem Java possam ser chamados e processados por *softwares* criados em outra linguagem, como **C++**, por exemplo.

-----

# Implementação de MVC no ambiente Java

O desenvolvimento de um sistema utilizando a arquitetura MVC no ambiente Java é importante. Ele define como os componentes serão implementados nas camadas de banco e modelo. Contudo, esse tipo de situação costuma envolver muitas linhas de código, devido à repetição de código (código *boilerplate*).

Neste tópico, você vai encontrar um bom exemplo de implementação de um sistema de banco de dados para armazenar dados e a regra de negócio na camada de banco de dados e a de apresentação de dados na camada View.

Para saber mais sobre a implementação de MVC no Java, assista ao vídeo a seguir:

*(Conteúdo visual - Vídeo: "Implementação de MVC no ambiente Java")*

## Roteiro de prática

Utilizar o exemplo do sistema de produtos a seguir com as camadas correspondentes à arquitetura **"três camadas"** do MVC, que contém: **Model** (que são os dados), **Controller** (o motor que define o que deve ser feito) e **View** (a apresentação da informação ao usuário).

1.  Criar as classes e inserir os dados de amostra à persistência.
2.  Criar um objeto de conexão à persistência para a classe Model.
3.  Criar um **DAO** (*Data Access Object*) para gerenciamento da entidade Produto.
4.  Criar um **Servlet** (*Controller*) para a chamada das regras de negócio.
5.  Criar um **JSP** (*View*) para a apresentação dos dados.

-----

## Códigos de Exemplo

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
    
    // ... [código continua]
```

### Produto.java

```java
package br.model;

public class Produto {
    private int id;
    private String nome;
    private double preco;
    private int quantidade;
    
    // ... [construtores e métodos getters/setters]
    
    @Override
    public String toString() {
        return "Produto [id=" + id + ", nome=" + nome + ", preco=" + preco + ", quantidade=" + quantidade + "]";
    }
}
```

### Entidade.java

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

### ProdutoDAO.java

```java
package br.DAO;

public class ProdutoDAO extends DAO {
    // ... [método listarProdutos() e outros métodos de acesso ao BD]
    
    public ArrayList<Produto> listarProdutos() {
        ArrayList<Produto> produtos = new ArrayList<>();
        // ... [lógica para buscar produtos no BD]
        return produtos;
    }
}
```

### ProdutoFactory.java

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

### Produtos.jsp

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

### Exemplo de Saída (Servlet Simples)

*(Conteúdo visual - Imagem de tela mostrando a saída de um Servlet, exibindo uma lista de produtos.)*

**Servlet Exemplo Simples**

  * Morango
  * Banana
  * Manga
  * Abacate