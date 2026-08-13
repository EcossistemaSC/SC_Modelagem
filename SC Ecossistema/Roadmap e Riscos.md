# Roadmap e Riscos

[Voltar à visão geral](Visão%20Geral%20do%20Ecossistema%20SC.md)

## Roadmap dependente

| Fase | Entregáveis | Dependências | Gate |
| --- | --- | --- | --- |
| 0 — Fundação | SC AG HA, Redis, bancos, CA/mTLS e esqueleto do manifesto | Infraestrutura | Failover e circuit breaker testados |
| 1 — Identidade e sessão | Cadastro, login, recuperação, provisionamento, cookie e CSRF | Fase 0 | UC-001, UC-004, UC-005 e UC-006 |
| 2 — Catálogo e RBAC | Manifesto, CORS, rate limit, token delegado e RBAC | Fase 1 | UC-007, UC-008 e UC-009 com aplicação piloto |
| 3 — Multi-tenancy avançado | Aplicação exclusiva, MFA e telemetria | Fase 2 | UC-003, UC-012, UC-013 e UC-014 |
| 4 — Motores | SC Sign, SC Wpp, PSP, NFS-e e consumo | Fase 3 | Fluxos reais e reconciliação financeira |
| 5 — Sandbox monetizado | Isolamento sandbox, faturamento e onboarding | Fase 4 | SDK/documentação e consumidores piloto |

Prototipação visual pode ocorrer em paralelo, mas não muda contratos sem ADR e atualização da documentação modular.

## Riscos por proprietário

| Risco | Probabilidade | Impacto | Mitigação | Proprietário |
| --- | --- | --- | --- | --- |
| SC AG como SPOF lógico | Baixa | Alto | Réplicas multi-AZ, health check e circuit breaker | SRE / SC AG |
| Redis indisponível ou inconsistente | Média | Alto | TTL, versão, fallback local e reconciliação | SC AG / SC CP |
| Rotação de certificados falhar | Baixa | Alto | Renovação automática e alerta antecipado | SRE / SC SSO |
| Credencial M2M vazar | Alta | Alto | Rolling keys, cofre e revogação imediata | SC SSO / SC AG |
| Acesso cross-tenant | Baixa | Crítico | Contexto confiável e testes negativos | Todos |
| Banimento ou indisponibilidade WhatsApp | Média | Alto | Cloud API oficial e fallback | SC Wpp |
| Disputa sobre assinatura | Média | Alto | Evidência verificável, linguagem jurídica revisada e opção ICP-Brasil | SC Sign / Jurídico |
| Inadimplência pós-paga | Alta | Médio | Grace period, notificação e bloqueio auditável | SC Billing / SC CP |
| Reajuste de preço contestado | Baixa | Médio | Versionamento e notificação comprovável | SC Billing / Jurídico |
| Webhook duplicado ou forjado | Média | Alto | Assinatura, idempotência e reconciliação | SC Wpp / SC Billing |

## Gates antes de avançar fase

- Contratos da fase estão publicados e versionados.
- Testes cross-tenant e de revogação passam.
- Runbooks de dependências críticas estão exercitados.
- Observabilidade não registra segredos.
- Migração e rollback de schema foram testados.
- Custos externos e limites operacionais foram atualizados.

## Decisões pendentes prioritárias

1. ADR da separação física entre SC CP, SC SSO e SC AG.
2. Contrato versionado de eventos e estratégia de broker.
3. Política de revogação de JWT e duração de grace periods.
4. Terminologia e produto jurídico do SC Sign.
5. PSP, provedor de NFS-e e política fiscal do SC Billing.
6. Provedores de fallback e retenção de consentimento do SC Wpp.
