# Requisitos e Rastreabilidade

[Voltar à visão geral](Visão%20Geral%20do%20Ecossistema%20SC.md)

## Requisitos funcionais consolidados

| ID | Requisito | Proprietário | Colaboradores | Fase |
| --- | --- | --- | --- | --- |
| REQ-001 | Login por senha com sessão BFF e cookie seguro | SC SSO | SC AG | 1 |
| REQ-002 | Login Google com token validado | SC SSO | SC AG | 2 |
| REQ-003 | MFA por TOTP, WhatsApp ou e-mail | SC SSO | SC Wpp | 2 |
| REQ-004 | Recuperação de senha com OTP e anti-enumeração | SC SSO | SC Wpp | 1 |
| REQ-005 | Cadastro público e verificação de e-mail | SC SSO | SC Wpp | 1 |
| REQ-006 | Provisionamento administrativo com ativação única | SC CP | SC SSO, SC Wpp | 1 |
| REQ-007 | Prévia, aplicação e exportação de manifesto | SC CP | — | 2 |
| REQ-008 | CORS dinâmico por contrato | SC CP | SC AG | 2 |
| REQ-009 | Rate limit por tenant, aplicação e IP | SC AG | SC CP | 2 |
| REQ-010 | Aplicação exclusiva com tenant beneficiário fixo | SC CP | SC SSO | 3 |
| REQ-011 | Token delegado para gestão de RBAC | SC SSO | SC CP | 2 |
| REQ-012 | mTLS e client credentials em S2S | SC SSO | Todos | 1 |
| REQ-013 | Circuit breaker e fallback Redis | SC AG | SC CP/SRE | 1 |
| REQ-014 | Rotação automática de certificados e cookies | SRE | SC SSO, SC AG | 2 |
| REQ-015 | WebSocket isolado por tenant | Observabilidade | SC AG, SC CP | 3 |

## Regras de domínio herdadas

| ID | Regra | Documentos proprietários |
| --- | --- | --- |
| DR-01 | Browser não recebe senha persistida, token, segredo ou `sessionId` remoto. | SC AG / Segurança compartilhada |
| DR-02 | Login exige identidade `ACTIVE` e e-mail verificado. | SC SSO |
| DR-03 | Membership usa `OWNER`, `ADMIN` ou `MEMBER`; capabilities administrativas são explícitas. | SC CP |
| DR-04 | Aplicação exclusiva exige beneficiário diferente do owner e bloqueia contratos incompatíveis. | SC CP |
| DR-05 | Acesso depende de toda a cadeia de identidade, tenant, contrato e RBAC estar ativa. | SC CP / SC SSO / SC AG |
| DR-06 | `tenantSubdominio` é obrigatório apenas em aplicação não exclusiva. | SC CP / SC SSO |
| DR-07 | Credencial BFF usa HTTP Basic e segredo aparece em claro somente na rotação. | SC CP / Segurança compartilhada |
| DR-08 | Falhas de login não revelam senha, usuário, tenant ou regra. | SC SSO / SC AG |
| DR-09 | Painel SC e aplicações clientes não compartilham cookie. | SC AG |
| DR-10 | CORS exige origem cadastrada para aplicação ativa; não há allowlist global. | SC CP / SC AG |

## Casos de uso por domínio

| Caso | Fluxo | Serviços |
| --- | --- | --- |
| UC-001/002 | Login por senha ou Google | SC SSO, SC AG, BFF |
| UC-003 | MFA | SC SSO, SC Wpp |
| UC-004/005 | Recuperação, cadastro e verificação | SC SSO, SC Wpp |
| UC-006 | Provisionamento e ativação | SC CP, SC SSO, SC Wpp |
| UC-007 | Gestão integrada de RBAC | SC CP, SC SSO |
| UC-008 | Sincronização de manifesto | SC CP |
| UC-009/015 | CORS e rate limit dinâmicos | SC CP, SC AG |
| UC-010 | Assinatura e verificação | SC Sign, SC Billing |
| UC-011 | Notificação transacional | SC Wpp, SC Billing |
| UC-012 | Telemetria ao vivo | SC AG, observabilidade |
| UC-013/014 | Aplicação exclusiva | SC CP, SC SSO |
| UC-016/017 | Rolling keys e revogação M2M | SC CP, SC SSO, SC AG |

## Critério de rastreabilidade

Uma mudança está rastreada quando informa:

1. requisito e caso de uso afetados;
2. serviço proprietário e colaboradores;
3. contrato de entrada/saída alterado;
4. teste positivo, negação de acesso e falha de dependência;
5. atualização necessária nos documentos transversais.

## Lacunas de requisitos

- A V4 usa identificadores informais como `REQ-NOT` e `REQ-M2M` em alguns casos sem defini-los na tabela principal.
- SC Sign, SC Wpp e SC Billing precisam de requisitos funcionais numerados próprios antes da implementação completa.
- Critérios de aceite para eventos, idempotência, retenção e reconciliação precisam ser formalizados.
- A meta NFR-06 exige definição verificável do que constitui evento de assinatura completo.
