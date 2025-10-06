
-----

## Comparison: ArrayList vs. LinkedList

### Main Title

ArrayList or LinkedList? Which one to use in Java?

### Common Doubt

This is a common question among developers, but the right choice can directly impact the **performance** and **clarity** of the code.

-----

## 1\. ArrayList (Based on Dynamic Array)

  * Based on a **dynamic array**.
  * Fast access by index **(O(1))**.
  * Insertions and removals in the middle of the list are more costly **(O(n))**.

### Code Example (ArrayList)

```java
import java.util.ArrayList;

public class ExemploArrayList {
    public static void main(String[] args) {
        // Creating a list of Strings
        ArrayList<String> lista = new ArrayList<>();

        // Adding elements
        lista.add("Java");
        lista.add("Python");
        lista.add("C#");

        // Accessing elements by index
        System.out.println("Primeiro elemento: " + lista.get(0));

        // Iterating over the list
        System.out.println("Linguagens na lista:");
        for (String linguagem : lista) {
            System.out.println(linguagem);
        }

        // Removing an element
        lista.remove("Python");

        System.out.println("Após remover Python: " + lista);
    }
}
```

-----

## 2\. LinkedList (Based on Doubly Linked List)

  * Based on a **doubly linked list**.
  * Insertions and removals are efficient at the beginning and in the middle of the list ($O(1)$ for known references).
  * Access by index is **slower ($O(n)$)**.

### Code Example (LinkedList)

```java
import java.util.LinkedList;

public class ExemploLinkedlist {
    public static void main(String[] args) {
        // Creating a list of Strings
        LinkedList<String> lista = new LinkedList<>();

        // Adding elements
        lista.add("Java");
        lista.add("Python");
        lista.add("C#");

        // Adding to the beginning and end of the list
        lista.addFirst("Go");
        lista.addLast("JavaScript");

        // Accessing elements by index
        System.out.println("Primeiro elemento: " + lista.getFirst());
        System.out.println("Último elemento: " + lista.getLast());

        // Iterating over the list
        System.out.println("Linguagens na lista:");
        for (String linguagem : lista) {
            System.out.println(linguagem);
        }

        // Removing elements
        lista.removeFirst(); // Remove o primeiro
        lista.remove("Python"); // Remove um elemento específico

        System.out.println("Após remoções: " + lista);
    }
}
```

-----

## Java Collections API and Extra Tip

### Diagram Text

  * List(I)
  * Set(I)
  * Map(I)
  * Java Collections API

### Extra Tip 💡

In 90% of cases, **ArrayList** will be sufficient, but having good knowledge of the differences allows for writing more efficient and professional code.