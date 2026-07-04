# Deployment Checklist

This document provides a quality gate checklist to validate Docker container builds, Kubernetes cluster manifests, CI/CD workflows, rollback protocols, and telemetry dashboard parameters prior to production launches, in alignment with [Architecture Standards](file:///d:/projects/Nexulyt-AI-OS/standards/architecture-standards.md).

---

## 1. Overview

* **Objective**: Ensure container builds are secure, cluster manifests are hardened, CI/CD checks pass, rollback steps are defined, and production telemetry is active.
* **When to Use**: During deployment planning, CI/CD configuration edits, or final production release verification phases.
* **Prerequisites**: Containerized application code, cluster credentials, environment configurations, and target deployment guidelines.

---

## 2. Checklist Items

### Docker Container Hardening
* [ ] **Multi-Stage Builds**: Verify Dockerfile uses multi-stage builds to isolate compile-time packages from runtime containers.
* [ ] **Minimal Base Image**: Use secure, lightweight base images (e.g., Alpine, distroless).
* [ ] **Non-Root User Configuration**: Ensure containers run with non-root privileges (`USER node` or similar).
* [ ] **`.dockerignore` Presence**: Verify `.dockerignore` file exists and filters out source control (git), local config files, and dev packages.

### Kubernetes Manifest Configurations
* [ ] **Resource Limits Enforced**: Ensure all container pods declare explicit CPU and Memory requests and limits.
* [ ] **Liveness & Readiness Probes**: Verify HTTP probes (`/healthz`, `/readyz`) are configured with appropriate timeouts and thresholds.
* [ ] **SecurityContext Enforcement**: Restrict container access (read-only root filesystem, drop Linux capabilities).
* [ ] **Horizontal Pod Autoscaling (HPA)**: Configure HPA rules to scale pods automatically when CPU/Memory usage exceeds 70%.

### CI/CD Pipeline Gates
* [ ] **Automated lint & Test pass**: Verify that pipeline logs confirm all code lints, type checks, and test suites pass.
* [ ] **Immutable Image Tagging**: Tag Docker images with unique commit hashes rather than using the mutable `latest` tag.
* [ ] **Secret Isolation**: Verify that CI/CD environments pull secrets dynamically from key vaults rather than plain text.

### Telemetry, Rollback & Backups
* [ ] **Alerting Thresholds**: Configure Prometheus alert rules to fire on HTTP 5xx spikes or high resource usage.
* [ ] **Rollback Manual Verification**: Verify that the roll-back script or command is documented and dry-run tested.
* [ ] **Database Backup Confirm**: Confirm that automated daily backups and PITR configurations are active before deployment starts.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Scan Docker images locally for security issues, verifying zero high-priority alerts.
* Test Kubernetes manifests locally using dry-run commands: `kubectl apply --dry-run=client -f manifest.yaml`.
* Confirm that health check paths return 200 OK responses on the target container.

### Exit Criteria
* **No Lint Violations**: Build pipelines compile successfully.
* **Probes Healthy**: Liveness and readiness endpoints return valid status metrics.
* **Rollback Plan Verified**: Emergency rollback steps documented.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Deploying containers without resource limits, allowing a single compromised container to exhaust cluster node memory.
* Tagging production container builds with the `latest` tag, making rollbacks difficult and trackless.
* Omitting readiness probes, allowing traffic to route to starting containers before they are initialized.

### Professional Recommendations
1. **Enforce Read-Only Filesystems**: Configure container runtimes to use read-only root filesystems, mitigating local code modification attacks.
2. **Automate Ingress Auditing**: Use automated linters (e.g., Kube-Linter, Conftest) to scan Kubernetes manifests for security configuration issues.
3. **Dry-Run Rollbacks**: Test rollback triggers regularly in staging environments to verify database migration rollbacks function correctly.
