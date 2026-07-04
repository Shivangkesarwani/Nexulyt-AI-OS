# Backend Engineer Review & QA Checklist

Use this checklist to perform a comprehensive code, security, and performance audit of backend services before staging commits.

---

## 1. REQUIREMENTS
- [ ] **Architectural Compliance:** The service layer and data model structure align perfectly with the boundaries defined by the Software Architect.
- [ ] **Functional Alignment:** Every API route implements the exact business workflows and calculations required.
- [ ] **System Dependencies:** All integrations (e.g., mail servers, third-party payment providers) match system designs.

---

## 2. API DESIGN
- [ ] **Route Conventions:** Endpoint paths represent resource collections using plural nouns (e.g., `/users`, `/invoices`).
- [ ] **HTTP Verbs:** Methods match standard actions (`GET` to read, `POST` to create, `PATCH` to update, `DELETE` to remove).
- [ ] **Response Status Codes:** Output responses return correct HTTP codes (e.g., `201 Created` on post success, `409 Conflict` on unique collisions).
- [ ] **Response Formats:** Payloads use structured JSON envelopes, and errors adhere to uniform error details (e.g., RFC 7807).
- [ ] **OpenAPI Spec:** Swagger schemas match active endpoints.

---

## 3. AUTHENTICATION
- [ ] **Password Hashing:** Passwords are hashed using argon2 or bcrypt with high work factors.
- [ ] **Token Expiration:** JWT access tokens are short-lived ($\le 15$ minutes) and use strong signature secrets (HS256/RS256).
- [ ] **Token Delivery:** Auth tokens are sent inside secure, httpOnly, SameSite=Strict, Encrypted cookies.
- [ ] **Refresh Token Rotation:** Refresh tokens are rotated on exchange and verified against a database blocklist.

---

## 4. AUTHORIZATION
- [ ] **Server Validation:** Access rules are validated on the server side; client user role parameters are ignored.
- [ ] **Deny Default:** All endpoints are protected by default, requiring explicit permissions to grant access.
- [ ] **RBAC/ABAC Check:** Role or attribute checks run on every secure controller before business logic executes.
- [ ] **Data Ownership:** Checks confirm the authenticated user owns the resource being updated or deleted.

---

## 5. VALIDATION
- [ ] **Strict Schemas:** All request bodies, query params, and parameters are validated using Zod at the route boundary.
- [ ] **Sanitization:** String inputs are trimmed and sanitized to prevent cross-site scripting (XSS) and script injections.
- [ ] **Strict Typing:** TypeScript type checking checks compile with no type bypass overrides.

---

## 6. DATABASE
- [ ] **Connection Pooling:** Connection pools are configured with appropriate limits to prevent exhaustion during load spikes.
- [ ] **Indexing Strategy:** Database indexes are set on columns used in search filters or join conditions (e.g., `email`, `userId`, `slug`).
- [ ] **Transaction Integrity:** Multiple related database writes run within transactions to prevent incomplete updates.
- [ ] **Migration Scripts:** Database updates are managed using versioned migration files checked into git.

---

## 7. SECURITY
- [ ] **Helmet Integration:** Helmet middleware is enabled to configure standard browser security headers.
- [ ] **CORS Rules:** Origins are restricted to explicit domain whitelists in production.
- [ ] **Rate Limiting:** Request rate limiting is configured for all public API routes, with strict limits on authentication paths.
- [ ] **Secrets Security:** Private API keys, database credentials, and secrets are loaded from environment variables and never committed to code.

---

## 8. LOGGING
- [ ] **Omit Secrets:** Logs never contain sensitive data (e.g., passwords, access tokens, bank details).
- [ ] **Structured Format:** Log outputs use structured JSON formatting to assist index search tools.
- [ ] **Correlation IDs:** Request logs include unique correlation IDs to trace operations across services.

---

## 9. MONITORING
- [ ] **Health Endpoint:** A `/healthz` route is active, checking database, cache, and queue connection states.
- [ ] **Resource Alerts:** Telemetry tools alert operators if CPU, memory, or connection usage spikes.
- [ ] **APM Tracing:** Critical execution paths export traces to monitoring tools using OpenTelemetry.

---

## 10. ERROR HANDLING
- [ ] **No Raw Crashes:** Global error boundary handlers catch exceptions and prevent server crashes.
- [ ] **Mask Diagnostics:** Raw database error logs, server file paths, and stack traces are masked from client responses.
- [ ] **Helpful Messages:** Validation failures return clear, specific field error messages to help users recover.

---

## 11. PERFORMANCE
- [ ] **Non-blocking Loop:** CPU-heavy calculations are moved to workers to keep the Node.js event loop free.
- [ ] **Cache Integration:** High-read, static datasets are cached in Redis with defined TTL expiration limits.
- [ ] **Pagination limits:** All database list queries enforce strict query limits to prevent memory bloat.
- [ ] **Compression:** HTTP responses use gzip/brotli compression middleware.

---

## 12. TESTING
- [ ] **Unit Tests:** Critical business logic services and helpers pass Vitest/Jest unit tests.
- [ ] **Integration Tests:** API endpoints pass Supertest integration tests against mock databases.
- [ ] **Test Coverage:** Code coverage remains at or above the repository's 80% limit.

---

## 13. DOCKER
- [ ] **Base Image:** Builds use official, lightweight alpine/slim Node.js base images.
- [ ] **Non-root Execution:** Containers run with a dedicated system user (e.g., `USER node`) instead of root access.
- [ ] **Multi-stage Builds:** Final runtime container images exclude dev dependencies and local testing assets.
- [ ] **Vulnerability Audit:** Automated scanners check container images for vulnerabilities.

---

## 14. DEPLOYMENT
- [ ] **Rolling Updates:** Deployment pipelines deploy updates gradually, preventing downtime.
- [ ] **Verify Staging:** Staging sites deploy to preview environments for visual and functional validation.
- [ ] **Automated Rollbacks:** Pipelines revert updates immediately if system health checks fail.

---

## 15. FINAL REVIEW
- [ ] **No Dead Code:** console logs, debuggers, and unused imports are removed from production branches.
- [ ] **Access Control:** API keys and environment variables are active on production hosts.
- [ ] **Error Scenarios:** Skeletons, spinners, empty views, error alerts, and success messages are verified.
