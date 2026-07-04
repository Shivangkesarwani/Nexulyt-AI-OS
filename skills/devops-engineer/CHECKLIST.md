# DevOps Engineer Production Validation Checklist

A comprehensive quality assurance audit checklist to verify that cloud-native infrastructure, pipelines, and application workloads satisfy production-grade security, scalability, and availability constraints before release.

---

## 1. Infrastructure
- [ ] **Infrastructure as Code:** All resources (VPCs, databases, load balancers, DNS records) are declared programmatically (e.g., using Terraform). No manual resources exist.
- [ ] **State Security:** Remote state backend is encrypted, versioned, and state-locking is enabled (e.g., S3 Bucket + DynamoDB).
- [ ] **Module Design:** Infrastructure code is split into small, version-pinned modules with strict inputs and output definitions.
- [ ] **VPC Isolation:** Network topology segregates public subnets (ingress/load balancers), private subnets (app servers), and isolated database subnets (no internet route).
- [ ] **Static Scans:** Code passes `tflint` and `checkov` checks with zero high-severity warnings.

---

## 2. Docker
- [ ] **Multi-Stage Builds:** Dockerfiles separate compilation environments from production runtime layers.
- [ ] **Minimal Base Images:** Production layers use lightweight, secure, and version-pinned base images (e.g., Alpine or Distroless).
- [ ] **Non-Root Execution:** A custom `USER` directive is explicitly declared; container processes do not run as root.
- [ ] **Caching Layer Optimization:** `COPY` package manifests (e.g., `package.json`, `go.mod`) and dependency installations run before copying the application code.
- [ ] **Dockerignore Exclusions:** `.dockerignore` file exists and explicitly excludes local secrets, build logs, and node dependencies.
- [ ] **Health Checks:** Native `HEALTHCHECK` parameters verify container health (e.g., using curl/wget to query local health endpoints).
- [ ] **Vulnerability Audit:** Base image passes `trivy` container CVE scanning with zero high or critical findings.

---

## 3. Kubernetes
- [ ] **Resource Budgets:** Every deployment specifies explicit CPU/Memory request and limit budgets.
- [ ] **Lifecycle Probes:** Liveness, readiness, and startup probes are configured with realistic timeouts and period parameters.
- [ ] **Security Context:** Pod specs configure `runAsNonRoot: true`, block privilege escalation, and set the root filesystem to read-only (`readOnlyRootFilesystem: true`).
- [ ] **High Availability Routing:** Anti-affinity rules distribute replicas across separate worker nodes and Availability Zones.
- [ ] **Pod Network Policy:** Traffic flow is restricted using native NetworkPolicies to prevent unauthorized pod-to-pod network routes.
- [ ] **ConfigMap Isolation:** Dynamic configuration settings are stored in ConfigMaps separate from the core container execution spec.

---

## 4. CI/CD
- [ ] **Build Separation:** Build artifacts are compiled once and shared across downstream pipeline environments.
- [ ] **Dependency Cache:** Pipeline cache is configured for package directories (e.g., `.npm`, `/go/pkg/mod`) to speed up execution.
- [ ] **Security Scans:** Pipeline includes automated source scanners (SonarQube) and image vulnerability checking steps.
- [ ] **Manual Gates:** Release promotions to production require explicit approval gates.
- [ ] **Automated Rollbacks:** Pipelines detect post-deployment test failures and trigger immediate rollbacks to the last stable release.

---

## 5. Secrets
- [ ] **No Git Secrets:** Repository is audited using `git-secrets` or `trufflehog` to confirm no keys are checked in.
- [ ] **Managed Key Vault:** Database passwords, private keys, and API tokens are hosted in managed key vaults (e.g., AWS Secrets Manager, HashiCorp Vault).
- [ ] **Dynamic Injection:** Secrets are loaded in memory at startup or injected as encrypted volumes; they are never printed in logs or environment dumps.
- [ ] **Automatic Rotation:** Secret rotation policies are configured with standard lifetimes (e.g., every 30 or 90 days).

---

## 6. Monitoring
- [ ] **Golden Signals Tracking:** Latency, traffic rate, HTTP error rates, and resource saturation are recorded at the edge.
- [ ] **APM Integration:** Distributed tracing SDKs (OpenTelemetry) propagate headers across all service hops.
- [ ] **Scraping Config:** Prometheus collects metrics from applications at standard intervals (e.g., 15s).
- [ ] **Health Endpoints:** Application exposes dedicated `/health/live` and `/health/ready` endpoints.

---

## 7. Logging
- [ ] **Structured JSON:** Applications write logs in structured JSON format with standard schemas.
- [ ] **Log Level Control:** Operational log levels are configurable dynamically (e.g., `LOG_LEVEL=info` default, `debug` for troubleshooting).
- [ ] **Trace Propagation:** Logs include correlation ID and trace ID variables to link actions across microservices.
- [ ] **Sensitive Data Scrubbing:** Log configurations contain regex masks to scrub credit cards, emails, passwords, and tokens.
- [ ] **Centralized Forwarding:** Agents (e.g., Promtail, FluentBit) forward log streams to central search indexers (Loki, Elasticsearch).

---

## 8. Alerts
- [ ] **Severity Scopes:** Alerts are classified (e.g., `warning`, `critical`, `page`) based on service impact.
- [ ] **Rule Thresholds:** Alerting thresholds are adjusted based on staging data to minimize false positives and alert fatigue.
- [ ] **Actionable Data:** Alerts contain dashboard links, query summaries, and playbook references.
- [ ] **Fail-Safe Routing:** Critical alerts route directly to on-call tools (PagerDuty, OpsGenie) with secondary backup channels.

---

## 9. SSL
- [ ] **Enforced HTTPS:** HTTP routes redirect automatically to HTTPS using permanent redirects (301 status).
- [ ] **Strong Cipher Suites:** Modern TLS protocols (TLS 1.2 / TLS 1.3) and secure cipher suites are enforced.
- [ ] **Auto Renewal:** Cert-manager or Let's Encrypt renewal actions trigger automatically (e.g., 30 days before expiration).
- [ ] **Strict Security Headers:** Ingress/proxy sets HSTS headers (`max-age=63072000; includeSubDomains; preload`).

---

## 10. CDN
- [ ] **Edge Caching Rules:** Page rules specify caching behavior for static files (e.g., image, js, css routes) and pass-through parameters for dynamic routes.
- [ ] **Gzip / Brotli Compression:** Compression is configured at the CDN level to reduce asset file transfer sizes.
- [ ] **WAF Security Policies:** CDN enables Web Application Firewall (WAF) rule sets protecting against OWASP Top 10 exploits.
- [ ] **Origin Verification:** Origin servers accept traffic only from valid CDN IP addresses (e.g., Cloudflare IP ranges).

---

## 11. DNS
- [ ] **DNSSEC Protection:** DNSSEC is enabled to prevent DNS spoofing and cache poisoning attacks.
- [ ] **CNAME Flattening:** Root domains route through CNAME flattening or alias records.
- [ ] **Low TTL Settings:** TTL properties on load balancers are set to low parameters (e.g., 60s) to allow rapid failover.
- [ ] **Email Authentication Records:** SPF (`v=spf1 ...`), DKIM, and DMARC records are configured to prevent domain spoofing.

---

## 12. Scaling
- [ ] **Metrics-based Autoscaling:** HPAs monitor CPU and Memory utilization targets (typically 70%-80%).
- [ ] **Scale Limits:** Auto-scaling configurations specify maximum replica bounds to manage costs.
- [ ] **Pre-warming Rules:** Scale targets are pre-warmed ahead of scheduled high-volume events.
- [ ] **Graceful Shutdowns:** Containers configure `preStop` hooks or signal handlers to process active requests before terminating.

---

## 13. Backup
- [ ] **Automated Schedules:** Dynamic databases are backed up daily, with transaction logs backed up hourly.
- [ ] **Data Encryption:** Backup files are encrypted before transport (using AES-256 or GPG keys).
- [ ] **Offsite Storage:** Archives are copied to separate regions or air-gapped storage buckets.
- [ ] **Retention Rules:** Backup retention rules automatically delete outdated files (e.g., keep daily backups for 30 days, monthly for 1 year).
- [ ] **Restore Testing:** Restoring backups is tested weekly in isolated environments to confirm data validity.

---

## 14. Disaster Recovery
- [ ] **Defined Target Metrics:** Recovery Time Objective (RTO) and Recovery Point Objective (RPO) values are defined.
- [ ] **Secondary Site Readiness:** Hot/cold replica environments exist in a secondary geographic region.
- [ ] **One-Click Deployment:** DR infrastructure is generated automatically via Terraform templates.
- [ ] **Failover Playbooks:** Step-by-step playbooks document database promotions and DNS switch procedures.

---

## 15. Cost Optimization
- [ ] **Resource Right-sizing:** Container resource limits match actual historical utilization averages.
- [ ] **Spot Instances Usage:** Stateless workloads and batch workers are scheduled on Spot instances.
- [ ] **Automatic Cleanups:** Unattached EBS volumes, orphan Elastic IPs, and old logs are automatically purged.
- [ ] **Off-hours Shutdown:** Non-production environments auto-shutdown during weekends and off-hours.

---

## 16. Production Deployment
- [ ] **Database Pre-flight:** Migration files are backward-compatible and tested separately in staging.
- [ ] **Pre-checks Complete:** Static linters, package scanning, unit tests, and integration tests have run and passed.
- [ ] **Deployment Schedule:** Rollouts are scheduled outside high-traffic times to reduce user impact.
- [ ] **Active Monitoring:** On-call teams monitor error rate graphs during deployment rollouts.

---

## 17. Final Review
- [ ] **Checklist Verification:** All checkboxes in sections 1-16 are completed.
- [ ] **Approval Signature:** Operations, security, and development teams have approved the release.
