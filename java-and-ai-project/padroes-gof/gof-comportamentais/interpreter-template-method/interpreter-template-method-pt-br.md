## Padrões de projeto comportamentais Interpreter e Template Method

-----

### Padrão Template Method

#### Introdução ao Template Method

Por meio do padrão Template Method, definimos uma série de departamentos, sendo definidos em superclasses, que especificam a estrutura do algoritmo, porém, a implementação dos passos podem ser modificados por suas subclasses.

Por meio deste padrão, o Template Method permite que implementemos métodos com rotinas definidas em uma superclasse, e esses métodos, por sua vez, fazem chamadas para métodos com rotinas específicas que devem ser implementadas em subclasses. Assim, o Template Method consiste em definir uma estrutura de um algoritmo em um método de uma superclasse, deixando para as subclasses a implementação de uma determinada parte do algoritmo, permitindo que um determinado comportamento (rotina ou tarefa) possa ser modificado sem alterar a estrutura do algoritmo.

Desse modo, o padrão Template Method, permite que definamos a estrutura do algoritmo em uma superclasse, enquanto a implementação dos passos é definida em suas subclasses.

Assim, o Template Method consiste em definir um algoritmo em uma superclasse, em que as subclasses podem redefinir algumas operações desse algoritmo, sem alterar sua estrutura.

#### Intenção do padrão Template Method

Define a estrutura de um algoritmo em uma operação de uma superclasse, deixando a implementação de algumas operações para suas subclasses. O Template Method permite que as subclasses redefinam as partes do algoritmo sem mudar sua estrutura.

#### Problema do padrão Template Method

Suponha que você esteja implementando um sistema que deve gerar alguns relatórios textuais e precisa garantir que todos os relatórios gerados tenham a mesma estrutura, porém, permitindo que a forma de obtenção dos dados e a forma de impressão dos dados sejam modificados.

Imagine, por exemplo, que você tenha que implementar um relatório de veículos e um relatório de acessórios, em que cada um tem uma rotina de obtenção e impressão de dados específica.

O problema requer que possamos garantir a mesma estrutura para ambos os relatórios, mas permitir que a rotina de obtenção dos dados e a rotina de impressão dos dados seja implementada pelas classes filhas.

```java
public class RelatorioDeVeiculos extends Relatorio {
    @Override
    public void geraDadosRelatorio() {
        // Rotina de geração de dados
    }

    @Override
    public void imprimeRelatorio() {
        // Rotina de impressão de dados
    }

    @Override
    public void templateMethod() {
        geraDadosRelatorio();
        imprimeRelatorio();
    }
}
```

Essa solução não é elegante, pois por ser baseada na duplicação de código com estrutura mais ou menos definida, ela abre margem para que os desenvolvedores implementem métodos com estruturas diferentes nas classes filhas, o que viola o requisito inicial do problema. O padrão Template Method permite controlar a duplicação de código e exige um alto reuso e reaproveitamento.

#### Solução do padrão Template Method

A estrutura de solução proposta pelo padrão TemplateMethod está representada no diagrama de classes.

*(Ignorando o diagrama de classes)*

#### Métodos hook (ganchos)

O Template Method define um método em uma classe abstrata que implementa a estrutura do algoritmo e faz chamadas a métodos abstratos (operação primitiva) ou a métodos concretos (métodos hook) na ordem em que devem ser executados. Os métodos hook (ganchos) são métodos que definem rotinas que podem ser implementadas na classe abstrata, porém, é permitido que suas implementações nas subclasses sejam opcionais.

Os métodos hook (ganchos) podem ser:

| Operações Abstratas | Métodos hook (ganchos) |
| :--- | :--- |
| São métodos que definem as subclasses como responsáveis pela sua implementação. | São métodos que definem as subclasses como responsáveis pela sua implementação, mas com o código definido em superclasses (eles têm uma implementação padrão). |

O código abaixo ilustra a hierarquia de classes implementada para dar solução, com o detalhe, dessa vez com um método `templateMethod` na classe `Relatorio`, onde a estrutura do algoritmo é definida. A classe `RelatorioDeVeiculos` e `RelatorioDeAcessorios` estendem `Relatorio` e implementam os métodos `geraDadosRelatorio` e `imprimeRelatorio`, o que resolve o problema inicial, pois garante a mesma estrutura do algoritmo, porém permitindo a implementação dessas operações específicas.

```java
public abstract class Relatorio {
    public void templateMethod() {
        geraDadosRelatorio();
        imprimeRelatorio();
    }

    public abstract void geraDadosRelatorio();
    public abstract void imprimeRelatorio();
}
```

```java
public class RelatorioDeVeiculos extends Relatorio {
    @Override
    public void geraDadosRelatorio() {
        // Implementação de como obter os dados de veículos
    }

    @Override
    public void imprimeRelatorio() {
        // Implementação de como imprimir os dados de veículos
    }
}
```

Desse modo, a rotina de geração e impressão de dados de um relatório é delegada às classes filhas, o que permite que o algoritmo tenha partes variando de acordo com as necessidades. Com o Template Method, garantimos que o desenvolvedor siga a estrutura definida.

#### Consequências e padrões relacionados ao Template Method

O padrão Template Method é muito utilizado na criação de *frameworks* de classes genéricas de classes e métodos, em que o *framework* define a estrutura do algoritmo, e o usuário deve preencher as lacunas, implementando os métodos abstratos, que dão a referência e a experiência dentro do *framework* especificado nas subclasses e nas *hook methods*.

O Template Method é utilizado, inclusive, para implementar parte dos padrões comportamentais, como o Interpreter, Strategy, Factory Method, e outros padrões.

Eis uma amostra de como se relacionam alguns desses específicos com Template Method:

| Template Method | Strategy |
| :--- | :--- |
| **Template Method:** É um padrão de um algoritmo, em que o algoritmo se encontra em uma única hierarquia de herança. | **Strategy:** É um padrão que desejamos criar algoritmo que possa ser substituído, mas que não possuem uma estrutura comum. |

-----

### Padrão Interpreter

#### Introdução ao padrão Interpreter

O padrão Interpreter é um padrão de projeto que define um interpretador de comandos, normalmente baseado em um conjunto de regras gramaticais.

Por meio desse padrão, é possível definir a gramática para a linguagem e, na sequência, apresentar regras gramaticais de como usar o interpretador. Na prática, o padrão Interpreter define uma representação para a gramática de uma linguagem e, ao mesmo tempo, um interpretador de sentenças nessa linguagem.

Ele cria, para cada regra gramatical, que são construídas na forma de texto, alguma regra gramatical, um objeto que representa essa regra. Em seguida, as sentenças são interpretadas ao criar uma árvore de sintaxe, onde cada nó da árvore corresponde a uma regra gramatical dessa linguagem.

Essas classes aplicam a aplicação do padrão de projeto Interpreter, que possibilita definir uma representação gramatical para uma linguagem.

#### Intenção do padrão Interpreter

Dada uma linguagem, define uma representação para sua gramática junto a um interpretador que usa essa representação para interpretar sentenças na linguagem.

#### Problema do padrão Interpreter

Imagine que você esteja trabalhando em um sistema que tem que ser capaz de interpretar e avaliar expressões matemáticas como a adição, subtração, multiplicação e divisão, sendo que cada usuário consegue definir essas expressões usando a linguagem matemática padrão, mas com certas restrições.

O problema requer que possamos construir classes que definam uma estrutura que possa ser usada para interpretar sentenças na linguagem matemática e retornar um resultado.

#### Solução do padrão Interpreter

A estrutura de solução proposta pelo padrão Interpreter está representada no diagrama de classes a seguir.

*(Ignorando o diagrama de classes)*

Diagrama de classes.

Os elementos da gramática da linguagem são definidos por padrões que formam uma árvore sintática abstrata. O padrão Interpreter exige que os elementos dessa linguagem sejam organizados em uma arvore abstrata. Cada nó da árvore sintática define um objeto da linguagem, que pode ser interpretado.

Veja, na imagem a seguir, como a expressão **A + (10 + 15) - 5** pode ser representada por uma estrutura de árvore sintática.

*(Ignorando a imagem da árvore sintática)*

Estrutura de árvore sintática.

#### Consequências e padrões relacionados ao Interpreter

Esse padrão facilita a modificação e a extensão da sua gramática. É mais fácil de implementar a estrutura da sua linguagem com esse padrão, pois ele utiliza o design pattern Composite, o que facilita a adição de novas regras.

| Indicado | Não Indicado |
| :--- | :--- |
| Quando precisamos avaliar sentenças de uma linguagem, como uma sentença em SQL, regex, expressões matemáticas, em que a gramática não seja muito complexa. | O padrão Interpreter não é indicado para linguagens que contêm muitas regras gramaticais. Nesses casos, é melhor utilizar outras soluções, como **compiladores** ou **parser generators**. |

A aplicação do padrão Interpreter define nos participantes do padrão, para servir como um processamento de dados. Nos padrões comportamentais, o Interpreter pode ser considerado como um padrão de interpretação, em que o processamento poderá ser calcular o resultado, processar dados, ou mesmo outras operações.

O padrão Interpreter se preocupa, em essência, com três aspectos:

  * Verificação sintática.
  * Verificação semântica.
  * Obtenção do texto de resultado.

É comum o uso do padrão Composite e do padrão Visitor em conjunto com o Interpreter, em que o Interpreter, utiliza o padrão Composite para a linguagem, enquanto o padrão Visitor pode implementar as operações com Visitor específico.