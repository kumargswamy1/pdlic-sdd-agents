# Copilot Instructions - Spec-Driven Development

This repository uses a spec-driven PDLC workflow for software development.

## Default Behavior
- Prefer the agents in `.github/agents/`.
- Follow centralized rules in `.github/rules/`.
- Use templates in `.github/templates/`.
- Preserve traceability through every artifact.
- Treat GitHub issues, branches, commits, and pull requests as trace metadata when available.

## Context Management (Prerequisite for All Workflows)

**Context creation and maintenance is foundational to all PDLC workflows.**

### Initial Context Setup
For new projects or when context does not exist, use a single unified agent:

```text
Context Creator (Unified - Merged)
  → Extracts tech stack from source code (package.json, config files, folder structure)
  → Gathers business context from user/documentation
  → Applies standards DURING context creation (integrated, not separate)
  → Creates `.github/instructions/project-context.md` (business + tech baseline with standards refs)
  → Creates `.github/instructions/frontend-standards.md` (patterns extracted from code)
  → Creates `.github/instructions/backend-standards.md` (patterns extracted from code)
  → Creates `.github/instructions/testing-standards.md` (patterns extracted from code)
  → Creates `requirement/Context_{ProjectName}.md` (detailed context with CTX-* IDs)
  → Updates `requirement/Traceability_Matrix.md` with CTX-* IDs
```

**No MCP file validation needed** - all context extracted directly from source code and project structure.
**Standards are embedded, not separate** - as context is discovered, standards are applied to ensure consistency.

### Context Maintenance
As project evolves, keep context synchronized:

```text
Context Updater
  (triggered by: dependency updates, folder reorganization, tech stack changes, or scheduled runs)
  → scans project structure and configuration
  → detects changes (MINOR/MODERATE/MAJOR)
  → updates `.github/instructions/project-context.md` and standards files
  → tracks changes with CTX-UPDATE-* IDs
  → escalates MAJOR changes for approval
  → updates `requirement/Traceability_Matrix.md`
```

## Required Flow
For existing source requirement folders:

```text
[Context exists per Context Creator]
-> Requirement Intake Consolidator
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
[Context exists per Context Creator]
-> Context Clarifier
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

## Parallel Context Updates
**At any point in the PDLC, Context Updater may run automatically to:**
- Sync technology versions and dependencies
- Update architectural guidance when code patterns evolve
- Flag conflicts between detected state and approved specs/plans
- Maintain alignment between project reality and documented standards

## Domain-Specific Controls
Apply regulatory and domain controls only when in scope:
- Privacy controls (e.g., GDPR, PDPA, CCPA).
- KYC and AML checks when applicable.
- Customer risk profiling and suitability assessments.
- Consent and disclosure capture.
- Approval workflows (e.g., maker-checker).
- Role-based access control and audit trails.
- Regulatory reporting evidence and compliance.

## Data Protection
Never include real customer PII, sensitive account information, credentials, secrets, tokens, or production data in generated artifacts, examples, tests, commits, PR descriptions, or documentation.
