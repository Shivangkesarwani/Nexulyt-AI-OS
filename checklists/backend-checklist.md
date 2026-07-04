# Backend Quality Checklist

This document provides a quality gate checklist to validate backend controllers, route handlers, payload validation schemas, auth middlewares, structured logging standards, and global exception captures in compliance with [API Design Standards](file:///d:/projects/Nexulyt-AI-OS/standards/api-design-standards.md) and [Coding Standards](file:///d:/projects/Nexulyt-AI-OS/standards/coding-standards.md).

---

## 1. Overview

* **Objective**: Ensure backend APIs are secure, stateless, validated, trace-friendly, and return standardized error payloads.
* **When to Use**: During endpoint engineering phases, pre-merge reviews, or when auditing API configurations.
* **Prerequisites**: Target API design contracts (OpenAPI specifications), validation schema selections, and security guidelines.

---

## 2. Checklist Items

### Route Handlers & Controllers
* [ ] **Stateless Execution**: Verify that all API endpoints are stateless, storing user session states in external caches or tokens.
* [ ] **Idempotent Write Handlers**: Enforce unique transaction keys or idempotency token headers on write operations (POST, PUT).
* [ ] **HTTP Status Compliance**: Ensure route responses return accurate status codes (e.g., 201 Created for inserts, 400 for validation errors, 409 for conflicts).

### Payload Schema Validation
* [ ] **Request Schema Check**: Validate that all query, body, and path parameters pass through schemas (e.g., Zod, Pydantic) at the API boundary.
* [ ] **Sanitization Pipeline**: Strip script tags, trim text whitespaces, and normalize email addresses before querying database rows.
* [ ] **Type Mapping**: Enforce TypeScript type extraction from validation schemas to maintain compile-time type safety.

### Auth & Authorization Scopes
* [ ] **Auth Token Validation**: Middleware validates token signatures, expiration claims, and issuer profiles on secure routes.
* [ ] **Scope Enforcements**: Verify role-based permissions (RBAC) or attribute constraints (ABAC) on every data-access endpoint.
* [ ] **Tenant Parameter Binding**: Securely bind the active tenant identifier context from auth tokens directly to database transaction blocks.

### Structured Logging & Diagnostics
* [ ] **JSON Log Layout**: Output logs in structured JSON format to enable ingestion by search engines.
* [ ] **Trace Correlation IDs**: Map a unique `correlationId` or `requestId` to every incoming request context, printing it in all downstream log outputs.
* [ ] **PII Scrubbing**: Strip passwords, credit card numbers, and API secrets from logs before they hit console prints.

### Exception Interceptions (Error Handling)
* [ ] **Global Error Capture**: Configure interceptor middlewares to catch unhandled runtime errors, returning safe 500 status messages.
* [ ] **RFC 7807 problem details**: Format error responses using standard RFC 7807 problem schemas: `{ "type": string, "title": string, "status": integer, "detail": string, "instance": string }`.
* [ ] **Trace ID Exposure**: Include target request trace identifiers in error payloads to help developers locate issues in logs.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Confirm that validation middleware rejects malformed request body payloads with 400 Bad Request statuses.
* Verify that unauthorized requests receive 401 Unauthorized or 403 Forbidden responses.
* Inspect console logs during errors to check for correct JSON formats and correlation IDs.

### Exit Criteria
* **Validation Coverage**: All secure route handlers configure validation middleware.
* **RFC 7807 Compliant Errors**: Error structures match RFC specifications.
* **Test Suite Verification**: Integration test suites pass with zero failures.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Relying on client-side constraints for validation while leaving backend APIs vulnerable to malformed payloads.
* Printing database credentials, JWT verification keys, or stack traces in production error outputs.
* Running long database operations inside main execution threads without connection timeout limits.

### Professional Recommendations
1. **Enforce Schema Validation by Default**: Build middleware wrappers that apply validation rules automatically on all controllers.
2. **Propagate Correlation IDs**: Ensure all HTTP headers and internal microservice events propagate correlation tokens.
3. **Use Transaction Boundaries**: Run multi-step database writes within transaction blocks, rolling back on error exceptions.
