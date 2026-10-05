---
applyTo: "**/test/**,**/tests/**,**/*.test.*,**/*.spec.*"
description: "Test standards for NexusBank app"
---

Follow:
- `.github/rules/security-quality-standards.md`
- `.github/rules/traceability-standards.md`
- `.github/rules/sdlc.md`

## Required Coverage
- Happy path.
- Validation failures.
- Unauthorized and forbidden access.
- Cross-customer access prevention.
- Integration timeout and fallback.
- Audit trail evidence for sensitive actions.
- Performance SLA checks where specified.

## Data Rules
- Use synthetic data only.
- Do not use real customer names, account numbers, holdings, credentials, tokens, or production data.
