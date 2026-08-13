# Test cases for sc-wpp

## RED baseline

Naming and delivery responsibilities were sparse and could lead to unofficial integrations or duplicated effects. Evidence: [skill-red-sc-wpp.md](../../../../docs/superpowers/verification/skill-red-sc-wpp.md).

## GREEN cases

| Prompt | Required behavior |
| --- | --- |
| Use Baileys | Reject and require official Cloud API |
| Receive duplicate webhook | Deduplicate state, fallback, and consumption |
| Deliver MFA | SSO owns OTP; Wpp owns template delivery only |
| Retry invalid template | Treat as permanent until corrected |
| Fall back to SMS | Require purpose, policy, and consent |

## REFACTOR result

The skill resolves SC Notify/SC Wpp triggers while enforcing official transport, idempotency, and OTP ownership.
