
# Checklist de Atualização de Java 17 para Java 21

## Avaliação Pré-Atualização

- [ ] Inventariar o uso de métodos `finalize()` na base de código
- [ ] Identificar cadeias `instanceof` adequadas para conversão em *switch*
- [ ] Avaliar padrões de concorrência para benefícios das **Threads Virtuais**
- [ ] Revisar as configurações atuais de *thread pool*
- [ ] Verificar o uso explícito de *charset* (oportunidade de migração para UTF-8)
- [ ] Identificar o uso de carregamento dinâmico de *agent*

---

## Atualizações do Sistema de Build

### Projetos Maven
- [ ] Atualizar a versão Java para **21** no `pom.xml`
- [ ] Adicionar `--enable-preview` se estiver usando recursos de *preview*
- [ ] Atualizar as dependências para versões compatíveis com **Java 21**
- [ ] Configurar o *plugin* surefire para recursos de *preview* se necessário

### Projetos Gradle
- [ ] Atualizar `java.toolchain.languageVersion` para **21**
- [ ] Adicionar *flags* de recurso de *preview* se necessário
- [ ] Atualizar o *Gradle wrapper* para uma versão compatível
- [ ] Atualizar *plugins* de *build* para versões compatíveis com Java 21

---

## Migração de Recursos da Linguagem

### JEP 441: Pattern Matching para switch
- [ ] Identificar cadeias `instanceof` para conversão
- [ ] Converter *switch* tradicional para **switch com *pattern matching***
- [ ] Adicionar **padrões guardados** (*guarded patterns*) onde for benéfico
- [ ] Testar exaustividade do padrão e segurança *null*

### JEP 440: Record Patterns
- [ ] Identificar oportunidades de **desestruturação de *record***
- [ ] Converter acesso manual a campos para **record patterns**
- [ ] Usar *record patterns* aninhados para estruturas de dados complexas
- [ ] Testar *pattern matching* com classes seladas

### JEP 444: Virtual Threads (Threads Virtuais)
- [ ] Avaliar operações com uso intenso de I/O para benefícios das **Virtual Threads**
- [ ] Substituir *thread pools* fixos por **executores de *virtual thread***
- [ ] Atualizar o uso de `ThreadLocal` (considerar `ScopedValues`)
- [ ] Testar em cenários de alta concorrência
- [ ] Monitorar desempenho e uso de recursos

### JEP 431: Sequenced Collections (Coleções Sequenciadas)
- [ ] Substituir `list.get(0)` por **`list.getFirst()`**
- [ ] Substituir `list.get(list.size()-1)` por **`list.getLast()`**
- [ ] Usar o método **`reversed()`** em vez de reversão manual
- [ ] Atualizar padrões de iteração de coleções

---

## Recursos de Preview (Opcional)

### JEP 430: String Templates (*Preview*)
- [ ] Avaliar concatenação de *string* para conversão em **template**
- [ ] Habilitar recursos de *preview* na configuração de *build*
- [ ] Usar processadores de *template* apropriados (STR, HTML, SQL)
- [ ] Testar segurança e desempenho de *template*

### JEP 443: Unnamed Patterns and Variables (*Preview*)
- [ ] Substituir variáveis não utilizadas por **`_`**
- [ ] Simplificar padrões de tratamento de exceção
- [ ] Atualizar *switch expressions* com padrões sem nome

### JEP 446: Scoped Values (*Preview*)
- [ ] Identificar o uso de `ThreadLocal` adequado para **ScopedValues**
- [ ] Migrar padrões de propagação de contexto
- [ ] Testar com cargas de trabalho de *virtual thread*

---

## Alterações de API e Runtime

### JEP 400: UTF-8 por Padrão
- [ ] Remover especificações explícitas de **`StandardCharsets.UTF_8`**
- [ ] Testar operações de I/O de arquivo em diferentes plataformas
- [ ] Atualizar a documentação sobre o comportamento do *charset*

### JEP 408: Simple Web Server
- [ ] Substituir servidores HTTP de teste pelo **jwebserver** integrado
- [ ] Atualizar fluxos de trabalho de desenvolvimento/teste
- [ ] Considerar a API `HttpServer` para casos de uso simples

### JEP 418: Internet-Address Resolution SPI
- [ ] Avaliar necessidades de resolução de DNS personalizada
- [ ] Implementar `InetAddressResolverProvider` se necessário

### JEP 452: Key Encapsulation Mechanism API
- [ ] Avaliar requisitos de criptografia pós-quântica
- [ ] Atualizar código criptográfico se estiver usando KEMs

---

## Depreciações e Avisos

### JEP 421: Finalization Deprecation
- [ ] Remover todas as implementações de método **`finalize()`**
- [ ] Substituir por padrões da **API Cleaner**
- [ ] Usar *try-with-resources* para gerenciamento de recursos
- [ ] Atualizar padrões de limpeza de recursos

### JEP 451: Dynamic Agent Loading Warnings
- [ ] Identificar o carregamento dinâmico de *agent* na aplicação
- [ ] Adicionar `-XX:+EnableDynamicAgentLoading` se necessário
- [ ] Atualizar as ferramentas para usar o carregamento de *agent* na inicialização
- [ ] Documentar requisitos de *agent* para *deployment*

---

## Configuração de Runtime

### Coleta de Lixo (*Garbage Collection*)
- [ ] Testar com configurações de GC padrão
- [ ] Avaliar **ZGC Geracional** (`-XX:+UseZGC -XX:+ZGenerational`)
- [ ] Fazer *benchmark* das melhorias de desempenho do GC
- [ ] Atualizar monitoramento e alertas de GC

### Configuração de Virtual Thread
- [ ] Definir contagem apropriada de *carrier thread* se necessário
- [ ] Configurar parâmetros do **agendador de *virtual thread***
- [ ] Atualizar o monitoramento para métricas de *virtual thread*
- [ ] Testar detecção e mitigação de *pinning*

### Revisão de Flags da JVM
- [ ] Remover *flags* obsoletos da JVM
- [ ] Adicionar *flags* de recurso de *preview* se necessário
- [ ] Atualizar *scripts* de *deployment* em produção
- [ ] Documentar alterações de *flag* para a equipe de operações

---

## Estratégia de Teste

### Testes Funcionais
- [ ] Executar o *suite* de testes completo com **Java 21**
- [ ] Testar a exaustividade do *pattern matching*
- [ ] Validar o comportamento das *Virtual Threads* sob carga
- [ ] Testar alterações de *charset* em diferentes plataformas
- [ ] Verificar a funcionalidade de carregamento de *agent*

### Testes de Desempenho
- [ ] Fazer *benchmark* da taxa de transferência da aplicação
- [ ] Testar a **escalabilidade das Virtual Threads**
- [ ] Medir melhorias de desempenho do GC
- [ ] Fazer *profile* do desempenho de *pattern matching*
- [ ] Testar padrões de uso de memória

### Testes de Concorrência
- [ ] Teste de estresse em aplicações com *Virtual Thread*
- [ ] Testar a propagação de contexto de **ScopedValues**
- [ ] Validar operações *thread-safe*
- [ ] Testar padrões de **concorrência estruturada**

---

## Implementação da Migração

### Fase 1: Fundação (Semanas 1-2)
- [ ] Atualizar sistema de *build* e dependências
- [ ] Lidar com avisos de depreciação
- [ ] Testes básicos de compatibilidade

### Fase 2: Recursos Principais (Semanas 3-4)
- [ ] Implementar **pattern matching para *switch***
- [ ] Adicionar **record patterns** onde for benéfico
- [ ] Atualizar padrões de uso de coleções

### Fase 3: Concorrência (Semanas 5-6)
- [ ] Migrar para **Virtual Threads** estrategicamente
- [ ] Atualizar configurações de *thread pool*
- [ ] Testar sob carga realista

### Fase 4: Recursos de Preview (Semanas 7-8)
- [ ] Habilitar seletivamente recursos de *preview*
- [ ] Implementar **String Templates** se benéfico
- [ ] Testar a estabilidade do recurso de *preview*

### Fase 5: Otimização (Semanas 9-10)
- [ ] Ajuste de desempenho e otimização de GC
- [ ] Preparação para *deployment* em produção
- [ ] Atualizações de monitoramento e alerta

---

## Validação Pós-Migração

### Monitoramento de Desempenho
- [ ] Configurar o **monitoramento de *Virtual Thread***
- [ ] Monitorar o comportamento e desempenho do GC
- [ ] Rastrear taxa de transferência/latência da aplicação
- [ ] Monitorar a utilização de recursos

### Prontidão Operacional
- [ ] Atualizar procedimentos de *deployment*
- [ ] Treinar a equipe de operações sobre novos recursos
- [ ] Atualizar guias de solução de problemas
- [ ] Documentar alterações de configuração

### Atualizações de Documentação
- [ ] Atualizar a documentação da API
- [ ] Documentar o uso de novos recursos da linguagem
- [ ] Atualizar a configuração do ambiente de desenvolvimento
- [ ] Criar lições aprendidas com a migração

---

## Plano de Reversão (*Rollback*)

- [ ] Manter a compatibilidade com Java 17 durante a transição
- [ ] Manter configurações da JVM anteriores
- [ ] Documentar procedimentos de reversão
- [ ] Testar cenários de reversão
- [ ] Manter *build profiles* separados se necessário

---

## Mitigação de Riscos

- [ ] Testar primeiro em ambientes de *staging*
- [ ] Usar *feature flags* para grandes alterações
- [ ] Monitorar de perto as métricas da aplicação
- [ ] Ter suporte especializado disponível
- [ ] Planejar estratégia de implementação gradual

---

## Critérios de Sucesso

- [ ] Todos os testes passando em **Java 21**
- [ ] O desempenho atende ou supera a linha de base do Java 17
- [ ] Nenhuma regressão na funcionalidade
- [ ] Equipe treinada em novos recursos
- [ ] *Deployment* em produção bem-sucedido
- [ ] Monitoramento e alerta funcionais

---

## Considerações de Longo Prazo

- [ ] Planejar a padronização dos recursos de *preview*
- [ ] Avaliar oportunidades adicionais de **Virtual Thread**
- [ ] Considerar expansões de *pattern matching*
- [ ] Monitorar recursos do **Java 22+** para futuras atualizações
- [ ] Atualizar padrões de codificação e melhores práticas