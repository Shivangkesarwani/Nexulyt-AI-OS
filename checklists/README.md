# Nexulyt-AI-OS Engineering Checklists

Welcome to the **Nexulyt-AI-OS Engineering Checklists** library. This directory contains a reusable collection of production-grade checklists designed to act as quality gates at every stage of the software engineering lifecycle (from requirements gathering to system deployment).

---

## 1. Purpose

The checklists serve to enforce compliance with codebase rules, architecture standards, security controls, and performance budgets. By using these checklists, engineering teams can:
1. **Reduce Human Error**: Enforce standardized verification checks prior to release, preventing common configuration issues or structural drift.
2. **Accelerate Reviews**: Facilitate faster pull request approvals by establishing clear exit criteria.
3. **Configure AI Assistants**: Bootstrap AI agents (e.g. Claude Code, Cursor, Windsurf) into quality assurance roles to automatically audit outputs against these lists.

---

## 2. Integration with Skills & Workflows

* **Workspace Skills (e.g., [code-reviewer](file:///d:/projects/Nexulyt-AI-OS/skills/code-reviewer/SKILL.md))**: Skills describe *how* an agent runs audits. The checklists represent the *concrete criteria* the agent evaluates to determine if a task is complete.
* **Workflows (e.g., [code-review-workflow.md](file:///d:/projects/Nexulyt-AI-OS/workflows/code-review-workflow.md))**: Workflows define the *sequence of pipeline stages*. Checklists define the *entry and exit gates* required to transition from one stage to another.

---

## 3. How to Use the Checklists

1. **Copy the Target Checklist**: Select the checklist matching your active development phase (e.g., [security-checklist.md](file:///d:/projects/Nexulyt-AI-OS/checklists/security-checklist.md) during pull request compilation).
2. **Execute Audits Incrementally**: Mark items as completed (`- [x]`) as you perform code updates.
3. **Verify Exit Criteria**: Confirm all validation constraints and exit metrics are met before submitting code for peer review.
4. **Feed Context to AI Agents**: Provide the checklist to your AI agent when delegating audits:
   > "Using the [backend-checklist.md](file:///d:/projects/Nexulyt-AI-OS/checklists/backend-checklist.md) as a quality gate, audit our new endpoint code and report any incomplete checks..."

---

## 4. Directory Map & Recommended Order

We recommend using these checklists chronologically throughout your sprints:

### Phase 1: Discovery & Planning
* **[Project Lifecycle Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/project-checklist.md)**: Overall lifecycle validation from discovery to post-launch.
* **[Requirement Analysis Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/requirement-analysis-checklist.md)**: Verifying alignment, scope boundaries, and functional specs.

### Phase 2: Design & System Architecture
* **[Architecture Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/architecture-checklist.md)**: Verifying topology boundaries, service decoupling, and protocols.
* **[UI/UX Design Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/ui-ux-checklist.md)**: Verifying layout hierarchy, responsiveness, and WCAG AA accessibility.
* **[Database Design Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/database-checklist.md)**: Verifying schema normalizations, index setups, partitions, and backups.

### Phase 3: Development & Engineering
* **[Frontend Quality Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/frontend-checklist.md)**: Verifying component structures, state models, Web Vitals, and SEO.
* **[Backend Quality Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/backend-checklist.md)**: Verifying routes controllers, validation schemas, and structured logging.
* **[AI Engineering Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/ai-engineering-checklist.md)**: Verifying prompt templates, RAG chunking similarity checks, and guardrails.

### Phase 4: Verification, Security & Review
* **[Security Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/security-checklist.md)**: Verifying threat models, cryptographic encryptions, OWASP, and credentials safety.
* **[Performance Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/performance-checklist.md)**: Verifying CPU profiling, CDN caching, query speeds, and token budgets.
* **[Testing Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/testing-checklist.md)**: Verifying Unit, Integration, E2E, and regression coverage targets.
* **[Code Review Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/code-review-checklist.md)**: Peer review parameters for readability, maintainability, and clean code.
* **[Debugging Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/debugging-checklist.md)**: Root cause analysis (RCA), logs trace isolation, and fix verifications.

### Phase 5: Deployment, Release & Post-Launch
* **[Deployment Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/deployment-checklist.md)**: Verifying Docker configs, Kubernetes manifests, CI/CD gates, and rollbacks.
* **[Release Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/release-checklist.md)**: Verifying version tag numbers, changelogs, and approval gates.
* **[Documentation Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/documentation-checklist.md)**: Verifying technical accuracy, links resolution, and format compliance.
* **[Repository Review Checklist](file:///d:/projects/Nexulyt-AI-OS/checklists/repository-review-checklist.md)**: Complete workspace cleanup, directory consistency checks, and pre-release audits.
