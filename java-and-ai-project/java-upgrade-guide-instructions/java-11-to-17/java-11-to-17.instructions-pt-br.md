
# GitHub Copilot Instructions: Guia de Atualização de Java 11 para Java 17

## Contexto do Projeto

Este guia fornece instruções abrangentes para o GitHub Copilot sobre a atualização de projetos Java do **JDK 11** para o **JDK 17**, cobrindo os principais recursos da linguagem, alterações de API e padrões de migração baseados nos **47 JEPs** integrados entre estas versões.

-----

## Recursos da Linguagem e Alterações de API

### JEP 395: Records (Java 16)

**Padrão de Migração**: Converter classes de dados em *records*

```java
// Old: Traditional data class
public class Person {
    private final String name;
    private final int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public String name() { return name; }
    public int age() { return age; }
    
    @Override
    public boolean equals(Object obj) { /* boilerplate */ }
    @Override
    public int hashCode() { /* boilerplate */ }
    @Override
    public String toString() { /* boilerplate */ }
}

// New: Record (Java 16+)
public record Person(String name, int age) {
    // Compact constructor for validation
    public Person {
        if (age < 0) throw new IllegalArgumentException("Age cannot be negative");
    }
    
    // Custom methods can be added
    public boolean isAdult() {
        return age >= 18;
    }
}
```

-----

### JEP 409: Sealed Classes (Classes Seladas) (Java 17)

**Padrão de Migração**: Usar classes seladas para herança restrita

```java
// New: Sealed class hierarchy
public sealed class Shape 
    permits Circle, Rectangle, Triangle {
    
    public abstract double area();
}

public final class Circle extends Shape {
    private final double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double area() {
        return Math.PI * radius * radius;
    }
}

public final class Rectangle extends Shape {
    private final double width, height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double area() {
        return width * height;
    }
}

public non-sealed class Triangle extends Shape {
    // Non-sealed allows further inheritance
    private final double base, height;
    
    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }
    
    @Override
    public double area() {
        return 0.5 * base * height;
    }
}
```

-----

### JEP 394: Pattern Matching for instanceof (Java 16)

**Padrão de Migração**: Simplificar verificações `instanceof`

```java
// Old: Traditional instanceof with casting
public String processObject(Object obj) {
    if (obj instanceof String) {
        String str = (String) obj;
        return str.toUpperCase();
    } else if (obj instanceof Integer) {
        Integer num = (Integer) obj;
        return "Number: " + num;
    } else if (obj instanceof List<?>) {
        List<?> list = (List<?>) obj;
        return "List with " + list.size() + " elements";
    }
    return "Unknown type";
}

// New: Pattern matching for instanceof (Java 16+)
public String processObject(Object obj) {
    if (obj instanceof String str) {
        return str.toUpperCase();
    } else if (obj instanceof Integer num) {
        return "Number: " + num;
    } else if (obj instanceof List<?> list) {
        return "List with " + list.size() + " elements";
    }
    return "Unknown type";
}

// Works great with sealed classes
public String describeShape(Shape shape) {
    if (shape instanceof Circle circle) {
        return "Circle with radius " + circle.radius();
    } else if (shape instanceof Rectangle rect) {
        return "Rectangle " + rect.width() + "x" + rect.height();
    } else if (shape instanceof Triangle triangle) {
        return "Triangle with base " + triangle.base();
    }
    return "Unknown shape";
}
```

-----

### JEP 361: Switch Expressions (Expressões Switch) (Java 14)

**Padrão de Migração**: Converter instruções *switch* em expressões

```java
// Old: Traditional switch statement
public String getDayType(DayOfWeek day) {
    String result;
    switch (day) {
        case MONDAY:
        case TUESDAY:
        case WEDNESDAY:
        case THURSDAY:
        case FRIDAY:
            result = "Workday";
            break;
        case SATURDAY:
        case SUNDAY:
            result = "Weekend";
            break;
        default:
            throw new IllegalArgumentException("Unknown day: " + day);
    }
    return result;
}

// New: Switch expression (Java 14+)
public String getDayType(DayOfWeek day) {
    return switch (day) {
        case MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY -> "Workday";
        case SATURDAY, SUNDAY -> "Weekend";
    };
}

// With yield for complex logic
public int calculateScore(Grade grade) {
    return switch (grade) {
        case A -> 100;
        case B -> 85;
        case C -> 70;
        case D -> {
            System.out.println("Consider improvement");
            yield 55;
        }
        case F -> {
            System.out.println("Needs retake");
            yield 0;
        }
    };
}
```

-----

### JEP 406: Pattern Matching for switch (Preview no Java 17)

**Padrão de Migração**: *Switch* aprimorado com padrões (Recurso de *Preview*)

```java
// Requires --enable-preview flag
public String formatValue(Object obj) {
    return switch (obj) {
        case String s -> "String: " + s;
        case Integer i -> "Integer: " + i;
        case null -> "null value";
        case default -> "Unknown: " + obj.getClass().getSimpleName();
    };
}

// With guarded patterns
public String categorizeNumber(Object obj) {
    return switch (obj) {
        case Integer i when i < 0 -> "Negative integer";
        case Integer i when i == 0 -> "Zero";
        case Integer i when i > 0 -> "Positive integer";
        case Double d when d.isNaN() -> "Not a number";
        case Number n -> "Other number: " + n;
        case null -> "null";
        case default -> "Not a number";
    };
}
```

-----

### JEP 378: Text Blocks (Blocos de Texto) (Java 15)

**Padrão de Migração**: Usar blocos de texto para *strings* multi-linha

```java
// Old: Concatenated strings
String html = "<html>\n" +
              "  <body>\n" +
              "    <h1>Hello World</h1>\n" +
              "    <p>Welcome to Java 17!</p>\n" +
              "  </body>\n" +
              "</html>";

String sql = "SELECT p.id, p.name, p.email, " +
             "       a.street, a.city, a.state " +
             "FROM person p " +
             "JOIN address a ON p.address_id = a.id " +
             "WHERE p.active = true " +
             "ORDER BY p.name";

// New: Text blocks (Java 15+)
String html = """
              <html>
                <body>
                  <h1>Hello World</h1>
                  <p>Welcome to Java 17!</p>
                </body>
              </html>
              """;

String sql = """
             SELECT p.id, p.name, p.email,
                    a.street, a.city, a.state
             FROM person p
             JOIN address a ON p.address_id = a.id
             WHERE p.active = true
             ORDER BY p.name
             """;

// With string interpolation methods
String json = """
              {
                "name": "%s",
                "age": %d,
                "city": "%s"
              }
              """.formatted(name, age, city);
```

-----

### JEP 358: Helpful NullPointerExceptions (Java 14)

**Orientação de Migração**: Melhor depuração de *NPEs* (habilitada por padrão no Java 17)

```java
// Old NPE message: "Exception in thread 'main' java.lang.NullPointerException"
// New NPE message shows exactly what was null:
// "Cannot invoke 'String.length()' because the return value of 'Person.getName()' is null"

public class PersonProcessor {
    public void processPersons(List<Person> persons) {
        // This will show exactly which person.getName() returned null
        persons.stream()
            .mapToInt(person -> person.getName().length())  // Clear NPE if getName() returns null
            .sum();
    }
    
    // Better error messages help with complex expressions
    public void complexExample(Map<String, List<Person>> groups) {
        // NPE will show exactly which part of the chain is null
        int totalNameLength = groups.get("admins")
                                  .get(0)
                                  .getName()
                                  .length();
    }
}
```

-----

### JEP 371: Hidden Classes (Classes Ocultas) (Java 15)

**Padrão de Migração**: Uso para *framework* e geração de *proxies*

```java
// For frameworks creating dynamic proxies
public class DynamicProxyExample {
    public static <T> T createProxy(Class<T> interfaceClass, InvocationHandler handler) {
        // Hidden classes provide better encapsulation for dynamically generated classes
        MethodHandles.Lookup lookup = MethodHandles.lookup();
        
        // Framework code would use hidden classes for better isolation
        // This is typically handled by frameworks, not application code
        return interfaceClass.cast(
            Proxy.newProxyInstance(
                interfaceClass.getClassLoader(),
                new Class<?>[]{interfaceClass},
                handler
            )
        );
    }
}
```

-----

### JEP 334: JVM Constants API (Java 12)

**Padrão de Migração**: Uso para constantes em tempo de compilação

```java
import java.lang.constant.*;

// For advanced metaprogramming and tooling
public class ConstantExample {
    // Use dynamic constants for computed values
    public static final DynamicConstantDesc<String> COMPUTED_CONSTANT = 
        DynamicConstantDesc.of(
            ConstantDescs.BSM_INVOKE,
            "computeValue",
            ConstantDescs.CD_String
        );
    
    // Primarily used by compiler and framework developers
    public static String computeValue() {
        return "Computed at runtime, cached as constant";
    }
}
```

-----

### JEP 415: Context-Specific Deserialization Filters (Java 17)

**Padrão de Migração**: Segurança aprimorada para a desserialização de objetos

```java
import java.io.*;

public class SecureDeserialization {
    // Set up deserialization filters for security
    public static void setupSerializationFilters() {
        // Global filter
        ObjectInputFilter globalFilter = ObjectInputFilter.Config.createFilter(
            "java.base/*;java.util.*;!*"
        );
        ObjectInputFilter.Config.setSerialFilter(globalFilter);
    }
    
    public <T> T deserializeSecurely(byte[] data, Class<T> expectedType) throws IOException, ClassNotFoundException {
        try (ByteArrayInputStream bis = new ByteArrayInputStream(data);
             ObjectInputStream ois = new ObjectInputStream(bis)) {
            
            // Context-specific filter
            ObjectInputFilter contextFilter = ObjectInputFilter.Config.createFilter(
                expectedType.getName() + ";java.lang.*;!*"
            );
            ois.setObjectInputFilter(contextFilter);
            
            return expectedType.cast(ois.readObject());
        }
    }
}
```

-----

### JEP 356: Enhanced Pseudo-Random Number Generators (Java 17)

**Padrão de Migração**: Usar novas interfaces de gerador de números aleatórios

```java
import java.util.random.*;

// Old: Limited Random class
Random oldRandom = new Random();
int oldValue = oldRandom.nextInt(100);

// New: Enhanced random generators (Java 17+)
RandomGenerator generator = RandomGeneratorFactory
    .of("Xoshiro256PlusPlus")
    .create(System.nanoTime());

RandomGenerator.SplittableGenerator splittableGenerator = 
    RandomGeneratorFactory.of("L64X128MixRandom").create();

// Better for parallel processing
splittableGenerator.splits(4)
    .parallel()
    .mapToInt(rng -> rng.nextInt(1000))
    .forEach(System.out::println);

// Streamable random values
generator.ints(10, 1, 101)
    .forEach(System.out::println);
```

-----

## Melhorias de I/O e Rede

### JEP 380: Unix-Domain Socket Channels (Java 16)

**Padrão de Migração**: Usar *sockets* de domínio Unix para IPC local

```java
import java.net.UnixDomainSocketAddress;
import java.nio.channels.*;

// Old: TCP sockets for local communication
// ServerSocketChannel server = ServerSocketChannel.open();
// server.bind(new InetSocketAddress("localhost", 8080));

// New: Unix domain sockets (Java 16+)
public class UnixSocketExample {
    public void createUnixDomainServer() throws IOException {
        Path socketPath = Path.of("/tmp/my-app.socket");
        UnixDomainSocketAddress address = UnixDomainSocketAddress.of(socketPath);
        
        try (ServerSocketChannel server = ServerSocketChannel.open(StandardProtocolFamily.UNIX)) {
            server.bind(address);
            
            while (true) {
                try (SocketChannel client = server.accept()) {
                    // Handle client connection
                    handleClient(client);
                }
            }
        }
    }
    
    public void connectToUnixSocket() throws IOException {
        Path socketPath = Path.of("/tmp/my-app.socket");
        UnixDomainSocketAddress address = UnixDomainSocketAddress.of(socketPath);
        
        try (SocketChannel client = SocketChannel.open(address)) {
            // Communicate with server
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            client.read(buffer);
        }
    }
    
    private void handleClient(SocketChannel client) throws IOException {
        ByteBuffer buffer = ByteBuffer.allocate(1024);
        int bytesRead = client.read(buffer);
        // Process client data
    }
}
```

-----

### JEP 352: Non-Volatile Mapped Byte Buffers (Java 14)

**Padrão de Migração**: Uso para operações de memória persistente

```java
import java.nio.MappedByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.file.StandardOpenOption;

public class PersistentMemoryExample {
    public void usePersistentMemory() throws IOException {
        Path nvmFile = Path.of("/mnt/pmem/data.bin");
        
        try (FileChannel channel = FileChannel.open(nvmFile, 
                StandardOpenOption.READ, 
                StandardOpenOption.WRITE,
                StandardOpenOption.CREATE)) {
            
            // Map as persistent memory
            MappedByteBuffer buffer = channel.map(
                FileChannel.MapMode.READ_WRITE, 0, 1024,
                ExtendedMapMode.READ_WRITE_SYNC
            );
            
            // Write data that persists across crashes
            buffer.putLong(0, System.currentTimeMillis());
            buffer.putInt(8, 12345);
            
            // Force write to persistent storage
            buffer.force();
        }
    }
}
```

-----

## Configuração do Sistema de Build

### Configuração Maven

```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
    <maven.compiler.release>17</maven.compiler.release>
</properties>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.11.0</version>
            <configuration>
                <release>17</release>
                                <compilerArgs>
                    <arg>--enable-preview</arg>
                </compilerArgs>
            </configuration>
        </plugin>
        
                <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>3.0.0</version>
            <configuration>
                <argLine>--enable-preview</argLine>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### Configuração Gradle

```kotlin
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}

tasks.withType<JavaCompile> {
    options.release.set(17)
    // Enable preview features if needed
    options.compilerArgs.addAll(listOf("--enable-preview"))
}

tasks.withType<Test> {
    useJUnitPlatform()
    // Enable preview features for tests
    jvmArgs("--enable-preview")
}
```

-----

## Depreciações e Remoções

### JEP 411: Deprecate the Security Manager for Removal

**Padrão de Migração**: Remover dependências do *Security Manager*

```java
// Old: Using Security Manager
SecurityManager sm = System.getSecurityManager();
if (sm != null) {
    sm.checkPermission(new RuntimePermission("shutdownHooks"));
}

// New: Alternative security approaches
// Use application-level security, containers, or process isolation
// Most applications don't need Security Manager functionality
```

### JEP 398: Deprecate the Applet API for Removal

**Padrão de Migração**: Migrar de *Applets* para tecnologias web modernas

```java
// Old: Java Applet (deprecated)
public class MyApplet extends Applet {
    @Override
    public void start() {
        // Applet code
    }
}

// New: Modern alternatives
// 1. Convert to standalone Java application
public class MyApplication extends JFrame {
    public MyApplication() {
        setTitle("My Application");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        // Application code
    }
    
    public static void main(String[] args) {
        SwingUtilities.invokeLater(() -> {
            new MyApplication().setVisible(true);
        });
    }
}

// 2. Use Java Web Start alternative (jlink)
// 3. Convert to web application using modern frameworks
```

### JEP 372: Remove the Nashorn JavaScript Engine

**Padrão de Migração**: Usar *engines* JavaScript alternativos

```java
// Old: Nashorn (removed in Java 17)
// ScriptEngine engine = new ScriptEngineManager().getEngineByName("nashorn");

// New: Alternative approaches
// 1. Use GraalVM JavaScript engine
ScriptEngine engine = new ScriptEngineManager().getEngineByName("graal.js");

// 2. Use external JavaScript execution
ProcessBuilder pb = new ProcessBuilder("node", "script.js");
Process process = pb.start();

// 3. Use web-based approach or embedded browser
```

-----

## Melhorias de JVM e Desempenho

### JEP 377: ZGC - A Scalable Low-Latency Garbage Collector (Java 15)

**Padrão de Migração**: Habilitar ZGC para aplicações de baixa latência

```bash
# Enable ZGC
-XX:+UseZGC
-XX:+UnlockExperimentalVMOptions  # Not needed in Java 17

# Monitor ZGC performance
-XX:+LogVMOutput
-XX:LogFile=gc.log
```

### JEP 379: Shenandoah - A Low-Pause-Time Garbage Collector (Java 15)

**Padrão de Migração**: Habilitar Shenandoah para latência consistente

```bash
# Enable Shenandoah
-XX:+UseShenandoahGC
-XX:+UnlockExperimentalVMOptions  # Not needed in Java 17

# Shenandoah tuning
-XX:ShenandoahGCHeuristics=adaptive
```

### JEP 341: Default CDS Archives (Java 12) & JEP 350: Dynamic CDS Archives (Java 13)

**Padrão de Migração**: Desempenho de inicialização aprimorado

```bash
# CDS is enabled by default, but you can create custom archives
# Create custom CDS archive
java -XX:DumpLoadedClassList=classes.lst -cp myapp.jar com.example.Main
java -Xshare:dump -XX:SharedClassListFile=classes.lst -XX:SharedArchiveFile=myapp.jsa -cp myapp.jar

# Use custom CDS archive
java -XX:SharedArchiveFile=myapp.jsa -cp myapp.jar com.example.Main
```

-----

## Estratégia de Teste e Migração

### Fase 1: Fundação (Semanas 1-2)

1.  **Atualizar o sistema de build**
       - Modificar a configuração Maven/Gradle para Java 17
       - Atualizar os *pipelines* de CI/CD
       - Verificar a compatibilidade das dependências

2.  **Lidar com remoções e depreciações**
       - Remover o uso do *engine* JavaScript Nashorn
       - Substituir as APIs *Applet* depreciadas
       - Atualizar o uso do *Security Manager*

### Fase 2: Recursos da Linguagem (Semanas 3-4)

1.  **Implementar Records**
       - Converter classes de dados em *records*
       - Adicionar validação em construtores compactos
       - Testar a compatibilidade de serialização

2.  **Adicionar Pattern Matching**
       - Converter cadeias `instanceof`
       - Implementar padrões de *casting* com segurança de tipo

### Fase 3: Recursos Avançados (Semanas 5-6)

1.  **Switch Expressions**
       - Converter instruções *switch* em expressões
       - Usar a nova sintaxe de seta (`->`)
       - Implementar lógica complexa com `yield`

2.  **Text Blocks**
       - Substituir *strings* multi-linha concatenadas
       - Atualizar a geração de SQL e HTML
       - Usar métodos de formatação

### Fase 4: Sealed Classes (Semanas 7-8)

1.  **Projetar hierarquias seladas**
       - Identificar restrições de herança
       - Implementar padrões de classes seladas
       - Combinar com *pattern matching*

2.  **Testes e validação**
       - Cobertura de teste abrangente
       - *Benchmarking* de desempenho
       - Verificação de compatibilidade

-----

## Considerações de Desempenho

### Records vs Classes Tradicionais

  - *Records* são mais eficientes em memória
  - Criação e verificações de igualdade mais rápidas
  - Suporte automático à serialização
  - Considerar para objetos de transferência de dados (*DTOs*)

### Desempenho de Pattern Matching

  - Elimina verificações de tipo redundantes
  - Reduz a sobrecarga de *casting*
  - Melhores oportunidades de otimização pela JVM
  - Usar com classes seladas para exaustividade

### Otimização de Switch Expressions

  - Geração de *bytecode* mais eficiente
  - Melhor *constant folding*
  - Previsão de desvio aprimorada
  - Usar para lógica condicional complexa

-----

## Melhores Práticas

1.  **Usar Records para Classes de Dados**
       - Contêineres de dados imutáveis
       - Objetos de transferência de dados de API
       - Objetos de configuração

2.  **Aplicar Pattern Matching Estrategicamente**
       - Substituir cadeias `instanceof`
       - Usar com classes seladas
       - Combinar com *switch expressions*

3.  **Adotar Text Blocks para Conteúdo Multi-linha**
       - Consultas SQL
       - *Templates* JSON
       - Conteúdo HTML
       - Arquivos de configuração

4.  **Projetar com Sealed Classes**
       - Modelagem de domínio
       - Máquinas de estado
       - Tipos de dados algébricos
       - Controle de evolução de API

5.  **Aproveitar Enhanced Random Generators**
       - Cenários de processamento paralelo
       - Números aleatórios de alta qualidade
       - Aplicações estatísticas
       - Jogos e simulação

Este guia abrangente permite que o GitHub Copilot forneça sugestões contextualmente apropriadas ao atualizar projetos Java 11 para Java 17, com foco em aprimoramentos da linguagem, melhorias de API e práticas modernas de desenvolvimento Java.