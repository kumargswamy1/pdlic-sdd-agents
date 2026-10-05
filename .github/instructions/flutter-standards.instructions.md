---
applyTo: "frontend/**/*.dart"
description: "Flutter frontend standards for NexusBank"
---

Follow:
- `.github/rules/architectural-standards.md`
- `.github/rules/security-quality-standards.md`
- `.github/rules/environment-standards.md`
- `.github/rules/versioning-standards.md`

## Flutter Standards
- Implement only approved requirements and plan tasks; preserve `FR-*`, `NFR-*`, `SPEC-*`, `TASK-*`, and acceptance-criteria traceability in implementation and tests.
- Use sound null safety and explicit Dart types for widget inputs, models, service results, route parameters, and API contracts; avoid `dynamic` and unchecked casts.
- Keep widgets focused on presentation and interaction; place orchestration and domain behavior in services, providers, or other established state-management layers.
- Use Provider for shared application state and expose immutable or read-only views where practical; avoid mutable global state and hidden widget-to-widget coupling.
- Use GoRouter for navigation and keep route names, parameters, redirects, and guarded flows centralized and deterministic.
- Use the generated OpenAPI client and shared API client/services for backend access; do not place ad-hoc HTTP calls or response parsing in widgets.
- Represent loading, success, empty, validation, unauthorized, and failure states explicitly for user-facing asynchronous flows.
- Use the shared theme, design tokens, and Google Fonts configuration; avoid duplicating hard-coded colors, typography, spacing, or component styling.
- Keep user-visible text localizable through the existing `assets/i18n/` bundles and update supported locales consistently.
- Store authentication secrets only through secure storage; never log tokens, credentials, PII, account numbers, portfolio holdings, or raw API payloads.
- Validate authorization, customer ownership, product suitability, consent, and approval state at the UI boundary when required by the approved flow; treat backend authorization as authoritative.
- Build accessible interfaces with semantic labels, sufficient contrast, logical focus/order, scalable text, and touch targets appropriate for mobile and web.
- Use deterministic widget, unit, and integration tests with synthetic data; cover positive, validation, authorization, empty, failure, and localization paths where applicable.
- Dispose controllers, focus nodes, streams, and other resources owned by a widget or service; avoid leaks across route changes.

## Spec-Driven Delivery Gates
- Every implemented screen or user flow must map to a requirement and have corresponding verification evidence.
- Any deviation from the approved specification must document the rationale and required approval.
- Changes affecting KYC, AML, PDPA, consent, suitability, maker-checker, or audit evidence require compliance review before merge.