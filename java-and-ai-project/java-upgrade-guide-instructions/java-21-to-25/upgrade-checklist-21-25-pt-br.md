
---

## *Checklist* de Atualização do Java 21 para o Java 25

---

## Avaliação Pré-Atualização

- [ ] Inventariar o uso de **`sun.misc.Unsafe`** no código-fonte e nas dependências.
- [ ] Identificar o uso de **JNI** (*Java Native Interface*) (direto ou através de bibliotecas).
- [ ] Verificar o uso da biblioteca **ASM** para manipulação de *bytecode*.
- [ ] Revisar comentários **JavaDoc** com muito HTML para conversão para **Markdown**.
- [ ] Avaliar as instruções `switch` atuais em busca de oportunidades de **casamento de padrões** (*pattern matching*).

---

## Atualizações do Sistema de *Build*

### Projetos Maven
- [ ] Atualizar a versão do Java para **25** no `pom.xml`.
- [ ] Atualizar o **POM *parent*** ou **BOM** (*Bill of Materials*) para versões compatíveis com **JDK 25**.
- [ ] Usar `<release>25</release>` no *plugin* do compilador do Maven.

### Projetos Gradle
- [ ] Atualizar `java.toolchain.languageVersion` para **25**.
- [ ] Atualizar o *wrapper* do Gradle para uma versão compatível.

---

## Alterações de Código por JEP (*Java Enhancement Proposal*)

### JEP 466/484: API de Arquivos de Classe (*Class-File API*)
- [ ] Substituir dependências **ASM** pela **Class-File API** padrão.
- [ ] Migrar `ClassReader`/`ClassWriter` para **`ClassModel`**/**`ClassFile`**.
- [ ] Atualizar a lógica de manipulação de *bytecode*.
- [ ] Testar a funcionalidade de transformação de classes.

### JEP 471: Descontinuação do *Unsafe*
- [ ] Substituir o acesso à memória via `sun.misc.Unsafe` por **`VarHandle`**.
- [ ] Migrar operações *off-heap* (fora do *heap*) para **`MemorySegment`** (API FFM).
- [ ] Atualizar bibliotecas de terceiros que utilizam *Unsafe*.
- [ ] Testar exaustivamente os padrões de acesso à memória.

### JEP 472: Restrições de JNI
- [ ] Adicionar a *flag* **`--enable-native-access`** para aplicações JNI.
- [ ] Atualizar descritores de módulo para acesso nativo.
- [ ] Considerar a **FFM API** (*Foreign Function & Memory API*) para novas integrações nativas.
- [ ] Documentar dependências nativas.

### JEP 473: Coletores de *Stream* (*Stream Gatherers*)
- [ ] Identificar operações de *stream* complexas para conversão com **gatherers**.
- [ ] Substituir padrões de *stream* com estado por **gatherers**.
- [ ] Utilizar *gatherers* embutidos de `java.util.stream.Gatherers`.
- [ ] Comparar o desempenho (*benchmark*) das melhorias.

---

## Configuração de Tempo de Execução (*Runtime*)

### Revisão das *Flags* JVM
- [ ] Adicionar **`--enable-native-access=ALL-UNNAMED`** se estiver usando JNI.

### Atualizações do Sistema de Módulos
- [ ] Adicionar declarações de acesso nativo ao `module-info.java`.
- [ ] Atualizar configurações de provedores de serviço (*service provider*).

---

## Estratégia de Teste

### Testes Funcionais
- [ ] Executar o conjunto de testes completo com Java 25.
- [ ] Validar a funcionalidade JNI com os novos avisos.
- [ ] Testar as alterações na manipulação de *bytecode*.

### Testes de Integração
- [ ] Testar com dependências atualizadas.
- [ ] Verificar a compatibilidade de bibliotecas de terceiros.
- [ ] Testar as alterações no *pipeline* de implantação (*deployment*).
- [ ] Validar ferramentas de monitoramento/observabilidade.

---

## Validação Pós-Atualização

## Plano de Reversão (*Rollback*)

- [ ] Documentar o procedimento de reversão.
- [ ] Manter *branches* separadas para Java 21/25, se necessário.

---

## Recomendações de Cronograma

1.  **Fase 1** (Semanas 1-2): Sistema de *build* e compatibilidade básica.
2.  **Fase 2** (Semanas 3-4): Resolver avisos de descontinuação.
3.  **Fase 3** (Semanas 5-6): Adotar seletivamente novos recursos de linguagem.
4.  **Fase 4** (Semanas 7-8): Testes de desempenho e otimização.
5.  **Fase 5** (Semanas 9-10): Implantação em produção e monitoramento.

---

## Mitigação de Riscos

- [ ] Testar primeiro no **ambiente de *staging*** (preparação).
- [ ] Monitorar de perto as métricas da aplicação.
- [ ] Ter um plano de reversão pronto.
- [ ] Atualizar gradualmente entre os ambientes.