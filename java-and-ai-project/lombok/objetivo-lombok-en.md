
-----


**Do you really know what Lombok does behind the scenes?**

-----


## What is Lombok?

**Lombok** is a Java library that automatically generates repetitive code such as:

**Getters/Setters, Constructors, equals(), hashCode(), toString(), Builders etc...**

All of this is done at **compilation time**, using annotations, **saving time** and **making the code cleaner.**

```java
@Getter
@Setter
@AllArgsConstructor
public class Produto {
    private String nome;
    private double preco;
}
```

**Without Lombok, you would have to manually write all the methods.**

-----


## What does Lombok do behind the scenes?

When you use an annotation like **@Getter**, **@Setter**, or **@Builder**, Lombok **automatically generates the equivalent code at compilation time**, as if you had written it manually.

**This means that:**

  * **You don't see the generated code in your editor,**
  * **But it exists in the application's bytecode,**
  * **And it appears if you decompile the generated .class file.**

-----


## Practical Example

```java
@Getter
@Setter
public class Produto {
    private String nome;
    private Double preco;
}
```

**What Lombok generates behind the scenes:**

```java
public class Produto {
    private String nome;
    private Double preco;

    public String getNome() { return nome; }
    public void setNome(String nome) { this.nome = nome; }

    public Double getPreco() { return preco; }
    public void setPreco(Double preco) { this.preco = preco; }
}
```

**That's why it is important to understand what each annotation generates, and not to use them in the automatic mode without awareness.**

-----


## Be careful with @Data\!

The **@Data** annotation is one of the most **controversial** ones in Lombok because it automatically generates many things: **getters**, **setters**, **equals()**, **hashCode()**, **toString()** and even the **constructor**.

```java
@Data
public class Usuario {
    private String nome;
    private String senha;
}
```

**Why you should be careful:**

  * **toString() can expose sensitive data**
  * **Automatic equals() and hashCode() can cause bugs in collections**
  * **You don't always want all fields in the generated methods**