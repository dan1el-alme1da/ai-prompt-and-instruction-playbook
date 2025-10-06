
-----

## **Java Stream: Transforme seu código em poucas linhas**

-----

## **O que é Stream?**

É uma **API** que permite **processar coleções** de forma **simples**, **declarativa** e com **menos código**. Transforma listas e conjuntos em **fluxos de dados**, que podem ser **filtrados**, **mapeados** e **coletados** facilmente.

-----

## **Antes do Stream**

Para filtrar e processar listas, era comum usar **for** ou **for-each**, escrever **várias linhas de código** e **gerenciar loops manualmente**.

**Exemplo de código antes do Stream:**

```java
List<String> nomes = Arrays.asList("Bruno", "Rebeca", "Alex");
List<String> filtrados = new ArrayList<>();
for (String nome : nomes) {
    if (nome.startsWith("A")) {
        filtrados.add(nome);
    }
}
```

-----

## **Depois do Stream**

Com Stream, filtramos e processamos dados de forma mais **limpa, concisa e legível.**

**Exemplo de código com Stream:**

```java
List<String> nomes = Arrays.asList("Bruno", "Rebeca", "Alex");
List<String> filtrados = nomes.stream()
    .filter(nome -> nome.startsWith("A"))
    .toList();
```

**Benefícios:**

  * **Código mais curto e legível**
  * **Menos risco de erros manuais**
  * **Fácil de encadear operações**

-----

## **Principais operações**

Existem dois tipos de operações:

  * **Intermediárias** $\rightarrow$ **Transformam ou filtram dados** (o Stream continua).
  * **Terminais** $\rightarrow$ **Finalizam o processamento e retornam um resultado**.

### **Intermediárias:**

  * `filter()` $\rightarrow$ filtra elementos
  * `map()` $\rightarrow$ transforma elementos
  * `sorted()` $\rightarrow$ ordena
  * `distinct()` $\rightarrow$ remove duplicados
  * `limit()` $\rightarrow$ limita quantidade

### **Terminais:**

  * `forEach()` $\rightarrow$ executa ação para cada elemento
  * `collect()` $\rightarrow$ converte em lista, set, etc.
  * `reduce()` $\rightarrow$ reduz a um único valor
  * `count()` $\rightarrow$ conta elementos
  * `anyMatch()` / `allMatch()` $\rightarrow$ verificações

**Exemplo de encadeamento:**

```java
List<String> nomes = List.of("Ana", "Bruno", "Rebeca", "Alex");
nomes.stream()
    .filter(n -> n.length() > 4)    //Intermediária
    .map(String::toUpperCase)       //Intermediária
    .forEach(System.out::println);  // Terminal
```