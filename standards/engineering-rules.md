# Engineering Rules

This document outlines the core engineering guardrails and rules that must be followed by all development agents and human contributors within the **Nexulyt-AI-OS** workspace.

---

## Rule 1: Plan Before Execution
- No code modifications, additions, or deletions are permitted until an implementation plan has been submitted and approved.
- The plan must detail changes, potential risks, and testing steps.

---

## Rule 2: Reversibility (Tested Rollbacks)
- Every deployment manifest, migration script, and rollout strategy must include a documented and tested rollback procedure.
- If a rollout fails, the system must be capable of returning to a stable state in less than 5 minutes.

---

## Rule 3: Zero-Trust Secrets Management
- Secrets, credentials, private keys, and API tokens must never be written in source code, committed to version control, or logged.
- Utilize a key vault (Vault, AWS Secrets Manager, Azure Key Vault) for runtime configuration injection.

---

## Rule 4: Pinned Dependencies
- All runtime platforms, package managers, and container setups must specify exact, pinned dependency versions.
- Floating tags (e.g., `latest` or `*`) are strictly prohibited in production targets.

---

## Rule 5: Parameterized Inputs (SQLi Prevention)
- All database queries, system executions, and shell operations must utilize parameterized interfaces.
- String concatenation is forbidden on any database query or dynamic evaluation path.

---

## Rule 6: Structured Logging & Observability
- All logs must be written in structured JSON formats with `traceId`, `level`, `service`, and `timestamp` variables.
- Every API endpoint must expose separate `/health/live` and `/health/ready` check paths.
