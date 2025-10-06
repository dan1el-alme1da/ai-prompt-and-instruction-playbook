
-----

## Classes record em Java

-----

## O que são Records em Java?

  * **Records** são classes especiais do Java 14+ para representar **dados imutáveis** de forma simples e concisa.
  * Chega de POJOs ou DTOs gigantes\! Records eliminam a **verbosidade**.

-----

## Para que servem?

  * Evitam a **verbosidade** de classes tradicionais (POJOs/DTOs) que só armazenam dados.
  * Adeus getters, equals, hashCode e toString manuais\!

-----

## Magia em uma linha\!

```java
public record Pessoa(String nome, int idade) {}
```

Essa única linha gera automaticamente:

  * Campos `nome` e `idade` (privados e `final`)
  * Construtor
  * Métodos `nome()`, `idade()`, `toString()`, `equals()` e `hashCode()`

-----

## Características Essenciais

  * **Imutáveis**: Valores não mudam após a criação.
  * **Sem herança**: Não podem estender outras classes (mas podem implementar interfaces\!).
  * **Ideais**: Perfeitos para dados simples e seguros.

-----

## Como usar?

```java
Pessoa p = new Pessoa("João", 30);
System.out.println(p.nome()); // João
System.out.println(p.idade()); // 30
```

  * Crie instâncias facilmente.
  * Acesse os dados diretamente pelos métodos gerados.

-----

## Quando Usar Records?

Use **Records** quando precisar de uma estrutura leve **APENAS** para armazenar dados, como:

  * Parâmetros de APIs
  * Objetos de Transferência (**DTOs**)
  * Configurações **imutáveis**

-----

## Record vs. Classe Tradicional: A Batalha\!

  * **Classe Tradicional (POJO):**
      * **VERBOSO**: Muitos métodos boilerplate.
      * Exige código manual para construtor, getters, `equals()`, `hashCode()`, `toString()`.
  * **Record:**
      * **CONCISO**: Uma linha resolve tudo.
      * Gera todo o código boilerplate automaticamente.

-----

## Decisão Final: Quando escolher?

  * **Escolha RECORD:**

      * Quando precisar de classes para carregar **dados imutáveis**.
      * Se **não houver lógica interna complexa**.

  * **Escolha CLASSE TRADICIONAL:**

      * Quando precisar de **mutabilidade**.
      * Para adicionar **lógica complexa** nos métodos.
      * Se houver necessidade de **herança**.