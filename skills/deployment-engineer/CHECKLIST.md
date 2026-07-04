# Deployment Engineer Production Validation Checklist

A comprehensive validation checklist ensuring all deployment phases — from requirements review to final production deployment and rollback verification — are executed safely and reliably.

---

## 1. Requirement Review

- [ ] Business requirements documented and approved by stakeholders.
- [ ] Technical specifications finalized and reviewed by the engineering team.
- [ ] SLA/SLO targets defined and agreed upon (uptime, latency, error rate).
- [ ] Resource requirements calculated (CPU, memory, storage, bandwidth).
- [ ] Traffic patterns analyzed (peak vs. average, geographic distribution).
- [ ] Dependencies identified and documented (databases, APIs, third-party services).
- [ ] Integration points mapped and validated with external systems.
- [ ] Compliance requirements reviewed (GDPR, HIPAA, SOC2, PCI-DSS, ITAR).
- [ ] Data retention and lifecycle policies defined.
- [ ] Maintenance window schedules established and communicated.
- [ ] Stakeholder communication plan prepared and contact list verified.
- [ ] Success criteria and KPIs defined with measurable targets.
- [ ] Budget and cost projections approved by finance.
- [ ] Capacity planning completed for expected growth trajectory.
- [ ] Feature flags strategy defined for risky deployments.
- [ ] Multi-region requirements validated if applicable.

---

## 2. Environment Configuration

- [ ] Environment parity verified across dev/staging/production.
- [ ] Infrastructure as Code (IaC) templates validated and peer-reviewed.
- [ ] Network topology documented with architecture diagrams.
- [ ] VPC/subnet configuration verified with CIDR blocks documented.
- [ ] Security groups/firewall rules configured with least privilege principle.
- [ ] Load balancer configuration validated and health checks tested.
- [ ] Region and availability zone selection confirmed based on requirements.
- [ ] Resource tagging strategy implemented for cost allocation.
- [ ] Environment variables documented and validated per environment.
- [ ] Configuration drift detection enabled and monitored.
- [ ] Multi-region setup validated with failover testing (if applicable).
- [ ] Disaster recovery region configured and synchronized.
- [ ] NAT gateway and internet gateway configurations verified.
- [ ] VPC peering or transit gateway configured for multi-VPC architecture.
- [ ] Service endpoints configured to avoid public internet traffic.
- [ ] Network ACLs reviewed and aligned with security requirements.

---

## 3. Secrets Management

- [ ] Secrets management solution implemented (Vault, AWS Secrets Manager, Azure Key Vault).
- [ ] Secret rotation policies configured with appropriate schedules.
- [ ] Access control policies defined with role-based permissions.
- [ ] Secrets encrypted at rest with managed or customer-managed keys.
- [ ] Secrets encrypted in transit using TLS 1.2 or higher.
- [ ] API keys and tokens securely stored and never hardcoded.
- [ ] Database credentials managed via secrets manager with automatic rotation.
- [ ] Service account keys rotated and secured with minimum privileges.
- [ ] Certificate private keys secured in key management system.
- [ ] Audit logging enabled for all secret access and modifications.
- [ ] Emergency access procedures documented with break-glass workflows.
- [ ] Secrets never committed to version control verified (git history scanned).
- [ ] Development secrets separated from production with different key spaces.
- [ ] Secret versioning enabled for rollback capability.
- [ ] Automatic alerts configured for unauthorized secret access attempts.
- [ ] Secrets backup and recovery procedures tested.

---

## 4. Docker Validation

- [ ] Base images sourced from trusted registries only (official images preferred).
- [ ] Image vulnerability scanning completed with no high/critical CVEs.
- [ ] Multi-stage builds optimized to reduce final image size.
- [ ] Image size optimized and documented (target <500MB for application images).
- [ ] No secrets baked into images verified through image inspection.
- [ ] Image tags follow semantic versioning or commit SHA convention.
- [ ] Health check endpoints defined in Dockerfile with appropriate intervals.
- [ ] Non-root user configured for container runtime security.
- [ ] .dockerignore file properly configured to exclude unnecessary files.
- [ ] Image signing and verification enabled using Docker Content Trust or similar.
- [ ] Layer caching strategy optimized for fast builds.
- [ ] Resource limits defined in container configuration (memory, CPU).
- [ ] Registry authentication configured with service accounts.
- [ ] Image retention policies implemented to clean up old images.
- [ ] Build reproducibility verified (same inputs produce identical images).
- [ ] Container startup time measured and optimized (<30s target).
- [ ] HEALTHCHECK instruction included with appropriate timeout values.
- [ ] Labels applied for metadata (version, commit, build date).

---

## 5. Kubernetes Validation

- [ ] Cluster version compatibility verified with application requirements.
- [ ] RBAC policies configured and tested with least privilege access.
- [ ] Pod Security Standards applied (baseline, restricted, or privileged).
- [ ] Resource requests and limits defined for all containers.
- [ ] Horizontal Pod Autoscaler configured with appropriate metrics.
- [ ] PodDisruptionBudgets defined to ensure availability during updates.
- [ ] Network policies implemented to restrict pod-to-pod communication.
- [ ] Service mesh configured if applicable (Istio, Linkerd, Consul).
- [ ] Ingress controllers tested with routing rules validated.
- [ ] Persistent volume claims validated with appropriate storage classes.
- [ ] ConfigMaps and Secrets properly mounted as volumes or environment variables.
- [ ] Liveness probes configured with appropriate failure thresholds.
- [ ] Readiness probes configured to prevent premature traffic routing.
- [ ] Startup probes configured for slow-starting applications (>30s).
- [ ] Init containers validated for dependency initialization.
- [ ] Node affinity and anti-affinity rules set for high availability.
- [ ] Taints and tolerations configured for specialized workloads.
- [ ] Namespace isolation verified with resource quotas.
- [ ] Cluster autoscaler configured with min/max node counts.
- [ ] Pod priority classes defined for critical workloads.
- [ ] Service accounts created per application with minimal permissions.
- [ ] Helm charts or manifests version controlled and tested.

---

## 6. CI/CD Validation

- [ ] Pipeline stages clearly defined (build, test, scan, deploy).
- [ ] Automated testing gates implemented with minimum coverage thresholds.
- [ ] Code quality gates configured (linting, static analysis, code coverage).
- [ ] Security scanning integrated (SAST, DAST, dependency scanning).
- [ ] Container image vulnerability scanning enabled in pipeline.
- [ ] Build artifacts versioned, tagged, and stored in artifact repository.
- [ ] Deployment approval workflows configured for production environments.
- [ ] Rollback mechanisms tested and documented in runbooks.
- [ ] Environment promotion strategy validated (dev → staging → production).
- [ ] Pipeline failure notifications configured (email, Slack, PagerDuty).
- [ ] Deployment frequency metrics tracked and monitored.
- [ ] Lead time for changes measured from commit to production.
- [ ] Change failure rate monitored with alerting thresholds.
- [ ] Mean time to recovery (MTTR) tracked and optimized.
- [ ] Pipeline secrets secured using CI/CD platform secret management.
- [ ] Concurrent deployment handling configured to prevent conflicts.
- [ ] Build caching implemented for faster pipeline execution.
- [ ] Deployment gates verified (manual approvals, automated validations).
- [ ] Artifact signing and verification enabled for supply chain security.
- [ ] Pipeline as code stored in version control.

---

## 7. Domain Configuration

- [ ] Domain ownership verified with registrar access confirmed.
- [ ] Domain registrar access credentials secured and documented.
- [ ] Domain transfer lock status enabled to prevent unauthorized transfers.
- [ ] WHOIS privacy configured to protect registrant information.
- [ ] Domain auto-renewal enabled to prevent expiration.
- [ ] Administrative contacts updated with current team information.
- [ ] Domain expiration monitoring enabled with 90/30/7 day alerts.
- [ ] Subdomain strategy documented (www, api, cdn, etc.).
- [ ] Apex domain vs. www preference decided and configured.
- [ ] International domain support configured if needed (IDN).
- [ ] Domain backup/transfer authorization codes secured.
- [ ] Registrar lock enabled for production domains.

---

## 8. DNS Verification

- [ ] DNS provider selected and configured with redundancy.
- [ ] A/AAAA records configured correctly for IPv4/IPv6.
- [ ] CNAME records validated and pointing to correct targets.
- [ ] MX records configured if email services required.
- [ ] TXT records for domain verification added (Google, Microsoft, etc.).
- [ ] SPF records configured to prevent email spoofing.
- [ ] DKIM records configured for email authentication.
- [ ] DMARC policy implemented with reporting configured.
- [ ] TTL values optimized (300s during deployment, 3600s normal operations).
- [ ] DNS failover configured with health check monitoring.
- [ ] GeoDNS configured if geographic routing required.
- [ ] DNSSEC enabled and validated with DS records.
- [ ] DNS propagation verified globally using multiple DNS checkers.
- [ ] DNS monitoring and alerts configured for record changes.
- [ ] CAA records configured to restrict certificate issuance.
- [ ] Alias/ANAME records configured for apex domain if needed.

---

## 9. SSL Verification

- [ ] SSL certificates obtained from trusted CA (Let's Encrypt, DigiCert, etc.).
- [ ] Certificate chain completeness verified using SSL Labs.
- [ ] Private keys secured in key management system.
- [ ] Certificate auto-renewal configured and tested.
- [ ] Wildcard vs. SAN certificate decision made based on subdomain needs.
- [ ] Certificate expiration monitoring enabled with 30/14/7 day alerts.
- [ ] TLS 1.2 minimum version enforced (TLS 1.0/1.1 disabled).
- [ ] TLS 1.3 support enabled for improved performance.
- [ ] Strong cipher suites configured (forward secrecy enabled).
- [ ] SSL Labs rating A or higher achieved on all endpoints.
- [ ] HSTS headers configured with appropriate max-age.
- [ ] Certificate transparency logs verified and monitored.
- [ ] OCSP stapling enabled for improved performance.
- [ ] Certificate revocation procedure documented.
- [ ] Mixed content warnings resolved (all resources over HTTPS).
- [ ] Certificate backup stored securely for disaster recovery.

---

## 10. Reverse Proxy

- [ ] Reverse proxy solution selected (Nginx, HAProxy, Envoy, Traefik).
- [ ] Upstream server configuration validated with health checks.
- [ ] Load balancing algorithm configured (round-robin, least-conn, IP hash).
- [ ] Connection timeouts optimized (connect, send, read timeouts).
- [ ] Request/response buffering configured appropriately.
- [ ] Rate limiting implemented per IP/user/API key.
- [ ] Request size limits enforced (body size, header size).
- [ ] Compression enabled (gzip, brotli) with appropriate levels.
- [ ] Caching strategy implemented with cache control headers.
- [ ] WebSocket support configured if needed (upgrade headers).
- [ ] HTTP/2 and HTTP/3 support enabled for performance.
- [ ] Security headers configured (CSP, X-Frame-Options, X-Content-Type-Options).
- [ ] Access logs configured with appropriate format and retention.
- [ ] Error pages customized for better user experience.
- [ ] IP whitelisting/blacklisting configured for admin endpoints.
- [ ] DDoS protection measures implemented (connection limits, request rate).
- [ ] Sticky sessions configured if required for stateful applications.
- [ ] SSL termination configured at proxy level.

---

## 11. CDN Configuration

- [ ] CDN provider selected (CloudFront, Cloudflare, Fastly, Akamai).
- [ ] Origin server configuration validated and secured.
- [ ] Cache invalidation strategy defined (manual, API-triggered, versioned URLs).
- [ ] Cache TTL policies configured per content type.
- [ ] Edge locations optimized for target user geographic distribution.
- [ ] Custom domain configured on CDN with SSL certificate.
- [ ] SSL/TLS configured at CDN edge with modern protocols.
- [ ] Cache-Control headers optimized for static and dynamic content.
- [ ] Static asset versioning strategy implemented (hash-based filenames).
- [ ] Compression enabled at edge (gzip, brotli).
- [ ] Image optimization configured (WebP, AVIF support).
- [ ] Bot protection enabled to prevent scraping.
- [ ] WAF rules configured for common attack vectors.
- [ ] Geographic restrictions configured if content licensing requires.
- [ ] CDN failover to origin tested and validated.
- [ ] CDN analytics and monitoring enabled with real-time dashboards.
- [ ] Cache hit ratio monitored and optimized (target >80%).
- [ ] Purge/invalidation procedures documented and tested.

---

## 12. Monitoring

- [ ] Monitoring solution deployed (Prometheus, Datadog, New Relic, Grafana Cloud).
- [ ] Application metrics instrumented (RED: Rate, Errors, Duration).
- [ ] Infrastructure metrics collected (CPU, memory, disk, network).
- [ ] Custom business metrics defined and tracked.
- [ ] Dashboards created for key metrics per service.
- [ ] Alert thresholds configured based on baseline and SLOs.
- [ ] Alert routing and escalation configured (email, Slack, PagerDuty).
- [ ] PagerDuty/OpsGenie integration enabled for on-call rotation.
- [ ] Synthetic monitoring configured for critical user journeys.
- [ ] Real User Monitoring (RUM) enabled for frontend performance.
- [ ] APM (Application Performance Monitoring) configured with distributed tracing.
- [ ] Database performance monitoring enabled (query performance, connections).
- [ ] Network performance monitoring configured.
- [ ] Third-party service monitoring implemented (API dependencies).
- [ ] SLI/SLO dashboards created with error budget tracking.
- [ ] Anomaly detection configured for unusual patterns.
- [ ] Capacity planning metrics tracked (saturation, utilization trends).
- [ ] Alert fatigue prevented through proper threshold tuning.
- [ ] Runbook links added to alerts for faster incident resolution.

---

## 13. Logging

- [ ] Centralized logging solution deployed (ELK, Splunk, CloudWatch, Loki).
- [ ] Log aggregation configured from all services and infrastructure.
- [ ] Log retention policies defined per log type and compliance requirements.
- [ ] Log rotation configured to prevent disk space exhaustion.
- [ ] Structured logging implemented (JSON format preferred).
- [ ] Log levels appropriately configured (ERROR, WARN, INFO, DEBUG).
- [ ] PII/sensitive data redaction implemented and tested.
- [ ] Application logs captured with correlation IDs for tracing.
- [ ] Access logs captured with request/response metadata.
- [ ] Error logs captured with stack traces and context.
- [ ] Audit logs captured for compliance and security events.
- [ ] Log parsing and indexing optimized for search performance.
- [ ] Log search and filtering tested with common queries.
- [ ] Log-based alerts configured for critical error patterns.
- [ ] Log backup strategy implemented for disaster recovery.
- [ ] Compliance logging requirements met (immutable logs, retention).
- [ ] Log sampling implemented for high-volume services if needed.
- [ ] Log export configured for long-term archival.

---

## 14. Health Checks

- [ ] Application health endpoint implemented (/health, /healthz).
- [ ] Database connectivity check included in health endpoint.
- [ ] External dependency checks configured (APIs, queues, cache).
- [ ] Deep vs. shallow health checks defined appropriately.
- [ ] Health check timeout values set (typically 5-10 seconds).
- [ ] Health check intervals optimized (10-30 seconds typical).
- [ ] Load balancer health checks configured and validated.
- [ ] Kubernetes liveness probes configured with failure threshold.
- [ ] Kubernetes readiness probes configured to control traffic.
- [ ] Startup probes configured for slow-starting applications (>30s).
- [ ] Health check logging enabled without excessive noise.
- [ ] Graceful degradation logic implemented for partial failures.
- [ ] Circuit breaker pattern implemented for external dependencies.
- [ ] Health check monitoring and alerting enabled.
- [ ] Health check responses include version and build information.
- [ ] Health check endpoint secured or rate-limited if exposed publicly.

---

## 15. Autoscaling

- [ ] Autoscaling metrics identified (CPU, memory, request rate, queue depth).
- [ ] Scale-up thresholds configured (typically 70-80% utilization).
- [ ] Scale-down thresholds configured (typically 30-40% utilization).
- [ ] Minimum instance count set for high availability (typically 2-3).
- [ ] Maximum instance count set based on cost and capacity constraints.
- [ ] Cooldown periods configured to prevent flapping (5-10 minutes).
- [ ] Predictive scaling configured if traffic patterns are predictable.
- [ ] Scheduled scaling configured for known traffic patterns.
- [ ] Target tracking policies defined for dynamic scaling.
- [ ] Step scaling policies configured for rapid scale-up scenarios.
- [ ] Database connection pool sizing validated for scaled instances.
- [ ] Session affinity/stickiness configured if required.
- [ ] Autoscaling testing completed under simulated load.
- [ ] Cost implications of scaling analyzed and budgeted.
- [ ] Autoscaling events logged and monitored for optimization.
- [ ] Scale-down protection configured during critical business hours.
- [ ] Multi-dimensional autoscaling tested (multiple metrics simultaneously).

---

## 16. Rollback Strategy

- [ ] Rollback procedure documented with step-by-step instructions.
- [ ] Rollback decision criteria defined (error rate, latency, business impact).
- [ ] Automated rollback triggers configured in deployment pipeline.
- [ ] Manual rollback procedure tested in staging environment.
- [ ] Database migration rollback plan prepared and tested.
- [ ] Feature flags implemented for instant rollback without deployment.
- [ ] Blue-green deployment capability verified and tested.
- [ ] Canary deployment rollback tested with traffic shifting.
- [ ] Version pinning strategy defined for all dependencies.
- [ ] Artifact versioning enables easy rollback to previous versions.
- [ ] Configuration rollback procedure documented separately.
- [ ] Rollback time estimates documented per component.
- [ ] Rollback communication plan prepared (status page, notifications).
- [ ] Post-rollback validation checklist prepared.
- [ ] Rollback authority defined (who can authorize production rollback).
- [ ] Rollback testing performed quarterly to ensure readiness.

---

## 17. Backup Strategy

- [ ] Backup solution implemented with automation.
- [ ] Backup frequency defined based on RPO requirements.
- [ ] Backup retention policy configured (daily, weekly, monthly, yearly).
- [ ] Full backup schedule configured (typically weekly).
- [ ] Incremental backup schedule configured (typically daily).
- [ ] Database backup automated with point-in-time recovery capability.
- [ ] Application state backup configured if stateful.
- [ ] Configuration backup automated (IaC, configs, secrets metadata).
- [ ] Secrets backup secured separately with encryption.
- [ ] Backup encryption enabled at rest and in transit.
- [ ] Backup integrity verification automated (checksum validation).
- [ ] Backup restoration tested regularly (monthly minimum).
- [ ] Off-site/cross-region backup configured for disaster recovery.
- [ ] Backup monitoring and alerting enabled for failures.
- [ ] Backup access controls configured with least privilege.
- [ ] Backup restoration time documented (RTO targets).
- [ ] Backup storage costs analyzed and optimized.
- [ ] Backup documentation maintained with recovery procedures.

---

## 18. Disaster Recovery

- [ ] Disaster recovery plan documented with scenarios and procedures.
- [ ] RTO (Recovery Time Objective) defined per service tier.
- [ ] RPO (Recovery Point Objective) defined per data store.
- [ ] DR site/region configured and synchronized.
- [ ] Data replication to DR site configured (async or sync).
- [ ] DR failover procedure documented with runbooks.
- [ ] DR failback procedure documented and tested.
- [ ] DR testing schedule established (quarterly minimum).
- [ ] Last DR test results documented with lessons learned.
- [ ] Database replication lag monitored with alerting.
- [ ] Cross-region load balancing configured for automated failover.
- [ ] DNS failover procedure tested with Route 53/Traffic Manager.
- [ ] Communication plan for DR events prepared (stakeholders, customers).
- [ ] Runbook for common disaster scenarios created and maintained.
- [ ] DR team roles and responsibilities defined with contact information.
- [ ] Third-party DR dependencies identified and validated.
- [ ] DR infrastructure costs budgeted and approved.
- [ ] DR metrics tracked (failover time, data loss, recovery success rate).

---

## 19. Performance Validation

- [ ] Load testing completed with expected peak traffic (150% of peak).
- [ ] Stress testing completed to identify breaking points.
- [ ] Spike testing completed to validate autoscaling response.
- [ ] Soak/endurance testing completed (24-48 hours minimum).
- [ ] Performance benchmarks documented and baselined.
- [ ] Response time SLOs validated (p50, p95, p99 percentiles).
- [ ] Throughput targets validated (requests per second).
- [ ] Concurrent user targets validated with realistic scenarios.
- [ ] Database query performance optimized with indexing strategy.
- [ ] API response times measured and optimized (<200ms target).
- [ ] Page load times optimized (<3s target, <1s ideal).
- [ ] Time to First Byte (TTFB) optimized (<600ms target).
- [ ] Core Web Vitals validated (LCP, FID, CLS within thresholds).
- [ ] Performance regression tests automated in CI/CD.
- [ ] Performance bottlenecks identified and resolved.
- [ ] CDN cache hit ratio optimized (>80% target).
- [ ] Database connection pool tuned for optimal performance.
- [ ] N+1 query problems identified and eliminated.

---

## 20. Security Validation

- [ ] Security audit completed by internal or external team.
- [ ] Penetration testing performed and findings remediated.
- [ ] OWASP Top 10 vulnerabilities addressed and validated.
- [ ] SQL injection prevention verified with parameterized queries.
- [ ] XSS prevention verified with proper output encoding.
- [ ] CSRF protection implemented with tokens or SameSite cookies.
- [ ] Authentication mechanism validated (OAuth, SAML, JWT).
- [ ] Authorization controls tested with role-based access.
- [ ] Session management secured (httpOnly, secure, SameSite flags).
- [ ] Input validation implemented on all user inputs.
- [ ] Output encoding implemented to prevent injection attacks.
- [ ] Security headers configured (CSP, HSTS, X-Frame-Options, etc.).
- [ ] Dependency vulnerabilities scanned and patched.
- [ ] Container image vulnerabilities addressed (no high/critical).
- [ ] Network segmentation verified with private subnets.
- [ ] Principle of least privilege enforced for all accounts.
- [ ] Encryption at rest verified for all data stores.
- [ ] Encryption in transit verified (TLS 1.2+ everywhere).
- [ ] API rate limiting configured to prevent abuse.
- [ ] DDoS protection tested and validated.
- [ ] Incident response plan documented and team trained.
- [ ] Security logging and monitoring enabled with SIEM integration.

---

## 21. Production Readiness

- [ ] All previous checklist items completed and verified.
- [ ] Production infrastructure provisioned and validated.
- [ ] Production data migrated and validated for integrity.
- [ ] Production environment smoke tested with critical paths.
- [ ] Performance under production load validated.
- [ ] Monitoring dashboards reviewed and validated.
- [ ] Alert thresholds validated with baseline metrics.
- [ ] On-call rotation established with schedule published.
- [ ] Runbooks created and reviewed for common scenarios.
- [ ] Documentation updated (architecture, deployment, operations).
- [ ] Training completed for operations team.
- [ ] Customer support team briefed with escalation procedures.
- [ ] Marketing/communications team aligned on launch timeline.
- [ ] Legal/compliance sign-off obtained.
- [ ] Maintenance window scheduled and communicated.
- [ ] Rollback plan reviewed and understood by all stakeholders.
- [ ] Go-live communication sent to all stakeholders.
- [ ] Post-deployment validation plan prepared.
- [ ] Success metrics defined and dashboards ready.

---

## 22. Final Deployment Review

- [ ] Pre-deployment checklist completed and signed off.
- [ ] Deployment window confirmed with all stakeholders.
- [ ] All stakeholders notified with timeline and contact information.
- [ ] Deployment team assembled and roles assigned.
- [ ] Communication channels established (war room, Slack, Zoom).
- [ ] Deployment runbook reviewed and understood by all.
- [ ] Rollback criteria agreed upon with clear thresholds.
- [ ] Success criteria confirmed and monitoring in place.
- [ ] Database migrations ready and tested in staging.
- [ ] Feature flags configured and tested.
- [ ] Traffic routing strategy confirmed (blue/green, canary percentages).
- [ ] Monitoring dashboards open and visible to team.
- [ ] Alert channels monitored by on-call team.
- [ ] Deployment executed according to plan with checkpoints.
- [ ] Post-deployment validation completed successfully.
- [ ] Health checks passing across all services.
- [ ] Performance metrics within acceptable range.
- [ ] Error rates within acceptable range (<1% typical).
- [ ] User acceptance testing completed with sign-off.
- [ ] Stakeholders notified of successful deployment.
- [ ] Post-deployment retrospective scheduled.
- [ ] Documentation updated with deployment notes and lessons learned.
- [ ] Lessons learned documented for future deployments.