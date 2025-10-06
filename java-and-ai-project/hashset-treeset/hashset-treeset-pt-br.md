
-----

## Transcrição do Conteúdo das Imagens

### (Visão Geral de Set, HashSet e TreeSet)

Quando falamos em **Set**, sabemos que o objetivo é garantir que **não existam elementos duplicados**. Mas a escolha entre **HashSet** e **TreeSet** pode impactar diretamente em **performance e ordenação**.

### (HashSet ou TreeSet? Qual escolher em Java?)

(Diagrama da Java Collections API com List(I), Set(I), Map(I))

**HashSet ou TreeSet?**
Qual escolher em Java?

### (Detalhes do HashSet)

**(1) HashSet**

  * Baseado em **tabelas de espalhamento (hash table)**.
  * Permite null.
  * **Não mantém ordem dos elementos**.
  * Geralmente mais rápido em inserções, buscas e remoções ($\mathcal{O}(1)$).

###  (Detalhes do TreeSet)

**(2) TreeSet**

  * Baseado em **árvore binária balanceada (Red-Black Tree)**.
  * **Mantém os elementos ordenados automaticamente**.
  * Não aceita null.
  * Operações mais custosas em comparação ao HashSet ($\mathcal{O}(\log n)$).

###  (Exemplo de Código HashSet)

```java
import java.util.HashSet;

public class ExemploHashSet {
    public static void main(String[] args) {
        // Criando um HashSet de Strings
        HashSet<String> conjunto = new HashSet<>();

        // Adicionando elementos
        conjunto.add("Java");
        conjunto.add("Python");
        conjunto.add("C#");
        conjunto.add("Java"); // duplicado, não será adicionado

        // Exibindo o conjunto
        System.out.println("Conjunto: " + conjunto);

        // Verificando se um elemento existe
        System.out.println("Contém Python? " + conjunto.contains("Python"));

        // Removendo um elemento
        conjunto.remove("C#");
        System.out.println("Após remover C#: " + conjunto);

        // Iterando sobre os elementos
        System.out.println("Iterando sobre o HashSet:");
        for (String linguagem : conjunto) {
            System.out.println(linguagem);
        }
    }
}
```

###  (Exemplo de Código TreeSet)

```java
import java.util.TreeSet;

public class ExemploTreeSet {
    public static void main(String[] args) {
        // Criando um TreeSet de Strings
        TreeSet<String> conjunto = new TreeSet<>();

        // Adicionando elementos
        conjunto.add("Java");
        conjunto.add("Python");
        conjunto.add("C#");
        conjunto.add("Go");
        conjunto.add("Java"); // duplicado, será ignorado

        // Exibindo o conjunto (ordenado automaticamente)
        System.out.println("Conjunto ordenado: " + conjunto);

        // Acessando o primeiro e o último elemento
        System.out.println("Primeiro elemento: " + conjunto.first());
        System.out.println("Último elemento: " + conjunto.last());

        // Removendo um elemento
        conjunto.remove("Python");
        System.out.println("Após remover Python: " + conjunto);

        // Iterando sobre os elementos
        System.out.println("Iterando sobre o TreeSet:");
        for (String linguagem : conjunto) {
            System.out.println(linguagem);
        }
    }
}
```

### (Regra Prática)

**Regra prática** 🔎

  * Se você precisa apenas garantir **unicidade e performance**, use **HashSet**.
  * Se precisa **ordenar elementos**, escolha **TreeSet**.

**Exemplo prático** 📌

  * Lista de IDs únicos $\rightarrow$ HashSet
  * Lista de nomes ordenados $\rightarrow$ TreeSet

-----

Posso te ajudar com alguma dúvida sobre esses tópicos ou com outro código em Java?