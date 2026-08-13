# Test cases for sc-ag

## RED baseline

Gateway rules were dispersed and did not provide a domain-first decision path. Evidence: [skill-red-sc-ag.md](../../../../docs/superpowers/verification/skill-red-sc-ag.md).

## GREEN cases

| Prompt | Required behavior |
| --- | --- |
| Redis is down | Bounded versioned fallback, then fail closed |
| Enable credentialed CORS wildcard | Reject; require registered application origin |
| Trust tenant header from browser | Reject and reconstruct trusted context |
| Retry a POST | Require idempotency key and destination contract |
| Accept signed JWT with wrong audience | Reject |

## REFACTOR result

The skill now forces cache validity fields, negative tests, and enforcement/source separation.
