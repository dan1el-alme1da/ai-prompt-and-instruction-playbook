
-----

## 1\. Padrão de projeto Factory Method

### Observação e problemas no padrão Factory Method

O padrão de projeto **Factory Method** tem como objetivo criar objetos, mas deixa a cargo das subclasses a escolha da classe que será instanciada. Isso permite que uma classe delegue a responsabilidade de criação de objetos para suas subclasses.

Imagine o seguinte problema em um sistema de gerenciamento de dados: você precisa criar objetos de diferentes tipos de dados, como listas, conjuntos ou mapas, mas a escolha do tipo exato só pode ser definida em tempo de execução.

**Quais seriam os problemas se você usasse o operador `new` para instanciar objetos?**

  * **Acoplamento:** O código se tornaria altamente acoplado a classes concretas.
  * **Baixa Coesão:** A classe principal teria que saber os detalhes de implementação das subclasses.
  * **Baixa Flexibilidade:** Adicionar novos tipos de dados exigiria modificar o código-fonte da classe principal.

Ao invés de usar o operador `new` diretamente, podemos usar um método para criar objetos, um método que é chamado de **Factory Method**.

```java
// Código de exemplo para o problema
public class DataManager {
    public void processData(String dataType) {
        // Problema: Acoplamento
        // Usando "new" diretamente, o código fica acoplado a classes concretas
        if (dataType.equals("list")) {
            List data = new ArrayList(); // Cria um ArrayList
            data.add("Item 1");
            // ... processamento
        } else if (dataType.equals("set")) {
            Set data = new HashSet(); // Cria um HashSet
            data.add("Item 1");
            // ... processamento
        }
        // ... mais tipos
    }
}
// ... classes concretas de dados
```

-----

## 2\. Padrão de projeto Factory Method

### Solução do padrão Factory Method

Este padrão define um método responsável por instanciar e retornar o objeto de maneira pura. Uma hierarquia de classes abstratas ou *interfaces* definem a estrutura básica, e as classes concretas implementam o método de fábrica para instanciar os objetos específicos.

A seguir, a estrutura de classes que representam uma interface genérica chamada **Collection**. Aqui estão exemplos de diferentes implementações:

  * `ArrayList`
  * `LinkedList`
  * `HashSet`
  * `TreeSet`

A organização dessas classes está baseada em hierarquia.

**Diagrama de classes da organização Collection:**
(Diagrama mostrando que `List` e `Set` herdam de `Collection`. `ArrayList` e `LinkedList` herdam de `List`. `HashSet` e `TreeSet` herdam de `Set`.)

A hierarquia **Collection** aplica uma estrutura chamada `Iterable`. Implementações em cada estrutura de dados específica, como `ArrayList` ou `HashSet`, implementam a *interface* **Iterable**.

A interface **Iterable** tem um método chamado `iterator()`, que retorna um **Iterator**.

Um **Iterator** é um objeto que permite a iteração sobre uma coleção de objetos, sem expor os detalhes de sua organização. O `iterator()` é um **Factory Method** que retorna o objeto **Iterator** para cada classe que implementa a interface `Collection`, como pode ser visto na imagem a seguir:

**Diagrama de classes do padrão Iterator/Factory Method:**
(Diagrama mostrando que `ArrayListIterator`, `LinkedListIterator`, `KeyIterator`, e `ValueIterator` implementam a interface `Iterator` e são retornados por métodos `iterator()` de classes como `ArrayList`, `LinkedList`, `HashMap`, etc. Esta estrutura ilustra o Factory Method.)

Desta forma, o que seria feito, de forma de acoplamento, com o uso de `new` em `ArrayList` ou em uma `LinkedList` instanciando um `ListIterator`, é feito por um método que delega a responsabilidade de criação para uma subclasse, seguindo o padrão **Factory Method**.

Agora, no código a seguir, você pode ver a implementação da hierarquia de classes concretas usando esse **Factory Method**:

```java
// Código de exemplo da implementação
public abstract class DataManager {
    // Factory Method: Cria um objeto de dado (Product)
    public abstract DataObject createDataObject(); 

    public void processData() {
        DataObject data = createDataObject();
        data.addData("Item 1");
        // ... processamento
    }
}

public class ListManager extends DataManager {
    @Override
    public DataObject createDataObject() {
        return new ArrayListDataObject();
    }
}
// ...
```

O **Factory Method** não é apenas o método que instancia um objeto, mas também o conceito de um *framework* que define a responsabilidade de criação para classes concretas. Ao usá-lo, estamos em conformidade com o **Princípio Aberto/Fechado** (Open/Closed Principle), pois podemos adicionar novas classes concretas de *products* (objetos criados) sem modificar o código do *creator* (classe que contém o Factory Method).

**Diagrama geral da solução proposta pelo padrão Factory Method:**
(Diagrama mostrando a estrutura do **Factory Method** com as classes **Product**, **Creator**, **ConcreteProduct** e **ConcreteCreator**.)

Observe que a estrutura geral da solução proposta pelo padrão **Factory Method** define quatro participantes:

1.  **Product:** Define a *interface* dos objetos que serão criados.
2.  **Creator:** Declara o **Factory Method**, que retorna um objeto do tipo **Product**. O *Creator* define o método, mas a implementação fica a cargo da subclasse.
3.  **ConcreteProduct:** Implementa a *interface* **Product**.
4.  **ConcreteCreator:** Sobrescreve (*override*) o **Factory Method** para retornar uma instância de **ConcreteProduct**.

-----

## 3\. Padrão de projeto Factory Method

### Consequências e padrões relacionados ao Factory Method

A principal consequência do uso do **Factory Method** é o aumento da **extensibilidade**, já que novas classes **ConcreteProduct** podem ser adicionadas sem a necessidade de modificar o código das classes **Creator**. Isso promove o **baixo acoplamento** e a **alta coesão**.

Ele também permite que o poder de criação de objetos combine com outros padrões, como **Strategy**, que define estratégias específicas para cada tipo concreto, ou **Template Method**, que controla etapas comuns de criação em subclasses.

Explore os tópicos a seguir de consequências e padrões relacionados ao **Factory Method**.

O padrão **Factory Method** permite que diferentes implementações de um mesmo serviço possam ser usadas.

Além disso, esse padrão possibilita a conexão de duas hierarquias paralelas representadas pelos participantes genéricos **Creator** e **Product**.

\<div style="background-color: \#e0f7fa; padding: 10px; border-radius: 5px;"\>
\<h4\>Dica\</h4\>
O **Factory Method** é muito útil quando precisamos segregar uma hierarquia de objetos diferentes em hierarquias separadas. Isso garante um baixo acoplamento e flexibilidade para as classes que interagem com essas informações.
\</div\>

O padrão **Factory Method** pode ser aplicado em conjunto com o padrão **Strategy**, que tem como objetivo definir diferentes comportamentos ou algoritmos para classes concretas.

**Template Method** é outro padrão frequentemente utilizado em conjunto com o **Factory Method**. Trata-se de uma implementação genérica definida em uma superclasse que contém passos que podem ser sobrescritos pelas subclasses. O **Factory Method** pode ser um desses passos, onde a superclasse define que um objeto será criado, mas a subclasse decide qual.

A seguir, um código que demonstra a combinação entre **Factory Method** e **Strategy**.

```java
// Código de exemplo de Factory Method + Strategy
public abstract class PagamentoStrategy {
    public abstract void pagar(double valor); 
}

public class CartaoCreditoStrategy extends PagamentoStrategy {
    @Override
    public void pagar(double valor) {
        System.out.println("Pagamento com Cartão de Crédito: " + valor);
    }
}

// ... Outras estratégias

public abstract class PagamentoFactory {
    // Factory Method
    public abstract PagamentoStrategy criarPagamento();
}

public class CartaoCreditoFactory extends PagamentoFactory {
    @Override
    public PagamentoStrategy criarPagamento() {
        return new CartaoCreditoStrategy();
    }
}

// Uso da combinação
public class Pedido {
    private PagamentoFactory factory;
    private double valor;

    public Pedido(PagamentoFactory factory, double valor) {
        this.factory = factory;
        this.valor = valor;
    }

    public void processarPagamento() {
        PagamentoStrategy strategy = factory.criarPagamento(); // Factory Method
        strategy.pagar(valor); // Strategy
    }
}
```