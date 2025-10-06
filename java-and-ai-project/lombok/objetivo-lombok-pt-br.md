
-----


**Você realmente sabe o que o Lombok faz por baixo dos panos?**

-----


## O que é o Lombok?

O **Lombok** é uma biblioteca Java que gera automaticamente códigos repetitivos como:

**Getters/Setters, Construtores, equals(), hashCode(), toString(), Builders etc...**

Tudo isso é feito em **tempo de compilação**, por meio de anotações, **economizando tempo** e **deixando o código mais limpo.**

```java
@Getter
@Setter
@AllArgsConstructor
public class Produto {
    private String nome;
    private double preco;
}
```

**Sem Lombok, você teria que escrever manualmente todos os métodos.**

-----


## O que o Lombok faz por baixo dos panos?

Quando você usa uma anotação como **@Getter**, **@Setter** ou **@Builder**, o Lombok **gera automaticamente o código equivalente na hora da compilação**, como se você tivesse escrito manualmente.

**Isso significa que:**

  * **Você não vê o código gerado no seu editor,**
  * **Mas ele existe no bytecode da aplicação,**
  * **E aparece se você descompilar a classe .class gerada.**

-----


## Exemplo prático

```java
@Getter
@Setter
public class Produto {
    private String nome;
    private Double preco;
}
```

**O que o lombok gera nos bastidores:**

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

**Por isso é importante entender o que cada anotação gera, e não usá-las no modo automático sem consciência.**

-----


## Cuidado com o @Data\!

O **@Data** é uma das anotações mais **polêmicas** do Lombok porque gera muitas coisas **automaticamente**: **getters**, **setters**, **equals()**, **hashCode()**, **toString()** e até o **constructor**.

```java
@Data
public class Usuario {
    private String nome;
    private String senha;
}
```

**Por que tomar cuidado:**

  * **toString() pode expor dados sensíveis**
  * **equals() e hashCode() automáticos podem causar bugs em coleções**
  * **Nem sempre você quer todos os campos nos métodos gerados**