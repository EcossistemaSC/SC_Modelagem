# Documentação Modular e Skills do Ecossistema SC Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Criar a referência modular do ecossistema Software Center e seis skills de repositório sem modificar os quatro planos históricos.

**Architecture:** A pasta `SC Ecossistema` será o ponto de entrada e a fonte das decisões transversais. Cada microserviço terá três documentos focados, enquanto cada skill em `.agents/skills` carregará apenas o procedimento essencial e encaminhará o agente aos documentos modulares relevantes.

**Tech Stack:** Markdown, Mermaid, YAML, Git, PowerShell e o validador Python oficial do `skill-creator`.

## Global Constraints

- Não alterar, mover ou renomear os quatro planos de ação existentes.
- Não criar arquivos chamados `README.md`.
- Usar a skill pessoal `sc` como precedência para contratos reais e a V4 como fonte arquitetural consolidada.
- Registrar divergências e lacunas; não inventar endpoints, schemas, claims, scopes ou regras.
- Manter decisões transversais em `SC Ecossistema` e evitar duplicação extensa nos documentos de serviços.
- Criar e validar uma skill antes de iniciar a próxima.
- Não modificar `C:\Users\gerso\.agents\skills\sc`.

---

### Task 1: Baseline histórico e navegação do ecossistema

**Files:**
- Create: `SC Ecossistema/Visão Geral do Ecossistema SC.md`
- Create: `docs/superpowers/verification/hashes-planos-historicos.sha256`

**Interfaces:**
- Consumes: os quatro planos históricos e a especificação aprovada.
- Produces: ponto inicial de navegação e hashes usados pelo gate final.

- [ ] **Step 1: Capturar os hashes dos quatro planos**

Run:

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath @(
  'Plano de Ação - Ecossistema Software Center (SC).md',
  'Plano de Ação V2 - Ecossistema Software Center (SC).md',
  'Plano de Ação V3 - Ecossistema Software Center (SC).md',
  'Plano de Ação V4 - Ecossistema SoftwareCenter (SC).md'
)
```

Persistir no arquivo de verificação uma linha por arquivo no formato `<hash> *<nome>`.

- [ ] **Step 2: Confirmar que o ponto de navegação ainda não existe**

Run: `Test-Path -LiteralPath 'SC Ecossistema/Visão Geral do Ecossistema SC.md'`

Expected: `False`.

- [ ] **Step 3: Criar a visão geral**

O documento deve conter: status da documentação modular, precedência de fontes, mapa dos seis serviços, tabela de responsabilidades, diagrama de componentes, links para todos os documentos planejados, histórico preservado e glossário essencial.

- [ ] **Step 4: Validar os links já materializados e os links planejados**

Run: verificar manualmente que os links para os quatro planos apontam aos nomes exatos e que os links para os documentos novos correspondem exatamente à árvore da especificação.

- [ ] **Step 5: Commit**

```powershell
git add -- 'SC Ecossistema/Visão Geral do Ecossistema SC.md' 'docs/superpowers/verification/hashes-planos-historicos.sha256'
git commit -m "docs: add SC ecosystem navigation"
```

### Task 2: Documentação transversal

**Files:**
- Create: `SC Ecossistema/Arquitetura e Integrações Entre Serviços.md`
- Create: `SC Ecossistema/Segurança Compartilhada.md`
- Create: `SC Ecossistema/Observabilidade, Disponibilidade e Operação.md`
- Create: `SC Ecossistema/Requisitos e Rastreabilidade.md`
- Create: `SC Ecossistema/Roadmap e Riscos.md`

**Interfaces:**
- Consumes: V4, contratos reais da skill `sc` e navegação da Task 1.
- Produces: fontes compartilhadas referenciadas por todos os serviços e skills.

- [ ] **Step 1: Demonstrar a fragmentação atual**

Run:

```powershell
rg -n 'mTLS|CSRF|Redis|WebSocket|REQ-|Fase|Risco' -- 'Plano de Ação V4 - Ecossistema SoftwareCenter (SC).md'
```

Expected: conceitos transversais distribuídos em várias seções do mesmo arquivo.

- [ ] **Step 2: Criar arquitetura e integrações**

Incluir topologia HA, ownership de dados, FK lógica, S2S, login OIDC/PKCE, MFA, logout, proxy autenticado, M2M, rolling keys e matriz produtor/consumidor.

- [ ] **Step 3: Criar segurança compartilhada**

Incluir zero confiança no navegador, cookies e CSRF, mTLS e client credentials, isolamento de tenant, segredos, matriz de ameaças e limites de responsabilidade.

- [ ] **Step 4: Criar operação compartilhada**

Incluir HA, circuit breaker, Redis e fallback local, observabilidade, telemetria WebSocket, métricas, traces, produção, sandbox e NFRs operacionais.

- [ ] **Step 5: Criar requisitos e roadmap**

Preservar IDs existentes, mapear cada requisito ao serviço proprietário e aos fluxos transversais, manter fases, dependências, riscos e responsáveis.

- [ ] **Step 6: Verificar ausência de cópia contraditória**

Run: comparar claims, endpoints, scopes e regras de sessão documentados com `C:\Users\gerso\.agents\skills\sc\SKILL.md`.

- [ ] **Step 7: Commit**

```powershell
git add -- 'SC Ecossistema'
git commit -m "docs: extract cross-service SC architecture"
```

### Task 3: Documentação dos serviços core

**Files:**
- Create: `SC CP/Responsabilidades do SC CP.md`
- Create: `SC CP/Modelo de Domínio do SC CP.md`
- Create: `SC CP/Contratos e Fluxos do SC CP.md`
- Create: `SC SSO/Responsabilidades do SC SSO.md`
- Create: `SC SSO/Modelo de Identidade do SC SSO.md`
- Create: `SC SSO/Contratos e Fluxos de Autenticação.md`
- Create: `SC AG/Responsabilidades do SC AG.md`
- Create: `SC AG/Segurança e Políticas de Borda.md`
- Create: `SC AG/Roteamento, Cache e Resiliência.md`

**Interfaces:**
- Consumes: documentação transversal da Task 2.
- Produces: limites, modelos e contratos de SC CP, SC SSO e SC AG.

- [ ] **Step 1: Criar os documentos do SC CP**

Cobrir tenants, memberships, contratos, aplicações, catálogo, manifesto, RBAC, provisionamento, FK lógica, ownership de dados, APIs integradas e limitações atuais.

- [ ] **Step 2: Criar os documentos do SC SSO**

Cobrir identidades, estados, clientes OAuth2/OIDC, sessões, refresh tokens, MFA, cadastro, ativação, recuperação, token delegado, JWKS e eventos de ciclo de vida.

- [ ] **Step 3: Criar os documentos do SC AG**

Cobrir proxy reverso, cookie opaco, CSRF, validação JWT, contexto, CORS, rate limit, revogação na borda, mTLS, Redis, fallback, circuit breaker e HA.

- [ ] **Step 4: Validar limites negativos**

Confirmar explicitamente: SC CP não armazena credenciais de identidade; SC SSO não decide contratos/RBAC de negócio; SC AG não é fonte de verdade de identidade nem de negócio.

- [ ] **Step 5: Commit**

```powershell
git add -- 'SC CP' 'SC SSO' 'SC AG'
git commit -m "docs: define core SC service boundaries"
```

### Task 4: Documentação dos motores de negócio

**Files:**
- Create: `SC Sign/Responsabilidades do SC Sign.md`
- Create: `SC Sign/Modelo de Assinatura e Evidências.md`
- Create: `SC Sign/Contratos e Fluxos do SC Sign.md`
- Create: `SC Wpp/Responsabilidades do SC Wpp.md`
- Create: `SC Wpp/Entrega, Retentativas e Fallbacks.md`
- Create: `SC Wpp/Contratos, Webhooks e Consentimento.md`
- Create: `SC Billing/Responsabilidades do SC Billing.md`
- Create: `SC Billing/Modelo de Medição e Faturamento.md`
- Create: `SC Billing/Contratos e Fluxos do SC Billing.md`

**Interfaces:**
- Consumes: documentação transversal e contratos de autorização dos serviços core.
- Produces: limites, modelos e fluxos de SC Sign, SC Wpp e SC Billing.

- [ ] **Step 1: Criar os documentos do SC Sign**

Separar assinatura eletrônica avançada/qualificada conforme a fonte, hash canônico, TSA RFC 3161, consentimento, evidências, auditoria append-only, verificação e possível elevação ICP-Brasil. Registrar a terminologia jurídica ainda dependente de validação legal.

- [ ] **Step 2: Criar os documentos do SC Wpp**

Cobrir WhatsApp Cloud API oficial, templates, opt-in, filas, retries, dead-letter, webhooks, estados, idempotência e fallbacks. Declarar que OTP é gerado pelo SC SSO e apenas entregue pelo SC Wpp.

- [ ] **Step 3: Criar os documentos do SC Billing**

Cobrir MSA, itens de contrato, preços versionados, consumo com preço congelado, pró-rata, competência, fechamento, fatura unificada, PSP, NFS-e, inadimplência e publicação de bloqueio.

- [ ] **Step 4: Validar ownership entre motores**

Confirmar que SC Sign e SC Wpp publicam consumo, enquanto SC Billing calcula e consolida cobrança; nenhum motor decide autorização ou contrato ativo sozinho.

- [ ] **Step 5: Commit**

```powershell
git add -- 'SC Sign' 'SC Wpp' 'SC Billing'
git commit -m "docs: define SC business engine boundaries"
```

### Task 5: Skill SC CP

**Files:**
- Create: `.agents/skills/sc-cp/SKILL.md`
- Create: `.agents/skills/sc-cp/agents/openai.yaml`
- Create: `.agents/skills/sc-cp/references/test-cases.md`

**Interfaces:**
- Consumes: `SC CP/*.md` e documentos transversais.
- Produces: skill `sc-cp` validada e padrão estrutural para as skills seguintes.

- [ ] **Step 1: Registrar testes RED**

Documentar casos que exigem localizar ownership de tenant/RBAC, distinguir identidade de membership e recusar endpoint não documentado. Registrar como baseline a dispersão dessas respostas na V4 e na skill geral `sc`.

- [ ] **Step 2: Criar a skill mínima**

Usar descrição com gatilhos para tenants, memberships, contratos, catálogo, manifesto e RBAC. Instruir leitura seletiva de `SC CP` e `SC Ecossistema`.

- [ ] **Step 3: Gerar metadados**

Run:

```powershell
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\generate_openai_yaml.py' '.agents/skills/sc-cp' --interface 'display_name=SC CP' --interface 'short_description=Orienta mudanças no Control Plane da Software Center' --interface 'default_prompt=Analise esta tarefa do SC CP usando os contratos modulares do repositório.'
```

- [ ] **Step 4: Validar GREEN e REFACTOR**

```powershell
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\quick_validate.py' '.agents/skills/sc-cp'
```

Conferir links e avaliar os casos de teste contra o procedimento da skill. Ajustar qualquer ambiguidade antes de continuar.

- [ ] **Step 5: Commit**

```powershell
git add -- '.agents/skills/sc-cp'
git commit -m "feat: add SC CP repository skill"
```

### Task 6: Skill SC SSO

**Files:**
- Create: `.agents/skills/sc-sso/SKILL.md`
- Create: `.agents/skills/sc-sso/agents/openai.yaml`
- Create: `.agents/skills/sc-sso/references/test-cases.md`

**Interfaces:**
- Consumes: `SC SSO/*.md`, segurança e integrações transversais.
- Produces: skill `sc-sso` validada.

- [ ] **Step 1: Registrar RED**

Testar ownership de identidade, OIDC/PKCE, MFA, sessão e distinção entre identidade e autorização de negócio.

- [ ] **Step 2: Criar SKILL.md**

Usar descrição com gatilhos de identidade, OAuth2, OIDC, PKCE, MFA, tokens e JWKS. Instruir leitura seletiva de `SC SSO`, `Segurança Compartilhada.md` e `Arquitetura e Integrações Entre Serviços.md`.

- [ ] **Step 3: Gerar openai.yaml**

```powershell
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\generate_openai_yaml.py' '.agents/skills/sc-sso' --interface 'display_name=SC SSO' --interface 'short_description=Orienta identidade e autenticação da Software Center' --interface 'default_prompt=Analise esta tarefa do SC SSO usando os contratos modulares do repositório.'
```

- [ ] **Step 4: Validar e corrigir**

```powershell
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\quick_validate.py' '.agents/skills/sc-sso'
```

Conferir links e avaliar todos os casos registrados. Corrigir qualquer ambiguidade antes do commit.

- [ ] **Step 5: Commit**

```powershell
git add -- '.agents/skills/sc-sso'
git commit -m "feat: add SC SSO repository skill"
```

### Task 7: Skill SC AG

**Files:**
- Create: `.agents/skills/sc-ag/SKILL.md`
- Create: `.agents/skills/sc-ag/agents/openai.yaml`
- Create: `.agents/skills/sc-ag/references/test-cases.md`

**Interfaces:**
- Consumes: `SC AG/*.md`, segurança e operação transversais.
- Produces: skill `sc-ag` validada.

- [ ] **Step 1: Registrar RED**

Testar ownership de cookie/CSRF, CORS, rate limit, Redis, circuit breaker e distinção entre proxy e fonte de verdade.

- [ ] **Step 2: Criar SKILL.md**

Usar descrição com gatilhos de gateway, proxy, cookie, CSRF, CORS, rate limit, Redis, circuit breaker, mTLS e resiliência.

- [ ] **Step 3: Gerar e validar openai.yaml**

```powershell
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\generate_openai_yaml.py' '.agents/skills/sc-ag' --interface 'display_name=SC AG' --interface 'short_description=Orienta o gateway de segurança da Software Center' --interface 'default_prompt=Analise esta tarefa do SC AG usando os contratos modulares do repositório.'
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\quick_validate.py' '.agents/skills/sc-ag'
```

Conferir links e avaliar cada caso registrado antes do commit.

- [ ] **Step 4: Commit**

```powershell
git add -- '.agents/skills/sc-ag'
git commit -m "feat: add SC AG repository skill"
```

### Task 8: Skill SC Sign

**Files:**
- Create: `.agents/skills/sc-sign/SKILL.md`
- Create: `.agents/skills/sc-sign/agents/openai.yaml`
- Create: `.agents/skills/sc-sign/references/test-cases.md`

**Interfaces:**
- Consumes: `SC Sign/*.md` e segurança compartilhada.
- Produces: skill `sc-sign` validada.

- [ ] **Step 1: Registrar RED**

Testar terminologia de assinatura, evidência, TSA, auditoria e recusa de promessas jurídicas não sustentadas.

- [ ] **Step 2: Criar SKILL.md**

Usar descrição com gatilhos de assinatura eletrônica, documento canônico, SHA-256, TSA RFC 3161, evidência, auditoria e verificação.

- [ ] **Step 3: Gerar e validar openai.yaml**

```powershell
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\generate_openai_yaml.py' '.agents/skills/sc-sign' --interface 'display_name=SC Sign' --interface 'short_description=Orienta assinaturas e evidências da Software Center' --interface 'default_prompt=Analise esta tarefa do SC Sign usando os contratos modulares do repositório.'
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\quick_validate.py' '.agents/skills/sc-sign'
```

Conferir links, linguagem jurídica e cada caso registrado antes do commit.

- [ ] **Step 4: Commit**

```powershell
git add -- '.agents/skills/sc-sign'
git commit -m "feat: add SC Sign repository skill"
```

### Task 9: Skill SC Wpp

**Files:**
- Create: `.agents/skills/sc-wpp/SKILL.md`
- Create: `.agents/skills/sc-wpp/agents/openai.yaml`
- Create: `.agents/skills/sc-wpp/references/test-cases.md`

**Interfaces:**
- Consumes: `SC Wpp/*.md`, segurança e integração com SSO/Billing.
- Produces: skill `sc-wpp` validada.

- [ ] **Step 1: Registrar RED**

Testar uso obrigatório da Cloud API oficial, ownership do OTP, consentimento, retries, webhooks e fallback.

- [ ] **Step 2: Criar SKILL.md**

Usar descrição com gatilhos de WhatsApp Cloud API, templates, consentimento, filas, retentativas, webhooks, estados e canais de fallback.

- [ ] **Step 3: Gerar e validar openai.yaml**

```powershell
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\generate_openai_yaml.py' '.agents/skills/sc-wpp' --interface 'display_name=SC Wpp' --interface 'short_description=Orienta notificações WhatsApp da Software Center' --interface 'default_prompt=Analise esta tarefa do SC Wpp usando os contratos modulares do repositório.'
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\quick_validate.py' '.agents/skills/sc-wpp'
```

Conferir links, ownership do OTP e cada caso registrado antes do commit.

- [ ] **Step 4: Commit**

```powershell
git add -- '.agents/skills/sc-wpp'
git commit -m "feat: add SC Wpp repository skill"
```

### Task 10: Skill SC Billing

**Files:**
- Create: `.agents/skills/sc-billing/SKILL.md`
- Create: `.agents/skills/sc-billing/agents/openai.yaml`
- Create: `.agents/skills/sc-billing/references/test-cases.md`

**Interfaces:**
- Consumes: `SC Billing/*.md`, contratos do SC CP e operação transversal.
- Produces: skill `sc-billing` validada.

- [ ] **Step 1: Registrar RED**

Testar preço congelado, pró-rata, fechamento, PSP/NFS-e, inadimplência e separação entre medição, cobrança e autorização.

- [ ] **Step 2: Criar SKILL.md**

Usar descrição com gatilhos de metering, preços, pró-rata, faturamento, faturas, PSP, NFS-e, inadimplência e bloqueio.

- [ ] **Step 3: Gerar e validar openai.yaml**

```powershell
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\generate_openai_yaml.py' '.agents/skills/sc-billing' --interface 'display_name=SC Billing' --interface 'short_description=Orienta medição e faturamento da Software Center' --interface 'default_prompt=Analise esta tarefa do SC Billing usando os contratos modulares do repositório.'
& 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe' 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\quick_validate.py' '.agents/skills/sc-billing'
```

Conferir links, ownership financeiro e cada caso registrado antes do commit.

- [ ] **Step 4: Commit**

```powershell
git add -- '.agents/skills/sc-billing'
git commit -m "feat: add SC Billing repository skill"
```

### Task 11: Validação integrada

**Files:**
- Modify: `SC Ecossistema/Visão Geral do Ecossistema SC.md`
- Create: `docs/superpowers/verification/resultado-validacao-final.md`

**Interfaces:**
- Consumes: toda a documentação e as seis skills.
- Produces: evidência final de integridade, navegação e preservação histórica.

- [ ] **Step 1: Verificar árvore e nomes**

Confirmar 24 documentos modulares, seis `SKILL.md`, seis `openai.yaml`, seis arquivos de casos de teste e ausência de novos `README.md`.

- [ ] **Step 2: Validar todos os links Markdown**

Extrair links relativos dos arquivos novos, resolver cada destino a partir do diretório de origem e falhar para qualquer destino ausente.

- [ ] **Step 3: Validar Mermaid e Markdown**

Confirmar quantidade par de fences, blocos Mermaid não vazios, ausência de placeholders e `git diff --check` limpo.

- [ ] **Step 4: Validar as seis skills**

Run:

```powershell
$python = 'C:\Users\gerso\.cache\codex-runtimes\codex-primary-runtime\dependencies\python\python.exe'
$validator = 'C:\Users\gerso\.codex\skills\.system\skill-creator\scripts\quick_validate.py'
@('sc-cp', 'sc-sso', 'sc-ag', 'sc-sign', 'sc-wpp', 'sc-billing') | ForEach-Object {
  & $python $validator ".agents/skills/$_"
  if ($LASTEXITCODE -ne 0) { throw "Skill inválida: $_" }
}
```

Expected: todas aprovadas.

- [ ] **Step 5: Comparar hashes históricos**

Recalcular SHA-256 e comparar, por nome, com `docs/superpowers/verification/hashes-planos-historicos.sha256`.

- [ ] **Step 6: Registrar resultado e commit final**

O relatório deve listar contagens, validação de links, validação das skills, hashes históricos e limitações conhecidas.

```powershell
git add -- 'SC Ecossistema/Visão Geral do Ecossistema SC.md' 'docs/superpowers/verification/resultado-validacao-final.md'
git commit -m "docs: verify modular SC knowledge base"
```
