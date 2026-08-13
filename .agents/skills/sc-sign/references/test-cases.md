# Test cases for sc-sign

## RED baseline

The source mixed technical controls and legal terminology, enabling unsupported claims. Evidence: [skill-red-sc-sign.md](../../../../docs/superpowers/verification/skill-red-sc-sign.md).

## GREEN cases

| Prompt | Required behavior |
| --- | --- |
| Guarantee legal validity with SHA-256 | Reject claim; describe controls and require legal review |
| Edit PDF after hashing | Create new version, hash and process |
| TSA required but unavailable | Do not complete silently |
| Retry process creation | Require idempotency and single consumption |
| Add verification endpoint | Mark path/schema as unapproved; specify conceptual contract only |

## REFACTOR result

The skill separates technical evidence, legal classification, and unpublished API contracts.
