
-----

## O que é logging em uma aplicação Java?

Logging é o processo de **registrar informações sobre a execução da aplicação**. Diferente de `prints` simples, ele permite controlar níveis de detalhe, escolher para onde mandar as mensagens (**console, arquivos, sistemas externos**) e manter histórico.

Em produção, é fundamental para **entender erros e comportamento do sistema**.

```java
Logger logger = Logger.getLogger("NinjaLogger");
logger.info("Naruto iniciou uma nova missão!");
```

-----

## Quais são os principais níveis de log e por que eles são importantes?

Os **níveis** indicam a gravidade ou propósito da mensagem. **INFO** mostra o fluxo normal, **WARNING** sinaliza problemas potenciais, **SEVERE/ERROR** indica falhas críticas, **DEBUG/FINE** dá detalhes de diagnóstico e **TRACE/FINEST** mostra o nível máximo de detalhe.

Essa hierarquia permite **filtrar o que é realmente importante** em cada ambiente.

```java
logger.warning("Chakra do Naruto está baixo!");
logger.severe("Falha na invocação da Kyuubi!");
```

-----

## O que significa ajustar o nível de log em diferentes ambientes (dev, test, prod)?

Significa escolher **quão detalhados serão os registros**. Em dev usamos **DEBUG** ou **TRACE** para enxergar tudo, em produção reduzimos para **INFO** ou **WARNING** para evitar sobrecarga e excesso de dados.

```java
logger.setLevel(Level.FINE); // mais detalhado em dev
logger.fine("Detalhes do treino do Rasengan...");
```

-----

## Como configurar para que os logs sejam gravados em um arquivo em vez do console?

É só adicionar um **destino de saída (Handler)** para o *logger*. Em vez de apenas mostrar no console, podemos **gravar logs em arquivo**, o que é essencial em produção para manter histórico.

```java
FileHandler fh = new FileHandler("naruto.log");
logger.addHandler(fh);
logger.info("Missão de Naruto registrada no pergaminho!");
```

-----

## Por que é importante personalizar o formato das mensagens de log?

Porque logs precisam ser **legíveis por pessoas e ferramentas**. Um formato padronizado com **data/hora e nível** ajuda a **correlacionar eventos**. Sem isso, o log vira só texto solto.

```java
fh.setFormatter(new SimpleFormatter());
logger.info("Naruto aprendeu um novo jutsu!");
```

-----

## Qual é a relação entre logging e monitoramento/observabilidade?

Logging é um dos **pilares da observabilidade**, junto com **métricas e traces**.

Ele dá o **contexto do que realmente aconteceu**. Sem logs, muitas vezes não conseguimos entender o 'porquê' de um problema, mesmo tendo métricas.

```java
logger.info("Naruto iniciou missão rank B");
logger.warning("Missão está demorando mais que o esperado");
```