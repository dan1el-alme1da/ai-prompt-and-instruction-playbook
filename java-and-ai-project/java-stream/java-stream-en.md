
-----

## **Java Stream: Transform Your Code with Just a Few Lines**

-----

## **What is Stream?**

It is an **API** that allows you to **process collections** in a **simple, declarative** way with **less code**. It transforms lists and sets into **data streams**, which can be easily **filtered, mapped, and collected**.

-----

## **Before Stream**

To filter and process lists, it was common to use a **for** or **for-each** loop, write **multiple lines of code**, and **manage loops manually**.

**Code example before Stream:**

```java
List<String> nomes = Arrays.asList("Bruno", "Rebeca", "Alex");
List<String> filtered = new ArrayList<>();
for (String nome : nomes) {
    if (nome.startsWith("A")) {
        filtered.add(nome);
    }
}
```

-----

## **After Stream**

With Stream, we filter and process data in a more **clean, concise, and readable** way.

**Code example with Stream:**

```java
List<String> nomes = Arrays.asList("Bruno", "Rebeca", "Alex");
List<String> filtered = nomes.stream()
    .filter(nome -> nome.startsWith("A"))
    .toList();
```

**Benefits:**

  * **Shorter and more readable code**
  * **Less risk of manual errors**
  * **Easy to chain operations**

-----

## **Main Operations**

There are two types of operations:

  * **Intermediate** $\rightarrow$ **Transform or filter data** (the Stream continues).
  * **Terminal** $\rightarrow$ **Finalize the processing and return a result**.

### **Intermediate Operations:**

  * `filter()` $\rightarrow$ filters elements
  * `map()` $\rightarrow$ transforms elements
  * `sorted()` $\rightarrow$ sorts
  * `distinct()` $\rightarrow$ removes duplicates
  * `limit()` $\rightarrow$ limits quantity

### **Terminal Operations:**

  * `forEach()` $\rightarrow$ executes action for each element
  * `collect()` $\rightarrow$ converts to a list, set, etc.
  * `reduce()` $\rightarrow$ reduces to a single value
  * `count()` $\rightarrow$ counts elements
  * `anyMatch()` / `allMatch()` $\rightarrow$ checks/verifications

**Chaining example:**

```java
List<String> nomes = List.of("Ana", "Bruno", "Rebeca", "Alex");
nomes.stream()
    .filter(n -> n.length() > 4)    //Intermediate
    .map(String::toUpperCase)       //Intermediate
    .forEach(System.out::println);  // Terminal
```