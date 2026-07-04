# Skills Overview

This document describes the purpose, responsibilities, input criteria, output formats, and dependencies for each of the 13 specialized engineering skills inside **Nexulyt-AI-OS**.

---

## 1. Full-Stack Orchestrator
- **Purpose:** Coordinates execution, resolves design conflicts, and manages context sharing.
- **Responsibilities:** Requirements analysis, complexity classification, skill selection, and final delivery assembly.
- **Inputs:** Natural language project brief.
- **Outputs:** Merged engineering plan, risk register, and phase checklists.
- **Dependencies:** Software Architect (requires system boundaries to run).

---

## 2. Software Architect
- **Purpose:** Formulates system designs and selects technology stack matrices.
- **Responsibilities:** System topology design (C4 Level 2/3), technology stack matrices, writing ADRs, and database cutover planning.
- **Inputs:** Project brief, functional requirements.
- **Outputs:** Architectural specifications, stack decisions, and ADRs.
- **Dependencies:** None.

---

## 3. UI/UX Designer
- **Purpose:** Translates architectures into visual mockups and user experiences.
- **Responsibilities:** Component states, responsive layouts, design systems, and WCAG accessibility guidelines.
- **Inputs:** Architecture specification, user persona data.
- **Outputs:** Wireframes, UI mockups, and typography definitions.
- **Dependencies:** Software Architect.

---

## 4. Frontend Engineer
- **Purpose:** Implements views, component states, and routing logic.
- **Responsibilities:** Responsive coding, state management, API integration, and accessibility tags.
- **Inputs:** UI mockups, API contracts, design systems.
- **Outputs:** Production-ready frontend codebase, build assets, and testing suites.
- **Dependencies:** UI/UX Designer, Software Architect.

---

## 5. Backend Engineer
- **Purpose:** Builds APIs, services, queues, and logic layers.
- **Responsibilities:** API design, input validations, logic execution, background workers, and billing integrations.
- **Inputs:** Architecture design, Database schema.
- **Outputs:** API routes, OpenAPI schemas, business logic files, and unit tests.
- **Dependencies:** Software Architect, Database Architect.

---

## 6. Database Architect
- **Purpose:** Designs normalized schemas, migration templates, and indexing structures.
- **Responsibilities:** Logical database layouts, B-Tree/Composite indexing, transactional isolation, and data replication.
- **Inputs:** System requirements, entity relationship constraints.
- **Outputs:** SQL migration scripts (`up`/`down` files) and database design diagrams.
- **Dependencies:** Software Architect.

---

## 7. AI Engineer
- **Purpose:** Builds context-aware RAG pipelines and custom tools.
- **Responsibilities:** Prompt cataloging, embedding schemas, context retrieval logic, and LLM output parsing.
- **Inputs:** System scope, vector database schema.
- **Outputs:** Prompt scripts, tool functions, context parsers, and integration code.
- **Dependencies:** Database Architect.

---

## 8. DevOps Engineer
- **Purpose:** Provisions infrastructure, local runtimes, and secrets.
- **Responsibilities:** Infrastructure as Code (Terraform), cluster configurations, network routes, and secret configurations.
- **Inputs:** Deployment environments target, network architecture.
- **Outputs:** IaC code, Docker networks, and pipeline definitions.
- **Dependencies:** Software Architect.

---

## 9. Security Engineer
- **Purpose:** Audits security vulnerabilities and structures data protection controls.
- **Responsibilities:** STRIDE threat models, DREAD risk matrices, input filter designs, and encryption checks.
- **Inputs:** Codebases, container manifests, and schemas.
- **Outputs:** Audit logs, threat registers, and validation codes.
- **Dependencies:** All implementation skills (requires built code to inspect).

---

## 10. Performance Engineer
- **Purpose:** Measures system latency, profiles runtimes, and optimizes workloads.
- **Responsibilities:** APM profiling, N+1 query elimination, CDN caching setups, and load testing.
- **Inputs:** Implementation codebases, target SLO definitions.
- **Outputs:** Latency benchmarks, optimized SQL queries, and caching codes.
- **Dependencies:** Security Engineer.

---

## 11. Code Reviewer
- **Purpose:** Enforces coding standards and style guidelines before code merges.
- **Responsibilities:** Code audits, logic validation, style consistency, and check verification.
- **Inputs:** Branch diff files, testing coverage.
- **Outputs:** Review approvals and change tickets.
- **Dependencies:** All implementation skills.

---

## 12. Debugging Expert
- **Purpose:** Investigates staging and production failures using structured evidence.
- **Responsibilities:** Stack trace analysis, query diagnostics (`EXPLAIN ANALYZE`), log correlation, and heap profiling.
- **Inputs:** Logs, trace IDs, stack trace outputs, and bug tickets.
- **Outputs:** Root Cause Analysis (RCA) docs, target fixes, and regression tests.
- **Dependencies:** All skills.

---

## 13. Deployment Engineer
- **Purpose:** Deploys services using automated pipelines and checks.
- **Responsibilities:** CI/CD setups, SSL provisioning, canary weight transitions, and rollbacks.
- **Inputs:** Approved build artifacts, staging verification signals.
- **Outputs:** Pipeline actions, Kubernetes service updates, and DNS records.
- **Dependencies:** Code Reviewer.
