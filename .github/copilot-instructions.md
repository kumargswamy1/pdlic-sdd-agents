# Copilot Instructions - NexusBank App

This repository uses a spec-driven SDLC workflow for a NexusBank banking application.

## Default Behavior
- Prefer the agents in `.github/agents/`.
- Follow centralized rules in `.github/rules/`.
- Use templates in `.github/templates/`.
- Preserve traceability through every artifact.
- Treat GitHub issues, branches, commits, and pull requests as trace metadata when available.

## Required Flow
For existing source requirement folders:

```text
Requirement Intake Consolidator
-> Traceability Manager
-> Technical Specifier
-> Compliance Reviewer
-> Implementation Planner
-> Compliance Reviewer
-> Implementation Builder
-> Quality Tester
-> Compliance Reviewer
-> Technical Documenter
-> Traceability Manager
```

For new business problems:

```text
Context Clarifier
-> BRD Generator
-> Requirements Expander
-> User Story Generator
-> Technical Specifier
-> Implementation Planner
-> Implementation Builder
-> Quality Tester
-> Compliance Reviewer
-> Technical Documenter
-> Traceability Manager
```

## Banking Controls
Apply only when in scope:
- Thailand PDPA privacy controls.
- KYC and AML checks.
- Customer risk profiling.
- Product suitability and eligibility.
- Consent and disclosure capture.
- Maker-checker approvals.
- Advisor/relationship-manager assignment.
- Audit trail and retention.
- Regulatory reporting evidence.

## Data Protection
Never include real customer PII, account numbers, card numbers, portfolio holdings, credentials, secrets, tokens, or production data in generated artifacts, examples, tests, commits, PR descriptions, or documentation.
