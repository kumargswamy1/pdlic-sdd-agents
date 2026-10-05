---
applyTo: "backend/**/*.js"
description: "Node.js and Express backend standards for NexusBank"
---

Follow:
- `.github/rules/architectural-standards.md`
- `.github/rules/security-quality-standards.md`
- `.github/rules/environment-standards.md`
- `.github/rules/versioning-standards.md`

## Node.js Standards
- Implement only approved requirements and plan tasks; preserve `FR-*`, `NFR-*`, `SPEC-*`, `TASK-*`, and acceptance-criteria traceability in implementation and tests.
- Target the repository-supported Node.js runtime (`>=18.0.0`) and use consistent CommonJS module conventions already established by the backend.
- Keep route registration, controllers, services, middleware, and mock data in their existing layers; controllers should coordinate requests while services own business rules and state changes.
- Validate and normalize request input at route boundaries with `express-validator` or the established equivalent; reject malformed, missing, duplicate, or out-of-range values before business logic runs.
- Enforce authentication, authorization, customer ownership, product eligibility, and approval rules in backend services or middleware; never rely on Flutter or other clients for access control.
- Use stable versioned API routes and the documented response envelope; keep HTTP status codes and error messages consistent with the OpenAPI contract.
- Centralize error handling through the existing `notFound` and `errorHandler` middleware; do not leak stack traces, secrets, internal paths, or sensitive implementation details to clients.
- Keep asynchronous handlers and downstream service failures observable and safely propagated; avoid unhandled promise rejections and partial financial updates.
- Preserve atomicity for transfers and other financial mutations: validate first, update related records consistently, and roll back or compensate on mid-operation failure.
- Configure CORS, Helmet, body parsing, logging, and environment-dependent behavior through the application middleware boundary; do not weaken production controls for convenience.
- Load configuration from environment variables, keep secrets out of source and logs, and fail safely when required configuration is absent or invalid.
- Never log plaintext PII, credentials, tokens, account numbers, portfolio holdings, or full request/response bodies; use masked synthetic data in tests and examples.
- Keep Swagger/OpenAPI documentation synchronized with routes, parameters, response envelopes, authentication requirements, and error statuses.
- Use deterministic Jest and Supertest tests with synthetic fixtures; cover success, validation, authentication/authorization, cross-customer access, business-rule, atomicity, and integration-failure paths.
- Keep test state isolated and restore in-memory mocks between tests; do not depend on execution order or real external services.

## Spec-Driven Delivery Gates
- Every endpoint or business-rule change must map to a requirement and have corresponding unit or integration verification.
- Any deviation from the approved API contract or specification must document the rationale and required approval.
- Changes affecting KYC, AML, PDPA, consent, suitability, maker-checker, or audit evidence require compliance review before merge.