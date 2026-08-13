---
name: sc-ag
description: Use when working on the Software Center Auth Gateway, reverse proxy, cookies, CSRF, CORS, JWT validation, rate limiting, Redis policies, revocation, routing, retries, circuit breakers, high availability, or degraded mode.
---

# SC AG

## Overview

Treat SC AG as an enforcement and routing boundary. It consumes trusted identity and business policies; it never creates them.

## Workflow

1. Read [Responsabilidades do SC AG](../../../SC%20AG/Responsabilidades%20do%20SC%20AG.md).
2. Read [Segurança e Políticas de Borda](../../../SC%20AG/Segurança%20e%20Políticas%20de%20Borda.md) for cookies, CSRF, CORS, JWT, tenant context, rate limit, headers, and revocation.
3. Read [Roteamento, Cache e Resiliência](../../../SC%20AG/Roteamento,%20Cache%20e%20Resiliência.md) for routes, Redis, fallback, circuit breakers, retries, deploys, and chaos tests.
4. Read [Operação compartilhada](../../../SC%20Ecossistema/Observabilidade,%20Disponibilidade%20e%20Operação.md) for SLOs, telemetry, environments, and runbooks.
5. Identify the policy source, cache key dimensions, schema/version, TTL, grace period, and fail behavior.
6. Add negative tests for wrong tenant, origin, audience, CSRF, revocation, stale cache, and unhealthy downstream.
7. Record any missing policy value or route contract as a decision gap.

## Non-negotiable boundaries

- SC CP authors business policy; SC SSO authors identity/token truth; SC AG enforces both.
- Remove untrusted identity/tenant headers before propagating trusted context.
- CORS with credentials never uses wildcard.
- Mutations authenticated by cookie require CSRF protection.
- Validate signature, issuer, destination audience, expiration, `jti`, scopes, and required context.
- Cached data cannot grant new access after its validity window.
- Retry only safe operations or mutations with an explicit idempotency contract.

## Quick reference

| Scenario | Required behavior |
| --- | --- |
| Redis unavailable | use versioned local snapshot within grace; then fail closed |
| SC SSO unavailable | deny new login; handle existing sessions only by explicit risk policy |
| Downstream latency | bounded timeout and circuit breaker |
| Revoked client | reject affected token according to revocation policy |
| Cross-tenant header | discard caller value and rebuild trusted context |
| Preflight | resolve registered active origin with isolated cache key |

## Example

For a POST retry request, require an idempotency key and destination semantics. Otherwise return the downstream failure instead of risking a duplicated signature, message, or charge.

## Common mistakes

- Treating Redis as the persistent source of truth.
- Falling open when policy is missing or expired.
- Validating only the JWT signature.
- Logging tokens, cookies, OTPs, or raw payloads.
