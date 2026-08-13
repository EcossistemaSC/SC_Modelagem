# Test cases for sc-cp

## RED baseline

Without this repository skill, ownership and contracts were dispersed across a 876-line V4 plan and the broad personal `sc` skill. The control could not select SC CP documents first and risked inventing an assignment status endpoint. Evidence: [skill-red-sc-cp.md](../../../../docs/superpowers/verification/skill-red-sc-cp.md).

## GREEN retrieval and application cases

| Prompt | Required behavior | Evidence |
| --- | --- | --- |
| Add suspension to membership | Select SC CP model; preserve audit and identity boundary | Responsibilities + model |
| Create PATCH to deactivate assignment | Report missing contract; do not invent path | Contracts limitations |
| Store password with tenant | Reject; password belongs to SC SSO | Negative boundary |
| Update permissions via manifest | Use preview, exact hash, apply, assess omissions | Manifest contract |
| Register exclusive application | Validate owner/beneficiary and conflicts | Domain invariants |

## REFACTOR result

The workflow explicitly separates current contracts from target architecture and contains the missing-contract rule that the baseline lacked.
