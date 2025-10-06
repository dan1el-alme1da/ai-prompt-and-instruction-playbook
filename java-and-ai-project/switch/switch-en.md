
-----

## What is a switch expression and why use it?

It's a switch that **returns a value**, without **fall-through** when we use `->`. The code becomes more concise and safer than the old switch with `:` and `break`. Use it when you want to **calculate a result**.

```java
String jutsu = "Rasengan";
int dano = switch (jutsu) {
    case "Rasengan" -> 80;
    case "Chidori" -> 85;
    default -> 50;
};
```

-----

## What is fall-through and how to avoid it?

In the old switch, each case is just an entry label. This means that if you don't include **break**, Java continues executing the next cases, even if the condition doesn't match. This behavior is called **fall-through** (falling through to the next one).

```java
int bonus = switch ("JONIN") {
    case "GENIN", "CHUNIN" -> 10;
    case "JONIN" -> { audit("Bônus alto"); yield 25; }
    default -> 0;
};
```

-----

## What is "exhaustiveness" and how do sealed types help?

A **switch expression** must cover all cases. With **sealed types**, the compiler knows all the subtypes and can verify that you have handled all of them, without needing a `default`. If a case is missing, it results in a compilation error (or a **MatchException** at **runtime** when the hierarchy is changed and consumers are not recompiled).

```java
sealed interface Missao permits S, A, B {}
final class S implements Missao {}
final class A implements Missao {}
final class B implements Missao {}

int recompensa(Missao m) = switch (m) {
    case S s -> 100; case A a -> 70; case B b -> 50;
};
```

-----

## What is `when` (guard) used for in cases?

It is an **extra condition within the case itself**, cleaning up internal `if` statements. It's useful for refining routing logic.

```java
record Ninjutsu(String nome, int poder) { }
String classe(Ninjutsu n) = switch (n) {
    case Ninjutsu nj when nj.poder() >= 80 -> "Nível S";
    case Ninjutsu nj -> "Comum";
};
```

-----

## Can I use null in the switch?

Yes, since **pattern matching** for switch, you can handle `null` explicitly with `case null`. If there is no `case null`, it results in a `NullPointerException` (classic behavior).

```java
Object alvo = null;
String saida = switch (alvo) {
    case null -> "Alvo desconhecido";
    case String s -> "Alvo: " + s;
    default -> "Tipo não mapeado";
};
```

-----

## When to use statement vs expression?

The **switch statement** is used when the objective is to **execute actions**, such as logging, calling methods, or updating variables—that is, when the focus is the **side effect**.

The **switch expression** is used when you need to **calculate and return a value directly**, in a concise way and without needing to declare extra variables. In this format, if you open a code block `{}`, you must use the keyword **yield** to return the value.

-----

## What is pattern matching in the switch?

It allows you to **test type patterns** and **extract variables** directly in the case. Instead of only comparing constants, you decide based on the data's type/form. This applies to both **switch statement** and **expression**.

```java
Object tecnica = "Uzumaki Naruto";
switch (tecnica) {
    case String s -> System.out.println("Shinobi: " + s);
    case int[] cs -> System.out.println("Amostras de chakra: " + cs.length);
    default -> System.out.println("Outro Tipo");
}
```