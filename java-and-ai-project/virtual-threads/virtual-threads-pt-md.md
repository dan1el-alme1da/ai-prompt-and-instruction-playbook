
-----


**O que são Virtual Threads?**

Virtual Threads são uma novidade trazida pelo **Project Loom**, que virou estável no **Java 21**. Elas são threads muito mais leves do que as tradicionais, criadas e gerenciadas pela **JVM**.

A ideia é simples: você pode criar **milhares de virtual threads** ( claro, respeitando os recursos do seu sistema) sem se preocupar em travar, porque elas **não mapeiam 1:1** para threads do sistema operacional.

-----

**Qual a diferença entre Virtual Thread e Thread tradicional?**

Uma **Platform Thread** (a tradicional) é um "wrapper" em cima de uma thread real do **SO**. Criar muitas dessas consome bastante **memória e recursos**.

Já uma **Virtual Thread** é agendada pela **JVM** em cima de um pequeno **pool de platform threads**. Assim, você pode ter **100 mil virtual threads** rodando em cima de apenas algumas threads nativas, sem perder desempenho em cenários de **I/O**.

-----

**Como criar uma Virtual Thread no Java?**

Bem simples, usando a nova API `Thread.ofVirtual()`. Exemplo:

```java
Thread.startVirtualThread(() -> {
    System.out.println("Rodando em virtual thread!");
});
```

-----

**Pode dar um exemplo de como usar Virtual Threads em um ExecutorService?**

A forma mais prática é usar o novo executor:

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
executor.submit(() -> System.out.println("Executando no executor!"));
}
```

Esse executor cria uma **virtual thread para cada tarefa submetida**. É indicado para **servidores web** ou **processadores de fila**.

-----


**O que acontece quando uma Virtual Thread bloqueia?**

Aí que tem um ponto bem interessante, quando uma virtual thread faz **I/O bloqueante**, a **JVM suspende essa thread** e **libera a thread nativa subjacente** para que outra virtual thread use.

Resumindo, você pode ter **milhares de threads** esperando resposta de **I/O** sem ocupar **recursos de SO**. Isso reduz muito a complexidade de lidar com concorrência.

-----


**Virtual Threads substituem as threads tradicionais, correto?**

**Não**. Elas são pensadas para resolver um problema específico: **concorrência em tarefas de I/O** (como chamadas HTTP, leitura de arquivos, banco de dados).

Para tarefas pesadas de **CPU bound** (cálculos intensos, compressão, criptografia), **não faz tanta diferença usar virtual threads**.

-----

## Documento Coeso: Virtual Threads no Java

-----

## Introdução ao Conceito de Virtual Threads

**Virtual Threads** são uma importante novidade no ecossistema Java, introduzidas pelo **Project Loom** e que se tornaram um recurso estável no **Java 21**.

Elas representam um tipo de *thread* muito mais **leve** do que as tradicionais. A principal ideia é permitir que o desenvolvedor crie **milhares de virtual threads** sem o receio de esgotar os recursos do sistema, pois elas são **criadas e gerenciadas pela própria JVM** e **não mapeiam 1:1** para as *threads* do sistema operacional (SO).

-----

## Diferença Fundamental: Virtual Thread vs. Platform Thread

É crucial entender como uma Virtual Thread se diferencia de uma Platform Thread, a thread tradicional:

| Característica | Platform Thread (Tradicional) | Virtual Thread |
| :--- | :--- | :--- |
| **Mapeamento** | É um "wrapper" em cima de uma **thread real do SO**. | É agendada pela **JVM** em cima de um pequeno **pool de platform threads**. |
| **Consumo** | Criar muitas consome bastante **memória e recursos**. | Permite ter **100 mil virtual threads** rodando em cima de poucas threads nativas. |
| **Cenário** | Indicada para tarefas de **CPU bound**. | Ideal para cenários de **I/O bloqueante**. |

### O Benefício em Cenários de I/O

O grande ganho é em cenários de **I/O bloqueante** (espera por rede, disco, banco de dados). Quando uma Virtual Thread **bloqueia** ao fazer uma operação de I/O:

1.  A **JVM suspende essa thread virtual**.
2.  A **thread nativa subjacente é liberada** para que outra virtual thread possa usá-la.

Isso significa que você pode ter **milhares de threads** esperando uma resposta de I/O **sem ocupar recursos do SO**, o que simplifica drasticamente a concorrência.

-----

## Casos de Uso e Aplicações

**Virtual Threads não substituem** as threads tradicionais. Elas são projetadas especificamente para melhorar a **concorrência em tarefas de I/O**, tais como:

  * Chamadas **HTTP**.
  * Leitura de **arquivos**.
  * Consultas a **banco de dados**.

Para tarefas pesadas de **CPU bound** (cálculos intensos, criptografia), o uso de *virtual threads* **não oferece uma grande vantagem**.

-----

## Implementação: Como Criar e Usar

### 1\. Criando uma Virtual Thread Simples

A criação é feita de forma simples, usando a nova API `Thread.ofVirtual()` ou o atalho estático:

```java
Thread.startVirtualThread(() -> {
    System.out.println("Rodando em virtual thread!");
});
```

### 2\. Usando com ExecutorService

A forma mais prática de integrar Virtual Threads em arquiteturas baseadas em `ExecutorService` é através do novo *executor*:

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
executor.submit(() -> System.out.println("Executando no executor!"));
}
```

Este executor tem o comportamento de criar uma **virtual thread para cada tarefa submetida**. Por essa razão, ele é altamente **indicado para servidores web** ou **processadores de fila**, ambientes com alta necessidade de concorrência baseada em I/O.