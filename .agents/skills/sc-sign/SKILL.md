---
name: sc-sign
description: Use when working on SC Sign document canonicalization, electronic signatures, consent, SHA-256 hashes, RFC 3161 timestamps, evidence trails, verification packages, audit retention, signing workflows, or ICP-Brasil elevation.
---

# SC Sign

## Overview

Build verifiable technical evidence without making unsupported legal claims. Keep the exact canonical document, consent, timestamp, and append-only history linked.

## Workflow

1. Read [Responsabilidades do SC Sign](../../../SC%20Sign/Responsabilidades%20do%20SC%20Sign.md).
2. Read [Modelo de Assinatura e Evidências](../../../SC%20Sign/Modelo%20de%20Assinatura%20e%20Evidências.md) for entities, states, canonical bytes, hashes, TSA, evidence, retention, or storage.
3. Read [Contratos e Fluxos](../../../SC%20Sign/Contratos%20e%20Fluxos%20do%20SC%20Sign.md) for creation, signing, verification, cancellation, idempotency, and consumption.
4. Read [Segurança Compartilhada](../../../SC%20Ecossistema/Segurança%20Compartilhada.md) for tenant, secrets, privacy, logging, and replay controls.
5. Identify the claimed signature modality and list its concrete controls. If legal classification is not approved, mark it for legal review.
6. Define canonical bytes, hash, evidence events, idempotency, failure behavior, retention, and verification output before code.
7. Do not publish a path or schema while the contract remains conceptual.

## Non-negotiable boundaries

- A SHA-256 hash proves integrity comparison; it is not by itself a digital signature or legal classification.
- Any byte change creates a new document version, hash, and signing process.
- Evidence corrections append events; they never overwrite history silently.
- If TSA is required, an unavailable or invalid TSA response cannot be treated as completion.
- Identity and authorization come from SC SSO/SC CP through trusted context.
- Billing events are idempotent and cannot modify signing evidence.
- State only what verification actually checked.

## Quick reference

| Concern | Required artifact |
| --- | --- |
| Integrity | exact canonical bytes + SHA-256 |
| Temporal proof | validated RFC 3161 response when required |
| Consent | text/version + signer + instant + document hash |
| Audit | append-only event chain and authorized access |
| Verification | explicit checks, result, limitations and package reference |
| Legal claim | approved modality and legal review |

## Example

For “guarantee full legal validity with SHA-256,” respond that the hash supports integrity only. Specify identity, consent, audit, timestamp and verification controls, and require legal approval for product terminology.

## Common mistakes

- Rehashing a regenerated PDF instead of preserving original bytes.
- Logging full documents or sensitive evidence.
- Marking a process complete before mandatory timestamp validation.
- Inventing an HTTP API from a conceptual flow.
