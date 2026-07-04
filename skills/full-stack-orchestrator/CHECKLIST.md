# Full-Stack Orchestrator — Production Checklist

A comprehensive orchestration validation checklist ensuring every specialist skill is correctly selected, sequenced, coordinated, and validated before final delivery.

---

## 1. Planning & Requirements

- [ ] The user's product request is fully understood — ambiguities are clarified before skill activation begins.
- [ ] The project type is classified: SaaS, API, AI system, e-commerce, dashboard, portfolio, or enterprise.
- [ ] Functional requirements are decomposed into engineering domains (frontend, backend, data, AI, infrastructure).
- [ ] Non-functional requirements are identified: performance SLOs, uptime targets, and scalability expectations.
- [ ] Compliance obligations are confirmed: GDPR, HIPAA, PCI-DSS, SOC2, or none.
- [ ] Technology preferences or existing stack constraints are documented before architectural decisions begin.
- [ ] Budget and infrastructure tier constraints are acknowledged and passed to the Deployment Engineer.
- [ ] Timeline constraints are noted and communicated to all activated skills.
- [ ] Success criteria are defined: what does "done" look like for this project?
- [ ] Breaking changes or migration concerns (existing codebase) are identified upfront.

---

## 2. Specialist Selection

- [ ] The full skill roster is evaluated against the project scope before any skill is activated.
- [ ] Skills not applicable to this project type are explicitly excluded with a documented reason.
- [ ] The activation order is established before any skill begins work.

### Software Architect
- [ ] Activated as the first skill on every project — no implementation begins without architectural approval.
- [ ] Delivered: System design document, technology stack decisions, and ADRs.
- [ ] Confirmed: All downstream skills have received the architecture document as their starting context.

### UI/UX Designer
- [ ] Activated only when a user interface is part of the project scope.
- [ ] Delivered: Wireframes, design system, and user flow documentation.
- [ ] Confirmed: Frontend Engineer has reviewed and acknowledged the UX specifications.

### Frontend Engineer
- [ ] Activated after UI/UX Designer delivers approved wireframes.
- [ ] Delivered: Component architecture, routing plan, and state management strategy.
- [ ] Confirmed: Implementation respects the architectural boundaries defined by the Software Architect.

### Backend Engineer
- [ ] Activated after Software Architect delivers the system design.
- [ ] Delivered: API contract (endpoints, request/response schemas, HTTP methods), service layer design.
- [ ] Confirmed: API contract is reviewed and accepted by the Frontend Engineer before implementation.

### Database Architect
- [ ] Activated after Backend Engineer defines the data requirements.
- [ ] Delivered: Schema design, index strategy, migration plan, and data access patterns.
- [ ] Confirmed: Schema reviewed for multi-tenancy, soft deletes, audit trail, and compliance data retention.

### AI Engineer
- [ ] Activated only when LLM integration, RAG pipelines, or ML features are in scope.
- [ ] Activated after Database Architect — AI pipeline design depends on vector store and schema decisions.
- [ ] Delivered: Prompt architecture, context window strategy, tool definitions, and RAG retrieval design.
- [ ] Confirmed: LLM integration reviewed for prompt injection risk before Security Engineer activation.

### DevOps Engineer
- [ ] Activated when infrastructure provisioning, CI/CD pipeline, or container orchestration is in scope.
- [ ] Delivered: Infrastructure as Code, environment configuration, and pipeline definition.
- [ ] Confirmed: Infrastructure design is consistent with the architectural environment strategy.

### Security Engineer
- [ ] Activated after all implementation skills complete their design phase — never skipped.
- [ ] For compliance-heavy projects (HIPAA, PCI-DSS): activated twice — before design and after implementation.
- [ ] Delivered: Threat model, vulnerability findings classified by severity, required security controls.
- [ ] Confirmed: All `[CRITICAL]` and `[MAJOR]` security findings are resolved before Performance Engineer activates.

### Performance Engineer
- [ ] Activated after Security Engineer confirms no blocking security issues remain.
- [ ] Delivered: Performance audit, optimization recommendations, and metric baselines (P95, P99 targets).
- [ ] Confirmed: Performance optimizations do not introduce security regressions.

### Code Reviewer
- [ ] Activated after Frontend, Backend, and Database implementations are complete.
- [ ] Delivered: Structured review report with severity-classified findings and merge decision.
- [ ] Confirmed: All `[CRITICAL]` and `[MAJOR]` findings are resolved before Deployment Engineer activates.

### Debugging Expert
- [ ] Activated when integration failures, test failures, or post-deployment regressions are detected.
- [ ] Delivered: Root cause analysis, targeted fix recommendation, and regression test specification.
- [ ] Confirmed: Identified root cause is fixed and regression test is added before re-activating Deployment Engineer.

### Deployment Engineer
- [ ] Activated last — only after Code Reviewer has approved all implementation.
- [ ] Delivered: CI/CD pipeline, deployment manifests, rollback plan, and production readiness confirmation.
- [ ] Confirmed: All production readiness checklist items are verified before deployment proceeds.

---

## 3. Handoff Validation

- [ ] Every activated skill received the complete output of all upstream skills as context before beginning work.
- [ ] No skill began implementation before its prerequisite deliverables were confirmed complete.
- [ ] The Software Architect's technology decisions were not overridden by any implementation skill without a documented ADR revision.
- [ ] The Database Architect's schema decisions were applied consistently across Backend and AI Engineer implementations.
- [ ] The Security Engineer's required controls were implemented and verified — not deferred to a future release.
- [ ] The Code Reviewer's blocking findings were resolved and the fix was verified before proceeding to deployment.
- [ ] Every skill handoff included: context summary, constraints from upstream skills, required deliverable, and review gate criteria.

---

## 4. Conflict Resolution

- [ ] All conflicts between specialist outputs are identified and documented.
- [ ] Security Engineer conflicts are resolved with Security taking priority — no exceptions.
- [ ] Data integrity conflicts (Database Architect vs. Backend Engineer) are resolved with Database Architect taking priority.
- [ ] Architectural boundary conflicts are resolved by escalating to the Software Architect for a revised ADR.
- [ ] Style or implementation preference conflicts (not affecting security, correctness, or architecture) are resolved by the Orchestrator presenting trade-offs for user decision.
- [ ] Resolved conflicts are documented so all affected skills are updated with the new constraint.

---

## 5. Output Merging

- [ ] All specialist deliverables are consolidated into a single engineering plan with no contradictions.
- [ ] The architecture document, API contract, schema design, and deployment configuration are cross-referenced for consistency.
- [ ] Technology choices are confirmed to be consistent across all skill outputs (same framework versions, same database, same auth library).
- [ ] The environment variable set is unified — all skills' required variables are merged into a single `.env.example` template.
- [ ] The security controls from the Security Engineer are confirmed to be reflected in the Backend, Frontend, and Deployment configurations.
- [ ] The performance SLOs from the Performance Engineer are confirmed to be reflected in the Deployment Engineer's monitoring alert thresholds.

---

## 6. Engineering Review

- [ ] The merged engineering plan is reviewed for internal consistency — no component references a service or schema that was not designed.
- [ ] All API endpoints defined by the Backend Engineer are confirmed to match the contract consumed by the Frontend Engineer.
- [ ] All database tables referenced in the Backend service layer are confirmed to exist in the Database Architect's schema.
- [ ] All environment variables consumed by the application are confirmed to be provisioned in the Deployment Engineer's configuration.
- [ ] All third-party integrations (payment, email, AI APIs) are confirmed to have credentials, error handling, and fallback strategies defined.
- [ ] The rollback strategy from the Deployment Engineer is confirmed to be compatible with the database migration plan from the Database Architect.
- [ ] The monitoring alerts from the Deployment Engineer are confirmed to cover every `[CRITICAL]` risk identified by the Security and Performance Engineers.

---

## 7. Final Validation

### Production Readiness
- [ ] All specialist outputs are complete and merged into a coherent engineering plan.
- [ ] All `[CRITICAL]` and `[MAJOR]` findings from Security Engineer and Code Reviewer are resolved.
- [ ] Health check endpoints (`/health/live`, `/health/ready`) are defined and verified.
- [ ] Database migrations are confirmed to run safely against the production-equivalent dataset.
- [ ] Rollback plan is documented, tested, and executable in under 5 minutes.
- [ ] Monitoring dashboards and alerting rules are live before production traffic is routed.
- [ ] On-call engineer is identified and available for the deployment window.

### Documentation Readiness
- [ ] Architecture document is finalized and reflects all design decisions made during orchestration.
- [ ] API documentation (OpenAPI/Swagger) is complete and matches the implemented contract.
- [ ] Database schema is documented with field descriptions and relationship diagrams.
- [ ] `.env.example` is complete — every required environment variable is documented with a description.
- [ ] Deployment runbook is written and accessible to all engineers, not only the deploying engineer.
- [ ] Postmortem template is prepared for the first production incident.

### Deployment Readiness
- [ ] CI/CD pipeline is fully configured: lint → typecheck → tests → security scan → build → deploy.
- [ ] Container images are built from pinned base versions with multi-stage Dockerfiles.
- [ ] Secrets are sourced from a secrets manager — no secrets in source code or environment files committed to version control.
- [ ] Kubernetes (or equivalent) manifests include resource `requests`/`limits`, `readinessProbe`, and `livenessProbe` for all services.
- [ ] The production deployment uses an immutable artifact tag (git SHA or semantic version) — never `:latest`.
- [ ] Post-deployment monitoring plan is confirmed: dashboards will be observed for 30 minutes after every production release.
