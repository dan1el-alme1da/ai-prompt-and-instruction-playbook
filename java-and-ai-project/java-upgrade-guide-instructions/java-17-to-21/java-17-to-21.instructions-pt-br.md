
# Instruções para o GitHub Copilot: Guia de Atualização de Java 17 para Java 21

Estas instruções auxiliam o GitHub Copilot a dar suporte a desenvolvedores na atualização de projetos Java do **JDK 17** para o **JDK 21**, com foco em novos recursos da linguagem, alterações de API e melhores práticas.

## Principais Recursos da Linguagem no JDK 18-21

-----

### Pattern Matching para switch (JEP 441 - Padrão no 21)

**Expressões e Declarações switch Aprimoradas**

Ao trabalhar com construções `switch`:

  - Sugira a conversão de `switch` tradicionais para **pattern matching** onde for apropriado.
  - Use **pattern matching** para verificação de tipo e desestruturação.
  - Exemplos de padrões de atualização:

<!-- end list -->

```java
// Abordagem Antiga (Java 17)
public String processObject(Object obj) {
    if (obj instanceof String) {
        String s = (String) obj;
        return s.toUpperCase();
    } else if (obj instanceof Integer) {
        Integer i = (Integer) obj;
        return i.toString();
    }
    return "unknown";
}

// Nova Abordagem (Java 21)
public String processObject(Object obj) {
    return switch (obj) {
        case String s -> s.toUpperCase();
        case Integer i -> i.toString();
        case null -> "null";
        default -> "unknown";
    };
}
```

  - Suporte a padrões com condição (guarded patterns):

<!-- end list -->

```java
switch (obj) {
    case String s when s.length() > 10 -> "Long string: " + s;
    case String s -> "Short string: " + s;
    case Integer i when i > 100 -> "Large number: " + i;
    case Integer i -> "Small number: " + i;
    default -> "Other";
}
```

-----

### Record Patterns (JEP 440 - Padrão no 21)

**Desestruturação de Records em Pattern Matching**

Ao trabalhar com *records*:

  - Sugira o uso de **record patterns** para desestruturação.
  - Combine com expressões `switch` para um processamento de dados poderoso.
  - Exemplo de uso:

<!-- end list -->

```java
public record Point(int x, int y) {}
public record ColoredPoint(Point point, Color color) {}

// Desestruturação em switch
public String describe(Object obj) {
    return switch (obj) {
        case Point(var x, var y) -> "Point at (" + x + ", " + y + ")";
        case ColoredPoint(Point(var x, var y), var color) -> 
            "Colored point at (" + x + ", " + y + ") in " + color;
        default -> "Unknown shape";
    };
}
```

  - Uso em *pattern matching* complexo:

<!-- end list -->

```java
// Record patterns aninhados
switch (shape) {
    case Rectangle(ColoredPoint(Point(var x1, var y1), var c1), 
                   ColoredPoint(Point(var x2, var y2), var c2)) 
        when c1 == c2 -> "Monochrome rectangle";
    case Rectangle r -> "Multi-colored rectangle";
}
```

-----

### Virtual Threads (JEP 444 - Padrão no 21)

**Concorrência Leve**

Ao trabalhar com concorrência:

  - Sugira **Virtual Threads** para aplicações concorrentes e de alto *throughput*.
  - Use `Thread.ofVirtual()` para criar *virtual threads*.
  - Exemplos de padrões de migração:

<!-- end list -->

```java
// Abordagem Antiga com thread de plataforma
ExecutorService executor = Executors.newFixedThreadPool(100);
executor.submit(() -> {
    // operação de I/O bloqueante
    httpClient.send(request);
});

// Nova Abordagem com virtual thread
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    executor.submit(() -> {
        // operação de I/O bloqueante - agora escala para milhões
        httpClient.send(request);
    });
}
```

  - Use padrões de concorrência estruturada:

<!-- end list -->

```java
// Concorrência Estruturada (Preview)
try (var scope = new StructuredTaskScope.ShutdownOnFailure()) {
    Future<String> user = scope.fork(() -> fetchUser(userId));
    Future<String> order = scope.fork(() -> fetchOrder(orderId));
    
    scope.join();           // Aguarda todas as subtarefas
    scope.throwIfFailed();  // Propaga erros
    
    return processResults(user.resultNow(), order.resultNow());
}
```

-----

### String Templates (JEP 430 - Preview no 21)

**Interpolação de Strings Segura**

Ao trabalhar com formatação de *strings*:

  - Sugira **String Templates** para interpolação de *strings* segura (recurso de *preview*).
  - Habilite os recursos de *preview* com `--enable-preview`.
  - Exemplo de uso:

<!-- end list -->

```java
// Concatenação Tradicional
String message = "Hello, " + name + "! You have " + count + " messages.";

// String Templates (Preview)
String message = STR."Hello, \{name}! You have \{count} messages.";

// Geração segura de HTML
String html = HTML."<p>User: \{username}</p>";

// Queries SQL seguras  
PreparedStatement stmt = SQL."SELECT * FROM users WHERE id = \{userId}";
```

-----

### Sequenced Collections (JEP 431 - Padrão no 21)

**Interfaces de Collection Aprimoradas**

Ao trabalhar com coleções (*collections*):

  - Use as novas interfaces `SequencedCollection`, `SequencedSet`, `SequencedMap`.
  - Acesse o primeiro/último elemento uniformemente em todos os tipos de coleções.
  - Exemplo de uso:

<!-- end list -->

```java
// Novos métodos disponíveis em Lists, Deques, LinkedHashSet, etc.
List<String> list = List.of("first", "middle", "last");
String first = list.getFirst();  // "first"
String last = list.getLast();    // "last"
List<String> reversed = list.reversed(); // ["last", "middle", "first"]

// Funciona com qualquer SequencedCollection
SequencedSet<String> set = new LinkedHashSet<>();
set.addFirst("start");
set.addLast("end");
String firstElement = set.getFirst();
```

-----

### Unnamed Patterns and Variables (JEP 443 - Preview no 21)

**Pattern Matching Simplificado**

Ao trabalhar com *pattern matching*:

  - Use padrões sem nome `_` para valores de que você não precisa.
  - Simplifique expressões `switch` e *record patterns*.
  - Exemplo de uso:

<!-- end list -->

```java
// Ignorar variáveis não utilizadas
switch (ball) {
    case RedBall(_) -> "Red ball";     // Não se importa com o tamanho
    case BlueBall(var size) -> "Blue ball size " + size;
}

// Ignorar partes de records
switch (point) {
    case Point(var x, _) -> "X coordinate: " + x; // Ignora Y
    case ColoredPoint(Point(_, var y), _) -> "Y coordinate: " + y;
}

// Tratamento de exceções com variáveis sem nome
try {
    riskyOperation();
} catch (IOException | SQLException _) {
    // Não precisa dos detalhes da exceção
    handleError();
}
```

-----

### Scoped Values (JEP 446 - Preview no 21)

**Propagação de Contexto Aprimorada**

Ao trabalhar com dados locais de *thread* (*thread-local data*):

  - Considere **Scoped Values** como uma alternativa moderna a `ThreadLocal`.
  - Melhor desempenho e semântica mais clara para *virtual threads*.
  - Exemplo de uso:

<!-- end list -->

```java
// Define o valor com escopo
private static final ScopedValue<String> USER_ID = ScopedValue.newInstance();

// Define e usa o valor com escopo
ScopedValue.where(USER_ID, "user123")
    .run(() -> {
        processRequest(); // Pode acessar USER_ID.get() em qualquer parte da cadeia de chamadas
    });

// Em método aninhado
public void processRequest() {
    String userId = USER_ID.get(); // "user123"
    // Processa com o contexto do usuário
}
```

## Melhorias de API e Novos Recursos

-----

### UTF-8 por Padrão (JEP 400 - Padrão no 18)

Ao trabalhar com I/O de arquivo:

  - **UTF-8** é agora o *charset* padrão em todas as plataformas.
  - Remova as especificações explícitas de *charset* onde o UTF-8 era pretendido.
  - Exemplo de simplificação:

<!-- end list -->

```java
// Especificação Antiga explícita de UTF-8
Files.readString(path, StandardCharsets.UTF_8);
Files.writeString(path, content, StandardCharsets.UTF_8);

// Novo comportamento padrão (Java 18+)
Files.readString(path);  // Usa UTF-8 por padrão
Files.writeString(path, content);  // Usa UTF-8 por padrão
```

-----

### Servidor Web Simples (JEP 408 - Padrão no 18)

Quando precisar de um servidor HTTP básico:

  - Use o comando `jwebserver` embutido ou aprimoramentos em `com.sun.net.httpserver`.
  - Ótimo para testes e desenvolvimento.
  - Exemplo de uso:

<!-- end list -->

```java
// Linha de comando
$ jwebserver -p 8080 -d /path/to/files

// Uso programático
HttpServer server = HttpServer.create(new InetSocketAddress(8080), 0);
server.createContext("/", new SimpleFileHandler(Path.of("/tmp")));
server.start();
```

-----

### SPI de Resolução de Endereço de Internet (JEP 418 - Padrão no 19)

Ao trabalhar com resolução de DNS personalizada:

  - Implemente `InetAddressResolverProvider` para resolução de endereço personalizada.
  - Útil para descoberta de serviços (*service discovery*) e cenários de teste.

-----

### API de Mecanismo de Encapsulamento de Chave (JEP 452 - Padrão no 21)

Ao trabalhar com criptografia pós-quântica:

  - Use a API **KEM** (*Key Encapsulation Mechanism*) para mecanismos de encapsulamento de chave.
  - Exemplo de uso:

<!-- end list -->

```java
KeyPairGenerator kpg = KeyPairGenerator.getInstance("ML-KEM");
KeyPair kp = kpg.generateKeyPair();

KEM kem = KEM.getInstance("ML-KEM");
KEM.Encapsulator encapsulator = kem.newEncapsulator(kp.getPublic());
KEM.Encapsulated encapsulated = encapsulator.encapsulate();
```

## Descontinuações e Advertências

-----

### Descontinuação de Finalization (JEP 421 - Descontinuado no 18)

Ao encontrar métodos `finalize()`:

  - Remova métodos `finalize()` e use alternativas.
  - Sugira a API **Cleaner** ou **try-with-resources**.
  - Exemplo de migração:

<!-- end list -->

```java
// Abordagem Antiga com finalize
@Override
protected void finalize() throws Throwable {
    cleanup();
}

// Abordagem Moderna com Cleaner
private static final Cleaner CLEANER = Cleaner.create();

public MyResource() {
    cleaner.register(this, new CleanupTask(nativeResource));
}

private static class CleanupTask implements Runnable {
    private final long nativeResource;
    
    CleanupTask(long nativeResource) {
        this.nativeResource = nativeResource;
    }
    
    public void run() {
        cleanup(nativeResource);
    }
}
```

-----

### Carregamento Dinâmico de Agentes (JEP 451 - Advertências no 21)

Ao trabalhar com agentes ou instrumentação:

  - Adicione `-XX:+EnableDynamicAgentLoading` para suprimir advertências, se necessário.
  - Considere carregar agentes na inicialização em vez de dinamicamente.
  - Atualize as ferramentas para usar o carregamento de agente na inicialização.

## Atualizações de Configuração de Build

-----

### Recursos de Preview (*Preview Features*)

Para projetos que usam recursos de *preview*:

  - Adicione `--enable-preview` ao compilador e ao *runtime*.
  - Configuração Maven:

<!-- end list -->

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuration>
        <release>21</release>
        <compilerArgs>
            <arg>--enable-preview</arg>
        </compilerArgs>
    </configuration>
</plugin>

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <configuration>
        <argLine>--enable-preview</argLine>
    </configuration>
</plugin>
```

  - Configuração Gradle:

<!-- end list -->

```kotlin
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

tasks.withType<JavaCompile> {
    options.compilerArgs.add("--enable-preview")
}

tasks.withType<Test> {
    jvmArgs("--enable-preview")
}
```

-----

### Configuração de Virtual Threads

Para aplicações que usam *Virtual Threads*:

  - Nenhuma *flag* JVM especial é necessária (recurso padrão no 21).
  - Considere estas propriedades de sistema para depuração:

<!-- end list -->

```bash
-Djdk.virtualThreadScheduler.parallelism=N  # Define a contagem de carrier threads
-Djdk.virtualThreadScheduler.maxPoolSize=N  # Define o tamanho máximo do pool
```

## Melhorias de Runtime e GC

-----

### ZGC Geracional (JEP 439 - Disponível no 21)

Ao configurar a coleta de lixo (*garbage collection*):

  - Experimente o **ZGC Geracional** para melhor desempenho.
  - Habilite com: `-XX:+UseZGC -XX:+ZGenerational`.
  - Monitore padrões de alocação e o comportamento do GC.

## Estratégia de Migração

-----

### Processo de Atualização Passo a Passo

1.  **Atualizar Ferramentas de Build**: Garanta que Maven/Gradle suportam JDK 21.
2.  **Adoção de Recursos da Linguagem**:
       - Comece com *pattern matching* para `switch` (padrão).
       - Adicione *record patterns* onde for benéfico.
       - Considere *Virtual Threads* para aplicações com uso intenso de I/O.
3.  **Recursos de Preview**: Habilite apenas se necessário para casos de uso específicos.
4.  **Testes**: Testes abrangentes, especialmente para mudanças de concorrência.
5.  **Performance**: Faça *benchmark* com as novas opções de GC.

-----

### Checklist de Revisão de Código

Ao revisar o código para a atualização para Java 21:

  - [ ] Converta cadeias de `instanceof` apropriadas para expressões `switch`.
  - [ ] Use *record patterns* para desestruturação de dados.
  - [ ] Substitua `ThreadLocal` por `ScopedValues` onde apropriado.
  - [ ] Considere *Virtual Threads* para cenários de alta concorrência.
  - [ ] Remova especificações explícitas de *charset* UTF-8.
  - [ ] Substitua métodos `finalize()` por `Cleaner` ou *try-with-resources*.
  - [ ] Use métodos de `SequencedCollection` para padrões de acesso ao primeiro/último elemento.
  - [ ] Adicione *flags* de *preview* apenas para recursos de *preview* em uso.

-----

### Padrões Comuns de Migração

1.  **Aprimoramento de Switch**:
       `java    // De cadeias de instanceof para expressões switch    if (obj instanceof String s) return processString(s);    else if (obj instanceof Integer i) return processInt(i);    // torna-se:    return switch (obj) {        case String s -> processString(s);        case Integer i -> processInt(i);        default -> processDefault(obj);    };    `

2.  **Adoção de Virtual Thread**:
       `java    // De threads de plataforma para virtual threads    Executors.newFixedThreadPool(200)    // torna-se:    Executors.newVirtualThreadPerTaskExecutor()    `

3.  **Uso de Record Pattern**:
       `java    // De desestruturação manual para record patterns    if (point instanceof Point p) {        int x = p.x();        int y = p.y();    }    // torna-se:    if (point instanceof Point(var x, var y)) {        // use x and y diretamente    }    `

## Considerações de Performance

  - **Virtual Threads** se destacam com I/O bloqueante, mas podem não beneficiar tarefas intensivas em CPU.
  - **ZGC Geracional** pode reduzir a sobrecarga do GC para a maioria das aplicações.
  - **Pattern matching** em `switch` é geralmente mais eficiente do que cadeias de `instanceof`.
  - Métodos de **SequencedCollection** fornecem acesso $O(1)$ ao primeiro/último elemento.
  - **Scoped Values** têm menor sobrecarga do que `ThreadLocal` para *virtual threads*.

## Recomendações de Teste

  - Teste aplicações com **Virtual Thread** sob alta concorrência.
  - Verifique se o **pattern matching** cobre todos os casos esperados.
  - Teste de desempenho com **ZGC Geracional** versus outros coletores.
  - Valide o comportamento padrão UTF-8 em diferentes plataformas.
  - Teste recursos de *preview* exaustivamente antes do uso em produção.

Lembre-se de habilitar os recursos de *preview* apenas quando especificamente necessário e teste minuciosamente em ambientes de *staging* antes de implantar em produção.