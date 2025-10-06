# Padrões de microsserviços: o que você precisa saber para criar sistemas escaláveis

Se você começou a se afastar dos monólitos ou está pensando nisso, os **microsserviços** provavelmente parecem ser o caminho a seguir. Mas dividir seu aplicativo em partes menores não é suficiente. Você precisa dos padrões certos para fazê-los funcionar de forma confiável e em escala.

Nesta postagem, vou guiá-lo pelos principais **padrões de microsserviços** para ajudá-lo a criar sistemas escaláveis e mais silenciosos.

Vamos entrar nisso.

## Padrões de decomposição

Esses padrões ajudam você a organizar os serviços de uma maneira que faça sentido e permaneça gerenciável.

* O **design orientado a domínio** ajuda a lógica de grupo em torno das necessidades reais de negócios.
* Os **contextos limitados** dão a cada serviço uma responsabilidade clara.
* Com o **Backend for Frontend**, cada interface do usuário recebe sua própria API personalizada.

## Padrões de integração

* Um **API Gateway** ou **Facade** simplifica o acesso para usuários externos.
* Use a **coreografia** quando os serviços se comunicarem por conta própria.
* Escolha **Orquestração** quando um serviço controlar o fluxo.
* Adicione **Service Mesh**, **Service Registration** e **Discovery** para lidar com a comunicação e a pesquisa de serviços nos bastidores.

## Padrões de configuração

* Mantenha as **configurações externas** para evitar a reimplantação para pequenas alterações.
* Use um **armazenamento de configuração centralizado** se precisar de configurações consistentes.
* Go **Distributed** quando cada serviço precisar operar de forma mais independente.

## Padrões de controle de versão

Deixe os serviços evoluírem sem quebrar as coisas.

* O **versionamento semântico** fornece uma abordagem estruturada para gerenciar alterações.
* O **controle de versão da API** ajuda os clientes a continuar trabalhando enquanto você atualiza nos bastidores.

## Padrões de banco de dados

* Dê a cada serviço seu **próprio banco de dados** para reduzir o acoplamento.
* Use o **CQRS** para separar comandos de consultas.
* Aplique **Saga** e **Compensador de Transações** quando precisar manter os dados consistentes em todos os serviços.

## Padrões de resiliência

Os serviços falharão. É melhor esperar e planejar isso.

* Use **Retry** e **Circuit Breaker** para lidar com erros transitórios.
* Defina **tempos limite** para que os serviços não fiquem pendurados para sempre.
* Adicione **fallbacks**, **failover** e **redundância** para melhorar o uptimme.

## Padrões de implantação

Você quer lançar recursos com confiança, não com medo.

* As **implantações Blue-Green** e **Canary** ajudam a reduzir o risco.
* Use **alternâncias de recursos** para testar sem implantar novo código.
* Planeje **lançamentos graduais** em vez de lançamentos **big-bang**.

## Padrões de observabilidade

Se você não puder monitorar os serviços, poderá apoiá-los.

* O **rastreamento distribuído** ajuda você a acompanhar as solicitações entre os serviços.
* As **APIs de verificação de integridade** e a **agregação de logs** ajudam a monitorar o status do serviço.
* Adicione **logs de auditoria**, **correlação de eventos** e **engenharia de Choas** para ficar à frente dos problemas antes que os usuários os percebam.

## Padrões de segurança

* Cubra o básico: **Autenticação**, **Autorização** e **Validação de Entrada**.
* Adicione **limitação de taxa** e **criptografia** para proteger os dados e o desempenho.
* Use **OAuth**, **RBAC** e **Valet Key** para acesso seguro e controlado.