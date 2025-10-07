
-----

## 1\. Padrão de projeto Builder

### Intenção e problema do padrão Builder

Em muitas situações, a construção de um objeto complexo pode levar à definição de múltiplos construtores no código. O problema se intensifica à medida que novos parâmetros são adicionados, o que exige a redefinição de todos esses construtores.

O padrão **Builder** permite desassociar os métodos geradores dos passos para a construção, para que estes passos possam ser reutilizados na construção de objetos que podem ter diferentes representações.
Note-se que cada vez mais está presente a presença do Builder em diversas bibliotecas. A **Javalin**, por exemplo, permite que o usuário defina uma interface Builder para compor os *handlers* da API. O **Lombok**, quando presente, permite a integração de dados em formatos como **JSON** e **XML**.

O padrão visa mitigar a limitação e o problema do padrão **Builder**.

#### Intenção do padrão Builder

Isolar a parte que sabe expor a construção de um objeto complexo da sua representação, de forma que o mesmo processo de construção possa construir diferentes representações desse objeto.

#### Problema do padrão Builder

Imagine que você precise de um mecanismo para uma conversa de sistema mediadora, e que esse sistema permita que o cliente exporte suas notas de negociação em diferentes formatos, tais como **JSON** e **XML**.

Imagine que o processo de construção de qualquer representação de nota de negociação seja definido pelo uso das seguintes operações:

  * **Consultar**
      * Consultar informações das notas.
  * **Identificar**
      * Identificar e catalogar as notas de negociação.
  * **Usar**
      * Documentar as operações das notas.
  * **Gerar**
      * Gerar um sumário com os totais e *tickets* de todas as operações do dia.

Uma solução frequentemente utilizada para tal problema é aplicar todas as operações conversadas em um método de exportação, conforme o código a seguir:

```java
public class ExportaNota {
    // ... outros atributos
    public String exportaNota(List<Negociacao> negociacoes, String formato, Exportador exportador) {
        String resultado = "";
        if (formato.equals("JSON")) {
            resultado += "{\n";
            resultado += " \"negociacoes\": [\n";
            // ... código de conversão para JSON
            resultado += "]\n";
            resultado += "}";
        } else if (formato.equals("XML")) {
            resultado += "<negociacoes>\n";
            // ... código de conversão para XML
            resultado += "</negociacoes>\n";
        }
        return resultado;
    }
}
```

A operação **exportaNota** recebe a lista de negociações e vai exportar para o formato de exportação definido em **formato**.

Essa abordagem apresenta três (3) adequações, pois, além de concentrar em um único método todas as etapas para exportar as negociações em formatos como **JSON** ou **XML**, o método também se preocupa com o retorno para cada formato específico. Além disso, o método deve ser modificado a cada nova forma de representação que o cliente queira. Em tais situações, o padrão **Builder** é a melhor alternativa.

-----

## Solução do padrão Builder

A solução consiste em ter um **Builder** (Construtor) em comum, que gerencia a criação de objetos complexos de quem o chama. O **Builder** é a peça central que esconde toda a complexidade envolvida na criação do objeto, expondo apenas os métodos necessários.

O **Director** gerencia o **Builder**, que constrói as partes do **Product** e o retorna.

### Diagrama de classes

A arquitetura **Builder** define as operações que criam as diferentes partes de um produto (uma forma para exportar nota de negociação em **JSON**, por exemplo). O **ConcreteBuilder** implementa as etapas de construção do **Builder** e define o produto a ser retornado, contendo todas as diferentes formas de representação (nota de negociação em **JSON**, **XML**, **CSV**, **TEXTO** e outros).

O **Director** gerencia o **Builder**, que constrói as partes do **Product** e o retorna.

**[Nesta parte da imagem, há um diagrama de classes/sequência que não posso transcrever textualmente, mas que ilustra a interação entre Director, Builder, ConcreteBuilder e Product.]**

### Padrão

O padrão **Builder** é um padrão de projeto criacional que permite construir objetos complexos passo a passo. O padrão **Builder** isola o código de construção do produto, de modo que a criação do objeto complexo possa ter diferentes representações (formatos para exportar notas de negociação: **JSON**, **XML**, **CSV**, **TEXTO** etc.), sendo controlado por um **Director**.

Os métodos concretos definidos no **Builder** não devem ser dependentes de plataformas, mas devem determinar a arquitetura e a ordem das operações para construir objetos complexos.

As responsabilidades de cada classe são:

  * **Builder**:
      * Define uma interface para criar as partes do objeto **Produto**.
  * **ConcreteBuilder**:
      * Constrói e monta as partes do **Produto** por meio da interface **Builder**.
      * Define a representação interna do **Produto** e fornece o acesso.
  * **Director**:
      * Constrói um objeto usando a interface **Builder**.
  * **Product**:
      * Representa o objeto complexo que está sendo construído.

A classe **NotaNegociacaoExportavelBuilderImpl** é um exemplo de cliente da interface **ExportavelBuilder**. O método **build()** retorna a instância de **NotaNegociacaoExportavelImpl**.

```java
// Código de exemplo para a classe NotaNegociacaoExportavelBuilderImpl
public class NotaNegociacaoExportavelBuilderImpl implements ExportavelBuilder<NotaNegociacaoExportavel> {
    private NotaNegociacaoExportavelImpl notaExportavel;

    public NotaNegociacaoExportavelBuilderImpl(NotaNegociacaoExportavelImpl notaExportavel) {
        this.notaExportavel = notaExportavel;
    }

    @Override
    public ExportavelBuilder<NotaNegociacaoExportavel> comTipo(String tipo) {
        this.notaExportavel.setTipo(tipo);
        return this;
    }

    // ... outros métodos builder
    
    @Override
    public NotaNegociacaoExportavel build() {
        return notaExportavel;
    }
}
```

A classe **NotaNegociacaoExportavelImpl** implementa as operações definidas em **Exportavel**.

```java
// Código de exemplo para a classe NotaNegociacaoExportavelImpl
public class NotaNegociacaoExportavelImpl implements NotaNegociacaoExportavel {
    private String tipo;
    private double valorTotal;
    // ... outros atributos

    // Getters e Setters

    @Override
    public void exportar() {
        System.out.println("Exportando nota de negociação do tipo: " + tipo + " com valor total: " + valorTotal);
        // Lógica de exportação
    }
}
```

Existem algumas questões importantes na implementação do padrão **Builder**:

O objeto **Builder** para ser acessado apenas possui um produto, após a execução do método **build()**, uma nova instância de **Builder** deve ser criada para construir um novo produto.

-----

## Consequências e padrões relacionados ao Builder

### Consequências do padrão Builder

O padrão **Builder** fornece um bom controle sobre o processo de construção do objeto. Essa propriedade fornece um bom controle sobre o processo de construção do objeto. Isso o torna um padrão poderoso, pois permite que cada passo de construção seja definido em interfaces e implementado em classes concretas, facilitando a alteração e extensão do código.

Algumas consequências e os padrões relacionados ao **Builder**:

  * **Permite variar a representação interna do produto:** É possível variar a representação interna do produto usando o **ConcreteBuilder** em cada variação.
  * **Encapsula o código de construção e representação:** O padrão **Builder** esconde a representação interna do produto.
  * **Fornece um controle refinado sobre o processo de construção:** O **Director** gerencia o **Builder**, definindo a ordem das operações do **Builder**.

### Padrões relacionados ao Builder

O padrão **Builder** é frequentemente usado em conjunto com outros padrões.

  * O padrão **Composite** é utilizado para representar objetos compostos por outros em uma hierarquia de representação de árvore. O **Builder** pode ser usado para construir árvores complexas do tipo **Composite**.
  * O padrão **Abstract Factory**, junto com o **Builder**, pode construir objetos complexos.
  * O padrão **Singleton** pode ser usado para garantir que apenas uma instância do **Director** exista.

### Aplicações do padrão de projeto Builder

Uma aplicação típica do padrão **Builder** é a substituição de múltiplas condições, permitindo a configuração apenas das classes concretas que implementam o construtor.

#### Exemplo de código do padrão Builder

```java
// Definição da interface ExportavelBuilder
public interface ExportavelBuilder<T extends Exportavel> {
    ExportavelBuilder<T> comTipo(String tipo);
    ExportavelBuilder<T> comValorTotal(double valorTotal);
    // ... outros métodos builder
    T build();
}
```

```java
// Definição da interface Exportavel
public interface Exportavel {
    void exportar();
}
```

Com base em constantes comuns, é possível definir a nossa **Product** e o **Builder** mais apropriado para a geração dos objetos.

```java
// Implementação da Product NotaNegociacaoExportavelImpl
public class NotaNegociacaoExportavelImpl implements NotaNegociacaoExportavel {
    private String tipo;
    private double valorTotal;
    // ... outros atributos

    // Getters e Setters

    @Override
    public void exportar() {
        System.out.println("Exportando nota de negociação do tipo: " + tipo + " com valor total: " + valorTotal);
        // Lógica de exportação
    }
}
```

```java
// Implementação do Builder NotaNegociacaoExportavelBuilderImpl
public class NotaNegociacaoExportavelBuilderImpl implements ExportavelBuilder<NotaNegociacaoExportavel> {
    private NotaNegociacaoExportavelImpl notaExportavel;

    public NotaNegociacaoExportavelBuilderImpl() {
        this.notaExportavel = new NotaNegociacaoExportavelImpl();
    }

    @Override
    public ExportavelBuilder<NotaNegociacaoExportavel> comTipo(String tipo) {
        this.notaExportavel.setTipo(tipo);
        return this;
    }

    @Override
    public ExportavelBuilder<NotaNegociacaoExportavel> comValorTotal(double valorTotal) {
        this.notaExportavel.setValorTotal(valorTotal);
        return this;
    }

    @Override
    public NotaNegociacaoExportavel build() {
        return notaExportavel;
    }
}
```

Por fim, é possível testar a construção do produto, utilizando as configurações definidas na classe **NotaNegociacaoExportavelBuilderImpl**.

```java
// Exemplo de uso
public class Cliente {
    public static void main(String[] args) {
        NotaNegociacaoExportavel nota = new NotaNegociacaoExportavelBuilderImpl()
            .comTipo("Compra")
            .comValorTotal(1500.75)
            .build();
        
        nota.exportar();
    }
}
```