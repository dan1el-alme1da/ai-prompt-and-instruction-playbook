
-----


**Consuming APIs in Java with HttpClient**
(Image of the Java Duke mascot diving into a coffee cup with the bar "http" underneath)

-----


The **HttpClient** is the modern and official way to make HTTP requests in Java **(since Java 11).**

  * **Simpler and more readable API**
  * **Native support for HTTP/2**
  * **Asynchronous requests with CompletableFuture**
  * **No need for external libs**
    (Image of a robotic arm shaking a human arm)

-----


**Making a GET request in Java has never been so simple with HttpClient.**

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


**Understand the classes used:**

  * **HttpClient** $\rightarrow$ creates the HTTP client that makes the requests.
  * **HttpRequest** $\rightarrow$ represents the request (URL, method, headers).
  * **HttpResponse** $\rightarrow$ stores the response received from the API.
  * **BodyHandlers** $\rightarrow$ defines how the response body will be handled (e.g.: text, file, bytes).

-----

**Response result**

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

The return comes as a **String (JSON)**, but to work better with the data, we can use a library like **Jackson** or **Gson** to convert it into a Java object.