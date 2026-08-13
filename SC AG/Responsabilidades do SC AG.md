# Responsabilidades do SC AG

[Visão geral](../SC%20Ecossistema/Visão%20Geral%20do%20Ecossistema%20SC.md) · [Operação compartilhada](../SC%20Ecossistema/Observabilidade,%20Disponibilidade%20e%20Operação.md)

## Propósito

O SC AG é o ponto de entrada de segurança e proxy reverso. Ele aplica políticas já definidas por fontes confiáveis e encaminha somente requisições autenticadas e contextualizadas.

## É responsabilidade do SC AG

- terminar ou participar do TLS e encaminhar por canais protegidos;
- validar cookie/sessão de borda e proteção CSRF;
- validar JWT, issuer, audience, expiração, `jti` e contexto;
- resolver rota e propagar identidade/contexto sanitizado;
- aplicar CORS dinâmico, rate limit e bloqueios;
- consumir políticas versionadas do Redis com fallback local controlado;
- aplicar circuit breaker, timeout, retry seguro e health checks;
- rejeitar credenciais revogadas conforme política;
- emitir telemetria redigida e correlacionável.

## Não é responsabilidade do SC AG

- ser fonte de verdade de identidade, contrato, RBAC ou cobrança;
- criar permissão quando cache estiver ausente;
- armazenar senha ou refresh token de aplicação cliente;
- executar regra de negócio dos serviços;
- confiar em tenant, aplicação ou role fornecidos livremente pelo browser.

## Invariantes

1. Falha de autorização é fechada; cache antigo não concede acesso novo.
2. CORS com credenciais nunca usa wildcard.
3. Mutação baseada em cookie exige proteção CSRF.
4. Retry só ocorre em operação idempotente ou com idempotency key.
5. Toda política em cache possui tenant/aplicação, versão e TTL.

## Requisitos associados

REQ-001, REQ-008, REQ-009, REQ-013, REQ-015 e partes de REQ-012/REQ-014.

## Lacunas

- A fronteira entre cookie do BFF e cookie central do SC AG precisa de ADR por tipo de cliente.
- Rotas, timeouts e policies necessitam configuração versionada publicada.
