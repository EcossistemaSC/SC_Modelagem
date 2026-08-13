---
name: sc-cp
description: Use when working on Software Center Control Plane tenants, memberships, contracts, applications, catalog, manifests, RBAC, provisioning, exclusivity, BFF credentials, or access-domain rules.
---

# SC CP

## Overview

Use the modular SC documentation as the contract for Control Plane work. Keep identity, edge security, engines, and billing outside the SC CP boundary.

## Workflow

1. Locate the repository root with `git rev-parse --show-toplevel`.
2. Read [Responsabilidades do SC CP](../../../SC%20CP/Responsabilidades%20do%20SC%20CP.md) for every task.
3. Read [Modelo de Domínio](../../../SC%20CP/Modelo%20de%20Domínio%20do%20SC%20CP.md) when changing entities, states, invariants, persistence, or migrations.
4. Read [Contratos e Fluxos](../../../SC%20CP/Contratos%20e%20Fluxos%20do%20SC%20CP.md) when changing APIs, manifests, provisioning, RBAC integration, or policy publication.
5. For cross-service effects, read only the relevant document under [SC Ecossistema](../../../SC%20Ecossistema/Visão%20Geral%20do%20Ecossistema%20SC.md).
6. Identify whether the source describes the current contract or target architecture. Preserve current paths until a migration contract exists.
7. State requirement IDs, owner, collaborators, invariants, negative tests, and documentation changes in the result.

## Non-negotiable boundaries

- SC CP owns tenant, membership, contract, catalog, manifest, role, assignment, and access-domain data.
- SC SSO owns password, OTP, MFA, identity credentials, sessions, and token issuance.
- SC AG applies edge policies; it does not author business access.
- Never trust tenant or application identifiers supplied freely by the browser.
- Never invent endpoints, claims, scopes, states, or events. Record a decision gap instead.
- Do not call `PATCH .../status` for roles or assignments: that integration contract is not currently published.

## Quick reference

| Change | Read next | Preserve |
| --- | --- | --- |
| Tenant/membership | Domain model | logical identity link and soft-delete |
| Contract/exclusivity | Responsibilities + model | owner differs from exclusive beneficiary |
| Manifest | Contracts | preview, exact hash, then apply |
| RBAC API | Contracts | audience, application, capability, and scope |
| CORS/rate policy | Cross-service architecture | version, TTL, and invalidation |

## Example

For “add assignment deactivation,” first report that no integrated status endpoint exists. Model the domain decision and compatibility contract before proposing a path.

## Common mistakes

- Copying identity secrets into the business database.
- Treating a permission rename as an in-place edit.
- Applying a manifest without reviewing omitted active items.
- Using a target V4 flow as if it were an already published endpoint.
