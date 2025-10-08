
-----

# Padrões GoF Estruturais

Os padrões estruturais GoF (Gang of Four) lidam com a **composição de classes e objetos** em estruturas maiores, mais flexíveis e eficientes, mantendo as estruturas flexíveis e eficientes. Os padrões estruturais apresentados neste conteúdo são: **Adapter**, **Bridge**, **Composite**, **Decorator**, **Facade**, **Flyweight** e **Proxy**.

Alexandre Luis Correa e Prof. Marcelo Pavan Antoni

## Itens Iniciais

**Preparação**

  * **Conceito de Padrões de Projeto**: É recomendado iniciar ou rever este conteúdo em um programa que permita ao leitor montar seus slides e a forma de diagramas UML 2, Engajamento (estrutura da Mensagem). Nessa seção você deve concentrar a atenção nos padrões que serão discutidos no capítulo 5 do livro **Use a Cabeça\! Padrões de Projeto**.
  * **Pesquisa**: Busque as formas de formular os três conjuntos de padrões (criação, estruturais e de interação) e a ligação em seu uso e também a estrutura de classes, com os métodos de criação e as suas características de implementação. Sugerimos que seja feito um estudo, representação para os padrões **Adapter, Bridge, Decorator, Composite, Flyweight** e **Proxy**.
  * **Desenvolvimento**: Sugerimos a utilização de ferramentas livres para modelagem de sistemas em UML (UML\_SAK, Trac), no qual o foco deve ser a aplicação dos padrões.
  * **Leitura**: Assim, recomendamos a realização de um ambiente de programação em Java. O ambiente de programação deve ter a versão mais recente do Eclipse e também do JRE (Java Runtime Environment) e do JDK (Java Development Kit) referentes à edição Java SE (Standard Edition), que pode ser encontrado gratuitamente no site da tecnologia (Oracle, para o Java).

**Objetivos**

  * Reconstituir o propósito, a estrutura e as situações de aplicação dos padrões de projeto **Adapter**, **Bridge** e **Decorator**.
  * Reconstituir o propósito, a estrutura e as situações de aplicação dos padrões de projeto **Bridge** e **Decorator**.
  * Reconstituir o propósito, a estrutura e as situações de aplicação dos padrões de projeto **Composite** e **Flyweight**.
  * Reconstituir o propósito, a estrutura e as situações de aplicação dos padrões de projeto **Flyweight** e **Proxy**.

**Introdução**
Os **Padrões GoF**, do inglês Gang of Four, são padrões de projeto orientados a objetos divididos em três grupos (Criação, Estruturais e Comportamentais), desenvolvidos por Erich Gamma, Richard Helm, Ralph Johnson e John Vlissides no livro **Design Patterns: Elements of Reusable Object-Oriented Software** em 1994. Esses padrões **estruturais** são aqueles que permitem modularizar a solução de um problema por meio de composição de objetos em que não se utiliza a herança e sim a composição de objetos. Nesta unidade, você aprenderá um dos sete padrões desta categoria, que são: **Adapter, Bridge, Composite, Decorator, Facade, Flyweight e Proxy**.

O padrão **Adapter** permite que módulos de um sistema possam interagir com diferentes implementações de bibliotecas externas (com interfaces incompatíveis), ou adaptar uma interface a outra, permitindo a reutilização de um objeto de sua implementação. De modo que ambos possam variar de forma independente. O padrão **Bridge** separa as responsabilidades entre classes ou módulos a fim de ter hierarquias independentes de abstrações e implementações. O padrão **Composite** mostra como podemos estruturar uma hierarquia de objetos construída a partir de dois tipos de elementos: primitivos e compostos, de forma que os clientes possam tratar os objetos de forma uniforme. O padrão **Decorator** possibilita estender o comportamento de um objeto de forma dinâmica em tempo de execução. O padrão **Flyweight** apresenta uma solução para o problema de compartilhamento de um número elevado de objetos, com o objetivo de economizar recursos de memória. O padrão **Facade** por sua vez, simplifica a interface complexa de um objeto para os clientes. Por último, o padrão **Proxy** oferece um substituto para controlar o acesso a um objeto real.

-----

# Padrão de projeto Adapter

## Intenção e problema resolvido pelo padrão Adapter

O padrão de projeto Adapter tem como intenção permitir que duas classes com interfaces incompatíveis possam colaborar entre si. Ele é um dos padrões estruturais do GoF (Gang of Four), que lida com a composição de classes e objetos em estruturas maiores, mantendo-as flexíveis e eficientes. Os padrões estruturais explicam como montar objetos e classes em estruturas maiores, mantendo essas estruturas flexíveis e eficientes.

**Intenção do padrão Adapter**
O Adapter é um padrão que permite adaptar responsabilidades ou a interface de uma classe existente a uma nova interface requerida pelo cliente, em tempo de execução.

**Problema resolvido pelo padrão Adapter**
**Classes e objetos** que possuem interfaces incompatíveis ou não podem se comunicar de forma direta em razão da diferença de interface. O Adapter atua como um tradutor, transformando a interface de uma classe (chamada de *Adaptee*) para a interface que o cliente (chamado de *Client*) espera, permitindo que elas trabalhem juntas.

O que acontece na prática, é que o código que se espera usar está implementado em uma interface que não pode ser usada pelo código-cliente.

**O que se deve fazer para adaptar o código de uma forma que o cliente possa usar?**

Devemos adaptar o código existente a uma interface que o cliente espera para usar o código sem alterá-lo.

O padrão **Adapter** é útil em projetos onde o sistema está em evolução ou precisa se integrar com um sistema externo que tem uma interface incompatível.

*Imagine que você está desenvolvendo um sistema para envio de dois tipos diferentes de cartões, sendo que cada tipo de cartão utiliza um sistema de pagamento específico, com APIs diferentes. Como fazer para que o seu código possa ser usado para todas as formas de pagamento sem alterar o código do cliente?*

Neste exemplo, você tem as interfaces `APIPagamentos` e `APIPagamentos2` e uma interface do seu programa, chamada `CartaoAdapter`.

-----

## Solução, consequências e padrões relacionados ao padrão Adapter

O padrão Adapter permite que classes com interfaces incompatíveis trabalhem juntas. É um padrão estrutural de objeto que utiliza a **composição de objetos** para permitir que uma classe (chamada de *Adaptee* ou Adaptação) funcione com uma outra classe (chamada de *Target* ou Alvo) que espera uma interface diferente.

**Estrutura do padrão Adapter**
O diagrama a seguir representa a estrutura do padrão Adapter:

*Diagrama de Classes: Interfaces `Target` e `Adaptee`. Classe `Client` se relaciona com `Target`. Classe `Adaptee` tem um método `SpecificRequest()`. Classe `Adapter` implementa `Target` e se relaciona com `Adaptee`. O método `Request()` de `Adapter` chama `SpecificRequest()` de `Adaptee`.*

No diagrama, a interface **Target** (Alvo) representa a interface que o cliente espera. A interface **Adaptee** (Adaptação) representa a interface incompatível existente. A classe **Adapter** (Adaptador) implementa a interface **Target** e envolve a classe **Adaptee**, que atua como uma ponte para compatibilizar as interfaces.

### Adaptação de Interfaces

Em alguns cenários, a adaptação é crucial, seja pela necessidade de integração com sistemas legados ou pela busca por flexibilidade no design.

**A interface do Adaptee (adaptação) representa a generalização dos serviços alvos de forma inadequada.**

A solução consiste em redefinir a interface existente através de uma classe adaptadora, que implementa a interface requerida pelo cliente, e envolve o objeto que precisa ser adaptado (Adaptee).

A solução de Adapter se encaixa perfeitamente no cenário de necessidade de integração de sistemas legados, onde há um conjunto de dados ou um código legado que o sistema cliente não pode usar.

*Imagine um cenário onde um cliente precisa de um serviço específico, mas o serviço disponível possui uma interface diferente daquela que o cliente espera.*

**Solução de pagamento 1**

Um cartão precisa realizar duas operações: `Pagamento` e `Estorno`. O sistema de pagamento (Adaptee) oferece `Debit` e `Credit`.

**Solução de pagamento 2**

Um cartão precisa realizar duas operações: `Pagamento` e `Estorno`. O sistema de pagamento (Adaptee) oferece `FazerPagamento` e `FazerEstorno`.

Percebe que, além de nomes diferentes, os padrões de operação também são diferentes.

**Código (exemplo em Java)**

```java
// Interface do cliente que o código espera
public interface CartaoAdapter {
    void processarPagamento(String numeroCartao, double valor);
    void processarEstorno(String numeroCartao, double valor);
}

// Classe adaptada 1 (Adaptee)
public class AdaptadorPagamento implements CartaoAdapter {
    private final APIPagamentos apiPagamentos;

    public AdaptadorPagamento(APIPagamentos apiPagamentos) {
        this.apiPagamentos = apiPagamentos;
    }

    @Override
    public void processarPagamento(String numeroCartao, double valor) {
        apiPagamentos.debitar(numeroCartao, valor);
    }

    @Override
    public void processarEstorno(String numeroCartao, double valor) {
        apiPagamentos.creditar(numeroCartao, valor);
    }
}
//... (código da APIPagamentos e outros)
```

**Código (exemplo em Java com outra API)**

```java
// Classe adaptada 2 (Outro Adaptee)
public class AdaptadorPagamento2 implements CartaoAdapter {
    private final APIPagamentos2 apiPagamentos2;

    public AdaptadorPagamento2(APIPagamentos2 apiPagamentos2) {
        this.apiPagamentos2 = apiPagamentos2;
    }

    @Override
    public void processarPagamento(String numeroCartao, double valor) {
        apiPagamentos2.fazerPagamento(numeroCartao, valor);
    }

    @Override
    public void processarEstorno(String numeroCartao, double valor) {
        apiPagamentos2.fazerEstorno(numeroCartao, valor);
    }
}
//... (código da APIPagamentos2 e outros)
```

Dessa forma, como cada API possui uma interface específica e métodos para a realização do pagamento do cartão de crédito, uma solução é criar uma classe `Adapter` para cada API, permitindo que a aplicação use o mesmo código para processar cada forma de pagamento.

**Consequências e padrões relacionados ao padrão Adapter**

### Consequências

O padrão **Adapter** traz uma série de benefícios, sendo o mais relevante a possibilidade de que as classes com interfaces incompatíveis colaborem entre si. Isso é importante em projetos de grande porte que precisam se integrar com sistemas legados ou de terceiros, onde a interface não pode ser modificada e o cliente não pode alterá-la para usar o código existente.
A desvantagem é que a cada nova implementação incompatível, uma nova classe **Adapter** deve ser criada.

### Padrões Relacionados

| Bridge | Adapter |
| :--- | :--- |
| Aplica-se quando desejamos desacoplar a abstração de sua implementação, permitindo que ambas possam variar de forma independente. | Permite que uma classe com interface incompatível trabalham juntas, encapsulando um objeto e fornecendo a ele uma nova interface. |
| Geralmente utilizado em tempo de projeto para permitir a evolução de duas hierarquias de classes de forma independente. | Geralmente utilizado para fazer com que uma classe existente trabalhe com uma classe cliente que espera uma interface diferente. |

-----

# Padrões de projeto Bridge e Decorator

## Intenção e problema resolvido pelo padrão Bridge

O padrão de projeto **Bridge** tem como intenção desacoplar a abstração de sua implementação, de modo que as duas possam variar independentemente. Ele é um dos padrões estruturais do GoF (Gang of Four), que lida com a composição de classes e objetos em estruturas maiores, mantendo-as flexíveis e eficientes. Os padrões estruturais explicam como montar objetos e classes em estruturas maiores, mantindo essas estruturas flexíveis e eficientes, permitindo que objetos possam ser construídos por uma ou mais camadas de *decorators*, facilitando a reutilização de código e a manutenção do sistema.

**Intenção do padrão Bridge**
O padrão Bridge permite desacoplar uma **abstração** de sua **implementação**, permitindo que ambas possam variar independentemente.

**Problema resolvido pelo padrão Bridge**
Em um projeto de grande porte, quando as classes possuem diferentes implementações em sistemas ou plataformas, com o uso da herança, o código pode se tornar inflexível, com classes em excesso, tornando o código difícil de ser mantido e alterado.
O padrão **Bridge** tenta resolver este problema trocando a herança por composição do objeto.

**O que acontece quando se trabalha com diferentes implementações em um código em tempo de execução, com o uso da herança?**
A hierarquia de classes irá crescer exponencialmente porque adicionar um novo componente ou suportar uma nova plataforma irá requerer a criação de novas classes.

Esses problemas comuns ocorrem quando combinamos diferentes perspectivas de classificação em uma abstração.

**Qual é a solução para o problema de crescimento exponencial de classes devido à variação de abstrações e implementações?**

A solução consiste em separar as hierarquias de abstrações e de implementações, permitindo que elas variem de forma independente por meio de uma ponte (*bridge*). A ponte é realizada por meio de uma instância da implementação dentro da abstração.

**Estrutura do padrão Bridge**
A solução de projeto do padrão Bridge está representada no diagrama de classes a seguir:

*Diagrama de Classes: Interfaces `Implementador` e `Abstracao`. Classes `ImplementacaoA` e `ImplementacaoB` implementam `Implementador`. Classe `RefinedAbstracao` herda de `Abstracao`. Classe `Abstracao` contém uma referência para `Implementador` e tem um método `operacao()`, que chama o método `operacaoImplementada()` do `Implementador`.*

A ideia central é separar a hierarquia de abstrações da hierarquia de implementações, de forma que o código-cliente interaja apenas com a hierarquia de abstrações. A abstração, por sua vez, deve delegar a execução para a implementação específica que estiver conectada à abstração.

**Exemplo Prático (em Java)**
O padrão Bridge é aplicado em dois cenários:

1.  Em projetos onde é necessário desenvolver implementações para diferentes plataformas (Windows, Linux, macOS)
2.  Em projetos que possuem diversas abstrações e implementações (Controle Remoto e Dispositivo).

**Código (exemplo em Java)**

```java
public interface Component {
    void draw();
}

public class TextField implements Component {
    private String text;
    private int width;
    private int height;

    public TextField(String text, int width, int height) {
        this.text = text;
        this.width = width;
        this.height = height;
    }

    @Override
    public void draw() {
        System.out.println("Text: " + text + ", Size: " + width + "x" + height);
    }
}
//... (outras classes e código)
```

Note que o código da classe `TextField` é específico para a plataforma. Para ter diferentes implementações, a solução com Bridge é criar uma hierarquia de abstrações (`Component` e suas concretizações) e uma hierarquia de implementações (`PlatformImpl` e suas concretizações).

**Consequências e padrões relacionados ao padrão Bridge**

### Consequências

O padrão **Bridge** permite o **desacoplamento** entre a abstração e a implementação, o que é útil em projetos de grande porte, em que se tem diversas abstrações e implementações. Por ser um padrão que utiliza a composição de objetos em vez da herança, ele fornece flexibilidade para criar e configurar objetos em tempo de execução. Isso é fundamental quando a abstração e a implementação precisam variar de forma independente.

### Padrões Relacionados

| Adapter | Bridge |
| :--- | :--- |
| Aplica-se quando desejamos que uma classe com interface incompatível trabalhe com uma classe cliente que espera uma interface diferente. | Permite criar uma ponte para variar não apenas a sua implementação, como também as suas abstrações. |
| Geralmente utilizado para fazer com que uma classe existente trabalhe com uma classe cliente que espera uma interface diferente, após a aplicação estar em desenvolvimento. | Geralmente utilizado em tempo de projeto para permitir a evolução de duas hierarquias de classes de forma independente. |

-----

# Padrões de projeto Bridge e Decorator

## Intenção e problema resolvido pelo padrão Decorator

O padrão de projeto **Decorator** tem como intenção adicionar responsabilidades a objetos de modo dinâmico e transparente, sem alterar a sua estrutura interna. Ele é um dos padrões estruturais do GoF (Gang of Four), que lida com a composição de classes e objetos em estruturas maiores, mantendo-as flexíveis e eficientes. Os padrões estruturais explicam como montar objetos e classes em estruturas maiores, permitindo que objetos possam ser construídos por uma ou mais camadas de *decorators*, facilitando a reutilização de código e a manutenção do sistema.

**Intenção do padrão Decorator**
O Decorator é um padrão que permite adicionar responsabilidades a um objeto de forma **dinâmica** e **transparente**, sem alterar a sua estrutura interna.

**Problema resolvido pelo padrão Decorator**
Uma forma comum de estender a funcionalidade de classes é por meio da herança de subclasses.
O problema com a herança é que a funcionalidade é adicionada em tempo de compilação, tornando o código inflexível e requer a escrita de novo código.

**Qual é a solução para o problema de estender a funcionalidade de uma classe em tempo de execução sem alterar o código?**

Devemos, então, criar um **envolvedor** (*wrapper*) para uma instância de objeto. O *wrapper* implementa a mesma interface do objeto envolvido, adicionando responsabilidades em tempo de execução.

**Código (exemplo em Java)**

```java
public interface Component {
    void doOperation();
}

public class ConcreteComponent implements Component {
    @Override
    public void doOperation() {
        System.out.println("Executando a operação principal.");
    }
}

public abstract class Decorator implements Component {
    protected Component component;

    public Decorator(Component component) {
        this.component = component;
    }

    @Override
    public void doOperation() {
        component.doOperation(); // Delega a chamada para o objeto envolvido
    }
}
//... (outras classes e código)
```

Para adicionar a capacidade de carregar texto compactado, podemos usar uma solução baseada em **herança**, mas se houver a necessidade de incluir novos comportamentos, como criptografia, o código ficará complexo.

Nessa solução, sugerimos que a compactação ocorra somente no momento de salvamento e carregamento.

**Código (exemplo em Java com Decorators)**

```java
// Decorator concreto para compressão
public class CompressionDecorator extends Decorator {
    public CompressionDecorator(Component component) {
        super(component);
    }

    @Override
    public void doOperation() {
        System.out.println("Compactando o conteúdo.");
        super.doOperation();
        System.out.println("Conteúdo compactado.");
    }
}

// Decorator concreto para criptografia
public class EncryptionDecorator extends Decorator {
    public EncryptionDecorator(Component component) {
        super(component);
    }

    @Override
    public void doOperation() {
        System.out.println("Criptografando o conteúdo.");
        super.doOperation();
        System.out.println("Conteúdo criptografado.");
    }
}

// Uso dos Decorators
public class Client {
    public static void main(String[] args) {
        Component component = new ConcreteComponent();
        Component decoratedComponent = new CompressionDecorator(new EncryptionDecorator(component));
        decoratedComponent.doOperation();
    }
}
```

Soluções baseadas em herança para problemas dessa natureza são pouco flexíveis. Você consegue imaginar o que acontece se tivermos várias opções de funcionalidade (criptografia, compactação, autenticação, *logging*, etc.)?

A herança pode gerar um código pouco flexível e complexo. Veja a seguir o que poderia ser feito e a situação que seria criada:

| Compressão/Inverter | Situação problemática |
| :--- | :--- |
| Poderíamos definir a classe `Compressao` que herda de `Inverter`. E o mesmo com a classe `Inverter` que herda de `Compressao`. E se tivéssemos outras opções como: `Criptografia`, `Autenticacao`, `Logging`, etc. | O número de classes iria explodir (ex: `CompressionEncryption`, `CompressionInversion`, `EncryptionInversion`, `EncryptionCompression`, `AuthEncryption`, `AuthLogging`, por exemplo). |

Portanto, o problema resolvido pelo padrão **Decorator** consiste em adicionar novos comportamentos de forma dinâmica e transparente a um objeto, sem alterar sua estrutura interna. O objeto é envolvido por *decorators* que implementam a mesma interface que ele. Isso permite que esses elementos possam ser combinados de diversas maneiras, oferecendo **flexibilidade** e **extensão**.

-----

## Solução, consequências e padrões relacionados ao padrão Decorator

O padrão **Decorator** permite adicionar responsabilidades a objetos de modo dinâmico e transparente, sem alterar a sua estrutura interna. É um padrão estrutural de objeto que utiliza a **composição de objetos** para estender ou alterar a funcionalidade de uma classe.

**Estrutura do padrão Decorator**
A solução de projeto do padrão Decorator está representada no diagrama de classes a seguir:

*Diagrama de Classes: Interfaces `Component` e `Decorator`. Classe `ConcreteComponent` implementa `Component`. Classe `Decorator` implementa `Component` e contém uma referência para `Component`. Classes `ConcreteDecoratorA` e `ConcreteDecoratorB` herdam de `Decorator`.*

Agora, vamos entender o papel dos participantes da estrutura apresentada a seguir:

1.  **Component**: Define a interface para objetos que podem ter responsabilidades adicionadas dinamicamente.
2.  **ConcreteComponent**: Define o objeto que se deseja adicionar responsabilidades.
3.  **Decorator**: Mantém uma referência para o objeto **Component** e está em conformidade com a interface **Component**.
4.  **ConcreteDecorator**: Adiciona responsabilidades ao **Component**.

Uma característica importante do padrão **Decorator** é que a composição é **recursiva**, ou seja, um **Decorator** pode envolver outro **Decorator** (que por sua vez envolve outro **Decorator**, e assim por diante), o que não é possível com o padrão **Adapter**.

*Diagrama de Classes: Instâncias de `ConcreteComponent`, `ConcreteDecoratorA` e `ConcreteDecoratorB`. `ConcreteDecoratorB` aponta para `ConcreteDecoratorA`, que aponta para `ConcreteComponent`.*

**Agora, vamos entender o papel das classes apresentadas a seguir:**

1.  **Componente Concreto**: Classe original (sem modificações).
2.  **Decorator Concreto A**: Adiciona uma funcionalidade.
3.  **Decorator Concreto B**: Adiciona outra funcionalidade, diferente de A.

O que acontece é que ao invocar o método `doOperation()` no **Decorator Concreto B**, ele delega a chamada para o **Decorator Concreto A**, que por sua vez delega para o **Componente Concreto**, adicionando as funcionalidades de forma hierárquica (B, A e Componente).

O código apresentado utiliza a capacidade de criar objetos que implementam a interface `Component`, por meio de *decorators* que aprimoram a funcionalidade da classe principal de forma dinâmica em tempo de execução.

**Código (exemplo em Java)**

```java
public class ConcreteComponent implements Component {
    @Override
    public void doOperation() {
        System.out.println("Executando a operação principal.");
    }
}
//... (outras classes e código dos Decorators)
```

A **classe `ConcreteComponent`** é a classe que contém a **funcionalidade principal**, sem modificações.

**Código (exemplo de uso em Java)**

```java
// Cria o objeto base
Component component = new ConcreteComponent();

// Decora com Criptografia e depois com Compressão
Component decoratedComponent = new CompressionDecorator(new EncryptionDecorator(component));

System.out.println("Iniciando a operação decorada...");
decoratedComponent.doOperation();
System.out.println("Operação decorada finalizada.");

// Cria outro objeto decorado
Component anotherDecorated = new EncryptionDecorator(component);
System.out.println("\nIniciando outra operação decorada...");
anotherDecorated.doOperation();
System.out.println("Outra operação decorada finalizada.");
```

### Consequências

O padrão **Decorator** é usado para adicionar ou estender o comportamento de um objeto de forma dinâmica. É muito flexível, pois a combinação de *decorators* permite adicionar diferentes funcionalidades em tempo de execução sem alterar a estrutura interna das classes.
A desvantagem é que um objeto envolvido por muitos *decorators* se torna difícil de depurar e rastrear a execução do código.

### Padrões Relacionados

| Adapter | Decorator |
| :--- | :--- |
| O Adapter é usado para fazer com que uma interface existente trabalhe com uma classe cliente que espera uma interface diferente, alterando a interface. | O Decorator é usado para estender o comportamento de um objeto de forma dinâmica, mantendo a mesma interface. |
| Não suporta composição recursiva. | Suporta composição recursiva, permitindo que um *decorator* envolva outro *decorator*. |