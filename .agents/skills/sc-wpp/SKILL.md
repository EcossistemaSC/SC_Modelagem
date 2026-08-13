---
name: sc-wpp
description: Use when working on SC Wpp or SC Notify WhatsApp Cloud API messages, approved templates, consent, opt-in or opt-out, queues, retries, dead-letter handling, delivery webhooks, message states, MFA delivery, fallbacks, or notification metering.
---

# SC Wpp

## Overview

Use the official WhatsApp Cloud API and model delivery as an asynchronous, idempotent workflow. Keep authentication secrets and billing calculations in their owning services.

## Workflow

1. Read [Responsabilidades do SC Wpp](../../../SC%20Wpp/Responsabilidades%20do%20SC%20Wpp.md).
2. Read [Entrega, Retentativas e Fallbacks](../../../SC%20Wpp/Entrega,%20Retentativas%20e%20Fallbacks.md) for queues, states, retry classification, dead-letter, fallback, reconciliation, and metrics.
3. Read [Contratos, Webhooks e Consentimento](../../../SC%20Wpp/Contratos,%20Webhooks%20e%20Consentimento.md) for requests, provider events, opt-in, MFA delivery, and validation.
4. Read [Segurança Compartilhada](../../../SC%20Ecossistema/Segurança%20Compartilhada.md) for tenant, webhook, secret, PII, and logging controls.
5. Classify failures as permanent or transient before adding retry.
6. Define request and provider-event idempotency, state transitions, consent, fallback policy, and consumption semantics.
7. Keep paths and schemas conceptual until a contract is approved.

## Non-negotiable boundaries

- Use WhatsApp Business Platform Cloud API; do not use Baileys, `whatsapp-web.js`, or session emulation.
- SC SSO generates and validates OTP. SC Wpp only delivers the approved template and reports delivery state.
- Verify webhook signature and timestamp, deduplicate events, and validate state transition.
- Retries preserve the original idempotency key.
- Template/consent/destination errors are not transient by default.
- Fallback requires an allowed purpose, tenant policy, and consent for the alternate channel.
- SC Billing receives idempotent consumption; SC Wpp does not calculate invoices.

## Quick reference

| Event | Action |
| --- | --- |
| Network, `429`, provider `5xx` | bounded backoff, jitter, provider limits |
| Invalid template or recipient | terminal failure until corrected |
| Duplicate webhook | acknowledge without duplicate effects |
| Retry exhausted | dead-letter, audit, optional authorized fallback |
| Consent revoked | block new non-mandatory sends |
| MFA request | deliver only; never inspect or validate OTP result |

## Example

For a duplicate `delivered` webhook, keep one state transition and one consumption effect. Acknowledge the duplicate after signature and correlation checks.

## Common mistakes

- Treating provider submission as confirmed delivery.
- Using phone numbers or message content as metric labels.
- Falling back to a less secure MFA channel automatically.
- Using historical example prices as current provider pricing.
