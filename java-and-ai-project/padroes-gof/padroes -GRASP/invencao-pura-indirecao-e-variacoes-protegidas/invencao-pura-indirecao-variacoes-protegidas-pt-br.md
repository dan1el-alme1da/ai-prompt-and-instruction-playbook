# Padrões Invenção Pura, Indireção e Variações Protegidas

## O padrão Invenção Pura

O padrão **Invenção Pura** é um padrão de criação de classes artificiais que não representam conceitos do domínio, mas que são criadas para alcançar princípios de design, como baixo acoplamento e alta coesão. Essas classes são usadas para atribuir responsabilidades complexas a um objeto sem complexar classes que já são complexas.

### Identificando o problema

Ao desenvolver um software, um dos primeiros problemas que podem impactar em um sistema envolve a atribuição de responsabilidades. O padrão **Invenção Pura** é um padrão para atribuição de responsabilidades, que sugere atribuir a uma classe artificial a responsabilidade de realizar uma tarefa complexa, em vez de atribuí-la a uma classe que já possui complexidade alta.

**Por exemplo:** em um sistema de **Alocação de Recurso**, trata-se de decidir que disciplina ligada a um certo curso pode ter a responsabilidade de salvar os dados de um `Pedido` em um banco de dados relacional. Para resolver esse problema, é aplicado o padrão **Invenção Pura**.

Seguindo o padrão Invenção Pura, criamos a classe **PersistentePedido** que será responsável por realizar as tarefas de armazenamento e recuperação de dados.

O código e a figura mostram a implementação da classe `Pedido` com as 4 atribuições e a responsabilidade de salvar o pedido. Essa responsabilidade, no código sem o padrão, é exercida pela própria classe `Pedido`. Com a aplicação do padrão **Invenção Pura**, a responsabilidade é transferida para a classe `PersistentePedido`, que é uma classe artificial que não representa um conceito no domínio, mas que será responsável por fazer a persistência de dados.

**Código (Sem Padrão - Responsabilidade na própria classe):**

```java
class Pedido {
    private String id;
    private Data data;
    private Cliente cliente;

    // Atributos do Pedido
    // Atribuições:
    // 1 - Conhecer o total do pedido
    // 2 - Criar um item de pedido
    // 3 - Conhecer o status do pedido
    // 4 - Conhecer o valor do frete
    
    public void salvar() {
        // Lógica para salvar os dados no banco de dados.
    }
    
    public void criarItem() {
        // ...
    }
}
```

### Consequências da Invenção Pura

A Invenção Pura permite a criação de classes artificiais, sendo estas classes com coesão e acoplamento baixo, sendo muito simples. A principal consequência da Invenção Pura é que ela permite uma maior flexibilidade e reuso do código.

| Estratégia | Descrição |
| :--- | :--- |
| **Baixo acoplamento** | O padrão Invenção Pura garante que a classe `Pedido` fique acoplada somente a abstrações de classes artificiais, como a `PersistentePedido` (invenção pura). |
| **Alta coesão** | O padrão garante que a classe `Pedido` e a `PersistentePedido` tenham alta coesão. |
| **Reuso** | O padrão promove o reuso de código. |

**Decomposição do Sistema:**

| DECOMPOSIÇÃO POR REPRESENTAÇÃO | DECOMPOSIÇÃO POR COMPORTAMENTO |
| :--- | :--- |
| A decomposição de um módulo define a maneira como um subsistema pode ser dividido em módulos que contêm as suas representações. | Os padrões GOF (Gang of Four), que se destina a organizar os objetos em um sistema. Os objetos são organizados pelo conceito de domínio do alvo. |

Uma linha pura deve ter diferentes políticas de desenho para padrões dependendo do domínio do alvo. A Invenção Pura é um padrão de atribuição de responsabilidades. Outros padrões que se baseiam em Invenção Pura, como Adapter, Strategy e Command, são exemplos de aplicação de padrão Invenção Pura, pois eles são baseados na decomposição do comportamento.

## O padrão Indireção

O padrão **Indireção** é um padrão de design que atribui responsabilidades a objetos intermediários (intermediários) para desacoplar componentes em um sistema.

**Indireção** é um padrão para atribuição de responsabilidades, que sugere criar um objeto intermediário para mediar a comunicação para reduzir o acoplamento entre classes. O objeto intermediário é um *delegate* ou *adaptador*.

### Identificando o problema

O problema mais evidente é a utilização de uma das técnicas mais simples e mais utilizadas em projetos de software, que pode ser percebida pela falta de padrões de design. A maior parte dos problemas que ocorrem no sistema pode ser resolvida pelo padrão Indireção.

Ao interagir com o sistema, por exemplo, ao criar uma operação de um objeto `Serviço A`, como falar com um objeto `Serviço B`, a forma mais simples e direta de comunicação entre os objetos é o acoplamento direto.

### Solução da Indireção

O padrão sugere criar um objeto intermediário, como um *delegate* ou *adaptador*, para mediar a comunicação de forma indireta entre duas classes ou dois subsistemas.

**Regras de design**

  * **Coesão:** É a medida de quão fortemente relacionadas e focadas estão as responsabilidades de um elemento.
  * **Acoplamento:** É a medida de quão fortemente um elemento é conectado, tem conhecimento, ou depende de outros elementos.

Exemplo de como o padrão Indireção pode ser utilizado nas situações descritas anteriormente:

  * **Comunicação entre objetos remotos:**
    O padrão Indireção pode ser aplicado em sistemas que precisam de comunicação entre objetos e/ou sistemas distribuídos. Usa-se o conceito de **Stub** e **Skeleton** no RMI e de **Web Services** para implementar o padrão.

  * **API de serviços:**
    É um objeto intermediário que serve de fachada de serviços fornecidos por um subsistema. Esses objetos atuam como intermediários entre os clientes e os serviços.

Com isso, podemos perceber como diversos padrões de projeto (GOF) são aplicações específicas do padrão **Indireção**.

### Consequências da Indireção

O padrão Indireção pode gerar uma série de benefícios para o sistema, tais como:

  * Reduzir o acoplamento entre as classes.
  * Aumentar a flexibilidade e o reuso de código, pois permite o reuso de classes e objetos.
  * Aumentar a coesão.
  * Pode melhorar o desempenho, dependendo da carga e PAsS (Plataform as a Service) utilizadas.

## O padrão Variações Protegidas

O padrão **Variações Protegidas** identifica pontos de variação e protege o design do sistema. Ele é implementado utilizando **Indireção** e **Polimorfismo** para encapsular o que pode variar.

O padrão **Variações Protegidas** é um padrão de design que auxilia na atribuição de responsabilidades. Ele envolve o uso de **Indireção** e **Polimorfismo** para encapsular elementos que são propensos a variações.

O padrão é aplicado a sistemas que precisam se adaptar a diferentes tecnologias (ex: banco de dados, comunicação, etc.).

### Identificando o problema

Quando as operações de manutenção se tornam mais difíceis, a maior parte dos problemas pode impactar o sistema, a manutenção se torna difícil quando mudanças em uma plataforma afetam as outras partes do sistema. Por exemplo, mudar o banco de dados.

### Solução da Variação

O padrão **Variações Protegidas** sugere a criação de uma interface para abstrair a variação e evitar o acoplamento direto.

**Aspectos de interação com o sistema:**

  * **Interfaces:** Definem o contrato de interação entre classes.
  * **Adapters:** Adaptam a interface de uma classe para outra que o cliente espera.
  * **Encapsulamento:** Esconde a complexidade de uma classe.

**Regras de design:**

  * **Acoplamento:** Reduzir o acoplamento é o objetivo.
  * **Coesão:** Aumentar a coesão das classes.

O padrão sugere a criação de uma **interface** para desacoplar as classes dependentes da classe que está variando.

**Exemplo:**
Considere um sistema que pode armazenar seus dados em diferentes bancos de dados, como SQL Server, MySQL, Oracle, ou um banco de dados NoSQL.

**Código (Sem Variações Protegidas - Acoplamento direto):**

```java
class Sistema {
    BD bd = new BD();
    public void salvarPedido() {
        bd.salvar();
    }
}

class BD {
    public void salvar() {
        // Lógica para salvar no banco de dados.
    }
}
```

O código sem o padrão **Variações Protegidas** acopla diretamente a classe `Sistema` à classe `BD`. Se o tipo de banco de dados mudar, a classe `Sistema` terá que ser alterada.

**Solução (Com Variações Protegidas):**
Criação de uma interface `IDataAccess` (abstração) e classes concretas (`SqlServerDataAccess`, `NoSqlDataAccess`).

  * **Solução para MySQL, Oracle ou SQL Server:** Utiliza classes concretas para cada banco de dados relacional.
  * **Solução para NoSQL:** Utiliza classes concretas para bancos de dados não relacionais.

**Código (Com Variações Protegidas - Acoplamento à Interface):**

```java
class Sistema {
    IDataAccess iDataAccess;

    public Sistema() {
        this.iDataAccess = new SqlServerDataAccess(); // Ou NoSqlDataAccess
    }

    public void salvarPedido() {
        iDataAccess.salvar(); // Acoplamento ao abstração (Interface)
    }
}
```

**Regras de Design:**

  * **Encapsulamento:** Esconder os detalhes de implementação das classes.
  * **Reuso:** Promover o reuso do código.

### Consequências das Variações Protegidas

  * **Baixo acoplamento:** Entre as classes dependentes e as que variam.
  * **Alta coesão:** As classes são mais coesas, pois estão focadas em uma única responsabilidade.
  * **Flexibilidade e reuso:** É mais fácil mudar a variação.
  * **Manutenibilidade:** O sistema se torna mais fácil de manter.
  * **Variações Protegidas:** O padrão protege contra variações futuras.