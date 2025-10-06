
-----

## What is logging in a Java application?

Logging is the process of **recording information about the application's execution**. Unlike simple `prints`, it allows controlling detail levels, choosing where to send the messages (**console, files, external systems**), and maintaining history.

In production, it is fundamental for **understanding errors and system behavior**.

```java
Logger logger = Logger.getLogger("NinjaLogger");
logger.info("Naruto iniciou uma nova missão!");
```

-----

## What are the main log levels and why are they important?

The **levels** indicate the severity or purpose of the message. **INFO** shows the normal flow, **WARNING** signals potential problems, **SEVERE/ERROR** indicates critical failures, **DEBUG/FINE** provides diagnostic details, and **TRACE/FINEST** shows the maximum level of detail.

This hierarchy allows **filtering what is truly important** in each environment.

```java
logger.warning("Chakra do Naruto está baixo!");
logger.severe("Falha na invocação da Kyuubi!");
```

-----

## What does it mean to adjust the log level in different environments (dev, test, prod)?

It means choosing **how detailed the logs will be**. In dev, we use **DEBUG** or **TRACE** to see everything; in production, we reduce it to **INFO** or **WARNING** to avoid overhead and excessive data.

```java
logger.setLevel(Level.FINE); // more detailed in dev
logger.fine("Detalhes do treino do Rasengan...");
```

-----

## How to configure logs to be written to a file instead of the console?

Just add an **output destination (Handler)** to the *logger*. Instead of just showing on the console, we can **write logs to a file**, which is essential in production to maintain history.

```java
FileHandler fh = new FileHandler("naruto.log");
logger.addHandler(fh);
logger.info("Missão de Naruto registrada no pergaminho!");
```

-----

## Why is it important to customize the format of log messages?

Because logs need to be **readable by people and tools**. A standardized format with a **date/time and level** helps to **correlate events**. Without this, the log just becomes loose text.

```java
fh.setFormatter(new SimpleFormatter());
logger.info("Naruto aprendeu um novo jutsu!");
```

-----

## What is the relationship between logging and monitoring/observability?

Logging is one of the **pillars of observability**, along with **metrics and traces**.

It provides the **context of what actually happened**. Without logs, we often cannot understand the 'why' of a problem, even when having metrics.

```java
logger.info("Naruto iniciou missão rank B");
logger.warning("Missão está demorando mais que o esperado");
```