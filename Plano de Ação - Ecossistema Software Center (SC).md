# Plano de Ação: Ecossistema Software Center (SC)

Este documento centraliza as fases de planejamento, os padrões arquiteturais e as diretrizes de comunicação para a construção da SC, da SC SSO e das aplicações clientes.

## 1. Fases de Planejamento e Documentação

### Fase 1: Prototipação Visual (Pencil / Figma)

- **Telas Nativas de Autenticação:** Desenhar os componentes de login, recuperação de senha e MFA que residirão no frontend das aplicações clientes (PWA/Vue.js). O objetivo é manter a experiência do usuário fluida (White-label), sem redirecionamentos visíveis para o SSO.
- **Gestão de RBAC:** Mapear a interface onde o Tenant (cliente) configurará Cargos e Permissões dentro da aplicação. O design deve refletir a integração via BFF com a SC, sem expor a complexidade da infraestrutura subjacente.

### Fase 2: Modelagem de Dados (Mermaid ERD)

É fundamental manter bancos de dados isolados para garantir segurança, escalabilidade e conformidade.

- **Banco da SC SSO (Identity):**
  - Tabelas focadas estritamente em identidade: `users`, `credentials`, `active_sessions`, `refresh_tokens`.
  - _Regra de Ouro:_ A SC SSO não sabe o que é um contrato, o que é um Tenant ou o que é RBAC. Ela apenas valida senhas e assina tokens.
- **Banco da SC (Control Plane):**
  - O cérebro do ecossistema B2B2B. Tabelas principais: `tenants`, `members` (para mapear a relação N:N entre `users` e `tenants`), `apps_catalog` (projetos abstratos), `projects` (contratos ativos por tenant), `roles`, e `permissions`.
  - _Nota:_ Um usuário é único (`users`), mas sua associação a um tenant se dá através da tabela `members`, que carrega as roles específicas dele naquele contexto.
- **Bancos das Aplicações Clientes:**
  - Tabelas de domínio específicas (ex: notas_fiscais, balancos) isoladas por um identificador universal (`tenant_id`).

### Fase 3: Especificação das APIs e Manifestos

- **Manifesto de Aplicações:** Definir o contrato JSON que as aplicações enviarão para a SC informando as rotas e permissões exigidas.
- **Contratos de API (Swagger/OpenAPI):** Documentar as APIs do BFF para o Frontend, e do BFF para a SC e SC SSO.

---

## 2. Soluções de Arquitetura e Comunicação

Para atender à necessidade de telas de login nativas sem redirecionamentos visíveis (o fim do HTTP 302 na visão do usuário) e mantendo a segurança exigida pelas práticas modernas (OAuth 2.1), a arquitetura adotará o padrão **BFF (Backend-For-Frontend)**.

### A. Como o BFF troca o Token JWT por Cookie?

O processo de autenticação é delegado ao BFF, que atua como um mediador seguro (Confidential Client):

1. O Frontend (Vue) envia um POST tradicional (`/api/login`) contendo credenciais e o cabeçalho `X-Tenant-ID` para o BFF.
2. O BFF inicia o _Authorization Code Flow_ com PKCE (ou uma chamada direta de API confidencial) junto à SC SSO.
3. A SC SSO valida as credenciais e retorna os tokens (Access Token, ID Token, Refresh Token) para o BFF.
4. **A Mágica:** O BFF armazena os tokens em cache ou no servidor e retorna ao Frontend **apenas um Cookie**.
5. Este Cookie deve ter as flags `Secure`, `HttpOnly` e `SameSite=Lax` ou `Strict`. Como o JavaScript não consegue ler este cookie, a aplicação fica protegida contra roubo de tokens por XSS. O navegador enviará esse cookie automaticamente nas próximas requisições.

### B. Segurança na Comunicação Nativa e Resolução de Contexto (O "Login Ninja")

Para que a aplicação saiba qual Tenant está acessando sem precisar perguntar ao usuário:

- **Identificação Autônoma:** O Frontend resolve o tenant com base no subdomínio (ex: `cliente.sua-aplicacao.com.br`) e embute essa informação no cabeçalho `X-Tenant-ID` de todas as requisições Axios.
- **Comunicação BFF <-> SC SSO:** A requisição de login ocorre via backend (_Server-to-Server_). O BFF utiliza credenciais próprias (`client_id` e `client_secret`) para se autenticar na SC SSO e repassa as credenciais do usuário e o `tenant_id`.
- **JWT Multi-Tenant:** A SC SSO decifra o contexto e emite um JWT que já contém a claim personalizada `tenant_id`. Todas as requisições subsequentes do BFF para a SC (ou outras APIs) utilizarão este JWT.
- **Defesa:** O BFF deve implementar políticas rigorosas de _Rate Limiting_ (por IP e por Tenant) para prevenir ataques de força bruta, já que a tela do frontend lidará com senhas em texto limpo até que cheguem ao BFF.

### C. A SC SSO Precisa de Frontend?

Neste cenário altamente customizado e White-label, onde o login ocorre nos componentes nativos da PWA do cliente, a SC SSO atua como um **Headless Identity Provider**.

- Ela será uma aplicação puramente de backend (um microsserviço).
- Seus endpoints cobrirão todo o ciclo de vida da autenticação: `/api/login`, `/api/logout`, `/api/refresh-token`, etc.
- O BFF da aplicação cliente cuidará de todo o fluxo visual.

### D. RBAC Gerido pelo Tenant (Interface Nativa, Persistência Remota)

A gestão de RBAC (Role-Based Access Control) será descentralizada na visão do cliente, mas centralizada na persistência.

1. O administrador (Tenant) acessa a tela de "Gestão de Acessos" na aplicação cliente.
2. Ao criar um novo cargo (Role) ou alterar permissões, o Frontend faz um POST para o BFF da aplicação.
3. O BFF atua como proxy, enviando a requisição para o Control Plane (a SC).
4. A SC persiste as alterações no banco de dados central (`roles`, `permissions`, `members_roles`), vinculadas ao `tenant_id` e `project_id`.
5. No próximo login (ou renovação de token) de um usuário daquele Tenant, a SC SSO consultará a SC, e o novo JWT será emitido com as claims de permissão atualizadas.
