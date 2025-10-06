
-----

### Imagem 1: O problema

**O problema** ❌

Esse é o trecho de um script que tinha em meu primeiro projeto com Spring Boot

```java
return userRepository.findByUsername(username)
.orElseThrow(() -> new 
RuntimeException("Usuário não encontrado"));
```

Sem tratamento personalizado o Spring pode devolver algo assim:

```json
{
  "timestamp": "2025-07-10T14:35:00.123",
  "status": 500,
  "error": "Internal Server Error",
  "message": "Usuário não encontrado",
  "path": "/users/login"
}
```

Beleza, mas quais os principais problemas do Spring retornar dessa forma?

### Imagem 2: Principais problemas

**Principais problemas** ❗

  * **Não padronizado:** cada erro pode vir de uma forma diferente
  * **Status errado:** sempre retorna 500 mesmo que o erro seja, por exemplo, 404
  * **Inseguro:** pode vazar exceções internas
  * **Inconsistente:** se mudar a versão do Spring ou ativar/desativar o whitelabel, o comportamento pode mudar

### Imagem 3: Criando DTO para resposta de erro

**Criando DTO para resposta de erro** 💬

Crie um DTO simples para formatar a resposta do erro

Em vez de retornar exceções cruas, você pode criar uma classe de resposta clara e estruturada:

```java
public record ErrorResponseDTO(
    int status,
    String message,
    LocalDateTime timestamp
) {}
```

  * Mais controle
  * Mais clareza para o front
  * Mais profissionalismo

### Imagem 4: Tratando erros com @RestControllerAdvice

**Tratando erros com @RestControllerAdvice** ✅

Agora com a excessão personalizada e o DTO criados, capture e trate a exceção globalmente com `@RestControllerAdvice`.

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ErrorResponseDTO> 
    handleNotFound(ResourceNotFoundException ex) {
        return 
        buildErrorResponse(ex.getMessage(), 
        HttpStatus.NOT_FOUND);
    }

    private ResponseEntity<ErrorResponseDTO> 
    buildErrorResponse(String message, HttpStatus 
    status) {
        ErrorResponseDTO error = new 
        ErrorResponseDTO(
            status.value(),
            message,
            LocalDateTime.now()
        );
        return new ResponseEntity<>(error, 
        status);
    }
}
```

### Imagem 5: Como funciona o GlobalExceptionHandler?

**Como funciona o GlobalExceptionHandler?**

  * `@RestControllerAdvice`: permite interceptar erros de forma global, fora dos controllers.
  * `@ExceptionHandler`: captura uma exceção específica (como UserNotFoundException) sempre que ela for lançada.
  * `buildErrorResponse(...)`: centraliza a construção do objeto que será retornado, garantindo padrão e organização.

**Resultado:** um JSON limpo, com status, mensagem e timestamp.

```json
{
  "status": 404,
  "message": "Usuário não encontrado",
  "timestamp": "2025-07-10T14:42:00"
}
```

### Imagem 6: Tratamento de exceções no Spring Boot

**Tratamento de exceções no Spring Boot**

Com exceções personalizadas e GlobalExceptionHandler

### Imagem 7: Exceção Personalizada

**Exceção Personalizada** ✔️

Crie uma exceção personalizada para cenários esperados

Ao invés de lançar diretamente `RuntimeException`, você pode criar uma classe com nome e propósito claro:

```java
public class UserNotFoundException extends 
RuntimeException {
    public UserNotFoundException(String message) {
        super(message);
    }
}
```

**Vantagens:**

  * Mensagem clara e específica
  * Código mais organizado
  * Facilita o tratamento global
