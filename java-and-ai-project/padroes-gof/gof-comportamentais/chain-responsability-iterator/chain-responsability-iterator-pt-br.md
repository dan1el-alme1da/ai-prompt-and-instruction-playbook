
-----

## Padrão Chain of Responsibility

O padrão de projeto comportamental **Chain of Responsibility** (Cadeia de Responsabilidade) permite que a solicitação de um objeto seja passada através de uma cadeia de possíveis objetos manipuladores. A cada manipulador é dada a oportunidade de manipular a solicitação ou passá-la para o próximo manipulador na cadeia.

### Benefícios do padrão Chain of Responsibility

  * **Evita o acoplamento** entre o remetente da solicitação e seus receptores. A solicitação pode ser tratada por qualquer um dos objetos na cadeia.
  * **Permite que múltiplos objetos manipulem uma solicitação**, sem que o remetente precise saber exatamente qual objeto fará o tratamento.
  * **Aumenta a flexibilidade** na atribuição de responsabilidades a objetos. A cadeia de manipuladores pode ser alterada em tempo de execução.

### Exemplo prático para o padrão Chain of Responsibility

Imagine um sistema de aprovação de despesas. A solicitação de despesa (objeto `Request`) pode ser:

1.  Aprovada pelo gerente imediato se for inferior a R$ 100,00.
2.  Aprovada pelo gerente de departamento se for inferior a R$ 1.000,00.
3.  Aprovada pelo diretor se for superior a R$ 1.000,00.
4.  Rejeitada se for superior a R$ 10.000,00.

O Chain of Responsibility permite que a solicitação percorra a cadeia de aprovadores, e cada um decide se a manipula ou a repassa.

### Solução do padrão Chain of Responsibility

O padrão de projeto Chain of Responsibility é composto por três participantes principais:

1.  **Handler (Manipulador):** Define a interface para manipular a solicitação e, opcionalmente, implementa o encadeamento.
2.  **ConcreteHandler (Manipulador Concreto):** Manipula as solicitações que é responsável e, se não, as repassa para seu sucessor na cadeia.
3.  **Client (Cliente):** Inicia a solicitação ao primeiro objeto na cadeia.

#### Diagrama de Classes (Representação Textual)

```
       +-----------------------+
       |        Handler        |
       +-----------------------+
       | + setNext(Handler)    |
       | + handleRequest(Request)|
       +-----------------------+
                ^
               / \
              /   \
             /     \
    +----------+----------+----------+
    |  ConcreteHandlerA  |  ConcreteHandlerB  |  ConcreteHandlerC  |
    +----------+----------+----------+
    | + handleRequest(Request) | + handleRequest(Request) | + handleRequest(Request) |
    +----------+----------+----------+
```

*O diagrama mostra a interface **Handler** com o método `setNext` (para definir o sucessor) e `handleRequest` (para processar a solicitação). As classes **ConcreteHandlerA, ConcreteHandlerB** e **ConcreteHandlerC** implementam a interface Handler e contêm a lógica de manipulação e repasse da solicitação.*

-----

## Padrão Command

O padrão de projeto comportamental **Command** (Comando) encapsula uma solicitação como um objeto, permitindo que você **parametrize** clientes com diferentes solicitações, enfileire ou registre solicitações, e suporte operações que podem ser desfeitas (undo).

### Introdução ao padrão Command

O padrão Command transforma uma solicitação em um objeto próprio, o que permite que você:

  * Passe requisições como argumentos de métodos.
  * Crie filas de requisições.
  * Defina callbacks para serem executados em momentos específicos.
  * Suporte a funcionalidade de "desfazer" (`undo`) e "refazer" (`redo`).

### Implementação do padrão Command

O padrão Command é composto por quatro participantes principais:

1.  **Command (Comando):** Declara uma interface para a execução de uma operação.
2.  **ConcreteCommand (Comando Concreto):** Define a ligação entre um objeto **Receiver** e uma ação. Ele implementa o método **Execute** invocando a operação relevante do Receiver.
3.  **Invoker (Invocador):** Pede que o comando realize a solicitação. Ele mantém uma referência para um objeto Command.
4.  **Receiver (Receptor):** Sabe como realizar as operações associadas à solicitação.

#### Tabela: Participantes do padrão Command

| Participante | Função |
| :--- | :--- |
| **Command** | Define a interface para executar a operação. |
| **ConcreteCommand** | Implementa a interface Command, armazena a referência ao **Receiver** e chama as operações do Receiver. |
| **Invoker** | Objeto que aciona o método **Execute()** do Command. |
| **Receiver** | Objeto que executa a lógica da operação. |

### Solução do padrão Command

O `Invoker` armazena o `Command`, mas não sabe nada sobre o `Receiver` ou a lógica da operação. O `Command` é responsável por invocar a ação no `Receiver`.

#### Diagrama de Classes (Representação Textual)

```
+----------------+      +-----------------+       +----------------+
|    Client      |----->|     Invoker     |------>|    Command     |
+----------------+      +-----------------+       +----------------+
                        | - Command       |       | + Execute()    |
                        +-----------------+       +----------------+
                                                         ^
                                                        / \
                                                       /   \
                                                      /     \
                                           +--------------+
                                           | ConcreteCommand |
                                           +--------------+
                                           | - Receiver     |
                                           | + Execute()     |----> +----------------+
                                           +--------------+      |    Receiver    |
                                                                 | + Action()     |
                                                                 +----------------+
```

*O diagrama mostra o **Client** criando um **ConcreteCommand** e um **Receiver**, e configurando o **Invoker** com o **ConcreteCommand**. Quando o **Invoker** chama `Execute()` no **Command**, o **ConcreteCommand** chama o método `Action()` no **Receiver** para realizar o trabalho.*

-----

## Padrão Iterator

O padrão de projeto comportamental **Iterator** (Iterador) fornece uma maneira de acessar sequencialmente os elementos de um objeto **agregado** subjacente sem expor sua representação interna.

### Introdução do padrão Iterator

O Iterator permite que você percorra coleções de objetos de uma maneira uniforme, independentemente do tipo específico de coleção (por exemplo, `List`, `Array`, `Map`).

O padrão define duas interfaces principais:

  * **Iterator:** Define a interface para acessar e percorrer os elementos de uma coleção.
  * **Aggregate:** Define a interface para criar o objeto Iterator correspondente.

### Solução do padrão Iterator

O padrão Iterator separa a responsabilidade de percorrimento da coleção da própria estrutura de dados. O objeto `Aggregate` (Coleção) é responsável por criar o `Iterator`.

#### Participantes de classes:

  * **Iterator:** Define uma interface para acessar e percorrer os elementos. Normalmente, contém métodos como `hasNext()`, `next()`, e opcionalmente `remove()`.
  * **ConcreteIterator:** Implementa a interface **Iterator** e gerencia a posição atual no percurso da agregação.
  * **Aggregate:** Define uma interface para criar um objeto **Iterator**.
  * **ConcreteAggregate:** Implementa a interface **Aggregate** e retorna uma instância do **ConcreteIterator** apropriado.

#### Diagrama de Classes (Representação Textual)

```
      +----------------+               +----------------+
      |    Iterator    |<------------- | ConcreteIterator |
      +----------------+               +----------------+
      | + hasNext()    |               | - Aggregate     |
      | + next()       |               | + hasNext()     |
      | + remove() (opcional)|        | + next()        |
      +----------------+               +----------------+
             ^
            / \
           /   \
          /     \
+--------------+               +-------------------+
|   Aggregate  |--------------->| ConcreteAggregate |
+--------------+               +-------------------+
| + createIterator() |           | + createIterator()|
+--------------+               +-------------------+
```

*O diagrama mostra a interface **Iterator** e sua implementação **ConcreteIterator**. O **ConcreteAggregate** implementa a interface **Aggregate** e tem a responsabilidade de instanciar o **ConcreteIterator** que conhece sua estrutura interna e sabe como percorrê-la.*