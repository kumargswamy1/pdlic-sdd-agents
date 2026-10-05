---
description: "NexusBank project context for SDD planning, implementation, testing, review, and documentation"
---

# NexusBank Project Context

Use this document as the repository-wide context baseline for SDD agents working on the NexusBank Flutter frontend and Node.js backend. Approved requirements, specifications, plans, and applicable rules remain authoritative when they impose stricter or more specific constraints.

## Product And Industry Context

NexusBank is a digital banking application for customer authentication, account visibility, cards, transaction history, beneficiaries, fund transfers, profile management, statements, and language preferences. It has a Flutter client and a Node.js/Express REST API.

Banking software must prioritize confidentiality, integrity, availability, non-repudiation, traceability, least privilege, and predictable failure handling. A convenient UI must never weaken server-side authorization or financial controls.

### Banking Domain Principles

- Treat the authenticated customer, account ownership, beneficiary ownership, transaction history, and transfer authorization as separate security boundaries.
- Apply KYC, AML, customer risk profiling, product suitability, consent, disclosure, maker-checker, transaction limits, cooling-off, audit, and retention controls only when required by the approved scope.
- Financial amounts require explicit currency, precision, rounding, sign, and limit rules. Never use locale-formatted strings as calculation inputs.
- Financial mutations should be idempotent where retries are possible, produce a stable reference, and preserve atomicity or use an approved compensation workflow.
- Status transitions must be explicit and auditable, for example `pending` to `completed`, `failed`, or `cancelled`; invalid transitions must be rejected.
- Customer-facing errors should be actionable without revealing account existence, credentials, internal identifiers, stack traces, or security-sensitive policy details.
- Audit records for sensitive actions should identify the actor, action, target, outcome, timestamp, correlation/reference ID, and reason where required, without storing unnecessary sensitive payloads.

## Technology Baseline

- Frontend: Flutter and Dart 3+, Provider for shared state, GoRouter for navigation, Dio for transport, a generated OpenAPI client, Flutter localization, secure storage dependencies, and shared theme tokens.
- Backend: Node.js 18+, Express 4, CommonJS modules, express-validator conventions, Helmet, CORS, Morgan, Swagger/OpenAPI, and dotenv-based configuration.
- Testing: Flutter tests under `frontend/test/`; Jest and Supertest integration tests under `backend/tests/`.
- Current backend persistence and identity flows are in-memory development stubs. Do not treat mock login, OTP, MPIN, biometric, token, or transfer behavior as production-ready security or transaction processing.

## Repository Architecture

### Flutter Frontend

#### Frontend Architecture

The Flutter app is a feature-oriented, layered application. Preserve these dependency directions when adding or changing frontend code:

```text
screens/widgets -> services/state -> generated API client or Dio -> backend /api/v1
screens/widgets -> models and core theme/router/localization
models -> no presentation or network dependencies
core -> cross-cutting configuration only; never feature-specific UI
```

- `frontend/lib/core/` owns cross-cutting configuration: API constants, Dio/auth networking, GoRouter, and the global theme.
- `frontend/lib/api/` contains the generated OpenAPI client and the hand-written client adapter that injects auth and unwraps `{ success, data }` envelopes. Keep generated files isolated from hand-written code because regeneration may overwrite generated files.
- `frontend/lib/models/` contains typed domain models and JSON mapping. Models must not depend on widgets, screens, or services.
- `frontend/lib/services/` owns API orchestration, request/response mapping, authentication, locale persistence, and feature business coordination. Do not place HTTP calls or response parsing in widgets.
- `frontend/lib/screens/` contains route-level feature screens. Current feature folders are `auth/`, `account/`, `beneficiary/`, `profile/`, `settings/`, and `splash/`.
- `frontend/lib/widgets/` is the reserved location for reusable presentation widgets. Reuse an existing widget before introducing a feature-specific duplicate; the directory is currently available for future shared components.
- `frontend/test/` contains Flutter unit/widget tests; test feature behavior, navigation, localization, validation, async states, and failure paths.
- `frontend/assets/i18n/` contains user-visible English and Thai JSON bundles. `frontend/assets/images/` contains image assets such as `logo.png`.

#### Verified Frontend Folder Structure

Use this structure as the implementation map. Do not create parallel folders with alternative responsibilities:

```text
frontend/
	assets/
		i18n/                 # en.json, th.json
		images/               # logo and future image assets
	lib/
		api/
			api_config.dart
			nexusbank_api_client.dart
			generated/          # generated OpenAPI package; do not hand-edit
		core/
			constants/          # API base URL, version, app constants
			network/             # Dio singleton, auth interceptor, TokenStore
			router/              # central GoRouter configuration
			theme/               # NexusBankColors, TextStyles, spacing, radii, ThemeData
		models/                # account, beneficiary, card, user models
		screens/
			account/
			auth/
			beneficiary/
			profile/
			settings/
			splash/
		services/              # auth, account, card, beneficiary, locale services
		main.dart              # bootstrap LocaleService and MaterialApp.router
	test/                    # Flutter tests
```

#### Design System And Global Styles

The source of truth is `frontend/lib/core/theme/app_theme.dart`. New UI must use these tokens and the global `NexusBankTheme.light` instead of inventing local brand values.

**Primary color codes**

| Token | Hex | Usage |
|---|---|---|
| `NexusBankColors.primary` | `#1F2E45` | Navy brand color, primary headings, navigation icons, dark text |
| `NexusBankColors.accent` | `#4B56FC` | Primary CTA buttons, focused controls, active states |
| `NexusBankColors.indigo` | `#4F46E5` | Secondary indigo accent and selected states where specified |
| `NexusBankColors.backgroundLight` | `#F4F6FA` | Default scaffold/page canvas |
| `NexusBankColors.backgroundAlt` | `#F5F6FA` | Alternate light canvas |
| `NexusBankColors.surface` | `#FFFFFF` | Cards, form fields, overlays |
| `NexusBankColors.textBody` | `#444444` | Body and secondary text |
| `NexusBankColors.textDark` | `#12141D` | High-contrast labels and dark UI text |
| `NexusBankColors.success` | `#22C55E` | Success state |
| `NexusBankColors.error` | `#EF4444` | Error state and destructive feedback |
| `NexusBankColors.warning` | `#F59E0B` | Warning state |

**Typography**

- Use Google Fonts already configured in `pubspec.yaml`: Inter, Outfit, and Figtree.
- `displayLarge`: Outfit 36px, weight 800.
- `headlineLarge`: Inter 36px, weight 800.
- `titleLarge`: Figtree 22px, weight 700.
- `titleMedium`: Outfit 20px, weight 800.
- `titleSmall`: Inter 18px, weight 500.
- `bodyLarge`: Figtree 16px, weight 700.
- `bodyMedium`: Inter 14px, weight 400.
- `labelLarge`: Outfit 14px, weight 600.
- `caption`: Outfit 15px, weight 600.
- `micro`: Figtree 11px, weight 700.
- Do not introduce default system typography for branded or user-facing controls when a shared text style exists.

**Spacing, shape, and Material defaults**

- Spacing tokens: `xs=4`, `sm=8`, `md=16`, `lg=24`, `xl=32`, `xxl=48` pixels.
- Radius tokens: `sm=8`, `md=12`, `lg=16`, `xl=24`; use the shared radius token rather than ad-hoc rounding.
- Material 3 is enabled globally.
- Scaffold background is `#F4F6FA`; surfaces and form controls default to white.
- Elevated buttons use the accent blue, white foreground, minimum height 52px, 12px radius, and shared `labelLarge` typography.
- Inputs use white fill, 12px radius, horizontal padding 16px, and shared body hint styling. Feature screens may add a design-approved border/focus treatment but must preserve the token palette.
- App bars use the light background, zero elevation, primary-colored icons, and shared title typography.

#### Runtime Architecture And Integration Rules

- `main.dart` initializes `LocaleService`, provides it with Provider, and creates `MaterialApp.router` using `NexusBankTheme.light` and `appRouter`.
- `LocaleService` supports English and Thai, loads `assets/i18n/{code}.json`, persists the selected locale, and exposes `context.tr(key)` for build-time reactive rendering. In event handlers, validators, async callbacks, and service error paths, use a non-listening lookup or a pre-resolved string; never call a listening provider lookup outside build.
- `ApiClient.instance.dio` is the singleton Dio boundary for hand-written services. It applies the stored Bearer token, standard JSON headers, timeouts, and clears the in-memory token after HTTP 401 responses.
- `NexusBankApiClient` wraps the generated OpenAPI client for typed API calls, applies Bearer auth, and unwraps successful `{ success, data }` envelopes. Use generated APIs where available; keep feature adapters in `services/`.
- API base URL is `http://localhost:3000` by default and is overridden with `--dart-define=API_BASE_URL=...`; the versioned URL is `/api/v1`.
- GoRouter is centralized in `lib/core/router/app_router.dart`. Current routes include `/`, `/login`, `/mfa`, `/mfa/fingerprint`, `/mfa/mpin`, `/accounts/:id`, `/profile`, `/profile/edit`, `/language`, `/beneficiaries`, and `/beneficiaries/add`.
- Route parameters and navigation extras must be typed or validated. Back navigation and async route results must be deterministic; do not put secrets or sensitive account data in route extras.

#### Required Guidance For SDD Agents

- **Technical Specifier:** use this section to identify reusable frontend components, existing tokens, folder ownership, route conventions, localization behavior, and service/client boundaries. Document any approved deviation from these conventions in `spec.md`.
- **Implementation Planner:** derive frontend tasks against the folder map and layer boundaries above. Include design-token fidelity, route/state behavior, localization, service integration, and focused Flutter tests. Do not plan a second theme, ad-hoc API client, or frontend fixture for an API-backed flow.
- **Implementation Builder:** implement in the existing folders and reuse `NexusBankTheme`, `NexusBankColors`, `NexusBankTextStyles`, `NexusBankSpacing`, `NexusBankRadius`, `appRouter`, `LocaleService`, and approved API client boundaries. Keep widgets presentational, keep async/domain behavior in services or providers, and validate the changed frontend slice with `flutter test` and diagnostics.
- **All frontend stages:** treat this repository context plus the approved `spec.md` and `plan.md` as authoritative. Preserve existing design language and architecture unless a requirement explicitly requires a documented change.

### Node.js Backend

- `backend/src/app.js` owns Express middleware and route registration; `src/index.js` owns process startup and listening.
- `src/routes/` defines thin endpoint wiring, `src/controllers/` translates requests and responses, and `src/services/` owns business rules and data changes.
- `src/middleware/` owns cross-cutting 404 and error handling. The global error handler must remain last in the middleware chain.
- Keep API routes versioned under `/api/v1/` and preserve the established `{ success, data }` and `{ success: false, ... }` response shapes unless an approved contract changes them.
- Keep Swagger/OpenAPI synchronized with endpoint behavior, parameters, response models, authentication, and error statuses.
- Validate and normalize request input at the API boundary. Services must enforce authorization, customer ownership, eligibility, limits, and financial rules independently of the client.

## Frontend Coding Best Practices

- Use sound null safety, explicit Dart types, typed model boundaries, and small focused widgets. Avoid `dynamic`, unchecked casts, and map-shaped domain state when a model is practical.
- Keep asynchronous operations in services or state providers. Expose predictable initial, loading, loaded, empty, validation failure, unauthorized, business failure, and technical failure states.
- Use Provider state, `FutureBuilder`, `StreamBuilder`, or the repository's established equivalent consistently; do not mix competing state ownership patterns within one feature.
- Use GoRouter with typed or validated path/query parameters. Handle deep links, back navigation, unauthorized redirects, and missing navigation payloads deterministically.
- Use Dio timeouts, centralized interceptors, cancellation where appropriate, and safe mapping of transport errors to user-facing domain errors.
- Store authentication secrets in secure storage. Do not put tokens, credentials, or sensitive account data in logs, query strings, analytics, shared preferences, screenshots, or error messages.
- Keep forms explicit and testable: trim and validate input, preserve numeric precision, prevent duplicate submissions, guard pending actions, and show field-level errors safely.
- Build accessible Flutter UI with semantic labels, focus support, sufficient contrast, scalable text, predictable traversal order, and touch targets suitable for mobile and web.
- Localize all user-facing text, dates, numbers, currencies, and errors through the existing localization approach.
- Dispose controllers, focus nodes, streams, timers, and subscriptions owned by widgets or services. Avoid work after widget disposal.
- Keep generated OpenAPI code isolated from hand-written adapters so regeneration does not overwrite domain behavior.

## Backend Coding Best Practices

- Use the repository's CommonJS style and Node.js `>=18.0.0` runtime. Keep route handlers small and delegate business behavior to services.
- Validate body, path, and query inputs with `express-validator` or the established equivalent before invoking business logic. Reject malformed, missing, duplicate, out-of-range, and inconsistent values explicitly.
- Enforce authentication, authorization, account ownership, role/entitlement checks, eligibility, and approval rules server-side for every protected operation.
- Use allowlists for enum-like values, normalize identifiers consistently, and avoid mass assignment from raw request bodies.
- Use async error propagation consistently with `next(err)` or an established async wrapper. Prevent unhandled promise rejections and do not return success before required state changes complete.
- Centralize error mapping in `errorHandler`. Return stable status codes and safe messages; never expose stack traces, internal paths, secrets, driver details, or sensitive records in production.
- Configure Helmet, CORS, body parsing, logging, timeouts, and environment behavior at the application boundary. Restrict CORS origins and avoid permissive production defaults.
- Keep secrets in environment or approved secret management. Validate required configuration at startup and never commit tokens, passwords, or private keys.
- Use structured, privacy-aware logs with correlation/reference IDs, event outcome, and timing. Redact authorization headers, credentials, PII, account numbers, and full request/response bodies.
- For financial mutations, validate all preconditions before mutation, use idempotency/reference controls, update related state consistently, and roll back or compensate on partial failure.
- Keep mock data isolated from production code paths and make test state resettable. Do not depend on test execution order or live external systems.
- Synchronize Swagger/OpenAPI after endpoint changes, including request validation, authentication, response envelopes, error statuses, and synthetic examples.

## Cross-Layer API And Integration Standards

- Treat OpenAPI, generated Dart models, backend response envelopes, route paths, status codes, and error contracts as versioned integration boundaries.
- Document authentication requirements, ownership rules, idempotency behavior, timeout/retry expectations, pagination/filter semantics, and error mapping for every new integration.
- Retries are safe only for operations designed to be idempotent. Never blindly retry a financial mutation without a stable idempotency/reference key.
- Keep transport concerns in API clients/services and domain concerns in models/services. Do not make UI widgets or route definitions responsible for API protocol details.
- Use synthetic fixtures and contract tests to detect drift between backend OpenAPI and Flutter integration code.

## Security, Privacy, And Compliance

- Never include real customer PII, credentials, tokens, account numbers, card numbers, portfolio holdings, or production secrets in source, tests, artifacts, commits, prompts, or documentation.
- Minimize data collection and retention. Mask sensitive values in UI, logs, exports, telemetry, and error reports according to the approved classification.
- Apply PDPA privacy and consent requirements, KYC/AML controls, suitability decisions, maker-checker approvals, audit trails, and regulatory evidence only when in scope, but do not omit them when requirements make them mandatory.
- Treat client-side checks as usability safeguards. Backend authorization and ownership validation are authoritative.
- Record security-relevant failures and sensitive actions with enough audit context to investigate without logging secret or unnecessary customer data.

## Testing And Validation

- Use deterministic fixtures, synthetic data, isolated mutable state, and fixed clocks/randomness where time or IDs affect assertions.
- Frontend tests should cover rendering, navigation, localization, validation, loading/empty/success/failure states, duplicate-submit prevention, and authorization-sensitive UI behavior as applicable.
- Backend tests should cover success, malformed input, authentication/authorization, cross-customer access, business-rule failures, idempotency, correct status codes, safe error envelopes, and atomicity for financial mutations.
- Run focused tests for the changed slice before broader validation: `flutter test` for frontend changes and `npm test` from `backend/` for backend changes.
- Keep OpenAPI, implementation, tests, requirements, and integration documentation synchronized.

## SDD Agent Workflow

- Context, requirements, specifications, plans, implementation, testing, review, documentation, and traceability are separate evidence-producing stages.
- Agents must read this context plus the applicable Flutter, Node.js, test, security, architectural, environment, versioning, and traceability instructions before acting.
- No agent should silently resolve a contradiction between requirements, design, API contract, and implementation. Record the conflict and request clarification or approval.
- Completion claims must identify changed artifacts, validation performed, unresolved risks, and traceability evidence.

## Definition Of Done

A change is ready for review only when its approved scope is implemented, relevant states and failure paths are covered, security and privacy controls are checked, tests pass for the changed slice, API/design documentation is synchronized, and requirement-to-code-to-test traceability is recorded.