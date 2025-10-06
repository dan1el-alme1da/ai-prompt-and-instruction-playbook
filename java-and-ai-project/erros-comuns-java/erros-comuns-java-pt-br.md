# 04 Erros Comuns em Java que Todo Desenvolvedor Comete

Este resumo aborda quatro erros frequentes cometidos por desenvolvedores Java, com a solução correta para cada um.

-----

## Erro 1: Usar `==` para comparar Strings

### O Erro

Usar o operador de igualdade (`==`) para comparar o conteúdo de duas strings.

```java
if(nome == "Bruno") {
    // ...
}
```

**O problema:** Isso não compara o **conteúdo** da `String`, mas sim a **referência** dela na memória. Em muitos casos, isso resultará em uma comparação incorreta.

### O Correto

Use o método `equals()` para comparar os valores das strings.

```java
if(nome.equals("Bruno")) {
    // ...
}
```

**Dica:** Sempre que precisar comparar valores de **String**, use `equals()`.

-----

## Erro 2: Não tratar exceções corretamente

### O Erro

Lançar exceções genéricas como `RuntimeException` sem personalização e sem controle centralizado.

```java
throw new RuntimeException("Erro ao buscar usuário");
```

**O problema:** Isso dificulta o entendimento do erro e o tratamento adequado dele no front-end ou em outras partes do sistema.

### O Correto

Crie **exceções personalizadas** (ex: `UserNotFoundException`) e **centralize** o tratamento delas usando `@RestControllerAdvice` e `@ExceptionHandler` (comuns no ecossistema Spring Boot).

-----

## Erro 3: Expor a senha do usuário no retorno da API

### O Erro

Retornar diretamente a entidade `User` (ou similar) com todos os campos, incluindo a senha, no retorno de uma API.

```java
return userRepository.findAll();
```

**O problema:** Isso pode fazer com que a senha ou outros dados sensíveis sejam incluídos no JSON de resposta. Expor dados sensíveis é uma **falha de segurança grave**.

### O Correto

Crie um **DTO (Data Transfer Object)**, como `UserResponseDTO`, contendo apenas os dados que **precisam** ser retornados.

**Dica:** Use **DTOs** para proteger os dados e manter as respostas da API limpas e seguras.

-----

## Erro 4: Retornar `null` em vez de uma lista vazia

### O Erro

Retornar `null` de um método que deveria retornar uma lista quando não há resultados.

```java
public List<Produto> listarProdutos() {
    if(estoqueVazio()) return null; // O Erro!
    return produtoRepository.findAll();
}
// ...
listaProdutos.size(); // Pode causar NullPointerException
```

**O problema:** Retornar `null` obriga quem consome o método a sempre fazer uma verificação `if (lista != null)` antes de usar a lista, como chamar `size()` ou iterar. Caso a verificação falhe, uma `NullPointerException` é lançada.

### O Correto

Retorne uma **lista vazia** quando não houver resultados.

```java
return new ArrayList<>(); // O Correto!
```

**Dica:** Retorne **listas vazias** em vez de `null` para evitar exceções (como `NullPointerException`) e tornar o código mais limpo e seguro.