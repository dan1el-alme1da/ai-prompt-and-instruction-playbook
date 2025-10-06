
-----

## English Translation of the Image Content

### (Set, HashSet, and TreeSet Overview)

When we talk about **Set**, we know the goal is to ensure that **no duplicate elements exist**. But the choice between **HashSet** and **TreeSet** can directly impact **performance and ordering**.

### (HashSet or TreeSet? Which to choose in Java?)

(Diagram of the Java Collections API with List(I), Set(I), Map(I))

**HashSet or TreeSet?**
Which to choose in Java?

### (HashSet Details)

**(1) HashSet**

  * Based on **hash tables**.
  * Allows null.
  * **Does not maintain the order of elements**.
  * Generally faster in insertions, searches, and removals ($\mathcal{O}(1)$).

### (TreeSet Details)

**(2) TreeSet**

  * Based on a **balanced binary tree (Red-Black Tree)**.
  * **Automatically keeps elements sorted**.
  * Does not accept null.
  * More costly operations compared to HashSet ($\mathcal{O}(\log n)$).

### (HashSet Code Example)

```java
import java.util.HashSet;

public class ExemploHashSet {
    public static void main(String[] args) {
        // Creating a HashSet of Strings
        HashSet<String> conjunto = new HashSet<>();

        // Adding elements
        conjunto.add("Java");
        conjunto.add("Python");
        conjunto.add("C#");
        conjunto.add("Java"); // duplicated, will not be added

        // Displaying the set
        System.out.println("Conjunto: " + conjunto);

        // Checking if an element exists
        System.out.println("Contém Python? " + conjunto.contains("Python"));

        // Removing an element
        conjunto.remove("C#");
        System.out.println("Após remover C#: " + conjunto);

        // Iterating over the elements
        System.out.println("Iterando sobre o HashSet:");
        for (String linguagem : conjunto) {
            System.out.println(linguagem);
        }
    }
}
```

**Note:** The variable names and print statements inside the code block were kept in Portuguese to be a faithful transcription of the code image.

### (TreeSet Code Example)

```java
import java.util.TreeSet;

public class ExemploTreeSet {
    public static void main(String[] args) {
        // Creating a TreeSet of Strings
        TreeSet<String> conjunto = new TreeSet<>();

        // Adding elements
        conjunto.add("Java");
        conjunto.add("Python");
        conjunto.add("C#");
        conjunto.add("Go");
        conjunto.add("Java"); // duplicated, will be ignored

        // Displaying the set (automatically sorted)
        System.out.println("Conjunto ordenado: " + conjunto);

        // Accessing the first and last element
        System.out.println("Primeiro elemento: " + conjunto.first());
        System.out.println("Último elemento: " + conjunto.last());

        // Removing an element
        conjunto.remove("Python");
        System.out.println("Após remover Python: " + conjunto);

        // Iterating over the elements
        System.out.println("Iterando sobre o TreeSet:");
        for (String linguagem : conjunto) {
            System.out.println(linguagem);
        }
    }
}
```

**Note:** The variable names and print statements inside the code block were kept in Portuguese to be a faithful transcription of the code image.

### (Practical Rule)

**Practical Rule** 🔎

  * If you only need to ensure **uniqueness and performance**, use **HashSet**.
  * If you need to **order elements**, choose **TreeSet**.

**Practical Example** 📌

  * List of unique IDs $\rightarrow$ HashSet
  * List of sorted names $\rightarrow$ TreeSet

-----
