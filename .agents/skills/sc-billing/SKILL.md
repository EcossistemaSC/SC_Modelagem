---
name: sc-billing
description: Use when working on SC Billing metering, price tables, effective dates, frozen unit prices, prorating, billing periods, invoice closing, payment providers, payment webhooks, NFS-e, delinquency, refunds, reconciliation, or financial audit.
---

# SC Billing

## Overview

Make every charge reproducible from immutable commercial inputs and idempotent events. Keep business authorization in SC CP and edge blocking in SC AG.

## Workflow

1. Read [Responsabilidades do SC Billing](../../../SC%20Billing/Responsabilidades%20do%20SC%20Billing.md).
2. Read [Modelo de Medição e Faturamento](../../../SC%20Billing/Modelo%20de%20Medição%20e%20Faturamento.md) for prices, consumption, periods, invoices, PSP, fiscal documents, states, persistence, or calculations.
3. Read [Contratos e Fluxos](../../../SC%20Billing/Contratos%20e%20Fluxos%20do%20SC%20Billing.md) for metering, closing, delinquency, webhooks, reconciliation, and failures.
4. Read [Roadmap e Riscos](../../../SC%20Ecossistema/Roadmap%20e%20Riscos.md) for financial, legal, provider, and rollout dependencies.
5. Define tenant/contract source, idempotency key, price version, currency, rounding, event time, billing timezone, and audit record.
6. Test duplicates, late events, concurrent closing, provider replay, failed fiscal issuance, refunds, and reconciliation gaps.
7. Keep endpoints, schemas, tax rules, and state catalogs conceptual until approved.

## Non-negotiable boundaries

- Accepted consumption freezes unit price and currency; later price versions do not rewrite it.
- The same idempotency key cannot create duplicate consumption or charge.
- A closed invoice is not silently rewritten; use explicit adjustment/credit events.
- Verify and deduplicate PSP/fiscal webhooks, then reconcile with provider state.
- SC Billing publishes delinquency status; SC CP changes contract state and SC AG enforces blocking.
- Do not store raw card data.
- SC Sign and SC Wpp produce consumption; they do not calculate invoices.

## Quick reference

| Change | Required check |
| --- | --- |
| Price adjustment | new effective version + notice evidence |
| Usage event | contract eligibility + frozen price + idempotency |
| Proration | explicit eligible interval and rounding policy |
| Period close | single concurrency owner + immutable calculation memory |
| Payment webhook | signature, timestamp, transition, deduplication, reconciliation |
| Delinquency | grace period + auditable publish/unblock flow |

## Example

For a retroactive price increase, create a future-effective price version. Do not update the frozen unit price of consumption already accepted or a closed invoice.

## Common mistakes

- Using the current price during invoice close instead of the frozen event price.
- Treating a webhook payload as proof without verification.
- Mixing payment, invoice, and fiscal-document states.
- Inventing tax rules or an API path from the architectural model.
