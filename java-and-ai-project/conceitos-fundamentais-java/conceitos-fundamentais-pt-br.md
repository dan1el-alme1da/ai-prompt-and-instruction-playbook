
-----

## Conceitos Essenciais de Java (`java.lang`)

### Qual a diferença entre `==` e `equals()` em objetos String?

O **`==` compara referências na memória**, enquanto **`equals()` compara conteúdo**. Essa diferença é crucial para evitar bugs.

```java
String ninja1 = "Naruto";
String ninja2 = new String("Naruto");

System.out.println(ninja1 == ninja2);       // false
System.out.println(ninja1.equals(ninja2)); // true
```

-----

### Strings em Java são mutáveis ou imutáveis?

**Strings em Java são imutáveis.**

Isso significa que qualquer alteração cria um novo objeto em memória, e o original continua intacto.

Essa característica traz segurança em ambientes multi-thread e permite cache, mas pode impactar performance em concatenações intensas. Nesses casos, o mais adequado é usar **`StringBuilder`** ou **`StringBuffer`**.

```java
String ninja = "Naruto";
ninja.concat(" Uzumaki"); 
System.out.println(ninja); // continua "Naruto"
```

-----

### O que a classe Object representa em Java?

A classe **Object é a raiz da hierarquia de classes em Java**, ou seja, todas as classes herdam dela, direta ou indiretamente. Ela define métodos fundamentais como **`equals()`**, **`hashCode()`**, **`toString()`**, **`clone()`** e **`getClass()`**, que servem como **contrato básico de comportamento para qualquer objeto**.

Esses métodos são muito importantes em operações de comparação, em coleções como **`HashMap`** e **`HashSet`**, e até em depuração.

Outro ponto é que *frameworks* como o Spring ou o Quarkus acabam se apoiando nesses contratos para gerenciar objetos internamente.

-----

### Qual é a diferença entre Throwable, Exception e Error?

Toda exceção ou erro em Java herda de **`Throwable`**. A diferença é que **`Exception`** representa problemas que o programa **pode prever e tratar**, como falha de I/O ou conexão.

Já **`Error`** representa **problemas graves de ambiente**, como falta de memória, e **geralmente não devem ser capturados**.

```java
try {
    throw new Exception("Falha em missão rank B!");
} catch (Exception e) {
    System.out.println("Tratando: " + e.getMessage());
}
```

-----

### Para que serve a classe Enum em Java?

O **`Enum`** em Java é usado quando precisamos representar um **conjunto fixo de constantes**, como dias da semana, meses ou vilas ninjas no universo de Naruto.

Diferente de `String` ou `int`, ele garante **segurança em tempo de compilação** e evita valores inválidos. Além disso, podemos associar atributos e métodos a cada constante, deixando o código mais legível e fácil de manter.

```java
enum Vila { KONOHA, SUNA, KIRI }

Vila vila = Vila.KONOHA;
System.out.println("Naruto é da vila: " + vila);
```

-----

### Como funciona o autoboxing com a classe Integer?

O **autoboxing converte automaticamente tipos primitivos em seus *wrappers***, como `int` para `Integer`. Já o **unboxing faz o inverso**.

Esse recurso facilita quando usamos coleções genéricas, que não aceitam tipos primitivos. Mas, é importante ter cuidado em *loops* grandes, porque o **autoboxing pode criar muitos objetos desnecessários**.

```java
Integer chakra = 100;     // autoboxing
int gasto = chakra - 40;  // unboxing
System.out.println("Chakra restante: " + gasto);
```

-----

### Como a classe Math trata números aleatórios?

**`Math.random()` retorna um número `double` entre $0.0$ e $1.0$**. Para inteiros, basta multiplicar e converter.

```java
int hokage = (int)(Math.random() * 7); // sorteio de Hokage 0 a 6
System.out.println("O Hokage escolhido foi o nº: " + hokage);
```