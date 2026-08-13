# Documentação Modular e Skills do Ecossistema SC

## Objetivo

Transformar a modelagem consolidada do ecossistema Software Center em uma documentação modular, navegável e orientada às responsabilidades de cada microserviço, sem alterar os quatro planos de ação históricos existentes.

A nova documentação será a referência operacional para evolução do ecossistema. Os planos originais, inclusive a V4, permanecerão como registro histórico e fonte de rastreabilidade.

## Escopo

O trabalho abrange estes domínios:

- SC Ecossistema, para decisões e fluxos transversais;
- SC CP, o Control Plane;
- SC SSO, o provedor de identidade e Authorization Server;
- SC AG, o gateway de autenticação e segurança de borda;
- SC Sign, o motor de assinaturas e evidências;
- SC Wpp, o motor de notificações via WhatsApp e canais alternativos;
- SC Billing, o motor de medição e faturamento consolidado;
- seis skills de repositório, uma para cada aplicação ou microserviço.

Não fazem parte do escopo:

- alterar, mover ou renomear os quatro planos de ação existentes;
- modificar a skill pessoal `sc` existente em `C:\Users\gerso\.agents\skills\sc`;
- implementar código dos microserviços;
- criar endpoints, schemas, claims, scopes ou regras que não estejam sustentados pela modelagem atual;
- transformar as skills em plugin distribuível.

## Princípios de organização

1. Cada documento deve ter uma responsabilidade clara e permanecer compreensível isoladamente.
2. Uma regra pertence ao serviço que a implementa. Fluxos ou decisões que envolvem vários serviços pertencem a `SC Ecossistema`.
3. Conteúdo compartilhado não deve ser copiado integralmente para pastas de serviços. Os documentos específicos devem apontar para a fonte transversal.
4. A V4 é a principal fonte histórica para extração, complementada pela skill `sc` quando ela expressar contratos reais já implementados.
5. Divergências entre a V4 e a skill `sc` devem privilegiar o contrato real da skill `sc` e ser registradas explicitamente como decisão ou limitação atual.
6. Lacunas devem ser declaradas como decisões pendentes. Nenhum contrato será inventado para completar a documentação.
7. Os arquivos terão nomes descritivos em português. Não será criado arquivo chamado `README.md`.

## Estrutura da documentação

```text
SC_Modelagem/
├── Plano de Ação - Ecossistema Software Center (SC).md
├── Plano de Ação V2 - Ecossistema Software Center (SC).md
├── Plano de Ação V3 - Ecossistema Software Center (SC).md
├── Plano de Ação V4 - Ecossistema SoftwareCenter (SC).md
├── SC Ecossistema/
│   ├── Visão Geral do Ecossistema SC.md
│   ├── Arquitetura e Integrações Entre Serviços.md
│   ├── Segurança Compartilhada.md
│   ├── Observabilidade, Disponibilidade e Operação.md
│   ├── Requisitos e Rastreabilidade.md
│   └── Roadmap e Riscos.md
├── SC CP/
│   ├── Responsabilidades do SC CP.md
│   ├── Modelo de Domínio do SC CP.md
│   └── Contratos e Fluxos do SC CP.md
├── SC SSO/
│   ├── Responsabilidades do SC SSO.md
│   ├── Modelo de Identidade do SC SSO.md
│   └── Contratos e Fluxos de Autenticação.md
├── SC AG/
│   ├── Responsabilidades do SC AG.md
│   ├── Segurança e Políticas de Borda.md
│   └── Roteamento, Cache e Resiliência.md
├── SC Sign/
│   ├── Responsabilidades do SC Sign.md
│   ├── Modelo de Assinatura e Evidências.md
│   └── Contratos e Fluxos do SC Sign.md
├── SC Wpp/
│   ├── Responsabilidades do SC Wpp.md
│   ├── Entrega, Retentativas e Fallbacks.md
│   └── Contratos, Webhooks e Consentimento.md
└── SC Billing/
    ├── Responsabilidades do SC Billing.md
    ├── Modelo de Medição e Faturamento.md
    └── Contratos e Fluxos do SC Billing.md
```

`Visão Geral do Ecossistema SC.md` será o ponto inicial de navegação. Ele explicará a fonte oficial, preservará links para o histórico e apontará para todos os domínios e documentos transversais.

## Conteúdo transversal

### Visão geral

Apresentará propósito, limites dos serviços, mapa de responsabilidades, glossário essencial e índice navegável. O diagrama de componentes da V4 será revisado somente para refletir a nomenclatura aprovada: SC CP, SC SSO, SC AG, SC Sign, SC Wpp e SC Billing.

### Arquitetura e integrações

Concentrará topologia, comunicação service-to-service, relações entre bancos, FK lógica, eventos de identidade, login, MFA, logout, proxy autenticado, licenças M2M e revogação. Cada fluxo deverá indicar claramente o proprietário de cada etapa.

### Segurança compartilhada

Concentrará princípios de zero confiança, mTLS, OAuth2, proteção de cookies e CSRF, isolamento por tenant, tratamento de segredos, matriz de ameaças e divisão das responsabilidades de segurança.

### Operação

Concentrará alta disponibilidade, circuit breaker, fallback do Redis, observabilidade, métricas, tracing, WebSocket isolado por tenant, ambientes de produção e sandbox e metas não funcionais operacionais.

### Requisitos, roadmap e riscos

Os requisitos serão reorganizados por serviço e por fluxo transversal, preservando seus identificadores. O roadmap manterá dependências entre fases. Os riscos serão associados ao serviço responsável e ao domínio transversal relacionado.

## Conteúdo por serviço

Cada serviço terá três documentos com o seguinte contrato editorial:

1. **Responsabilidades**: propósito, capacidades, dados dos quais é dono, dependências, limites explícitos e responsabilidades que não pertencem ao serviço.
2. **Modelo**: entidades, estados, invariantes e relacionamentos relevantes ao serviço. No SC AG, que não é dono de domínio persistente principal, este documento é substituído por políticas de borda.
3. **Contratos e fluxos**: entradas, saídas, integrações, erros esperados, requisitos rastreáveis e decisões pendentes.

Os documentos específicos incluirão links relativos para conteúdos transversais e para documentos vizinhos quando houver dependência direta.

## Fonte e tratamento de divergências

A extração seguirá esta precedência:

1. Contratos reais descritos pela skill pessoal `sc`;
2. decisões consolidadas no Plano de Ação V4;
3. versões V1 a V3 apenas para contexto histórico ou informação não contraditória que tenha sido omitida da V4.

Quando duas fontes divergirem, o novo documento deverá conter uma seção `Decisões e lacunas` que explique:

- qual comportamento foi adotado;
- qual fonte o sustenta;
- qual ponto ainda depende de decisão arquitetural ou implementação.

As referências internas da V4, como `§5.3`, serão substituídas por links relativos e títulos explícitos. A nova documentação não dependerá da numeração de seções dos planos históricos.

## Skills de repositório

As skills serão criadas em:

```text
.agents/skills/
├── sc-cp/
├── sc-sso/
├── sc-ag/
├── sc-sign/
├── sc-wpp/
└── sc-billing/
```

Cada diretório conterá:

```text
<skill>/
├── SKILL.md
└── agents/
    └── openai.yaml
```

As skills serão focadas em orientar trabalho de análise, implementação e manutenção no domínio correspondente. Cada `SKILL.md` terá:

- `name` único e descritivo;
- `description` iniciada por `Use when...`, com gatilhos e limites explícitos;
- instrução para ler somente os documentos modulares necessários;
- mapa das responsabilidades e invariantes que não podem ser violados;
- procedimento para validar mudanças contra contratos transversais;
- checklist curto de implementação e revisão;
- orientação para registrar lacunas em vez de inventar contratos.

Os arquivos `agents/openai.yaml` fornecerão `display_name`, `short_description` e `default_prompt`, derivados do conteúdo de cada skill.

As skills poderão ser invocadas explicitamente e terão descrições adequadas à ativação implícita. Não duplicarão toda a documentação: usarão links para os documentos do repositório, preservando a divulgação progressiva de contexto.

## Estratégia de teste das skills

Cada skill será criada e validada individualmente antes da próxima, seguindo RED, GREEN e REFACTOR:

1. Definir prompts de recuperação e aplicação para o domínio sem usar a nova skill.
2. Registrar falhas do comportamento base, como confusão de responsabilidade, invenção de contrato ou leitura de fonte errada.
3. Criar a versão mínima da skill que corrige as falhas observadas.
4. Validar frontmatter, estrutura, metadados e resolução dos links.
5. Executar os mesmos prompts com a skill disponível e verificar que o agente localiza a documentação correta, preserva limites e não inventa contratos.
6. Corrigir ambiguidades identificadas e repetir o teste.

Por serem majoritariamente skills de referência e procedimento, os testes privilegiarão recuperação correta, aplicação dos limites e identificação de lacunas. Não haverá scripts adicionais sem uma necessidade determinística comprovada.

## Validação da documentação

A entrega será aceita quando:

- os hashes dos quatro planos históricos permanecerem iguais aos anteriores à mudança;
- todas as pastas e arquivos previstos existirem;
- não houver arquivo novo chamado `README.md`;
- todos os links Markdown relativos apontarem para arquivos existentes;
- todos os blocos Mermaid estiverem corretamente delimitados;
- os seis serviços tiverem propósito, responsabilidades, dados próprios, dependências, limites, contratos, requisitos e lacunas documentados;
- responsabilidades transversais estiverem na pasta `SC Ecossistema`, sem duplicação extensa;
- as seis skills tiverem frontmatter válido, metadados coerentes e testes documentados;
- `git diff --check` não reportar problemas de whitespace;
- a árvore final estiver navegável a partir de `Visão Geral do Ecossistema SC.md`.

## Política de commits

A implementação será organizada em commits pequenos e verificáveis:

1. especificação aprovada;
2. documentação transversal;
3. documentação dos serviços;
4. uma entrega validada por skill ou um agrupamento final somente se os testes individuais estiverem registrados;
5. correções de links e validação final.

Nenhum commit modificará os quatro planos históricos.
