## Behavioral Design Patterns: Interpreter and Template Method

-----

### Template Method Pattern

#### Introduction to Template Method

Through the **Template Method pattern**, we define a series of steps, which are defined in superclasses, that specify the **structure of the algorithm**. However, the implementation of the steps can be modified by its subclasses.

By means of this pattern, the Template Method allows us to implement methods with routines defined in a superclass, and these methods, in turn, make calls to methods with specific routines that must be implemented in subclasses. Thus, the **Template Method** consists of defining the structure of an algorithm in a method of a superclass, leaving the implementation of a certain part of the algorithm to the subclasses, allowing a certain behavior (routine or task) to be modified without changing the structure of the algorithm.

Therefore, the Template Method pattern allows us to define the algorithm's structure in a superclass, while the implementation of the steps is defined in its subclasses.

Thus, the **Template Method** consists of defining an algorithm in a superclass, where the subclasses can redefine some operations of this algorithm without changing its structure.

#### Intention of the Template Method Pattern

Defines the structure of an algorithm in an operation of a superclass, deferring the implementation of some operations to its subclasses. The Template Method allows subclasses to redefine parts of the algorithm without changing its structure.

#### Problem of the Template Method Pattern

Suppose you are implementing a system that must generate some textual reports and needs to ensure that all generated reports have the **same structure**, while allowing the way the data is obtained and the way the data is printed to be modified.

Imagine, for example, that you have to implement a vehicle report and an accessories report, where each one has a specific data retrieval and printing routine.

The problem requires us to ensure the same structure for both reports, but allow the data retrieval routine and the data printing routine to be implemented by the child classes.

```java
public class RelatorioDeVeiculos extends Relatorio {
    @Override
    public void geraDadosRelatorio() {
        // Data generation routine
    }

    @Override
    public void imprimeRelatorio() {
        // Data printing routine
    }

    @Override
    public void templateMethod() {
        geraDadosRelatorio();
        imprimeRelatorio();
    }
}
```

This solution is not elegant, because since it is based on code duplication with a more or less defined structure, it opens up the possibility for developers to implement methods with different structures in the child classes, which violates the initial requirement of the problem. The Template Method pattern allows you to control code duplication and demands high reuse and refactoring.

#### Solution of the Template Method Pattern

The solution structure proposed by the Template Method pattern is represented in the class diagram.

*(Ignoring the class diagram)*

Class diagram.

#### Hook Methods

The **Template Method** defines a method in an abstract class that implements the structure of the algorithm and makes calls to abstract methods (**primitive operations**) or concrete methods (**hook methods**) in the order in which they should be executed. **Hook methods** are methods that define routines that can be implemented in the abstract class, but their implementations in the subclasses are allowed to be optional.

Hook methods can be:

| Abstract Operations | Hook Methods |
| :--- | :--- |
| These are methods that define subclasses as responsible for their implementation. | These are methods that define subclasses as responsible for their implementation, but with the code defined in superclasses (they have a default implementation). |

The code below illustrates the class hierarchy implemented to provide a solution, with the detail, this time with a `templateMethod` method in the `Relatorio` class, where the algorithm structure is defined. The `RelatorioDeVeiculos` and `RelatorioDeAcessorios` classes extend `Relatorio` and implement the `geraDadosRelatorio` and `imprimeRelatorio` methods, which solves the initial problem, as it guarantees the same algorithm structure, but allows the implementation of these specific operations.

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
        // Implementation of how to obtain vehicle data
    }

    @Override
    public void imprimeRelatorio() {
        // Implementation of how to print vehicle data
    }
}
```

In this way, the data generation and printing routine of a report is delegated to the child classes, which allows parts of the algorithm to vary according to needs. With the Template Method, we ensure that the developer follows the defined structure.

#### Consequences and Patterns Related to Template Method

The **Template Method** pattern is widely used in the creation of **frameworks** of generic classes and methods, where the *framework* defines the structure of the algorithm, and the user must fill in the gaps, implementing the abstract methods, which give the reference and the experience within the specified *framework* in the subclasses and in the *hook methods*.

The Template Method is even used to implement part of the behavioral patterns, such as **Interpreter**, **Strategy**, **Factory Method**, and other patterns.

Here is a sample of how some of these specifics relate to Template Method:

| Template Method | Strategy |
| :--- | :--- |
| **Template Method:** It is a pattern of an algorithm, in which the algorithm is found in a single inheritance hierarchy. | **Strategy:** It is a pattern where we want to create an algorithm that can be replaced, but which does not have a common structure. |

-----

### Interpreter Pattern

#### Introduction to the Interpreter Pattern

The **Interpreter pattern** is a design pattern that defines a command interpreter, usually based on a set of grammatical rules.

Through this pattern, it is possible to define the grammar for the language and, subsequently, present grammatical rules on how to use the interpreter. In practice, the **Interpreter pattern** defines a representation for the grammar of a language and, at the same time, an interpreter of sentences in that language.

For each grammatical rule, which are constructed in the form of text, it creates a grammatical rule, an object that represents that rule. Subsequently, the sentences are interpreted by creating a **syntax tree**, where each node of the tree corresponds to a grammatical rule of that language.

These classes apply the implementation of the **Interpreter design pattern**, which makes it possible to define a grammatical representation for a language.

#### Intention of the Interpreter Pattern

Given a language, it defines a representation for its grammar together with an interpreter that uses this representation to interpret sentences in the language.

#### Problem of the Interpreter Pattern

Imagine that you are working on a system that has to be able to interpret and evaluate mathematical expressions such as addition, subtraction, multiplication, and division, where each user can define these expressions using the standard mathematical language, but with certain restrictions.

The problem requires that we can build classes that define a structure that can be used to interpret sentences in the mathematical language and return a result.

#### Solution of the Interpreter Pattern

The structure of the solution proposed by the **Interpreter pattern** is represented in the class diagram below.

*(Ignoring the class diagram)*

Class diagram.

The elements of the language's grammar are defined by patterns that form an **abstract syntax tree**. The Interpreter pattern requires that the elements of this language be organized into an abstract tree. Each node of the syntax tree defines a language object, which can be interpreted.

See, in the image below, how the expression **A + (10 + 15) - 5** can be represented by a syntax tree structure.

*(Ignoring the syntax tree image)*

Syntax tree structure.

#### Consequences and Patterns Related to Interpreter

This pattern facilitates the modification and extension of your grammar. It is easier to implement the structure of your language with this pattern, as it uses the **Composite** design pattern, which facilitates the addition of new rules.

| Indicated | Not Indicated |
| :--- | :--- |
| When we need to evaluate sentences of a language, such as a sentence in SQL, regex, mathematical expressions, where the grammar is not very complex. | The Interpreter pattern is not suitable for languages that contain many grammatical rules. In these cases, it is better to use other solutions, such as **compilers** or **parser generators**. |

The implementation of the **Interpreter pattern** defines the participants in the pattern, to serve as data processing. In behavioral patterns, the Interpreter can be considered an interpretation pattern, where the processing may be calculating the result, processing data, or even other operations.

The Interpreter pattern is essentially concerned with three aspects:

  * Syntactic verification.
  * Semantic verification.
  * Obtaining the result text.

It is common to use the **Composite pattern** and the **Visitor pattern** in conjunction with the Interpreter, where the Interpreter uses the Composite pattern for the language, while the Visitor pattern can implement the operations with a specific Visitor.