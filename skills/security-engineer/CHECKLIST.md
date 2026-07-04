# Security Engineer Production Validation Checklist

A comprehensive quality assurance validation checklist to audit systems and codebases, verifying compliance with industry security specifications (OWASP, ASVS) before release.

---

## 1. Authentication
- [ ] **Secure Hashing:** User passwords are encrypted using secure algorithms (Argon2id or bcrypt) with custom salt configurations.
- [ ] **Multi-Factor Auth (MFA):** Multi-factor authentication is enforced for all administrative profiles and user accounts.
- [ ] **Brute-Force Protection:** Rate limit rules block IP addresses after 5 unsuccessful login attempts.
- [ ] **Safe Errors:** Login fail messages return generic outputs (e.g., "Invalid credentials") to prevent user enumeration attacks.
- [ ] **Password Strength:** Password schemas require a minimum length (e.g., 12 characters) and complexity (letters, numbers, symbols).

---

## 2. Authorization
- [ ] **Deny by Default:** Access policies block resource endpoints by default; permissions must be granted explicitly.
- [ ] **RBAC/ABAC Enforcement:** Roles are validated on every request at the controller layer; client-side flags are not trusted.
- [ ] **IDOR Prevention:** Access ownership is validated when querying databases using resource IDs (e.g., verify `userId` matches database owner fields).
- [ ] **Privilege Escalation Block:** User registration and update schemas do not permit parameter modifications to key administrative fields (e.g., `is_admin`, `roles`).

---

## 3. Input Validation
- [ ] **Schema Definition:** Request payloads are checked against strict schema types (using Zod, Joi, or Marshmallow) at the boundary layer.
- [ ] **Sanitization Allowlist:** Inputs are checked using character allowlists; potentially dangerous input formats are rejected.
- [ ] **Output Encoding:** Variable values are escaped and encoded contextually (HTML, CSS, JS, URL) before rendering in client views.
- [ ] **SQLi Defenses:** Dynamic database queries use parameterized parameters or ORM frameworks exclusively.

---

## 4. API Security
- [ ] **CORS Isolation:** The `Access-Control-Allow-Origin` header specifies explicit domain ranges; wildcard headers (`*`) are blocked on authenticated routes.
- [ ] **Anti-CSRF Tokens:** State-changing requests (POST, PUT, DELETE) validate cryptographically signed anti-CSRF headers.
- [ ] **Rate Limiting:** IP rate limits restrict connections to public endpoints (e.g., max 100 requests per 15 minutes).
- [ ] **BOLA/BFLA Defenses:** Object-level and function-level authorizations are verified on every request.

---

## 5. Encryption
- [ ] **At-Rest Protection:** Databases, logs, and storage directories use secure symmetric encryption algorithms (AES-256-GCM).
- [ ] **In-Transit Protection:** HTTP interfaces redirect to HTTPS using strict TLS 1.3 or TLS 1.2 specifications and strong cipher suites.
- [ ] **HSTS Headers:** Secure transport headers redirect browsers to HTTPS automatically (`Strict-Transport-Security`).
- [ ] **Key Vault Separation:** Cryptographic keys are stored in hardware security modules (HSM) or cloud key vaults (KMS), isolated from data payloads.

---

## 6. Secrets
- [ ] **No Hardcoded Keys:** Repository source files are audited to verify that no passwords, API keys, or certificates are checked in.
- [ ] **Dynamic Fetching:** Secrets are loaded in runtime memory from environment fields or secure key vaults.
- [ ] **Log Scrubbing:** Secrets are masked or scrubbed automatically in application logs, audit logs, and trace exports.
- [ ] **Secrets Rotation:** Key rotation parameters are configured with standard lifetimes (e.g., 90 days) on all API keys and databases.

---

## 7. Logging
- [ ] **Structured Logs:** Logs write in structured JSON formats with uniform severity levels (debug, info, warn, error).
- [ ] **Audit Logs:** Security events (login, logout, privilege modification, transaction failures) generate detailed audit entries.
- [ ] **Trace Context:** Every log entry propagates trace and span IDs to support microservice log grouping.
- [ ] **Personal Data Scrubbing:** Log configurations contain filters that redact PII, passwords, credit card numbers, and tokens.

---

## 8. Monitoring
- [ ] **SIEM Ingestion:** Application logs are forwarded to a central log server (e.g., Grafana Loki, Splunk) for real-time analysis.
- [ ] **Anomaly Detection:** Alerts trigger on atypical behavior patterns (e.g., multiple authentication failures from a single IP within 1 minute).
- [ ] **APM Observability:** Outbound network calls and internal database query times are tracked to monitor anomalies.
- [ ] **Alert Escalations:** Security alerts route to dedicated notification tools with active escalation rules.

---

## 9. Cloud Security
- [ ] **Least Privilege IAM:** Roles assign minimum necessary permissions; wildcard access permissions (`"Resource": "*"`) are restricted.
- [ ] **Network Segmentation:** Cloud workloads deploy inside private subnets; egress gateways route traffic securely.
- [ ] **Activity Trails:** Audit tracking systems (e.g., AWS CloudTrail) are active across all cloud accounts.
- [ ] **Cloud WAF Rules:** WAF groups filter traffic on ingress points, blocking common web exploit payloads.

---

## 10. Container Security
- [ ] **Rootless Container:** Docker files run processes under a non-root system user.
- [ ] **Read-Only System:** Container execution contexts block write operations to root paths (`readOnlyRootFilesystem: true`).
- [ ] **No Capability Escalation:** Pod specifications explicitly disable privilege escalation.
- [ ] **Vulnerability Scans:** Containers are audited using image vulnerability checks (e.g., Trivy) during pipeline builds.

---

## 11. Dependency Security
- [ ] **Version Pinning:** Library packages are pinned to exact versions in manifest files.
- [ ] **SCA Checks:** Build stages include Software Composition Analysis (SCA) tools to detect outdated, vulnerable packages.
- [ ] **SBOM Generation:** A Software Bill of Materials (SBOM) is exported automatically on production releases.
- [ ] **Integrity Checks:** Packages are verified using hash signature checks during build times.

---

## 12. AI Security
- [ ] **Input Guardrails:** AI prompts pass input filter checks to identify and block instruction manipulation attempts.
- [ ] **Context Isolation:** Retrieval-Augmented Generation (RAG) vector searches validate tenant permissions before context retrieval.
- [ ] **Execution Sandboxing:** AI agent code generation runs in isolated sandbox environments with resource boundaries.
- [ ] **Output Escaping:** AI outputs are escaped and validated before rendering in client views.

---

## 13. Incident Response
- [ ] **Response Plan:** A documented SEV-1 incident triage playbook is updated and accessible to on-call engineers.
- [ ] **Key Rotation Playbooks:** Runbooks outline steps to rotate key database access passwords and token signatures during incidents.
- [ ] **Isolation Commands:** Diagnostic scripts permit rapid isolation of suspect server nodes or containers.

---

## 14. Final Security Review
- [ ] **Checklist Compliance:** All checkpoints in sections 1-13 have been verified.
- [ ] **Threat Model Review:** Threat models have been reviewed to confirm mitigations are in place for all high-risk threats.
- [ ] **Penetration Test Sign-off:** High-exposure services have passed verification checks.
