
-----

## **O que é uma Exceção?**

Uma **exceção** é um evento anormal que ocorre durante a execução do programa e que **interrompe o fluxo normal das instruções**.

**Por exemplo:**

  * Tentar acessar um índice fora dos limites de um array.
  * Tentar dividir um número por zero.
  * Tentar abrir um arquivo que não existe.

-----

## **Como tratar Exceções em Java?**

O tratamento é feito com os blocos **try**, **catch**, **finally** e a palavra-chave **throw**.

```java
try {
    // Código que pode gerar uma exceção
} catch (TipoDaExcecao e) {
    // Código para tratar a exceção
} finally {
    // (Opcional) Código que sempre será executado
}
```

-----

## **Exemplo Prático: Divisão por Zero**

```java
public class Exemplo {
    public static void main(String[] args) {
        try {
            int resultado = 10 / 0; // ArithmeticException
        } catch (ArithmeticException e) {
            System.out.println("Erro: divisão por zero.");
        } finally {
            System.out.println("Bloco finally executado.");
        }
    }
}
```

**Saída:** Erro: divisão por zero. Bloco finally executado.

**Observe como o catch lidou com o erro e o finally foi executado sempre.**

-----

## **Múltiplos catch: Lidando com Variedade**

Você pode ter vários blocos **catch** para diferentes tipos de exceção, tornando seu tratamento mais específico.

```java
try {
    int[] numeros = {1, 2, 3};
    System.out.println(numeros[5]); // ArrayIndexOutOfBoundsException
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Índice inválido no array.");
} catch (Exception e) {
    System.out.println("Erro genérico.");
}
```

**Priorize exceções mais específicas antes das mais genéricas\!**

-----

## **Lançar Exceções com throw**

Você também pode **lançar uma exceção manualmente** para indicar um problema em seu código.

```java
public void verificarIdade(int idade) {
    if (idade < 18) {
        throw new IllegalArgumentException("Menor de idade não permitido.");
    }
}
```

**Perfeito para validar entradas ou regras de negócio.**

-----

## **Exceções Checadas vs. Não Checadas**

| Tipo | Exemplo | Precisa de `try`/`catch` ou `throws` |
| :--- | :--- | :--- |
| **Checadas** | `IOException`, `SQLException` | Sim $\checkmark$ |
| **Não checadas** | `NullPointerException`, `ArithmeticException` | Não obrigatório $\times$ |

**Observação: Checadas são verificadas em tempo de compilação, não checadas em tempo de execução.**

-----

## **Boas Práticas: Código Limpo e Robusto**

  * Trate somente exceções que você pode realmente **resolver**.
  * Nunca deixe **`catch (Exception e)`** sem necessidade — ele é muito genérico.
  * Use **finally** para fechar recursos (como arquivos, conexões etc.).
  * Prefira usar **`try-with-resources`** ao lidar com recursos externos.

-----
