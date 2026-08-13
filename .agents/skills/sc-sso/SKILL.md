---
name: sc-sso
description: Use when working on Software Center identity, registration, password recovery, activation, OAuth2, OIDC, PKCE, MFA, sessions, JWT claims, JWKS, token exchange, client credentials, or credential rotation.
---

# SC SSO

## Overview

Treat SC SSO as the identity and token authority. Obtain business context from SC CP and leave edge enforcement to SC AG.

## Workflow

1. Read [Responsabilidades do SC SSO](../../../SC%20SSO/Responsabilidades%20do%20SC%20SSO.md).
2. Read [Modelo de Identidade](../../../SC%20SSO/Modelo%20de%20Identidade%20do%20SC%20SSO.md) for users, clients, sessions, tokens, MFA, keys, states, persistence, or migrations.
3. Read [Contratos e Fluxos](../../../SC%20SSO/Contratos%20e%20Fluxos%20de%20Autenticação.md) for BFF sessions, public identity flows, OIDC, delegated tokens, and errors.
4. Read [Segurança Compartilhada](../../../SC%20Ecossistema/Segurança%20Compartilhada.md) for browser, secret, tenant, replay, and logging controls.
5. For business-context interactions, read [Arquitetura e Integrações](../../../SC%20Ecossistema/Arquitetura%20e%20Integrações%20Entre%20Serviços.md).
6. Classify every path as current contract or target architecture before implementation.
7. Report grants, audience, scopes, claims, rotation/revocation, negative tests, and unresolved decisions.

## Non-negotiable boundaries

- SC SSO owns identity credentials, password hashes, MFA challenges, sessions, clients, signing keys, and tokens.
- SC CP owns tenant, membership, contract, role, assignment, permissions, and application rules.
- SC Wpp delivers an OTP message; it never generates or validates the OTP.
- Use `membro_tenant_id` for the membership claim in JWT, not `membership_id`.
- Keep BFF, OIDC, catalog, and M2M credentials separate.
- Do not assume token exchange or `sc.*` scopes are enabled for every client.
- Do not invent hosts, paths, claims, grants, or token lifetimes.

## Quick reference

| Change | Required checks |
| --- | --- |
| User login | `ACTIVE`, verified email, generic failure, rate limit |
| Password recovery | generic `202`, single-use OTP, attempt limit, session revocation |
| OIDC | authorization code, PKCE, registered redirect, issuer/audience |
| Delegated RBAC token | `RBAC_MANAGE`, `sc-management`, application and scope |
| Client credentials | mTLS, minimal scope, short TTL, rotation and revocation |
| JWT context | trusted SC CP source and `membro_tenant_id` |

## Example

For “send MFA through WhatsApp,” keep challenge generation and validation in SC SSO; request only template delivery from SC Wpp and consume delivery status separately.

## Common mistakes

- Copying membership or contract data into the identity source of truth.
- Returning different recovery responses for known and unknown users.
- Putting tokens in browser storage.
- Treating V4 target routing as an already migrated endpoint.
