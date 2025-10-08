

## 1\. Padrão de projeto Abstract Factory

### Consequências e padrões relacionados ao Abstract Factory

O Padrão de projeto **Abstract Factory** é um padrão de criação que oferece uma interface para criar famílias de objetos relacionados ou dependentes, sem especificar suas classes concretas. Ao fazer isso, ele promove o **baixo acoplamento** e o **encapsulamento** ao centralizar a criação e o uso dos objetos abstratos, enquanto as instâncias concretas são resolvidas em tempo de execução.

Esse padrão permite uma **configuração centralizada** e a **extensibilidade do sistema**, pois a adição de novos produtos ou famílias não requer modificação no código cliente.

Existem alguns padrões de criação que se assemelham ao Abstract Factory ou que são comumente utilizados em conjunto:

  * **Factory Method:** Este é um padrão de criação que tem como objetivo definir uma interface para criar um objeto, mas deixa as subclasses decidirem qual classe instanciar. É um método que é implementado nas subclasses.
  * **Singleton:** Este padrão garante que uma classe tenha apenas uma instância e fornece um ponto de acesso global a ela.

Explore as notas a seguir sobre as consequências e padrões relacionados ao Abstract Factory.

O padrão de Abstract Factory promove o **encapsulamento** do processo de criação de objetos, isolando os clientes dos detalhes específicos de como os objetos são criados. Isso simplifica a manutenção e a modificação nas implementações. Além disso, esse padrão promove a **coerência** entre produtos relacionados, isto é, garante que todos os objetos criados por uma fábrica concreta sejam da mesma família, evitando que objetos incompatíveis sejam misturados. Ele permite que o sistema seja **configurável** em tempo de execução ao utilizar uma nova nomeação de criação em cada fábrica de estrutura.

Ele é importante na **configuração centralizada**, pois as aplicações que utilizam a Abstract Factory podem ter o acoplamento implementado utilizando o padrão Factory Method. É possível configurar diversas regras, utilizando o arquivo de configuração, permitindo a substituição de classes concretas da estrutura. Por exemplo, uma classe pode receber um objeto Factory para instanciar outros objetos.

No exemplo apresentado anteriormente, poderíamos ainda adicionar a aplicação de código similar para implementação, a qual permite a substituição de objetos de maneira simples. Por exemplo, podemos ter um padrão genérico **Decorator** (um modo de encapsulamento específico) e transformarmos esses operacionais em objetos genéricos para que os clientes não precisem saber as particularidades dos objetos encapsulados.

$${\text{Exemplo de Decoração}}$$

Estude as notas a seguir sobre a estrutura do exemplo, aplicando os critérios de Decoração.

Para melhorar a aplicação inicial, o primeiro passo é a definição das interfaces (**Decorator**) e os produtos concretos. Assim, as classes concretas estendem as interfaces (Decorator) e o retorno organiza o padrão Singleton.

$${\text{Java (Decodificador UBL)}}$$

```java
public class UBLDecoder implements Decoder {
    private final String origin;

    public UBLDecoder(String origin) {
        this.origin = origin;
    }

    @Override
    public String decode(String msg) {
        // Implementação do decodificador UBL
        return "Decoded by UBL: " + msg.toUpperCase() + " (origin: " + origin + ")";
    }
}
```

Como existem muitos padrões comuns, o padrão **Template Method** pode ser utilizado, definindo o núcleo do algoritmo em uma superclasse, deixando as subclasses implementarem o resto.

$${\text{Java (Decodificador Genérico)}}$$

```java
public abstract class GenericDecoder implements Decoder {
    private final String origin;

    public GenericDecoder(String origin) {
        this.origin = origin;
    }

    @Override
    public final String decode(String msg) {
        // Lógica comum de decodificação
        String processedMsg = preprocess(msg);
        String decoded = doDecode(processedMsg);
        return postprocess(decoded);
    }

    protected abstract String doDecode(String msg);

    protected String preprocess(String msg) {
        return msg.trim();
    }

    protected String postprocess(String decodedMsg) {
        return decodedMsg + " (Processed - " + this.getClass().getSimpleName() + " from " + origin + ")";
    }
}
```

Finalmente, a serviço pode ser implementada de forma simples, com estrutura similar a um Factory Method com Decoração.

$${\text{Java (Serviço de Mensagem)}}$$

```java
public class MessageService {
    private final DecoderFactory decoderFactory;

    public MessageService(DecoderFactory decoderFactory) {
        this.decoderFactory = decoderFactory;
    }

    public String processMessage(String msg, String format) {
        Decoder decoder = decoderFactory.createDecoder(format);
        return decoder.decode(msg);
    }
}
```

-----

## 1\. Padrão de projeto Abstract Factory

### Intenção e problema do padrão Abstract Factory

Segundo o padrão **Abstract Factory**, não é permitido que as implementações variem sobre o formato, é o cliente quem as cria. A intenção principal do padrão é fornecer uma interface para criar famílias de objetos relacionados ou dependentes sem especificar suas classes concretas. Ao fazer isso, ele soluciona o problema de acoplamento e dependência de implementações concretas em tempo de execução, permitindo que a aplicação seja configurada para utilizar diferentes famílias de produtos sem alterar o código cliente.

Observe no vídeo a seguir a intenção e o problema do padrão Abstract Factory.

O **Abstract Factory** é um padrão que fornece uma interface para a criação de famílias de objetos relacionados ou dependentes, sem especificar suas classes concretas, permitindo que o código cliente manipule objetos através de suas interfaces abstratas.

Por exemplo, considere um sistema de mensagens que precisa enviar dados em diferentes formatos (XML, EDI, Texto Fixo etc.) e para diferentes organizações. Considere que os clientes ou as áreas de cada organização só podem usar o formato que lhes foi atribuído. Além disso, as regras de cada organização diferem na forma como as mensagens são armazenadas (banco de dados, arquivos, JMS etc.).

Abaixo, apresentamos uma tabela de exemplo que representa a necessidade de diferentes formatos de registro e destino para cada organização.

| Organização | Formato de Registro Interno | Formato de destino externo |
| :---: | :---: | :---: |
| Organização 1 | Registro Interno | XML |
| Organização 2 | Registro Curto | EDI |
| Organização 3 | Registro Interno | Texto Fixo |
| Organização 4 | Registro Curto | XML |
| Organização 5 | Registro Curto | Campo de tamanho fixo |

Podemos definir um conjunto de classes responsáveis pela decodificação de mensagens de um formato específico em objetos independentes desse formato. Assim, o Abstract Factory pode ser estruturado desta forma:

$${\text{Estrutura do Abstract Factory para Registros}}$$
$${\text{Diagrama de Classes: Estrutura do Abstract Factory}}$$

A interface **RegistroAbstractFactory** representa o serviço genérico que cria uma mensagem de registro de cliente, respeitando as regras definidas. A estrutura do padrão define uma interface para criar objetos de registro, mas deixa a cargo das implementações concretas (subclasses) a decisão de qual objeto instanciar. Assim, o código cliente não precisa saber as implementações concretas.

As implementações desta interface abstrata para cada formato específico são:

  * XML
  * EDI
  * Texto Fixo
  * Campo de tamanho fixo

Para integrar este padrão em nosso sistema (Registro Cliente, Registro Curto), deve ser criada uma estrutura de classes que o cliente deve usar.

O diagrama de classes mostra as interfaces e as classes concretas para diferentes tipos de registro e formatos de decodificação. As classes abstratas criam as mensagens (recebidas) de diferentes organizações, e as implementações concretas criam os objetos concretos (como **XMLFormatado** ou **EDIFormatado**). O cliente interage apenas com a interface abstrata **RegistroAbstractFactory** para criar seus produtos.

$${\text{Java (Interfaces e Classes Abstratas)}}$$

```java
// Interfaces para os produtos abstratos
interface Formatado {
    String formatar(String registro);
}

// Interfaces para a fábrica abstrata
interface RegistroAbstractFactory {
    Formatado criarFormatoMensagem();
}

// Implementação concreta da fábrica (exemplo para XML)
class RegistroInternoAbstractFactory implements RegistroAbstractFactory {
    @Override
    public Formatado criarFormatoMensagem() {
        return new XMLFormatado(); // Produto concreto
    }
}
```

Este padrão está presente em todos os tipos possíveis de decodificação, convencendo toda a complexidade de interação entre o cliente e a fábrica abstrata. As fábricas concretas encapsulam os detalhes de como os objetos são criados, garantindo que as classes de objetos relacionados sejam criadas de forma coesa. Assim, cada organização terá de ser modificada, o que configura uma centralização de criação de objetos. O cliente usa um dos princípios SOLID.

As **classes concretas de fábrica** implementam a interface da fábrica abstrata. Elas são responsáveis por criar e retornar as instâncias concretas dos produtos. Por exemplo, a **RegistroInternoAbstractFactory** é a fábrica concreta que cria a instância de **XMLFormatado** para a Organização 1 e a **RegistroCurtoAbstractFactory** para a Organização 2.

A aplicação deste padrão oferece as seguintes vantagens:

  * **Isola o cliente de uma família de produtos relacionados de suas implementações específicas.**
  * **Torna o sistema configurável**, permitindo que ele trabalhe com outros formatos de decodificação sem que o código do cliente precise ser alterado.
  * **Garante a coesão** entre os produtos criados.

-----

## 1\. Padrão de projeto Abstract Factory

### Solução do padrão Abstract Factory

Segundo os exemplos apresentados, o padrão **Abstract Factory** define uma interface abstrata (RegistroAbstractFactory) para criar famílias de produtos (Formatado). As fábricas concretas (RegistroInternoAbstractFactory, RegistroCurtoAbstractFactory, etc.) implementam essa interface para produzir produtos concretos (XMLFormatado, EDIFormatado, etc.) que pertencem à mesma família. Ao fazer isso, isolamos o cliente das classes concretas e garantimos que os produtos criados por uma fábrica concreta sejam compatíveis entre si. O cliente interage apenas com as interfaces e classes abstratas, mantendo as características de **baixo acoplamento** e **alta coesão**.

Observe no vídeo a seguir a solução do padrão Abstract Factory.

A estrutura de solução proposta pelo padrão Abstract Factory está representada no diagrama de classes a seguir.

$${\text{Diagrama de Classes: Solução do Abstract Factory}}$$

No **lado cliente**, são criados os objetos pelas classes fábrica. Cada tipo de produto é definido por uma interface abstrata, por exemplo, **AbstractProductA** e **AbstractProductB**. Essas interfaces definem os métodos que todos os produtos dessa categoria devem implementar.

As classes concretas, como **ProductA1** e **ProductA2**, são implementações específicas do **AbstractProductA**. Elas fornecem a lógica real para os métodos definidos na interface. Elas representam os produtos concretos **ProductA1** e **ProductA2**.

As classes concretas de fábrica, como **ConcreteFactory1** e **ConcreteFactory2**, implementam a interface **AbstractFactory**. Elas são responsáveis por criar e retornar as instâncias concretas dos produtos. Por exemplo, a **ConcreteFactory1** cria **ProductA1** e **ProductB1**, e a **ConcreteFactory2** cria **ProductA2** e **ProductB2**.

As classes concretas de fábrica (ConcreteFactory1 e ConcreteFactory2) implementam a interface da fábrica abstrata. Elas são responsáveis por criar e retornar as instâncias concretas dos produtos. Por exemplo, a **ConcreteFactory1** é a fábrica concreta que cria a instância de **ProductA1** e **ProductB1** da família 1. De forma similar, a **ConcreteFactory2** é a fábrica concreta que cria a instância de **ProductA2** e **ProductB2** da família 2.

Observe a seguir um diagrama de classes que ilustra a integração com a aplicação da estrutura proposta pelo padrão Abstract Factory.

$${\text{Diagrama de Classes: Integração do Abstract Factory com o Serviço de Mensagem}}$$

  * **Famílias de decodificadores:**

No exemplo apresentado, as classes abstratas **AbstractDecoderFactory** e **DecoderFactory** definem uma interface abstrata para criar famílias de objetos **Decoder** (UBLDecoder, EDIDecoder), responsáveis pela decodificação de mensagens no formato **UBL** ou **EDI**.

O cliente **MessageService**, que utiliza a interface **DecoderFactory**, pode ser configurado para usar qualquer fábrica concreta, como **UBLDecoderFactory** ou **EDIDecoderFactory**, sem saber as classes concretas que estão sendo utilizadas. Isso garante que a aplicação seja configurável para diferentes formatos de decodificação sem alterar o código cliente.

Apenas as classes concretas de decodificador (UBLDecoder e EDIDecoder) implementam a interface **Decoder**. Por exemplo, **UBLDecoder** é a implementação de decodificação para o formato UBL, enquanto **EDIDecoder** é a implementação de decodificação para o formato EDI.

$${\text{Java (Interfaces e Fatorías Abstratas)}}$$

```java
// Interfaces para os produtos abstratos
interface Decoder {
    String decode(String msg);
}

// Interfaces para as fábricas abstratas
interface DecoderFactory {
    Decoder createDecoder();
}

// Implementação concreta da fábrica (exemplo para UBL)
class UBLDecoderFactory implements DecoderFactory {
    @Override
    public Decoder createDecoder() {
        return new UBLDecoder(); // Produto concreto
    }
}
```

Agora vamos utilizar as técnicas para simplificar a organização da especialização da classe **DecoderAbstractFactory** e utilizar o padrão Decorator. A classe Decorator (DecoratorDecoder) será usada para encapsular o decodificador real e adicionar funcionalidades extras, como registro de log ou outras operações antes/depois da decodificação.

$${\text{Java (Decodificador Decorator)}}$$

```java
class DecoratorDecoder implements Decoder {
    private final Decoder wrappedDecoder;

    public DecoratorDecoder(Decoder wrappedDecoder) {
        this.wrappedDecoder = wrappedDecoder;
    }

    @Override
    public String decode(String msg) {
        System.out.println("LOG: Pre-decoding message...");
        String result = wrappedDecoder.decode(msg);
        System.out.println("LOG: Post-decoding finished.");
        return result;
    }
}
```

$${\text{Java (Serviço de Mensagem com Decorator)}}$$

```java
class MessageService {
    private final DecoderFactory decoderFactory;

    public MessageService(DecoderFactory decoderFactory) {
        this.decoderFactory = decoderFactory;
    }

    public String processMessage(String msg) {
        Decoder decoder = new DecoratorDecoder(decoderFactory.createDecoder());
        return decoder.decode(msg);
    }
}
```

Note que o novo código não precisará ser modificado para novos formatos de mensagem, pois bastará criar uma nova fábrica concreta e seus decodificadores.

## Esse padrão é útil, por exemplo, na implementação do **framework EJB** ou na interface com o usuário **GUI**, em que é necessário criar diferentes famílias de *widgets* (botões, *text fields*, etc.) que sejam consistentes em sua aparência. Assim, a **Abstract Factory** é uma técnica que permite a criação de objetos relacionados em diferentes famílias de produtos (como *Motif* e *Open Look*), e é uma técnica que oferece uma interface para criar famílias de objetos relacionados ou dependentes sem especificar suas classes concretas.

## 2\. Padrão de projeto Abstract Factory

### Abstract Factory X Injeção de dependências

**Injeção de dependência** e **Abstract Factory** são conceitos comuns na **programação orientada a objetos** e frequentemente trabalham juntos. Ao injetar uma dependência, como no uso de componentes de persistência no **Spring Framework**, a classe **DAO** a ser injetada, em vez de ser uma classe concreta, é uma interface que é substituída pelo padrão **Abstract Factory**, oferecendo uma abordagem mais simples para o desacoplamento. Isso acontece quando as classes concretas são resolvidas em tempo de execução, diferindo apenas o modelo de escrita, sem alterar a funcionalidade.

Acompanhe no vídeo a seguir como o *framework* de injeção de dependências pode substituir uma implementação manual do padrão Abstract Factory.

Um exemplo clássico de utilização do padrão **Abstract Factory** é a **implementação de persistência** em **bancos de dados**. Em vez de o cliente manipular diretamente o objeto **DAO** (Data Access Object), cria-se uma interface para a geração de um **DAO abstrato**, o qual prevê os métodos para inclusão, alteração, exclusão e consulta de registros. Ao direcionar para um banco de dados específico, temos a **fábrica concreta** e o **DAO concreto**, que é quem de fato implementa a manipulação de dados.

Em uma implementação, utilizando **SQL, ANSI**, boa parte do código de acesso, mais para a parte da busca e manutenção de dados, poderia ser terceirizada para a camada. Nesse caso, é aproveitado pelos *frameworks*, como **Hibernate** ou **Spring**, que centralizam as regras de negócio em suas localizações, promovendo a **injeção de dependência**, e deixando para a **fábrica** a responsabilidade de executar os comandos **SQL** necessários.

Internamente, temos uma **fábrica abstrata** e o **Spring** a utiliza para a **configuração da conexão**, ou seja, utilizando as informações de conexão, além do *driver* utilizado, para gerar cada **comando SQL** corretamente, a partir do banco de dados específico. A fábrica usa o **JDBC** e a **API específica** para retornar um objeto **Entity**. Todo o mecanismo é acionado a um **serviço** quando uma instância do **DAO** é anotada com **@Autowired**, o que configura a **injeção de dependência**, em que o controle dos processos invocados passa a consumir a **contextualização** da **classe** que sempre **gerencia** a **fábrica**.