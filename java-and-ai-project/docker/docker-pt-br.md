
Abaixo está o texto gerado em formato Markdown:

-----

# Docker para Desenvolvedores Java: Por que usar e como começar

## O que é o Docker?

**Docker** é uma plataforma que permite **empacotar sua aplicação e todas as dependências em containers**.

**Containers** são como caixinhas isoladas que garantem que o aplicativo rode **igual em qualquer lugar**. O principal benefício é **menos "funciona na minha máquina"** e **mais consistência** no ambiente de execução.

## O Problema Sem Docker

Quantas vezes o seu código rodou no seu PC, **mas não no servidor**?

**Dependências**, **configurações diferentes** e **ambientes quebrados** atrapalham muito o processo de *deploy*. Docker soluciona esse problema ao isolar o ambiente da aplicação.

## Benefícios para Desenvolvedores Java

Usar Docker traz vantagens significativas para quem desenvolve em Java:

  * **Portabilidade:** Seu aplicativo roda **igual** no Windows, Linux ou Mac.
  * **Consistência:** Não depende de configurações diferentes em cada máquina.
  * **Agilidade:** Permite levantar ambientes de desenvolvimento e teste **em segundos**.
  * **Integração:** Combina facilmente com frameworks Java populares como **Spring Boot**.

## Exemplo Simples com Dockerfile

Para rodar uma aplicação Java no Docker, você precisa de um **Dockerfile**. Este arquivo define como a imagem do container será construída.

Um exemplo simples para uma aplicação Java:

```dockerfile
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY target/meuapp.jar app.jar
CMD ["java", "-jar", "app.jar"]
```

Depois de criar o Dockerfile, basta rodar os seguintes comandos para construir a imagem e iniciar o container:

```bash
docker build -t meuapp .
docker run -p 8080:8080 meuapp
```

## Docker Compose na Prática

Com o **Docker Compose**, você pode subir **vários serviços juntos** (por exemplo: sua aplicação + um banco de dados) com um único comando. Isso é ideal para ambientes de desenvolvimento complexos.

Exemplo de configuração do `docker-compose.yml`:

```yaml
version: "3.8"
services:
  app:
    build: .
    ports:
      - "8080:8080"
  db:
    image: postgres:15
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: meuapp
    ports:
      - "5432:5432"
```

Para rodar tudo de uma vez, use o comando:

```bash
docker-compose up
```

-----