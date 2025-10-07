## Instruções para o GitHub Copilot: Guia de Atualização do Java 21 para o Java 25

Estas instruções ajudam o **GitHub Copilot** a auxiliar desenvolvedores na atualização de projetos Java do **JDK 21 para o JDK 25**, focando em novos recursos de linguagem, mudanças de API e melhores práticas.

-----

## Recursos de Linguagem e Mudanças de API no JDK 22-25

### Aprimoramentos de *Pattern Matching* (JEP 455/488 - *Preview* no 23)

**Tipos Primitivos em Padrões, `instanceof` e `switch`**

Ao trabalhar com *pattern matching* (casamento de padrões):

  - Sugerir o uso de **padrões de tipos primitivos** em expressões `switch` e verificações `instanceof`.
  - Exemplo de atualização do `switch` tradicional:

<!-- end list -->

```java
// Abordagem Antiga (Java 21)
switch (x.getStatus()) {
    case 0 -> "okay";
    case 1 -> "warning"; 
    case 2 -> "error";
    default -> "unknown status: " + x.getStatus();
}

// Nova Abordagem (Preview do Java 25)
switch (x.getStatus()) {
    case 0 -> "okay";
    case 1 -> "warning";
    case 2 -> "error"; 
    case int i -> "unknown status: " + i; // Padrão de tipo primitivo
}
```

  - Habilitar recursos *preview* com a *flag* `--enable-preview`.
  - Sugerir **padrões de guarda** (*guard patterns*) para condições mais complexas:

<!-- end list -->

```java
switch (x.getYearlyFlights()) {
    case 0 -> ...;
    case int i when i >= 100 -> issueGoldCard(); // Padrão de guarda
    case int i -> ... // trata o intervalo 1-99
}
```

-----

### API de Arquivos de Classe (*Class-File API*) (JEP 466/484 - Segundo *Preview* no 23, Padrão no 25)

**Substituindo o ASM pela API Padrão**

Ao detectar manipulação de *bytecode* ou processamento de arquivos de classe:

  - Sugerir a migração da biblioteca **ASM** para a **API padrão de Arquivos de Classe**.
  - Usar o pacote `java.lang.classfile` em vez de `org.objectweb.asm`.
  - Padrão de migração de exemplo:

<!-- end list -->

```java
// Abordagem Antiga com ASM
ClassReader reader = new ClassReader(classBytes);
ClassWriter writer = new ClassWriter(reader, 0);
// ... manipulação ASM

// Nova Abordagem com a Class-File API
ClassModel classModel = ClassFile.of().parse(classBytes);
byte[] newBytes = ClassFile.of().transform(classModel, 
    ClassTransform.transformingMethods(methodTransform));
```

-----

### Comentários de Documentação Markdown (JEP 467 - Padrão no 23)

**Modernização do JavaDoc**

Ao trabalhar com comentários **JavaDoc**:

  - Sugerir a conversão de JavaDoc com muito **HTML** para a sintaxe **Markdown**.
  - Usar `///` para comentários de documentação Markdown.
  - Exemplo de conversão:

<!-- end list -->

```java
// JavaDoc Antigo em HTML
/**
 * Returns the <b>absolute</b> value of an {@code int} value.
 * <p>
 * If the argument is not negative, return the argument.
 * If the argument is negative, return the negation of the argument.
 * * @param a the argument whose absolute value is to be determined
 * @return the absolute value of the argument
 */

// Novo JavaDoc em Markdown 
/// Returns the **absolute** value of an `int` value.
///
/// If the argument is not negative, return the argument.
/// If the argument is negative, return the negation of the argument.
/// 
/// @param a the argument whose absolute value is to be determined
/// @return the absolute value of the argument
```

-----

### Criação de *Records* Derivados (JEP 468 - *Preview* no 23)

**Aprimoramento de *Record***

Ao trabalhar com **records**:

  - Sugerir o uso de expressões `with` para criar *records* derivados.
  - Habilitar recursos *preview* para a criação de *records* derivados.
  - Padrão de exemplo:

<!-- end list -->

```java
// Em vez da cópia manual de record
public record Person(String name, int age, String email) {
    public Person withAge(int newAge) {
        return new Person(name, newAge, email);
    }
}

// Usar a criação de record derivado (Preview)
Person updated = person with { age = 30; };
```

-----

### Coletores de *Stream* (*Stream Gatherers*) (JEP 473/485 - Segundo *Preview* no 23, Padrão no 25)

**Processamento de *Stream* Aprimorado**

Ao trabalhar com operações complexas de *stream*:

  - Sugerir o uso de `Stream.gather()` para operações intermediárias personalizadas.
  - Importar `java.util.stream.Gatherers` para coletores (*gatherers*) embutidos.
  - Exemplo de uso:

<!-- end list -->

```java
// Operações personalizadas de janela
List<List<String>> windows = stream
    .gather(Gatherers.windowSliding(3)) // Exemplo de gatherer
    .toList();

// Filtragem personalizada com estado
List<Integer> filtered = numbers.stream()
    .gather(Gatherers.fold(0, (state, element) -> {
        // Lógica com estado personalizada
        return state + element > threshold ? element : null;
    }))
    .filter(Objects::nonNull)
    .toList();
```

-----

## Avisos de Migração e Descontinuações

### Métodos de Acesso à Memória `sun.misc.Unsafe` (JEP 471 - Descontinuado no 23)

Ao detectar o uso de `sun.misc.Unsafe`:

  - Alertar sobre os **métodos de acesso à memória descontinuados**.
  - Sugerir migração para alternativas padrão:

<!-- end list -->

```java
// Descontinuado: acesso à memória sun.misc.Unsafe
Unsafe unsafe = Unsafe.getUnsafe();
unsafe.getInt(object, offset);

// Preferido: API VarHandle
VarHandle vh = MethodHandles.lookup()
    .findVarHandle(MyClass.class, "fieldName", int.class);
int value = (int) vh.get(object);

// Ou para memória off-heap: API Foreign Function & Memory (FFM)
MemorySegment segment = MemorySegment.ofArray(new int[10]);
int value = segment.get(ValueLayout.JAVA_INT, offset);
```

-----

### Avisos de Uso de JNI (JEP 472 - Avisos no 24)

Ao detectar o uso de **JNI**:

  - Alertar sobre futuras **restrições no uso de JNI**.
  - Sugerir adicionar a *flag* `--enable-native-access` para aplicações que usam JNI.
  - Recomendar migração para a API **Foreign Function & Memory (FFM)** sempre que possível.
  - Adicionar entradas `module-info.java` para acesso nativo:

<!-- end list -->

```java
module com.example.app {
    requires jdk.unsupported; // para uso restante de JNI
}
```

-----

## Atualizações de Coleta de Lixo (*Garbage Collection*)

### Modo Geracional do ZGC (JEP 474 - Padrão no 23)

Ao configurar a coleta de lixo:

  - O **ZGC** agora usa o **modo geracional** por padrão.
  - Atualizar as *flags* JVM se estiver usando explicitamente o ZGC não geracional:

<!-- end list -->

```bash
# Modo não geracional explícito (mostrará aviso de descontinuação)
-XX:+UseZGC -XX:-ZGenerational

# Modo geracional padrão
-XX:+UseZGC
```

-----

### Aprimoramentos no G1 (JEP 475 - Implementado no 24)

Ao usar **G1GC**:

  - Não são necessárias alterações no código - é uma otimização interna da JVM.
  - Pode haver melhoria no desempenho de compilação com o compilador **C2**.

-----

## API de Vetores (*Vector API*) (JEP 469 - Oitava *Incubator* no 25)

Ao trabalhar com **computações numéricas**:

  - Sugerir a **Vector API** para operações **SIMD** (ainda em incubação).
  - Adicionar `--add-modules jdk.incubator.vector`.
  - Exemplo de uso:

<!-- end list -->

```java
import jdk.incubator.vector.*;

// Computação escalar tradicional
for (int i = 0; i < a.length; i++) {
    c[i] = a[i] + b[i];
}

// Computação vetorizada
var species = IntVector.SPECIES_PREFERRED;
for (int i = 0; i < a.length; i += species.length()) {
    var va = IntVector.fromArray(species, a, i);
    var vb = IntVector.fromArray(species, b, i);
    var vc = va.add(vb);
    vc.intoArray(c, i);
}
```

-----

## Configuração de Compilação e *Build*

### Recursos *Preview*

Para projetos que usam recursos *preview*:

  - Adicionar `--enable-preview` aos argumentos do compilador.
  - Adicionar `--enable-preview` aos argumentos de tempo de execução.
  - Configuração **Maven**:

<!-- end list -->

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <configuration>
        <release>25</release>
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

  - Configuração **Gradle** (Kotlin DSL):

<!-- end list -->

```kotlin
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(25)
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

## Estratégia de Migração

### Processo de Atualização Passo a Passo

1.  **Atualizar Ferramentas de *Build***: Garantir que Maven/Gradle suportem o JDK 25.
2.  **Atualizar Dependências**: Verificar a compatibilidade com o JDK 25.
3.  **Lidar com Avisos**: Resolver os avisos de descontinuação dos JEPs 471/472.
4.  **Habilitar Recursos *Preview***: Se estiver usando *pattern matching* ou outros recursos *preview*.
5.  **Testar Exaustivamente**: Especialmente para aplicações que usam JNI ou `sun.misc.Unsafe`.
6.  **Teste de Desempenho**: Verificar o comportamento do GC com os novos padrões do ZGC.

-----

### *Checklist* de Revisão de Código

Ao revisar o código para a atualização para o Java 25:

  - [x] Substituir o uso de **ASM** pela **Class-File API**.
  - [x] Converter JavaDoc complexo com HTML para **Markdown**.
  - [x] Usar **padrões primitivos** em expressões `switch` onde aplicável.
  - [x] Substituir `sun.misc.Unsafe` por **VarHandle** ou **FFM API**.
  - [x] Adicionar permissões de **acesso nativo** para uso de JNI.
  - [x] Usar **Stream *gatherers*** para operações de *stream* complexas.
  - [x] Atualizar a configuração de *build* para recursos *preview*.

-----

### Considerações de Teste

  - Testar com a *flag* `--enable-preview` para recursos *preview*.
  - Verificar se as aplicações JNI funcionam com os avisos de acesso nativo.
  - Testar o desempenho com o novo **modo geracional do ZGC**.
  - Validar a geração de JavaDoc com os comentários em **Markdown**.

-----

## Armadilhas Comuns (*Common Pitfalls*)

1.  **Dependências de Recursos *Preview***: Não usar recursos *preview* em código de biblioteca sem documentação clara.
2.  **Acesso Nativo**: Aplicações que usam **JNI** direta ou indiretamente podem precisar da configuração `--enable-native-access`.
3.  **Migração *Unsafe***: Não atrasar a migração de `sun.misc.Unsafe` - avisos de descontinuação indicam remoção futura.
4.  ***Pattern Matching* Escopo**: Padrões primitivos funcionam com todos os tipos primitivos, não apenas `int`.
5.  ***Record Enhancement***: A criação de *record* derivado requer a *flag* *preview* no Java 23.

-----

## Considerações de Desempenho

  - O **modo geracional do ZGC** pode melhorar o desempenho para a maioria das cargas de trabalho.
  - A **Class-File API** reduz a sobrecarga relacionada ao ASM.
  - Os **Stream *gatherers*** fornecem melhor desempenho para operações de *stream* complexas.
  - Os aprimoramentos no **G1GC** reduzem a sobrecarga de compilação JIT.

Lembre-se de **testar exaustivamente** em ambientes de *staging* antes de implantar as atualizações do Java 25 em sistemas de produção.