# Code Reviewer Production Validation Checklist

A comprehensive pre-merge quality gate checklist for validating code changes across requirement alignment, architecture, implementation, security, performance, accessibility, testing, documentation, and production readiness.

---

## 1. Requirement Review
- [ ] The PR description clearly states the business problem being solved and the expected outcome.
- [ ] The change is linked to a specific ticket, issue, or approved design document.
- [ ] The scope of the change matches what the linked ticket describes—no undisclosed scope creep.
- [ ] Edge cases and failure scenarios identified in the ticket are addressed in the implementation.
- [ ] Breaking changes are explicitly declared in the PR description with a documented migration path.
- [ ] The PR is appropriately sized—no single review exceeds 400 lines of changed logic without justification.

---

## 2. Planning
- [ ] The approach chosen is the simplest solution that fulfills the stated requirement.
- [ ] No premature abstractions have been introduced for requirements that do not yet exist (YAGNI).
- [ ] Dependencies on other in-progress PRs or unreleased services are documented and tracked.
- [ ] Rollback strategy is defined for changes that modify shared infrastructure, schemas, or auth flows.
- [ ] Database migrations are flagged for DBA review if they modify tables with more than 500,000 rows.
- [ ] Platform-wide changes (shared libraries, middleware, auth) are planned for incremental rollout, not simultaneous deployment.

---

## 3. Design Review
- [ ] The data model accurately represents the domain entities and their relationships.
- [ ] API contracts (request/response shapes, HTTP methods, status codes) are consistent with existing platform conventions.
- [ ] New public interfaces are designed to be stable—breaking changes will require a major version bump.
- [ ] Complex or non-obvious design decisions are documented in code comments or an ADR (Architecture Decision Record).
- [ ] UI/UX changes have been reviewed against the approved design mockup or specification.
- [ ] Error response structures follow the established platform envelope format (`code`, `message`, optional `details`).

---

## 4. Architecture Review
- [ ] The change respects established layer boundaries (e.g., Controller → Service → Repository). No boundary violations.
- [ ] No circular dependencies are introduced between modules, packages, or services.
- [ ] New services and modules follow the project's established dependency injection patterns.
- [ ] Business logic is not embedded in HTTP route handlers, database models, or UI components.
- [ ] Infrastructure concerns (database clients, cache clients, HTTP agents) are not imported directly into business logic layers.
- [ ] New abstractions are introduced only where they resolve existing complexity—not speculatively.
- [ ] Microservice changes do not introduce synchronous inter-service coupling where async event patterns are established.

---

## 5. Implementation Review
- [ ] All code paths produce the correct output for the defined happy path, error cases, and edge cases.
- [ ] No logic errors exist in conditional branches, loop boundaries, or state transitions.
- [ ] All async operations use `await` or `.then()`/`.catch()` correctly; unhandled promise rejections are absent.
- [ ] No empty `catch` blocks exist; all caught errors are logged and either re-thrown or handled explicitly.
- [ ] All functions have a single, well-defined responsibility and are under 30 lines in length.
- [ ] Magic numbers and magic strings are replaced with named constants defined at the module level.
- [ ] No `TODO` or `FIXME` comments are left in code that is intended for production merge.
- [ ] Nested conditional depth does not exceed 3 levels; complex conditions are extracted into named predicate functions.
- [ ] No `console.log` debug statements are present in production code paths.
- [ ] Return types of all public functions are explicitly typed in TypeScript codebases; `any` is absent.

---

## 6. Code Quality
- [ ] All identifiers (variables, functions, classes, files) use names that communicate intent without abbreviation.
- [ ] Boolean variables and functions use the `is`, `has`, `can`, or `should` prefix convention.
- [ ] Files, modules, and classes follow the `Single Responsibility Principle`—each has one reason to change.
- [ ] Duplicated logic blocks appearing more than twice are extracted into shared utility functions.
- [ ] No hardcoded environment-specific values (base URLs, feature flags, connection strings) exist in source files.
- [ ] All `switch` statements handling enum values include a `default` case.
- [ ] TypeScript strict mode is enabled; no `ts-ignore` or `ts-nocheck` suppressions are introduced without documented justification.
- [ ] Linting and formatting rules pass without suppression comments.
- [ ] Nested ternary expressions are not used; readability is maintained with explicit `if/else` or early returns.
- [ ] No dead code (unreachable branches, unused imports, commented-out blocks) is present in the diff.

---

## 7. Security Review
- [ ] All user-controlled input is validated against a strict schema (Zod, Joi, Marshmallow, or equivalent) at the request boundary.
- [ ] All database queries use parameterized statements or ORM query builders; no string concatenation is used to construct queries.
- [ ] Authorization is verified on every route and service method that accesses or modifies protected resources.
- [ ] Resource ownership is confirmed server-side using the authenticated token context—not a client-supplied parameter.
- [ ] No secrets, API keys, credentials, or tokens are hardcoded in source code or committed to version control.
- [ ] Secrets are loaded exclusively from environment variables or a secrets manager at runtime.
- [ ] Secrets are not logged, printed, or included in error response bodies.
- [ ] Outbound HTTP calls to external services enforce HTTPS; plain HTTP connections are not permitted.
- [ ] File upload endpoints validate file type using magic-number header inspection—not file extension or MIME type alone.
- [ ] LLM prompt endpoints place user input in a `user` role message only; user content is never concatenated into the `system` role prompt.
- [ ] Webhook endpoints verify payload signatures using the provider's HMAC signature verification function.
- [ ] Session cookies carry `HttpOnly`, `Secure`, and `SameSite=Strict` attributes.
- [ ] CORS origin policies specify explicit allowed domains; wildcard (`*`) is absent on authenticated routes.
- [ ] Rate limiting is enforced on all public-facing and authentication endpoints.

---

## 8. Performance Review
- [ ] No database queries execute inside loops; all list-based lookups use batch or `IN (...)` queries.
- [ ] All list-returning endpoints enforce cursor-based or offset pagination with a maximum page size.
- [ ] New database queries are verified to use index scans via `EXPLAIN ANALYZE`; full sequential scans on large tables are flagged.
- [ ] Expensive computations are not recalculated on every request; results are memoized or cached where appropriate.
- [ ] Redis TTL is set on all cache keys; no keys are inserted with infinite lifetime.
- [ ] No synchronous, blocking operations (file I/O, CPU-heavy loops) execute on the Node.js main thread.
- [ ] CPU-intensive operations are offloaded to worker threads or background queues.
- [ ] API response payloads are scoped to the minimum required fields; `SELECT *` is absent from all queries.
- [ ] Large data exports and file downloads stream responses instead of buffering the entire payload in memory.
- [ ] GraphQL resolvers use DataLoader to batch and deduplicate database calls; N+1 patterns are absent.
- [ ] Lighthouse performance score remains at or above 90 for frontend page changes.
- [ ] Bundle size budget is not exceeded; new JavaScript dependencies are evaluated for size impact.

---

## 9. Accessibility Review
- [ ] All interactive elements (`<button>`, `<a>`, `<input>`, `<select>`) have a visible label or a descriptive `aria-label` attribute.
- [ ] Icon-only buttons include `aria-label` text and mark the icon with `aria-hidden="true"`.
- [ ] Heading hierarchy is correct: exactly one `<h1>` per page, with `<h2>` and `<h3>` used in documented order.
- [ ] Color is not the sole means of conveying information (e.g., error states also use an icon or text label).
- [ ] All images have descriptive `alt` text; purely decorative images use `alt=""`.
- [ ] Interactive components are keyboard-navigable using Tab, Enter, and Arrow keys where applicable.
- [ ] Focus is visible on all interactive elements; `outline: none` is not applied without a custom visible focus indicator replacement.
- [ ] Dynamic content updates (modals, toasts, alerts) use ARIA live regions or `role="alert"` to notify screen reader users.
- [ ] Form fields are associated with their labels using `for`/`id` pairing or `aria-labelledby`.
- [ ] All CSS animations are gated by `@media (prefers-reduced-motion: no-preference)`.

---

## 10. Testing
- [ ] All new business logic functions have unit tests covering the happy path, at least two edge cases, and the primary error case.
- [ ] Bug fixes include a regression test that fails before the fix and passes after it.
- [ ] New API endpoints have integration tests covering: `200` success, `400` invalid input, `401`/`403` unauthorized, and `404` not found.
- [ ] Tests do not mock the function under test itself; they exercise real behavior with controlled dependencies.
- [ ] Test assertions verify outputs and side effects—not internal implementation details.
- [ ] Database migrations include a verified `down` migration that runs cleanly without data loss.
- [ ] Tests do not share mutable state between test cases; each test is isolated and independently runnable.
- [ ] The full test suite passes locally and in CI without flaky tests or skipped coverage.
- [ ] Code coverage for modified modules meets or exceeds the project's defined threshold.
- [ ] Performance-critical paths have benchmark or load test coverage confirming they meet the defined latency budget.

---

## 11. Documentation
- [ ] All public functions exported from a module have JSDoc or equivalent documentation comments.
- [ ] JSDoc comments document: parameters (type and description), return value, and any exceptions thrown.
- [ ] Non-obvious algorithmic or business logic decisions have inline comments explaining *why*—not *what*.
- [ ] README files are updated if public interfaces, configuration options, environment variables, or setup steps changed.
- [ ] OpenAPI / Swagger specifications are updated for any new or modified API endpoints.
- [ ] Database schema migration files include a comment describing the business reason for the schema change.
- [ ] Environment variable additions are documented in `.env.example` and the deployment configuration guide.
- [ ] Changelog or release notes are updated if the change introduces a user-visible feature or breaking change.

---

## 12. Production Readiness
- [ ] New environment variables required by this change are provisioned in all target deployment environments before merge.
- [ ] Database migrations are confirmed to run safely on the production dataset size (validated in staging).
- [ ] `NOT NULL` column additions to large tables include a `DEFAULT` value or are executed via a multi-step zero-downtime migration.
- [ ] New indexes are created with `CONCURRENTLY` to prevent table-level write locks during deployment.
- [ ] Graceful shutdown is implemented: the application drains in-flight requests before terminating on `SIGTERM`.
- [ ] Health check endpoints (`/health/live`, `/health/ready`) return correct status codes for all new services.
- [ ] Kubernetes resource `requests` and `limits` are defined for all new container workloads.
- [ ] Structured logging is implemented for all new code paths; logs include trace/span IDs for distributed tracing.
- [ ] Alerting rules exist for new critical code paths (e.g., payment failures, auth errors, queue backlogs).
- [ ] Feature flags are in place for high-risk features, enabling rollback without a code deployment.
- [ ] The change has been validated in a staging environment that mirrors production infrastructure.
- [ ] Rollback plan is documented and tested: previous version can be restored within 5 minutes if a regression is detected.

---

## 13. Final Review
- [ ] All `[CRITICAL]` and `[MAJOR]` findings from the review cycle are resolved and verified.
- [ ] All `[MINOR]` findings are either resolved or accepted with documented follow-up tickets.
- [ ] The engineering checklist in sections 1–12 has been completed in full.
- [ ] The PR has received the minimum required approvals per the team's review policy.
- [ ] CI pipeline is fully green: lint, type check, tests, security scan, and build all pass.
- [ ] No merge conflicts exist against the target branch.
- [ ] The PR author has self-reviewed the final diff and confirmed no unintended changes are included.
- [ ] Post-merge monitoring plan is confirmed: the team will observe dashboards and error rates for 30 minutes after deployment.
