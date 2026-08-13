# Test cases for sc-sso

## RED baseline

The broad sources mixed identity and business authorization and did not provide a domain-first reading path. Evidence: [skill-red-sc-sso.md](../../../../docs/superpowers/verification/skill-red-sc-sso.md).

## GREEN cases

| Prompt | Required behavior |
| --- | --- |
| Emit JWT with membership | Use trusted SC CP context and `membro_tenant_id` |
| Enable delegated RBAC token | Require configured grant, capability, audience, application and scope |
| Send MFA with WhatsApp | Generate/validate OTP in SSO; Wpp only delivers |
| Recover password | Generic response, expiring OTP, attempt limit and session revocation |
| Store contract in SSO | Reject ownership and redirect to SC CP |

## REFACTOR result

The skill now names each client type, prevents claim drift and requires current-versus-target classification.
