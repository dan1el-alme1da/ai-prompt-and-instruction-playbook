
---


TESTES EM JAVA:
TIPOS, QUAIS
USAR E EM QUAL
MOMENTO
(COM EXEMPLOS
REAIS)

---


Teste Unitário
JUnit + Mockito

Foco: testar métodos isolados, sem
depender de banco, framework ou
sistema externo.
Uso quando quero garantir que a lógica tá
funcionando sozinha, no seco.

Exemplos:
* calcularDesconto() com diferentes
tipos de cliente
* formatarCPF() com entradas válidas e
inválidas
* validarSenha() com combinações de
letras e números
* converterTemperatura() de Celsius
para Fahrenheit
* aplicarMulta() com base em dias de
atraso

---


Teste de Integração
SpringBootTest

Foco: testar a integração real entre partes
do sistema — controllers, services,
repositórios, banco de dados.
Uso quando quero testar se as camadas
realmente funcionam juntas.

Exemplos:
* Criar um usuário via UserController e
verificar se ele está salvo no banco
* Testar PedidoService com um banco
H2 em memória
* Verificar se uma chamada ao
repositório retorna dados corretos
* Testar se @Transactional faz rollback
quando há exceção
* Validar se entidades estão sendo
mapeadas corretamente pelo JPA

---


Teste de API /Contrato
MockMvc ou RestAssured

Foco: testar o comportamento das APIs
(status, corpo da resposta, erros).
Uso pra garantir que a API externa está se
comportando conforme esperado.

Exemplos:
* GET /produtos retorna 200 e lista
esperada
* POST /login retorna 401 com
credenciais erradas
* DELETE /usuario/{id} retorna 404
quando o usuário não existe
* PUT /cliente retorna erro de validação
se o CPF estiver mal formatado
* POST /pedido retorna 201 e o JSON do
pedido criado

---


Teste de Aceitação
E2E, Selenium, Cucumber,
Testcontainers

Foco: testar o comportamento das APIs
(status, corpo da resposta, erros).
Uso pra garantir que a API externa está se
comportando conforme esperado.

Exemplos:
* GET /produtos retorna 200 e lista
esperada
* POST /login retorna 401 com
credenciais erradas
* DELETE /usuario/{id} retorna 404
quando o usuário não existe
* PUT /cliente retorna erro de validação
se o CPF estiver mal formatado
* POST /pedido retorna 201 e o JSON do
pedido criado

---


Teste Manual
pra complementar o que **não é**
automatizado

Foco: interações visuais, fluxos únicos ou
verificação de integração com terceiros.
Uso quando o esforço pra automatizar é
maior que o valor real do teste.

Exemplos:
* Verificar se email chegou com
template correto
* Testar upload de arquivos com
tamanhos e extensões diferentes
* Testar layouts responsivos de
relatórios PDF
* Validar integração com ferramenta
externa de pagamento
* Checar animações e interações
visuais no frontend (quando não tem
testes automatizados)