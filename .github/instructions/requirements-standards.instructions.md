---
applyTo: "**/requirements.md,**/spec.md,**/specification.md,**/*requirement*.md,**/BRD_*.md,**/Context_*.md"
description: "Requirements and specification standards for NexusBank app"
---

When creating or reviewing requirements and specifications, follow:

- `.github/rules/requirement-standards.md`
- `.github/rules/traceability-standards.md`
- `.github/rules/architectural-standards.md`
- `.github/rules/security-quality-standards.md`

## Requirement Rules
- Use canonical sections from `.github/templates/requirements-input-template.md`.
- Use `FR-*` for functional requirements.
- Use `NFR-*` for non-functional requirements.
- Use `BR-*` for business rules.
- Use `AC-*` for Gherkin acceptance criteria.
- Use `INT-*` for integration points.
- Use `ERR-*` for error handling.
- Map all IDs in `requirement/Traceability_Matrix.md`.

## NexusBank Data Classification
- `[PII]`
- `[SENSITIVE_FINANCIAL]`
- `[CONFIDENTIAL]`
- `[PUBLIC]`

## Compliance Scope
Include PDPA, KYC, AML, suitability, product eligibility, consent, audit, and retention controls only when the source requirements make them applicable.

Never include real customer PII, account numbers, portfolio holdings, credentials, secrets, tokens, or production data.
