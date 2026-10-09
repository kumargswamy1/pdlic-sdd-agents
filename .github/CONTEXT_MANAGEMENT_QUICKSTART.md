---
title: Context Management Quick Start Guide
description: Step-by-step guide to using Context Creator and Context Updater agents
ms.date: 2026-10-07
---

# Context Management Quick Start Guide

## Before You Start

Ensure you have the following files in your repository:

```
.github/
├── agents/
│   ├── context-creator.agent.md          ✅ New
│   ├── context-updater.agent.md          ✅ New
│   ├── specify.agent.md                  ✅ Updated
│   ├── planner.agent.md                  ✅ Updated
│   ├── implement.agent.md                ✅ Updated
│   ├── tester.agent.md                   ✅ Updated
│   └── traceability-manager.agent.md     ✅ Updated
├── copilot-instructions.md               ✅ Updated
├── instructions/
│   └── project-context.md                        (Will be created by Context Creator)
└── CONTEXT_MANAGEMENT_GUIDE.md           ✅ New

requirement/
├── Traceability_Matrix.md                (Will be updated by Context Creator)
└── Context_{ProjectName}.md              (Will be created by Context Creator)
```

## Workflow 1: New Project Setup

### Step 1: Run Context Creator

**Who:** Project Lead or Business Analyst
**When:** Project kickoff, before any specs are created

**Invocation:**
```
Run: Context Creator agent
Input: 
  - Business name and purpose
  - Target users and key features
  - Technology stack (frontend, backend, testing)
  - Compliance/regulatory requirements
  - Success criteria
  - Scope boundaries
```

**Example Input:**
```
I'm creating context for a mobile/web application.

Business: Customer-focused application with key features and user workflows
Tech Stack:
  - Frontend: React/Vue/Flutter, TypeScript/Dart, state management, routing, HTTP client
  - Backend: Node.js/Python, framework (Express/Django), REST API with OpenAPI
  - Testing: Frontend test framework (Jest/Pytest), backend integration tests
  
Compliance: GDPR, applicable regulations, security controls
Success: Performance targets, uptime SLA, user satisfaction metrics
```

**What You Get:**
- ✅ `.github/instructions/project-context.md` - Unified business + tech baseline
- ✅ `.github/instructions/frontend-standards.md` - Frontend tech best practices
- ✅ `.github/instructions/backend-standards.md` - Backend tech patterns
- ✅ `.github/instructions/testing-standards.md` - Testing framework patterns
- ✅ `requirement/Context_ProjectName.md` - Detailed context with CTX-* IDs
- ✅ `requirement/Traceability_Matrix.md` - Context mappings

### Step 2: Verify Context Files

Check that all four standards files were created:

```bash
ls -la .github/instructions/
# Should show:
# - project-context.md
# - frontend-standards.md
# - backend-standards.md
# - testing-standards.md
```

### Step 3: Begin Specification Phase

**Now run:** Technical Specifier agent

The Specifier will automatically read your standards files and use them for design validation.

---

## Workflow 2: Ongoing Project Development

### Step 1: Regular Development Cycle

**Specifier → Planner → Builder → Tester → Documenter → Traceability Manager**

All agents automatically read from context files created by Context Creator.

### Step 2: Technology Upgrade Detection

**Scenario:** Your team upgrades a framework or major dependency

**Automatic Trigger:**
```
Context Updater agent (runs automatically when configuration files change)
  → Detects version or dependency changes in configuration files
  → Scans for breaking changes and new features
  → Updates relevant standards files with new patterns
  → Creates CTX-UPDATE-001 ID for change tracking
  → Alerts Traceability Manager if MAJOR impact
```

**Manual Trigger (if not automatic):**
```
Run: Context Updater agent
Input: Project root path (auto-detects changes)
```

### Step 3: Reassess Active Specs

**Who:** Technical Specifier or Traceability Manager
**When:** After Context Updater runs with MAJOR changes

```
Review Traceability_Matrix.md for:
  - New CTX-UPDATE-* entries
  - Affected specs that reference old standards
  
Re-run Specifier with updated context to validate
```

---

## Workflow 3: Architecture Refactoring

**Scenario:** Your team reorganizes backend services (e.g., split monolith into modules)

### Step 1: Document the Change

Update project folder structure. Context Updater will detect it.

### Step 2: Run Context Updater

```
Run: Context Updater agent
Input: Project root path
Output: Updated backend-standards.md with new architecture
```

### Step 3: Update Active Work

```
Traceability Manager
  → Assesses impact on active TASK-* entries
  → Flags implementations that may need updates
  → Recommends re-validation against new standards
```

---

## Workflow 4: Mid-Project Context Sync

**Scenario:** 2-3 weeks into implementation, multiple teams working on different flows

### Best Practice: Run Context Updater Weekly

```
Setup Automated Schedule:
  cron: "0 9 * * MON"  # Every Monday at 9 AM
  
Or Manual Trigger:
  Run: Context Updater agent (every Friday EOD)
```

**Benefits:**
- ✅ Catches all tech updates and dependency changes
- ✅ Keeps standards in sync across all teams
- ✅ Early detection of architecture drift
- ✅ Prevents specs/plans from becoming stale

---

## Common Tasks

### Task 1: Check Current Standards

**Question:** "What are the current frontend best practices?"

**Answer:**
```
Read: .github/instructions/frontend-standards.md
  → Architecture patterns
  → Component library
  → State management
  → Testing patterns
```

### Task 2: Review Context History

**Question:** "What context changes have been made?"

**Answer:**
```
Read: requirement/Context_{ProjectName}.md
  → "## Update History" section
  → Lists all CTX-UPDATE-* IDs
  → Shows what changed and when
```

### Task 3: Validate Spec Against Current Standards

**Question:** "Is my spec using current recommended patterns?"

**Answer:**
```
1. Open your spec.md
2. Read .github/instructions/{frontend|backend}-standards.md
3. Verify:
   - ✅ API design follows backend-standards.md patterns
   - ✅ Components use frontend-standards.md reusable widgets
   - ✅ Tests follow testing-standards.md structure
4. If mismatch found, either:
   - Update spec to use current standards, OR
   - Document deviation with reasoning
```

### Task 4: Understand Why a Standard Exists

**Question:** "Why do we have this specific router pattern?"

**Answer:**
```
1. Read: .github/instructions/project-context.md
   → "## Technology Baseline" section
2. Read: .github/instructions/frontend-standards.md
   → "## Router And Navigation" section
3. Read: requirement/Context_{ProjectName}.md
   → Search for router-related CTX-* IDs
```

---

## Troubleshooting

### Problem: "I can't find frontend-standards.md"

**Solution:**
```
Step 1: Check if it exists
  ls .github/instructions/frontend-standards.md

Step 2: If not, run Context Creator
  Context Creator agent → Follow prompts → Creates all standards files

Step 3: If it exists but outdated, run Context Updater
  Context Updater agent → Auto-syncs all standards
```

### Problem: "My implementation doesn't match standards"

**Solution:**
```
Option A: Update implementation to match standards
  → Read backend-standards.md or frontend-standards.md
  → Refactor to follow recommended patterns
  → Document changes as CODE-* IDs

Option B: Document exception
  → In implementation evidence, note deviation
  → Explain why deviation was necessary
  → Link to approval (spec clarification, CLAR-*, etc.)
```

### Problem: "The spec uses an outdated pattern"

**Solution:**
```
Step 1: Check Traceability_Matrix.md for CTX-UPDATE-* IDs
  → See what context changes affected this area

Step 2: Re-run Specifier with updated context
  → Specifier will validate against current standards
  → Update SPEC-* with new guidance

Step 3: If spec already finalized, document gap
  → Record in compliance-validation.md
  → Link to CTX-UPDATE-* that caused drift
```

### Problem: "Context Updater found MAJOR changes. Now what?"

**Solution:**
```
Step 1: Review CTX-UPDATE-* ID details
  → Understand what changed (dep version, architecture, etc.)

Step 2: Assess impact
  → Which specs reference affected standards?
  → Which implementations depend on old patterns?

Step 3: Prioritize re-validation
  → Update specs using affected standards
  → Re-validate active implementations
  → Update tests if test standards changed

Step 4: Close CTX-UPDATE-* entry
  → Mark as reviewed in requirement/Context_{ProjectName}.md
  → Link to re-validation tasks
```

---

## Key Concepts

### CTX-* IDs
- Created by Context Creator during initial setup
- Track business drivers, stakeholders, tech decisions
- Example: `CTX-TECH-001: Framework and version choice with rationale`
- Appear in `requirement/Context_{ProjectName}.md` and `Traceability_Matrix.md`

### CTX-UPDATE-* IDs
- Created by Context Updater when context changes
- Track what changed, when, and impact level
- Example: `CTX-UPDATE-001: Framework version upgrade (MINOR)`
- Enable tracking of context evolution over time

### Standards Files
- **frontend-standards.md** - Frontend tech patterns (created once, updated as needed)
- **backend-standards.md** - Backend tech patterns (created once, updated as needed)
- **testing-standards.md** - Testing tech patterns (created once, updated as needed)
- All are discoverable by downstream agents via `.applyTo` patterns

### Context Precedence
```
  1. Current standards files (.github/instructions/*)
  2. Approved specs (output/{RUN_ID}/specify/spec.md)
  3. Approved plans (output/{RUN_ID}/plan/plan.md)
  4. Industry best practices (fallback only)
```

---

## Summary

**New Context Management System enables:**

✅ **Unified baseline** - All agents read from same context files  
✅ **Tech detection** - Automatic stack discovery on project setup  
✅ **Synchronized standards** - Frontend/backend/testing standards stay in sync  
✅ **Continuous alignment** - Context Updater keeps standards current  
✅ **Complete traceability** - CTX-* and CTX-UPDATE-* IDs track all decisions  
✅ **Impact detection** - Breaking changes escalated for approval  

**Result:** Consistent, maintainable PDLC with single source of truth for all technical decisions.

---

## Next Steps

1. **Run Context Creator** for your project
2. **Verify** all four standards files were created
3. **Review** `.github/CONTEXT_MANAGEMENT_GUIDE.md` for detailed reference
4. **Begin** specification phase with updated Specifier agent
5. **Schedule** weekly Context Updater runs (or enable automatic trigger)

---

**Questions?** Refer to [CONTEXT_MANAGEMENT_GUIDE.md](./../CONTEXT_MANAGEMENT_GUIDE.md) for detailed documentation.
