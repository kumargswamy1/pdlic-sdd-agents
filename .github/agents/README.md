# SDLC Agents - Complete Usage Guide

This document describes all available agents in the spec-driven development lifecycle (SDLC) workflow and how to use them.

## Overview

The agents are organized into a sequential SDLC workflow that transforms business requirements into implemented, tested, and documented code. Each agent is specialized for a specific phase and reads/writes standardized artifacts with trace IDs.

## Agent Workflow Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                      SPEC-DRIVEN DEVELOPMENT FLOW                   │
└─────────────────────────────────────────────────────────────────────┘

Step 1: Context Foundation
   ↓
   Context Creator
   Context Updater (runs continuously)
   ↓
Step 2: Requirements & Specification
   ↓
   Technical Specifier
   ↓
Step 3: Planning & Architecture (includes unit test planning)
   ↓
   Implementation Planner
   ↓
Step 4: Implementation & Unit Testing
   ↓
   Implementation Builder (creates code + unit tests)
   ↓
Step 5: Quality Assurance - Functional Testing
   ↓
   Quality Tester (creates end-to-end and functional tests)
   ↓
Step 6: Review & Compliance
   ↓
   Compliance Reviewer
   ↓
Step 7: Documentation & Traceability
   ↓
   Technical Documenter
   Traceability Manager
   ↓
Step 8 (Design Support - Parallel)
   ↓
   Figma Extractor
   ↓
All Phases: Continuous Validation
   ↓
   Compliance Reviewer (throughout)
```

---

## Agents by Phase

### Phase 1: Context & Foundation

#### **Context Creator**
**File:** `context-creator.agent.md`

**When to use:** 
- Creating comprehensive project context from scratch
- Starting a new project or onboarding a new codebase
- Establishing business, technical, and standards documentation

**What it does:**
- Scans project structure and source code to detect technology stack
- Gathers business context, stakeholders, compliance requirements
- For greenfield projects: prompts user for tech stack and business overview
- Generates tech-specific standards files with best practices
- Creates unified project context with CTX-* trace IDs

**Output files created:**
- `.github/instructions/project-context.md`
- `.github/instructions/frontend-standards.md`
- `.github/instructions/backend-standards.md`
- `.github/instructions/testing-standards.md`
- `.github/instructions/general-standards.md`
- `requirement/Context_{ProjectName}.md`
- Updated `requirement/Traceability_Matrix.md`

**User-invocable:** Yes ✅

---

#### **Context Updater**
**File:** `context-updater.agent.md`

**When to use:**
- Automatically triggered when project files change
- After dependency updates or framework upgrades
- Detecting tech stack changes or folder reorganization

**What it does:**
- Scans configuration files for version/dependency changes
- Identifies breaking changes and new features
- Updates standards files with new patterns
- Creates CTX-UPDATE-* IDs for change tracking
- Alerts on MAJOR impact changes

**Triggers automatically on:**
- `package.json`, `pubspec.yaml`, `requirements.txt` changes
- Folder structure reorganization
- Framework version changes

**User-invocable:** No (automatic)

---

### Phase 2: Requirements & Specification

#### **Technical Specifier**
**File:** `specify.agent.md`

**When to use:**
- Converting requirements into technical specifications
- Starting implementation planning for a requirement
- Taking Jira issues as input and creating specs
- Detailing API contracts, database schemas, integration points

**What it does:**
- Accepts Jira issue keys, requirement files, or PRD files
- Fetches issue details and extracts requirements
- Creates canonical requirement markdown files
- Generates implementation-ready technical specifications
- Maps FR-* (Functional) and NFR-* (Non-Functional) to SPEC-* IDs
- Documents data models, API contracts, interface designs

**Input options:**
- Jira key: `ABCAPP-123`
- File path: `requirement/MyApp_Requirements.md`
- Active file (press Enter)

**Reads from:**
- `.github/instructions/project-context.md`
- `.github/instructions/frontend-standards.md`
- `.github/instructions/backend-standards.md`

**Output:**
- `requirement/{ProjectName}_Specification.md` with SPEC-* IDs
- Updated `requirement/Traceability_Matrix.md`

**User-invocable:** Yes ✅

---

### Phase 3: Planning & Architecture

#### **Implementation Planner**
**File:** `planner.agent.md`

**When to use:**
- Breaking down technical specifications into implementation tasks
- Creating task DAGs (directed acyclic graphs)
- Planning sprints and milestone delivery
- Identifying dependencies between tasks
- Planning unit tests alongside implementation

**What it does:**
- Reads technical specifications (SPEC-* IDs)
- Creates detailed implementation plans with task breakdowns
- Generates task DAGs showing dependencies
- **Plans unit tests as part of each implementation task** (not separate)
- **Plans functional/end-to-end test scenarios** for Quality Tester to execute
- Assigns effort estimates and sequencing
- Maps tasks to TASK-* IDs
- Identifies frontend/backend/testing/documentation tasks
- Includes unit test framework references (Jest, pytest, JUnit, etc.)
- Documents unit test coverage targets (80%+ for business logic, 90%+ for validation)

**Unit Test Planning Includes:**
- **Backend unit tests** for each service, controller, model, middleware
- **Frontend unit tests** for each component, hook, service, utility
- **Test framework** selection and configuration
- **Mock data** and fixture strategies
- **Coverage targets** and verification commands
- **Integration test** dependencies on backend contract verification

**Reads from:**
- Specifications with SPEC-* IDs
- `.github/instructions/frontend-standards.md`
- `.github/instructions/backend-standards.md`
- `.github/instructions/testing-standards.md` (unit test patterns and examples)

**Output:**
- `requirement/{ProjectName}_Plan.md` with TASK-* IDs
- `plan.md/tasks.md` including unit test tasks (BLOCKED_BY implementation tasks)
- `plan.md` Functional Test Scenarios section (for Quality Tester)
- Task dependency graph
- Updated `requirement/Traceability_Matrix.md`

**User-invocable:** Yes ✅

---

### Phase 4: Implementation & Unit Testing

#### **Implementation Builder**
**File:** `implement.agent.md`

**When to use:**
- Implementing code from approved plans
- Starting a specific task from the task file
- Creating features, components, or backend services
- Following established standards and patterns
- Creating unit tests alongside implementation (test-driven development)

**What it does:**
- Reads implementation plans (TASK-* IDs)
- Generates production-ready code
- **Creates unit tests as part of implementation** (not separately)
- Follows tech-stack-specific standards
- Reuses approved components and libraries
- Documents code inline with standards

**Unit Test Implementation:**
- **Frontend unit tests** for components, hooks, services, state management
- **Backend unit tests** for services, controllers, models, middleware
- **Test frameworks** (Jest, Vitest, pytest, JUnit, etc.)
- **Mock data** and fixtures for isolated testing
- **Coverage targets** (80%+ for business logic, 90%+ for validation)
- **Verification commands** documented in task description

**Frontend Unit Tests Created:**
- Component render and event tests
- Hook tests with state and side effects
- Service/API client tests with mocking
- Form validation tests
- Navigation/routing tests
- State management tests (reducers, selectors)

**Backend Unit Tests Created:**
- Service/business logic tests
- Controller/API endpoint tests
- Model/ORM tests
- Middleware tests (auth, validation)
- Integration tests (database, external services)

**Reads from:**
- Plans with TASK-* IDs
- `.github/instructions/frontend-standards.md`
- `.github/instructions/backend-standards.md`
- `.github/instructions/testing-standards.md` (unit test patterns and examples)
- `.github/agents/planner.agent.md` for unit test coverage targets

**Output:**
- Implemented source code files
- Unit test files (backend and frontend)
- Code comments and documentation
- Unit test coverage reports
- Updated `requirement/Traceability_Matrix.md` (CODE-* and TEST-* IDs)

**User-invocable:** Yes ✅

---

### Phase 5: Quality Assurance - Functional Testing

#### **Quality Tester**
**File:** `tester.agent.md`

**When to use:**
- Creating end-to-end and functional test scenarios after implementation is complete
- Testing complete user workflows spanning frontend, backend, and database
- Validating business logic completeness and user experience
- Creating BDD/Gherkin-style test scenarios

**What it does:**
- Reads technical specifications and functional requirements
- Creates end-to-end test scenarios for user workflows
- Plans functional test cases covering happy paths, error scenarios, edge cases
- Maps tests to SPEC-* requirements and acceptance criteria
- Focuses on **user-facing behavior**, not isolated components
- **Note:** Unit tests are created during implementation (planning and implementation phases), not here

**Tests user workflows like:**
- Complete user journeys (login → search → filter → select → checkout)
- Error handling (invalid inputs, API failures, network errors)
- State transitions and workflow progression
- Data integrity (create-read-update-delete operations)
- Cross-layer flows (UI → API → Backend → Database)
- Performance and accessibility

**Reads from:**
- Specifications with SPEC-* IDs
- `plan.md` Functional Test Scenarios section
- `.github/instructions/frontend-standards.md`
- `.github/instructions/backend-standards.md`

**Output:**
- Functional test scenarios (BDD/Gherkin format)
- End-to-end test cases
- Test execution reports
- Updated `requirement/Traceability_Matrix.md` with functional test references

**User-invocable:** Yes ✅

---

### Phase 6: Review & Compliance

#### **Compliance Reviewer**
**File:** `reviewer.agent.md`

**When to use:**
- Reviewing any artifact (specs, plans, code, tests, docs)
- Validating compliance with standards and regulations
- Checking security and quality requirements
- Ensuring traceability is maintained

**What it does:**
- Reviews artifacts against compliance criteria
- Validates security, quality, and architecture standards
- Checks for traceability breaks
- Identifies compliance gaps
- Provides severity-graded findings (CRITICAL, HIGH, MEDIUM, LOW)
- Recommends remediation actions
- Verifies regulatory requirement fulfillment

**Can review:**
- Technical specifications (SPEC-* artifacts)
- Implementation plans (TASK-* artifacts)
- Source code (CODE-* artifacts)
- Test suites (TEST-* artifacts)
- Documentation (DOC-* artifacts)

**Output:**
- Compliance review report
- Issues and findings
- Remediation recommendations

**User-invocable:** Yes ✅

---

### Phase 7: Documentation & Traceability

#### **Technical Documenter**
**File:** `technical-documenter.agent.md`

**When to use:**
- Creating technical documentation from specifications
- Writing API documentation
- Generating architecture documentation
- Creating deployment and operations guides
- Building developer runbooks

**What it does:**
- Reads specifications and implementations
- Generates comprehensive technical documentation
- Creates API reference documents
- Documents system architecture
- Writes deployment procedures
- Creates troubleshooting guides
- Generates final project documentation

**Reads from:**
- Specifications with SPEC-* IDs
- Implementation code with CODE-* IDs
- `.github/instructions/project-context.md`

**Output:**
- API documentation
- Architecture documentation
- Deployment guides
- Runbooks and troubleshooting guides
- Project README
- Updated `requirement/Traceability_Matrix.md` (DOC-* IDs)

**User-invocable:** Yes ✅

---

#### **Traceability Manager**
**File:** `traceability-manager.agent.md`

**When to use:**
- Validating end-to-end traceability
- Tracking requirements through implementation
- Identifying traceability gaps
- Generating traceability reports
- Ensuring all artifacts are linked

**What it does:**
- Maps all trace IDs across workflow phases
- Validates that every requirement is implemented and tested
- Identifies orphaned artifacts
- Detects circular dependencies
- Generates traceability reports
- Maintains `requirement/Traceability_Matrix.md`
- Provides visibility into requirement status

**Tracks:**
- CTX-* (Context) → BRD-* → FR-*/NFR-* (Requirements)
- FR-*/NFR-* → SPEC-* (Specifications)
- SPEC-* → TASK-* (Plans)
- TASK-* → CODE-* (Implementation)
- CODE-* → TEST-* (Tests)
- All → DOC-* (Documentation)

**Output:**
- Updated `requirement/Traceability_Matrix.md`
- Traceability gap reports
- Status dashboards

**User-invocable:** Yes ✅

---

### Phase 8: Design Integration (Parallel)

#### **Figma Extractor**
**File:** `figma-extractor.agent.md`

**When to use:**
- Extracting Figma design artifacts from requirements
- Converting design files to structured design data
- Creating effective screen tables and cropped images
- Mapping design screens to specification sections

**What it does:**
- Reads requirement file with Figma links
- Extracts design metadata from Figma
- Downloads screenshots and design assets
- Crops and organizes screen images
- Generates effective screen tables
- Creates figma_report.md with design analysis
- Maps design screens to requirement sections

**Input:**
- Requirement file path with Figma file key and node IDs

**Output:**
- `figma_report.md` - Design extraction report
- Parent JSON - Complete Figma tree structure
- Parent PNG - Full screenshot at scale
- Cropped screen PNGs - Individual screen images
- Effective screen table - Design structure mapping

**User-invocable:** Yes ✅

---

## Reference Files

#### **agent-frontmatter-reference.md**
**File:** `agent-frontmatter-reference.md`

**Purpose:**
- Reference guide for agent frontmatter configuration
- Template for creating new agents
- Best practices for agent design
- Contains real-world examples and patterns

**Contains:**
- 10 frontmatter parameters explained
- Best-practice checklist
- Starter template for new agents
- Real-world examples from actual agents

---

## How to Invoke Agents

### In VS Code Chat

1. **Open Copilot Chat** (Ctrl+Shift+I / Cmd+Shift+I)
2. **Type agent name** or use the agent dropdown
3. **Provide required inputs** (file paths, Jira keys, etc.)
4. **Agent executes** and produces artifacts

### Command Line

```bash
# From VS Code terminal or command line
# (if MCP server integration is configured)
copilot-agent "Context Creator"
copilot-agent "Technical Specifier" --file requirement/MyApp_Requirements.md
copilot-agent "Implementation Builder" --plan requirement/MyApp_Plan.md
```

### From Requirements

Agents can be triggered from requirement markdown files with frontmatter:

```markdown
---
trigger-agent: "Technical Specifier"
input-file: "requirement/MyApp_Requirements.md"
---

# My Requirement
...
```

---

## Traceability IDs

Each phase generates unique trace IDs for linking:

| Phase | ID Prefix | Example | Meaning |
|-------|-----------|---------|---------|
| Context | CTX-* | CTX-TECH-001 | Technology decision |
| Context Update | CTX-UPDATE-* | CTX-UPDATE-001 | Context change |
| Requirements | FR-*, NFR-*, BRD-* | FR-001, NFR-001 | Functional, Non-Functional requirement |
| Specification | SPEC-* | SPEC-001 | Technical specification |
| Planning | TASK-* | TASK-001 | Implementation task |
| Implementation | CODE-* | CODE-001 | Code artifact |
| Testing | TEST-* | TEST-001 | Unit test case/suite |
| Documentation | DOC-* | DOC-001 | Documentation artifact |

---

## Unit Testing & Quality Assurance Separation

### Unit Testing (Planning + Implementation Phases)

**When**: Planned during Implementation Planner phase, created during Implementation Builder phase

**Who**: Implementation Builder creates unit tests as part of implementation

**What**: Isolated component/module testing
- Frontend: Components, hooks, services, utilities, state management
- Backend: Services, controllers, models, middleware
- Coverage targets: 80%+ for business logic, 90%+ for validation

**How**: Created during implementation using tech-stack-specific frameworks
- Frontend: Jest, Vitest, React Testing Library, Flutter test
- Backend: pytest, Jest, JUnit, unittest

**Why**: Unit tests verify implementation correctness and enable safe refactoring

**Test Structure:**

Frontend:
```
frontend/test/unit/
├── components/          # Component unit tests
├── hooks/              # Hook tests
├── services/           # Service/API tests
├── utils/              # Utility function tests
├── state/              # State management tests
└── __mocks__/          # Mock data and fixtures
```

Backend:
```
backend/test/unit/
├── controllers/        # API endpoint tests
├── services/          # Business logic tests
├── models/            # Database model tests
├── middleware/        # Middleware tests
├── utils/             # Utility function tests
└── fixtures/          # Test data and mocks
```

---

### Functional Testing (Quality Assurance Phase)

**When**: After implementation is complete

**Who**: Quality Tester creates functional/end-to-end tests

**What**: Complete user workflow testing
- User journeys (login → search → select → checkout)
- Error scenarios and edge cases
- Cross-layer flows (UI → API → Backend → Database)
- Performance and accessibility
- End-to-end BDD/Gherkin-style scenarios

**How**: Created using BDD frameworks or end-to-end test tools
- BDD: Gherkin syntax for readable test scenarios
- E2E: Playwright, Cypress, or similar tools
- Manual testing when appropriate

**Why**: Functional tests verify complete system behavior and user experience

---

### Coverage Targets

| Layer | Target | Priority | Who Creates |
|-------|--------|----------|-------------|
| Business Logic Unit Tests | 80%+ | CRITICAL | Implementation Builder |
| Validation Unit Tests | 90%+ | CRITICAL | Implementation Builder |
| API Endpoint Tests | 75%+ | HIGH | Implementation Builder |
| UI Component Tests | 70%+ | HIGH | Implementation Builder |
| Error Handling Tests | 80%+ | HIGH | Implementation Builder |
| User Workflow Tests | 100% coverage | CRITICAL | Quality Tester (functional) |
| Edge Case Tests | 80%+ coverage | HIGH | Quality Tester (functional) |

---

## Recommended Workflow

### New Project
```
1. Context Creator (establish baseline)
   ↓
2. Business input → BRD Builder (if needed)
   ↓
3. Requirements → Technical Specifier
   ↓
4. Spec → Implementation Planner (plans implementation + unit tests + functional tests)
   ↓
5. Plan → Implementation Builder (creates code + unit tests)
   ↓
6. Code → Compliance Reviewer (validates quality and coverage)
   ↓
7. Implementation → Quality Tester (creates functional/end-to-end tests)
   ↓
8. Tests + Code → Compliance Reviewer (final validation)
   ↓
9. All artifacts → Technical Documenter
   ↓
10. All → Traceability Manager (validate end-to-end)
```

### Existing Project
```
1. Context Creator (scan and establish)
   ↓
2. Requirement input → Technical Specifier
   ↓
3-10. Same as New Project
```

### Continuous Updates
```
• Code changes → Context Updater (automatic trigger)
• New requirement → Technical Specifier
• Specification change → Implementation Planner (re-plan including unit tests)
• Implementation → Includes unit tests (planned + created together)
• Quality Tester → Functional tests after implementation complete
• Traceability Manager (runs between phases)
```

### Key Principles
1. **Unit Tests with Implementation**: Created during implementation phase, not after
2. **Functional Tests Separate**: Quality Tester creates end-to-end tests after implementation
3. **Planning Includes Testing**: Planner defines unit test strategy alongside implementation tasks
4. **Test Coverage Target**: 80%+ for business logic, 90%+ for validation (set in planning, achieved during implementation)

---

## Standards Files

Each project has standards files created/maintained by agents:

- **project-context.md** - Business and technology baseline
- **frontend-standards.md** - Frontend tech patterns and best practices
- **backend-standards.md** - Backend tech patterns and best practices
- **testing-standards.md** - Testing frameworks and strategies (includes unit test patterns)
- **general-standards.md** - Cross-cutting standards (git, CI/CD, security, infrastructure)

All agents read from and respect these standards during their work.

---

## Troubleshooting

### "Agent not found"
- Check agent name spelling (case-sensitive)
- Ensure agent file exists in `.github/agents/`
- Agent must have `user-invocable: true` in frontmatter

### "Missing required input"
- Check argument-hint in agent frontmatter
- Provide required file paths or Jira keys
- Active file must be in workspace

### "Traceability broken"
- Run Traceability Manager to identify gaps
- Check that all IDs reference correctly
- Ensure artifacts follow naming conventions

### "Standards not applied"
- Verify standards files exist in `.github/instructions/`
- Check that agent reads from correct standards
- Context Creator should be run first for new projects

### "Test coverage incomplete"
- Run Quality Tester for comprehensive test design
- Check testing-standards.md for tech-stack patterns
- Ensure Implementation Builder includes unit tests
- Verify test file locations follow project structure

---

## Best Practices

1. **Always start with Context Creator** for new projects
2. **Follow the workflow sequence** - don't skip phases
3. **Keep standards files updated** via Context Updater
4. **Run Quality Tester** to ensure comprehensive unit test coverage
5. **Run Traceability Manager periodically** to validate
6. **Use Compliance Reviewer** before merging to main
7. **Document decisions** in appropriate standards files
8. **Maintain trace IDs** consistently across all artifacts
9. **Update context** when technology stack changes
10. **Include unit tests** with every implementation task

---

## Additional Resources

- `.github/copilot-instructions.md` - Central orchestration guide
- `.github/SPEC_DRIVEN_WORKFLOW.md` - Workflow specifications
- `requirement/Traceability_Matrix.md` - Live traceability matrix
- `.github/instructions/testing-standards.md` - Unit test patterns and frameworks
- `.github/instructions/` - All standards and guidelines
- `.github/rules/` - Project-specific rules and architecture standards

