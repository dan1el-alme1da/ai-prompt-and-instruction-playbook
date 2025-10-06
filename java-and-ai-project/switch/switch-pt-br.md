
-----

## O que é uma switch expression e por que usar?

É um switch que **retorna valor**, sem **fall-through** quando usamos `->`. O código fica mais conciso e seguro que o switch antigo com `:` e `break`. Use quando você quer **calcular um resultado**.

```java
String jutsu = "Rasengan";
int dano = switch (jutsu) {
    case "Rasengan" -> 80;
    case "Chidori" -> 85;
    default -> 50;
};
```

-----

## O que é fall-through e como evitar?

No switch antigo, cada case é só um rótulo de entrada. Isso significa que, se você não colocar **break**, o Java continua executando os próximos casos, mesmo que a condição não combine. Esse comportamento é chamado de **fall-through** (cair para o próximo).

```java
int bonus = switch ("JONIN") {
    case "GENIN", "CHUNIN" -> 10;
    case "JONIN" -> { audit("Bônus alto"); yield 25; }
    default -> 0;
};
```

-----

## O que é "exaustividade" e como os tipos selados ajudam?

Uma **switch expression** deve cobrir todos os casos. Com **tipos selados (sealed)**, o compilador sabe todos os subtipos e pode verificar que você tratou todos, sem precisar de `default`. Se faltar caso, é erro de compilação (ou **MatchException** em **runtime** ao mudar a hierarquia e não recompilar consumidores).

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

## Para que serve when (guarda) nos casos?

É uma **condição extra no próprio case**, enxugando `if` internos. Bom para refinar a lógica de roteamento.

```java
record Ninjutsu(String nome, int poder) { }
String classe(Ninjutsu n) = switch (n) {
    case Ninjutsu nj when nj.poder() >= 80 -> "Nível S";
    case Ninjutsu nj -> "Comum";
};
```

-----

## Posso usar null no switch?

Sim, desde o **pattern matching** para switch, você pode tratar `null` explicitamente com `case null`. Se não houver `case null`, cai em `NullPointerException` (comportamento clássico).

```java
Object alvo = null;
String saida = switch (alvo) {
    case null -> "Alvo desconhecido";
    case String s -> "Alvo: " + s;
    default -> "Tipo não mapeado";
};
```

-----

## Quando usar statement vs expression?

O **switch statement** é usado quando o objetivo é **executar ações**, como registrar logs, chamar métodos ou atualizar variáveis, ou seja, quando o foco é o **efeito colateral**.

Já o **switch expression** serve quando você precisa **calcular e devolver um valor diretamente**, de forma concisa e sem precisar declarar variáveis extras. Nesse formato, se você abrir um bloco `{}`, deve usar a palavra-chave **yield** para retornar o valor.

-----

## O que é pattern matching no switch?

Permite **testar padrões de tipo** e **extrair variáveis** diretamente no case. Em vez de comparar só constantes, você decide pelo tipo/forma do dado. Isso vale para **switch statement** e **expression**.

```java
Object tecnica = "Uzumaki Naruto";
switch (tecnica) {
    case String s -> System.out.println("Shinobi: " + s);
    case int[] cs -> System.out.println("Amostras de chakra: " + cs.length);
    default -> System.out.println("Outro Tipo");
}
```