# Copilot Agents And Spec-Driven Flow

This `.github` folder configures GitHub Copilot for a NexusBank banking application.

## Agents

| Agent | Purpose |
|---|---|
| Requirement Intake Consolidator | Converts existing docs into canonical Context, BRD, Requirements, gap report, and traceability matrix |
| Context Clarifier | Captures business context for new requests |
| BRD Generator | Creates BRD from context |
| Requirements Expander | Creates canonical detailed requirements |
| User Story Generator | Creates epics, stories, and backlog mapping |
| Technical Specifier | Creates technical specification from detailed requirements |
| Implementation Planner | Creates plan and task DAG |
| Implementation Builder | Implements approved tasks |
| Quality Tester | Creates test design and executable tests |
| Frontend BDD Functional Tester | Writes executable `.feature` scenarios, generates Flutter widget tests, and runs them |
| Compliance Reviewer | Reviews specs, plans, code, tests, and docs |
| Technical Documenter | Generates initial and final documentation |
| Traceability Manager | Maintains end-to-end traceability |

## Workflows

Existing documents:

```text
Requirement Intake Consolidator -> Traceability Manager -> Technical Specifier -> Compliance Reviewer -> Implementation Planner -> Compliance Reviewer -> Implementation Builder -> Quality Tester -> Compliance Reviewer -> Technical Documenter -> Traceability Manager
```

New business problem:

```text
Context Clarifier -> BRD Generator -> Requirements Expander -> User Story Generator -> Technical Specifier -> Implementation Planner -> Implementation Builder -> Quality Tester -> Compliance Reviewer -> Technical Documenter -> Traceability Manager
```

## Prompt Files

Reusable prompt workflows:

```text
.github/prompts/intake-to-spec.prompt.md
.github/prompts/intake-to-implementation.prompt.md
```

## Template

Canonical requirements template:

```text
.github/templates/requirements-input-template.md
```

## Core Rules

```text
.github/rules/requirement-standards.md
.github/rules/traceability-standards.md
.github/rules/architectural-standards.md
.github/rules/security-quality-standards.md
.github/rules/environment-standards.md
.github/rules/versioning-standards.md
.github/rules/sdlc.md
```

## Data Protection
Do not include real customer PII, account numbers, card numbers, portfolio holdings, credentials, secrets, tokens, or production data in prompts, generated artifacts, tests, docs, commits, or pull requests.
