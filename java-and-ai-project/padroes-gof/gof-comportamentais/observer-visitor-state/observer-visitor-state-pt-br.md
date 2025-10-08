
-----

# Transcrição Fiel do Conteúdo

## Padrões de projeto comportamentais Observer, Visitor e State

### Padrão Visitor

O padrão de projeto **Visitor** permite separar algoritmos de objetos em classes diferentes. Isso é particularmente útil quando é necessário adicionar novas operações a um conjunto de classes de objetos sem modificar essas classes. Em vez de adicionar um novo método para cada operação em cada classe, essas operações são encapsuladas em objetos **Visitor**.

**Intenção do padrão Visitor**

Representa uma operação a ser executada nos elementos de uma estrutura de objetos. **Visitor** permite definir uma nova operação sem mudar as classes dos elementos sobre os quais opera.

**Problema resolvido pelo padrão Visitor**

Considere um sistema com diferentes tipos de objetos, como **Elefante**, **Leão** e **Macaco**, todos derivados de um tipo base **Animal**. Se fosse necessário introduzir novas operações, como **Comer** ou **Dormir**, cada uma exigiria a adição de um novo método em todas as classes, o que seria custoso e tornaria o código confuso.

Esta estrutura de classes é comum em linguagens para diferentes formatos, tais como: verificar se a instrução é válida, realizar uma otimização no código etc.

Caso exista essa necessidade, podemos adicionar um objeto desse padrão, chamado **Visitor**, que contém todos os métodos que o **Animal** pode executar e passamos o objeto **Animal** como parâmetro.

O **padrão Visitor** nos permite **acrescentar novas funcionalidades para classes existentes** sem a necessidade de modificar o código original dessas classes. Isso é especialmente útil em projetos com classes que não podem ser modificadas (por exemplo, bibliotecas).

**Solução do padrão Visitor**

A estrutura de classes do projeto para o padrão **Visitor** é apresentada no diagrama e segue:

-----

**Observação sobre o código:** O código-fonte a seguir mostra um exemplo da implementação da interface **Visitor** em Java, onde cada método `visit()` é sobrecarregado para um tipo específico de objeto.

```java
public interface Visitor {
    void visit(ElementA element);
    void visit(ElementB element);
    void visit(ElementC element);
}
```

E a seguir, um exemplo da implementação da interface **Elemento** (Element), que possui um método `accept()` para receber o **Visitor** e invocar o método `visit()` específico para ele.

```java
public interface Element {
    void accept(Visitor visitor);
}
```

Abaixo está a implementação de um **Elemento Concreto** (Concrete Element), que implementa a interface **Element** e fornece o código para o método `accept()`.

```java
public class ConcreteElementA implements Element {
    @Override
    public void accept(Visitor visitor) {
        visitor.visit(this); // Chama o método visit específico
    }
    // Outros métodos específicos do ConcreteElementA
    public String operationA() {
        return "Operação A executada.";
    }
}
```

E, por último, um exemplo da implementação do **Visitante Concreto** (Concrete Visitor), que implementa a interface **Visitor** e fornece a lógica para cada tipo de elemento concreto.

```java
public class ConcreteVisitor implements Visitor {
    @Override
    public void visit(ConcreteElementA element) {
        // Lógica de visitação específica para ConcreteElementA
        System.out.println("Visitando elemento A: " + element.operationA());
    }
    @Override
    public void visit(ConcreteElementB element) {
        // Lógica de visitação específica para ConcreteElementB
        System.out.println("Visitando elemento B.");
    }
    @Override
    public void visit(ConcreteElementC element) {
        // Lógica de visitação específica para ConcreteElementC
        System.out.println("Visitando elemento C.");
    }
}
```

-----

O padrão **Visitor** é útil em projetos nos quais se espera que o **conjunto de classes de objetos seja estável**, mas o **conjunto de operações que podem ser executadas** nesses objetos varie. Isso facilita a introdução de novos comportamentos sem modificar as classes dos objetos.

**Consequências e padrões relacionados ao Visitor**

O padrão **Visitor** tem como objetivo a **separação de interfaces** e promove a **coesão** dos comportamentos relacionados em um único objeto.

É útil quando:

  * Você tem muitas operações diferentes que precisam ser aplicadas em objetos de diferentes classes que compõem uma estrutura.
  * As classes que compõem a estrutura raramente mudam, mas você frequentemente precisa definir novas operações sobre a estrutura.
  * Você precisa separar o algoritmo do objeto em que ele opera.

**ATENÇÃO**
O padrão **Visitor** é muito útil para ser utilizado em conjunto com os padrões **Composite** e **Iterator**.

-----

-----

### Padrão Observer

O padrão de projeto **Observer** permite que um objeto, chamado **Subject** (ou Sujeito), notifique outros objetos, chamados **Observers** (ou Observadores), sobre qualquer mudança de seu estado. Assim, é possível estabelecer um relacionamento de **dependência** de um-para-muitos entre objetos, de forma que quando um objeto (**Subject**) muda de estado, todos os seus dependentes (**Observers**) são notificados e atualizados automaticamente.

**Intenção do padrão Observer**

Define uma dependência de um-para-muitos entre objetos, de modo que quando um objeto muda de estado, todos os seus dependentes são notificados e atualizados automaticamente.

**Problema resolvido pelo padrão Observer**

Um exemplo clássico de aplicação do padrão **Observer** ocorre na atualização de painéis em um **Dashboard**, onde diversos gráficos e tabelas (os Observers) precisam ser atualizados sempre que os dados de uma fonte de dados (o Subject) são alterados.

O primeiro painel é apresentado em **tabela**:

| Região 1 | Região 2 | Região 3 | Total |
| :--- | :--- | :--- | :--- |
| 40 | 30 | 100 | 170 |
| 120 | 80 | 70 | 270 |
| 80 | 50 | 90 | 220 |
| 240 | 160 | 260 | 660 |

**Tabela:** Painel 1.
Alexandre Luis Correa.

Os painéis 2 e 3 são apresentados em **gráficos**:

-----

O código-fonte pode ficar complexo quando o número de painéis aumenta, tornando difícil manter essa sincronização de forma manual. É necessário um mecanismo que gerencie essa relação de dependência.

O **padrão Observer** permite que o **Subject** mantenha uma lista de objetos **Observers** e ofereça métodos para **anexar** (adicionar), **desanexar** (remover) e **notificar** todos os **Observers** quando uma mudança de estado ocorrer.

**Solução do padrão Observer**

A estrutura de classes do projeto para o padrão **Observer** é apresentada no diagrama e segue:

-----

**Observação sobre o código:** O código-fonte abaixo mostra a implementação da interface **Observer** em Java, que possui o método `update()`, que será chamado pelo **Subject** para notificar o **Observer** sobre uma mudança de estado.

```java
public interface Observer {
    void update(Subject subject);
}
```

O código a seguir mostra a interface **Subject** em Java, que define os métodos para adicionar, remover e notificar os **Observers**.

```java
public interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers();
    // Outros métodos do Subject
}
```

A seguir, um exemplo do **Subject Concreto** (Concrete Subject), que implementa a interface **Subject** e mantém o estado de interesse.

```java
import java.util.ArrayList;
import java.util.List;

public class ConcreteSubject implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private int state;

    public int getState() {
        return state;
    }

    public void setState(int state) {
        this.state = state;
        notifyObservers(); // Notifica os observers quando o estado muda
    }

    @Override
    public void attach(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void detach(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(this);
        }
    }
}
```

E por último, um exemplo de **Observer Concreto** (Concrete Observer), que implementa a interface **Observer** e contém a lógica para se atualizar quando notificado.

```java
public class ConcreteObserver implements Observer {
    private String name;

    public ConcreteObserver(String name) {
        this.name = name;
    }

    @Override
    public void update(Subject subject) {
        if (subject instanceof ConcreteSubject) {
            int state = ((ConcreteSubject) subject).getState();
            System.out.println("Observer " + name + " atualizado. Novo estado: " + state);
            // Lógica de atualização específica do observer, como redesenhar um gráfico.
        }
    }
}
```

-----

O padrão **Observer** promove o **baixo acoplamento** entre o **Subject** e os **Observers**, permitindo que eles interajam sem ter conhecimento um do outro. Isso facilita a manutenção e a extensibilidade do sistema, já que novos **Observers** podem ser adicionados sem modificar o **Subject**.

**Consequências e padrões relacionados ao Observer**

O padrão **Observer** é amplamente utilizado em diversas áreas, como em frameworks de interface gráfica (GUIs), onde elementos da interface (como botões e campos de texto) atuam como **Subjects** e outras partes do sistema (os **Observers**) reagem às suas ações.

Ele é importante em sistemas orientados a eventos e permite a comunicação de forma eficiente e flexível.

**ATENÇÃO**
O padrão **Observer** é muito útil para projetos que precisam **manter a sincronização entre diversos objetos** quando o estado de um objeto central é alterado.

-----

-----

### Padrão State

O padrão de projeto **State** permite que um objeto altere seu comportamento quando seu estado interno muda. Externamente, o objeto parece ter mudado de classe. Ele encapsula o comportamento de um objeto em diferentes estados em classes separadas. Em vez de usar instruções condicionais (como `if/else` ou `switch`) para mudar de comportamento, o objeto delega as chamadas para a classe que representa o estado atual.

**Intenção do padrão State**

Permite que um objeto mude seu comportamento quando o seu estado interno muda. O objeto parecerá ter mudado de classe.

**Problema resolvido pelo padrão State**

O padrão **State** é muito útil para problemas relacionados à implementação de entidades com uma **máquina de estados**, ou seja, que tem comportamentos distintos em cada um de seus estados. Considere um objeto **Semaforo** que muda seu comportamento (a luz que acende) dependendo de seu estado (Vermelho, Amarelo ou Verde). Em uma implementação tradicional, o objeto **Semaforo** teria um método `MudarSinal()` com várias instruções condicionais (`if/else` ou `switch`) para determinar qual é o próximo estado e qual ação executar, o que tornaria o código complexo e difícil de manter.

O código-fonte segue uma implementação convencional sem a utilização do padrão **State**, mas essa abordagem se torna complexa e difícil de manter à medida que o número de estados e transições aumenta.

```java
public class SemaforoSemPadrao {
    private String estado; // "Vermelho", "Amarelo", "Verde"

    public SemaforoSemPadrao() {
        this.estado = "Vermelho";
    }

    public void MudarSinal() {
        if (this.estado.equals("Vermelho")) {
            this.estado = "Verde";
            System.out.println("Sinal Verde! Siga.");
        } else if (this.estado.equals("Verde")) {
            this.estado = "Amarelo";
            System.out.println("Sinal Amarelo! Atenção.");
        } else if (this.estado.equals("Amarelo")) {
            this.estado = "Vermelho";
            System.out.println("Sinal Vermelho! Pare.");
        }
    }
}
```

**Solução do padrão State**

A estrutura de classes do projeto para o padrão **State** é apresentada no diagrama e segue:

-----

**Observação sobre o código:** O código-fonte a seguir mostra a implementação da interface **State** em Java.

```java
public interface EstadoSemaforo {
    void handle(Semaforo context);
    String getNomeEstado();
}
```

O **Contexto** (Context) define a interface e mantém uma referência à instância de uma subclasse **State** (o estado atual).

```java
public class Semaforo {
    private EstadoSemaforo estadoAtual;

    public Semaforo() {
        // Estado inicial
        this.estadoAtual = new EstadoVermelho();
    }

    public void setEstado(EstadoSemaforo novoEstado) {
        this.estadoAtual = novoEstado;
        System.out.println("Sinal mudou para: " + novoEstado.getNomeEstado());
    }

    public void MudarSinal() {
        this.estadoAtual.handle(this);
    }

    public EstadoSemaforo getEstadoAtual() {
        return estadoAtual;
    }
}
```

A seguir, um exemplo de um **Estado Concreto** (Concrete State), que implementa o comportamento associado ao estado "Vermelho".

```java
public class EstadoVermelho implements EstadoSemaforo {
    @Override
    public void handle(Semaforo context) {
        System.out.println("Sinal Vermelho! Pare. Transicionando para Verde...");
        context.setEstado(new EstadoVerde()); // Transição de estado
    }

    @Override
    public String getNomeEstado() {
        return "Vermelho";
    }
}
```

-----

O padrão **State** permite que o objeto **Contexto** delegue seu comportamento ao objeto **State** atual. Quando o estado muda, o **Contexto** altera a referência para outra subclasse **State**, modificando o comportamento do objeto. Isso elimina as instruções condicionais do **Contexto**, tornando-o mais limpo e mais fácil de estender.

**Consequências e padrões relacionados ao State**

O padrão **State** promove a **coesão** ao agrupar todo o comportamento relacionado a um estado em uma classe e o **baixo acoplamento** entre o **Contexto** e os estados concretos, que podem ser alterados independentemente.

Ele facilita a adição de novos estados e a modificação de transições, sem impactar o código do **Contexto**. Isso torna o sistema mais flexível e mais fácil de manter.

**ATENÇÃO**
Este padrão também é conhecido como **Padrão de Máquina de Estados Baseada em Objetos**.

O padrão **State** pode ser combinado com o padrão **Strategy**, permitindo a compartimentalização de diferentes algoritmos. As classes concretas, representando o estado de um objeto, são úteis para lidar com transições complexas.