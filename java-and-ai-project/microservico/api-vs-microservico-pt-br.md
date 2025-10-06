# API não é Microserviço: Descomplicando a Diferença

Muita gente ainda confunde: **API não é microserviço**. E tá tudo bem — vamos descomplicar isso de uma vez por todas.

---

## A Analogia do Restaurante

Para entender a diferença de forma simples, imagine um **restaurante**:

| Componente Técnico | No Restaurante | Função |
| :--- | :--- | :--- |
| **A API** | **O Garçom** | Recebe seu pedido, leva-o até a cozinha (os serviços), e depois te entrega o prato pronto (o resultado final). **Você não precisa saber o que rolou lá dentro.** |
| **Os Microserviços** | **Os Cozinheiros** | Cada um tem uma especialidade: um faz a massa, outro o molho, outro a salada. Trabalham de forma **independente**, mas em sincronia para montar o prato. |

---

## Como Funciona na Prática

Na arquitetura de software, o fluxo é:

$$
\text{Seu aplicativo} \rightarrow \text{API (coordena tudo)} \rightarrow \text{Microserviços (executam as tarefas)}
$$

A **API** cuida da **orquestração** e esconde toda a complexidade dos microserviços por trás, apresentando uma interface limpa e única.

---

Essa analogia costuma ajudar muito na hora de explicar para clientes ou times não técnicos.