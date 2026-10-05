---
applyTo: "frontend/test/features/**/*.feature,frontend/test/step/**/*.dart"
description: "BDD and Gherkin standards for NexusBank frontend scenarios and step definitions."
---

# Frontend BDD Standards

Use these standards when writing `.feature` files or their step definitions under `frontend/test/`.

## References
- Follow the [Cucumber Gherkin Reference](https://cucumber.io/docs/gherkin/reference/) for feature-file structure and keywords.
- Follow [Cucumber's BDD Guide](https://cucumber.io/docs/bdd/) for the discovery, formulation, and automation approach. Gherkin syntax alone does not replace understanding the expected behavior.
- Apply `.github/instructions/test-standards.instructions.md` and `.github/instructions/flutter-standards.instructions.md` where relevant.

## Scenario Writing
- Write one `Feature` per `.feature` file. Describe the capability from the user's or business domain's point of view.
- Keep each `Scenario` focused on one behavior or business rule. Aim for concise examples, generally about 3-5 steps, without combining unrelated actions and outcomes.
- Use `Given` for meaningful starting conditions, `When` for the event or user action, and `Then` for the resulting observable behavior. Use `And` or `But` only to continue the preceding step type clearly.
- Use domain language. Keep Flutter widget types, class names, selectors, Provider details, and other implementation mechanics in step definitions, not in scenario prose.
- Make each `Then` an observable, deterministic assertion, such as a message shown to the user, a changed control state, or a navigation result. Avoid assertions about internal implementation state unless that state is itself an approved external contract.
- Use `Background` only for short setup genuinely shared by every scenario in the feature. Prefer a readable high-level setup step over long technical setup.
- Use `Scenario Outline` and synthetic `Examples` when several data variations exercise the same rule; keep separate scenarios when the behavior or expected outcome differs.
- Put a stable `TEST-*` identifier in each scenario name so the generated test is easy to identify and run with `--plain-name`. Include related `AC-*`, `FR-*`, or Jira identifiers in tags or comments when available.

## Evidence and Assumptions
- Search local requirements and Jira story artifacts for the requested feature before authoring scenarios. Confirm that a candidate artifact actually covers the feature.
- When matching criteria exist, derive expected outcomes from them and record their source identifiers. If implementation behavior conflicts with criteria, keep the expected behavior tied to the criteria and report the discrepancy.
- When no matching criteria/story exists, state that the scenarios are implementation-derived. Do not represent them as approved acceptance coverage.
- Use synthetic data only. Never include real customer information, account/card numbers, holdings, credentials, tokens, or production data.

## Step Definitions and Execution
- Reuse `bdd_widget_test` built-in steps when they express the behavior clearly. Add a custom step only for an app-specific action or assertion that built-ins cannot express cleanly.
- Keep each custom step small, deterministic, and named for its behavior. Do not use live services or production data; use established fakes/mocks where external responses are required.
- Assert the actual user-visible state. For example, verify that a password field is masked or revealed rather than checking only which eye icon is rendered.
- Generate and run only the requested feature's test. Do not run unrelated Flutter widget tests, unit tests, or the full suite as part of this BDD workflow.
- Do not weaken a scenario to turn an implementation failure into a passing test. Record the observed failure, evidence, likely cause, and next fix in the execution report.