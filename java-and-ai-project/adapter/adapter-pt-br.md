
---

## O QUE SÃO ADAPTERS? 5 EXEMPLOS DE USO COM JAVA NA VIDA REAL

---

### Padrão porta-adaptador (Arquitetura Hexagonal)

Uso **adapters** para separar a lógica de negócio da infraestrutura.

Exemplo: `UserPersistenceAdapter` implementa a porta `UserRepositoryPort`, e encapsula as chamadas JPA.

---

### Conversão de DTO para Entity (e vice-versa)

Pra não misturar as camadas, crio **adapters** (ou **mappers**) que convertem `UserDTO` em `UserEntity` e o contrário.

Isso mantém o controller limpo e o domínio separado da persistência.

---

### Uso com bibliotecas legadas ou APIs antigas

Quando o sistema precisa se comunicar com código legado ou SDKs antigos, uso um **adapter** pra traduzir a chamada.

Assim evito espalhar código antigo por todo o projeto.

---

### Integração com serviços de terceiros

Quando preciso chamar APIs de email (como AWS SES ou Mailgun), crio um **EmailServiceAdapter** que transforma meu modelo interno na requisição esperada pela API.

Assim, o resto do sistema não precisa conhecer os detalhes da integração.

---

### Troca fácil de implementação

Tenho um **StorageAdapter** com duas implementações: uma pra AWS S3 e outra pra salvar localmente em disco.

Isso permite trocar o backend de armazenamento sem mudar a lógica principal.

---
