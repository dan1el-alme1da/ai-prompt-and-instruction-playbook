
-----

# Docker for Java Devs: Why use it and how to get started

## What is Docker?

**Docker** is a platform that allows you to **package your application and all its dependencies into containers**.

**Containers** are like isolated boxes that ensure the app runs **the same way everywhere**. The main benefit is **less "it works on my machine"** and **more consistency** in the execution environment.

## The Problem Without Docker

How many times has your code run on your PC, **but not on the server**?

**Dependencies**, **different configurations**, and **broken environments** severely hinder the *deployment* process. Docker solves this problem by isolating the application's environment.

## Benefits for Java Devs

Using Docker brings significant advantages for those who develop in Java:

  * **Portability:** Your app runs **the same** on Windows, Linux, or Mac.
  * **Consistency:** It doesn't rely on different configurations on every machine.
  * **Agility:** Allows you to spin up dev and test environments **in seconds**.
  * **Integration:** Combines easily with popular Java frameworks like **Spring Boot**.

## Simple Example with Dockerfile

To run a Java application on Docker, you need a **Dockerfile**. This file defines how the container image will be built.

A simple example for a Java app:

```dockerfile
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY target/meuapp.jar app.jar
CMD ["java", "-jar", "app.jar"]
```

After creating the Dockerfile, just run the following commands to build the image and start the container:

```bash
docker build -t meuapp .
docker run -p 8080:8080 meuapp
```

## Docker Compose in Practice

With **Docker Compose**, you can bring up **multiple services together** (for example: your application + a database) with a single command. This is ideal for complex development environments.

Example of the `docker-compose.yml` configuration:

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

To run everything at once, use the command:

```bash
docker-compose up
```

-----