
-----

## 1\. Padrões de projeto Composite e Facade

### Intenção e problema resolvido pelo padrão Composite

O padrão **Composite** tem como intenção permitir a criação de estruturas de árvore para representar **hierarquias de parte-todo**. Ele resolve o problema de tratar **objetos individuais e composições de objetos de maneira uniforme**, o que significa que o código do cliente pode usar a mesma interface para manipular tanto objetos simples quanto coleções de objetos.

O **Composite** é ideal para representar estruturas hierárquicas em que partes podem ser tratadas de forma igual. Ele simplifica a utilização e a reestruturação do código e o manejo na manipulação de estruturas complexas dentro de um sistema.

### Intenção do padrão Composite

O padrão **Composite** permite **representar hierarquias de composição de objetos em uma estrutura de árvore**, onde cada objeto (individual) ou um grupo de objetos (composição) respondem ao mesmo conjunto de operações.

### Problema resolvido pelo padrão Composite

Suponha que você esteja desenvolvendo um programa de envio e recebimento de mensagens. Inicialmente, você tem classes para enviar e para receber mensagens de um único usuário (objetos simples).

$$\text{Diagrama representativo de um único usuário para enviar e receber mensagens.}$$

Você precisa aplicar operações em ambos os objetos (simples e agregados): pagar uma mensagem, criptografar uma mensagem, entre outras.

Essas mesmas operações se aplicam ao **padrão**, sendo que **delegar uma mensagem** a vários usuários, **criptografar mensagens** de vários usuários e **apagar mensagens** de vários usuários.

O padrão **Composite** é especialmente interessante para problemas de representação de hierarquias de elementos nos quais observamos a redundância na execução das operações aplicadas aos agregados.

O código apresentado a seguir ilustra a estrutura de implementação para o problema sem utilizar o padrão Composite:

```java
public interface ServicosMensagem {
	void enviarMensagem(Mensagem mensagem, Cliente cliente);
	void apagarMensagem(Mensagem mensagem, Cliente cliente);
	void criptografar(Mensagem mensagem, Cliente cliente);
}

public class Cliente implements ServicosMensagem {
	@Override
	public void enviarMensagem(Mensagem mensagem, Cliente cliente) {
		// Implementação do envio
	}
	@Override
	public void apagarMensagem(Mensagem mensagem, Cliente cliente) {
		// Implementação de exclusão
	}
	@Override
	public void criptografar(Mensagem mensagem, Cliente cliente) {
		// Implementação de criptografia
	}
}

public class GrupoClientes implements ServicosMensagem {
	@Override
	public void enviarMensagem(Mensagem mensagem, Cliente cliente) {
		// Implementação para todos os clientes
	}
	@Override
	public void apagarMensagem(Mensagem mensagem, Cliente cliente) {
		// Implementação para todos os clientes
	}
	@Override
	public void criptografar(Mensagem mensagem, Cliente cliente) {
		// Implementação para todos os clientes
	}
}
```

Esse código apresenta problemas, pois, além das operações com os diferentes elementos estarem com implementações distintas, o **GrupoClientes** precisa ter mais operações, como adicionar(mensagem ou posts), que identifica a operação a ser executada (enviarmensagem ou apagar).

A adição de novas operações e novos tipos de objeto, certamente, aumentarão a complexidade dessa implementação.

-----

## 1\. Padrões de projeto Composite e Facade

### Solução, consequências e padrões relacionados ao padrão Composite

A solução proposta para o problema resolvido pelo padrão **Composite** está baseada na ideia de usar uma única interface que servirá tanto para elementos simples e compostos. Essa interface é chamada de **Componente**.

O padrão **Composite** propõe a criação de uma estrutura de objetos de forma hierárquica, através de três elementos-chave: **Componente**, **Folha** e **Composite**.

  * O **Componente** (ou Component, no diagrama UML) é um módulo (interface) que define as operações que serão implementadas nos elementos da hierarquia. Ele pode definir uma implementação padrão para essas operações, que pode ser reescrita pelos elementos (Folhas ou outros agregados).
  * A **Folha** (ou Leaf) representa os elementos que não contêm outros objetos. Eles são os objetos mais simples. Eles representam os elementos primitivos do padrão.
  * O **Participante Cliente** é um módulo (cliente) qualquer que utiliza os elementos da hierarquia.
  * O **Composite** (ou Composto) representa os elementos que podem ter outros Componentes (Folhas ou outros Composites).
  * Além disso, o **Composite** define a implementação para operações de gerenciamento de objetos agregados, permitindo adicionar e remover elementos filhos, além de ter as operações definidas no **Componente**.

A estrutura de solução proposta pelo padrão **Composite** é representada no diagrama a seguir:

$$\text{Diagrama UML de Solução do padrão Composite}$$

$$
\begin{array}{c}
\text{Cliente} \\
\uparrow \\
\begin{array}{|c|} \hline \text{Componente} \\ \hline \text{Operação()} \\ \text{Adicionar(Componente)} \\ \text{Remover(Componente)} \\ \hline \end{array} \\
\swarrow \quad \downarrow \quad \searrow \\
\begin{array}{|c|} \hline \text{Folha} \\ \hline \text{Operação()} \\ \hline \end{array} \quad
\begin{array}{|c|} \hline \text{Composto} \\ \hline \text{Componentes filhos} \\ \hline \text{Operação()} \\ \text{Adicionar(Componente)} \\ \text{Remover(Componente)} \\ \hline \end{array}
\end{array}
$$A ideia central do padrão é representar a hierarquia de modo que todos os elementos tenham uma interface superclass genérica (a interface **Componente**), que permite que o código do cliente manipule de forma uniforme a **Folha** (objetos simples) e o **Composite** (agregados).

A classe **Componente** define as operações que devem ser implementadas por todos os elementos da hierarquia. A interface define métodos para acessar e manipular elementos descendentes, enquanto o participante **Componente** representa os agregadores de elementos (Folhas ou outros agregados).

Os agregados são representados pelo participante **Composite**, que contém e manipula a lista de elementos filhos.

O participante **Cliente** representa um módulo (cliente) qualquer que utiliza os elementos da hierarquia.

A classe **Folha** (ou Leaf) é a classe mais simples, que representa os elementos sem agregados.

A estrutura de código a seguir ilustra a aplicação do padrão no exemplo do programa de mensagens:

```java
public interface ServicosMensagem {
void enviarMensagem(Mensagem mensagem, Cliente cliente);
void apagarMensagem(Mensagem mensagem, Cliente cliente);
void criptografar(Mensagem mensagem, Cliente cliente);
void adicionar(ServicosMensagem servico);
void remover(ServicosMensagem servico);
}

public class Cliente implements ServicosMensagem {
// Implementações das operações...
// ...

@Override
public void adicionar(ServicosMensagem servico) {
// Não faz nada, Cliente é um elemento Folha.
}

@Override
public void remover(ServicosMensagem servico) {
// Não faz nada, Cliente é um elemento Folha.
}
}

public class GrupoClientes implements ServicosMensagem {
private List<ServicosMensagem> lista = new ArrayList<>();

// Implementações das operações (delegando para os elementos da lista)...
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

Veja que, a seguir, são compreendidos alguns elementos-chave do padrão:

* A interface **ServicosMensagem** é o **Componente**.
* O participante **GrupoClientes** é o **Composite** (ou Composto), que armazena a lista de elementos (objetos) de agregação, além das operações adicionar e remover. Ele aplica/executa uma mensagem a vários elementos como se fossem apenas um.
* O participante **Cliente** é a **Folha** (ou Leaf), que não possui a capacidade de agregar outros objetos, ele apenas implementa as operações para manipular uma mensagem.
* O código, agora, está mais robusto e conciso, pois a interface **ServicosMensagem** obriga a implementação das operações por todos os elementos da hierarquia. Além do comportamento específico para as operações **apagar** e **criptografar**.

**Consequências e padrões relacionados ao padrão Composite**

O padrão **Composite** possui um conjunto de consequências, tanto positivas quanto negativas.

| Vantagem | Desvantagem |
|---|---|
| O código do cliente torna-se mais simples, pois não precisa saber se está manipulando um objeto simples (Folha) ou uma composição (Composite), simplificando a programação e evitando a repetição de código. | A interface comum para todos os elementos pode se tornar muito genérica, com a inclusão de métodos de gerenciamento de filhos (como `adicionar` e `remover`) que não se aplicam às **Folhas**. |
| Simplifica a adição de novos tipos de componentes (Folhas ou Composites), pois eles só precisam implementar a interface **Componente**. | |

O padrão **Factory** é, muitas vezes, utilizado para fazer a parametrização dos elementos de uma hierarquia implementada com o padrão **Composite**.

O padrão **Decorator** também é empregado com frequência junto com o padrão **Composite**. Nesse caso, o **Decorator** adiciona novas responsabilidades aos elementos, e o **Composite** organiza a hierarquia em uma estrutura de árvore, permitindo que as novas responsabilidades sejam aplicadas a todos os participantes (**Componentes**).

-----

## 1\. Padrões de projeto Composite e Facade

### Solução, consequências e padrões relacionados ao padrão Facade

O padrão **Facade** fornece uma solução ao criar uma **interface simplificada e unificada** para um conjunto complexo de interfaces em um subsistema. A ideia central é definir uma **interface de alto nível** que simplifica o uso do subsistema para o cliente. No entanto, pode-se escolher uma interface mais específica que ainda pode ser necessária.

O padrão **Facade** geralmente é utilizado para trabalhar em estruturas já existentes. No entanto, ele pode ser usado em estruturas mais complexas. Ele resolve o problema de tratar objetos individuais e composições de objetos de maneira uniforme, o que significa que o código do cliente pode usar a mesma interface para manipular tanto objetos simples quanto coleções de objetos.

O padrão **Facade** visa oferecer a solução para o problema resolvido pelo padrão **Facade**, que permite fornecer uma interface simplificada para um subsistema complexo, fornecendo uma interface mais leve e simples, permitindo a comunicação entre objetos, evitando a criação de acoplamento excessivo entre o cliente e o subsistema.

### Intenção do padrão Facade

O padrão **Facade** tem o propósito de **oferecer uma interface de alto nível para um componente ou subsistema**, o que torna o subsistema mais fácil de usar.

### Problema resolvido pelo padrão Facade

Imagine que você esteja implementando um **sistema de compras de uma página de e-commerce**, com diversas funcionalidades complexas, como controle de estoque, validação de carrinho, cálculo de frete, regras de negócio e persistência de dados. A ideia é criar um ponto comum para essas funcionalidades e simplificar os componentes em camadas.

Nessa separação, em uma camada ficariam os elementos que compõem o sistema, como o **Estoque** (responsável pelo controle de estoque), o **LogicaNegocio** (responsável pelas regras de negócio), a **Persistencia** (responsável pela recuperação de dados), **Integracao** (com sistemas e dispositivos externos), entre outros.

Suponha que para implementar uma parte da página de compras, o código precise manipular:

* um objeto **CarrinhoRepository** para recuperar o carrinho do cliente;
* um objeto **Carrinho** para adicionar produtos ao carrinho do cliente;
* um objeto **ProdutoRepository** para recuperar as informações e movimentações do cliente;
* um objeto **ValidacaoCarrinho** para verificar se o carrinho atende às regras de negócio;
* um objeto **Frete** para calcular o valor do frete e os devidos prazos de entrega;
* um objeto **LogicaNegocio** para regras de negócio.

A estrutura de dependências desses subsistemas é apresentada nos diagramas de classes a seguir:

#### Diagrama UML: Facade (visão de alto nível)

Este diagrama mostra o relacionamento simplificado entre o Cliente, o Facade e os elementos do subsistema:

$$\\begin{array}{c}
\\text{Cliente} \\
\\downarrow \\
\\begin{array}{|c|} \\hline \\text{Facade} \\ \\hline \\end{array} \\
\\downarrow \\
\\text{Elemento\_1} \\quad \\text{Elemento\_2} \\quad \\text{Elemento\_n}
\\end{array}
$$\#\#\#\# Diagrama UML: Facade com subsistemas e classes dependentes (Estrutura de dependências)

A estrutura de dependências para implementação de novos subsistemas:

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
$$O padrão **Facade** resolve o problema, pois ele **simplifica a comunicação** e os requisitos de acoplamento do cliente com um subsistema. Com o **Facade**, o cliente precisa interagir apenas com o **Facade**, que delega a chamada para os subsistemas e classes que realmente realizam a operação.

Dessa forma, o problema resolvido pelo padrão **Facade** é que um módulo utiliza um subsistema ou uma camada complexa de um sistema sem precisar conhecer ou manipular diversos elementos.

**Consequências e padrões relacionados ao padrão Facade**

O padrão **Facade** possui um conjunto de consequências, tanto positivas quanto negativas.

* **Vantagem:** O cliente se acopla a uma interface simplificada e unificada, reduzindo a complexidade de uso do subsistema. Permite a organização das camadas do subsistema. O subsistema pode ser modificado sem afetar os clientes (módulos).
* **Desvantagem:** O padrão **Facade** pode se tornar um **"Deus Objeto"** se for implementado de forma incorreta, concentrando muitas responsabilidades em uma única classe.

Alguns padrões de projeto são aplicados em conjunto com o padrão **Facade**:

* **Padrão Abstract Factory:** O padrão **Abstract Factory** (ou **Kit de Ferramentas**) de criação de dependências podem ser utilizadas pelo **Facade** para fazer o **encapsulamento** da criação dos elementos que o **Facade** utiliza.
* **Padrão Mediator:** O padrão **Mediator** possui uma estrutura similar ao **Facade**, contudo, tem o propósito de **eliminar a comunicação** entre objetos de mesma camada, **automatizando** funcionalidades que não sejam do tipo **comando**.
* **Padrão Adapter:** O padrão **Facade** tem o propósito de **fornecer uma interface de alto nível** para clientes de baixa acoplamento, e o **Adapter** tem o propósito de **converter uma interface para outra** que o cliente espera.
$$