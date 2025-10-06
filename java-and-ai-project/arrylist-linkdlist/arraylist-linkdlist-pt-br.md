
-----

## Comparação: ArrayList vs. LinkedList

### Título Principal

ArrayList ou LinkedList? Qual usar em Java?

### Dúvida Comum

Essa é uma dúvida comum entre desenvolvedores, mas a escolha certa pode impactar diretamente na **performance** e na **clareza** do código.

-----

## 1\. ArrayList (Baseado em Array Dinâmico)

  * Baseado em **array dinâmico**.
  * Acesso rápido por índice **(O(1))**.
  * Inserções e remoções no meio da lista são mais custosas **(O(n))**.

### Exemplo de Código (ArrayList)

```java
import java.util.ArrayList;

public class ExemploArrayList {
    public static void main(String[] args) {
        // Criando uma lista de Strings
        ArrayList<String> lista = new ArrayList<>();

        // Adicionando elementos
        lista.add("Java");
        lista.add("Python");
        lista.add("C#");

        // Acessando elementos pelo índice
        System.out.println("Primeiro elemento: " + lista.get(0));

        // Iterando sobre a lista
        System.out.println("Linguagens na lista:");
        for (String linguagem : lista) {
            System.out.println(linguagem);
        }

        // Removendo um elemento
        lista.remove("Python");

        System.out.println("Após remover Python: " + lista);
    }
}
```

-----

## 2\. LinkedList (Baseado em Lista Duplamente Encadeada)

  * Baseado em lista **duplamente encadeada**.
  * Inserções e remoções são eficientes no início e meio da lista (O(1) para referências conhecidas).
  * Acesso por índice é **mais lento (O(n))**.

### Exemplo de Código (LinkedList)

```java
import java.util.LinkedList;

public class ExemploLinkedlist {
    public static void main(String[] args) {
        // Criando uma lista de Strings
        LinkedList<String> lista = new LinkedList<>();

        // Adicionando elementos
        lista.add("Java");
        lista.add("Python");
        lista.add("C#");

        // Adicionando no início e no fim da lista
        lista.addFirst("Go");
        lista.addLast("JavaScript");

        // Acessando elementos pelo índice
        System.out.println("Primeiro elemento: " + lista.getFirst());
        System.out.println("Último elemento: " + lista.getLast());

        // Iterando sobre a lista
        System.out.println("Linguagens na lista:");
        for (String linguagem : lista) {
            System.out.println(linguagem);
        }

        // Removendo elementos
        lista.removeFirst(); // Remove o primeiro
        lista.remove("Python"); // Remove um elemento específico

        System.out.println("Após remoções: " + lista);
    }
}
```

-----

## Java Collections API e Dica Extra

### Diagrama

  * List(I)
  * Set(I)
  * Map(I)
  * Java Collections API

### Dica Extra 💡

Em 90% dos casos, **ArrayList** será suficiente, mas conhecer bem as diferenças permite escrever código mais eficiente e profissional.