# Security Checklist

This document provides a quality gate checklist to validate application security parameters, authentication setups, authorization boundaries, database cryptographies, API rate limiting, and dependency audits in compliance with the [Security Standards](file:///d:/projects/Nexulyt-AI-OS/standards/security-standards.md).

---

## 1. Overview

* **Objective**: Systematically eliminate application security vulnerabilities, prevent data leaks, secure identities, and isolate cloud environments.
* **When to Use**: During system architecture reviews, pre-merge pull request evaluations, or pre-production compliance audits.
* **Prerequisites**: System architecture topology models, database schema specs, external integration mappings, and target security policies.

---

## 2. Checklist Items

### Authentication & Authorization
* [ ] **Stateless JWT Validation**: Verify access tokens are signed using cryptographic algorithms (e.g., RS256/ES256) and feature strict expirations (< 1 hour).
* [ ] **Secure Token Storage**: Refresh tokens are stored in HttpOnly, Secure, SameSite cookies to mitigate XSS interception.
* [ ] **RBAC/ABAC Gatekeepers**: Enforce role checks or attribute verification rules on all write/read API routes (no unauthenticated paths permitted except public static files).
* [ ] **Tenant Isolation (IDOR Prevent)**: Ensure endpoints validate user relationship context with requested record IDs, preventing unauthorized edits of other users' datasets.

### OWASP Top 10 Protections
* [ ] **SQL Injection Defense**: Parameterize all database inputs, bypassing dynamic SQL text assembly.
* [ ] **XSS Sanitization**: Sanitize all client-supplied HTML strings using libraries like DOMPurify prior to DOM injection.
* [ ] **SSRF Isolation**: API calls requesting external URLs run via secure proxy nodes with whitelist address restrictions.
* [ ] **CORS Configuration**: Configure Cross-Origin Resource Sharing (CORS) rules to allow only explicitly authorized client domains.

### Secrets Management & Encryption
* [ ] **Centralized Secret Vault**: Secure environment variables (database keys, API credentials) inside hosted vaults (e.g. AWS Secrets Manager, Vault) rather than local `.env` files in source repositories.
* [ ] **Envelope Data Encryption**: Protected Health Information (PHI) or personal financial data is encrypted at rest using keys managed by Cloud KMS.
* [ ] **TLS Transit Encryption**: Enforce TLS 1.3 encryption on all public network paths.

### Dependency Auditing & API Gateways
* [ ] **Automated SAST Checks**: Run dependency audit tools (e.g. npm audit, Snyk) inside CI workflows to detect vulnerable library packages.
* [ ] **API Gateway Rate Limiting**: Configure sliding window rate limit parameters (e.g. max 100 requests per minute) on public endpoints.
* [ ] **Payload Size Restrictions**: Limit payload parse sizes in configuration parameters (e.g. max 2MB) to prevent buffer exhaustion attacks.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Run static security checks locally, verifying zero critical security warnings.
* Verify that route handlers return 403 Forbidden statuses when requesting records belonging to separate tenants.
* Confirm that database dumps display only encrypted byte arrays inside classified PHI fields.

### Exit Criteria
* **Vulnerability Scans Clean**: SAST audit reports return zero unresolved CVEs.
* **Security Headers Configured**: Inspect HTTPS response headers, verifying CSP, HSTS, and X-Frame-Options are configured.
* **Secrets Verification Log**: Code inspection confirms zero plaintext credentials exist inside repository files.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Uploading API private keys or service account credentials directly to public GitHub repositories.
* Trusting client-side validation rules for security checks without replicating validations on backend APIs.
* Using unmaintained, vulnerable third-party packages in package.json files.

### Professional Recommendations
1. **Parameterize Every Query**: Ensure database layers utilize ORM frameworks or parameterized criteria by default, eliminating injection vectors.
2. **Automate Dependency Scans**: Configure automated pull request blockers that prevent merges if new packages contain security vulnerabilities.
3. **Isolate Production Environments**: Keep database servers in private subnets, allowing access only from authenticated application nodes.
