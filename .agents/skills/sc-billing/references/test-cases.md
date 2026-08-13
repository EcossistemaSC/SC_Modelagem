# Test cases for sc-billing

## RED baseline

Financial ownership and invariants were embedded in the broad V4 plan. Evidence: [skill-red-sc-billing.md](../../../../docs/superpowers/verification/skill-red-sc-billing.md).

## GREEN cases

| Prompt | Required behavior |
| --- | --- |
| Apply new price to old usage | Reject; create new effective version only |
| Receive duplicate paid webhook | Verify, deduplicate and reconcile |
| Close same period concurrently | Enforce one close and one invoice |
| Block delinquent tenant | Billing publishes; CP suspends; AG enforces |
| Create metering endpoint | Keep path/schema conceptual until approved |

## REFACTOR result

The skill forces frozen commercial inputs, state separation, provider verification, and owner-specific blocking.
