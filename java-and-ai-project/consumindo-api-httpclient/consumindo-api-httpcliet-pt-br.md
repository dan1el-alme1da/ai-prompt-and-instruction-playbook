
-----

## Imagem 1

**Consumindo APIs no Java com HttpClient**

-----

O **HttpClient** é a forma moderna e oficial de fazer requisições HTTP no Java **(desde o Java 11).**

  * **API mais simples e legível**
  * **Suporte nativo a HTTP/2**
  * **Requisições assíncronas com CompletableFuture**
  * **Não precisa de libs externas**
    (Imagem de um braço robótico apertando a mão de um braço humano)

-----

**Fazer uma requisição GET em Java nunca foi tão simples com o HttpClient.**

```java
public class Main {
    public static void main(String[] args) throws
    Exception {
        HttpClient client =
        HttpClient.newHttpClient();

        HttpRequest request =
        HttpRequest.newBuilder()
        .uri(URI.create("https://viacep.com.br/ws/01001000/json/"))
        .build();

        HttpResponse<String> response =
        client.send(request,
        HttpResponse.BodyHandlers.ofString());

        System.out.println(response.body());
    }
}
```

-----

**Entenda as classes usadas:**

  * **HttpClient** $\rightarrow$ cria o cliente HTTP que faz as requisições.
  * **HttpRequest** $\rightarrow$ representa a requisição (URL, método, headers).
  * **HttpResponse** $\rightarrow$ armazena a resposta recebida da API.
  * **BodyHandlers** $\rightarrow$ define como o corpo da resposta será tratado (ex.: texto, arquivo, bytes).

-----


**Resultado da resposta**

```json
{
"cep": "01001-000",
"logradouro": "Praça da Sé",
"complemento": "lado ímpar",
"bairro": "Sé",
"localidade": "São Paulo",
"uf": "SP",
"ibge": "3550308",
"gia": "1004",
"ddd": "11",
"siafi": "7107"
}
```

O retorno vem como **String (JSON)**, mas para trabalhar melhor com os dados, podemos usar uma biblioteca como **Jackson** ou **Gson** para converter em objeto Java.

-----
