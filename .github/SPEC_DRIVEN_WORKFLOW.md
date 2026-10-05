# Spec-Driven Workflow - NexusBank App

## Existing Documents Flow

```text
Existing requirement folder
-> Requirement Intake Consolidator
-> Context, BRD, Requirements, Gap Report, Traceability Matrix
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

## New Business Problem Flow

```text
Business problem
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

## Canonical Requirement Input

Use:

```text
.github/templates/requirements-input-template.md
```

Final `/specify` input:

```text
requirement/{ProjectName}_Requirements.md
```

## GitHub Traceability
Use one GitHub issue as the root anchor when available.

Track:
- Issue
- Branch
- Commit
- Pull request
- Run ID
- Trace IDs

## Quality Gate Rule

```text
If Critical gaps exist:
  Do not proceed to specification.
  Run consolidation or ask for clarification.

If only High or Medium gaps exist:
  Proceed only when gaps are accepted and documented.

If no Critical or High gaps exist:
  Proceed to specification.
```
