# Versioning Policy

This document describes the semantic versioning policy and release strategy for the **Nexulyt-AI-OS** repository.

---

## 1. Semantic Versioning (SemVer)

All repository releases and skill configurations follow Semantic Versioning rules:

```text
MAJOR.MINOR.PATCH
```

- **`MAJOR`**: Significant architectural updates, breaking API changes, or complete rework of core workflows (e.g. changes to `full-stack-orchestrator` execution loops).
- **`MINOR`**: Additions of new skills, guidelines, templates, or helper scripts that do not break backward compatibility.
- **`PATCH`**: Bug fixes, typo corrections, link verification updates, and documentation additions.

---

## 2. Release Strategy

- **Development:** Active work is performed on local feature branches (`feat/*`, `fix/*`) and merged into the `main` branch.
- **Pre-Release:** Release candidate branches (`rc-*`) are created for testing and validation before production deployment.
- **Production Tagging:** Releases are tagged on the `main` branch (e.g. `v1.0.0`) and documented in the changelog.

---

## 3. Handling Breaking Changes

A change is classified as **breaking** if:
- It alters the mandatory files contract inside the `skills/` folders.
- It changes the sequence parameters required by the `full-stack-orchestrator`.
- It deletes or renames existing configuration targets.

When proposing breaking changes:
1. Open an Architecture Decision Record (ADR) detailing the rationale.
2. Ensure the change is scheduled during a minor/major version shift.
3. Provide automated migration scripts where possible.
