# Frequently Asked Questions (FAQ)

This document contains detailed answers to the most common questions regarding the installation, configuration, utilization, and extension of **Nexulyt-AI-OS**.

---

## General Questions

### Q1: What is Nexulyt-AI-OS?
**A:** Nexulyt-AI-OS is a cognitive operating system for agentic software engineering. It defines role parameters (`SKILL.md`), validation logic (`CHECKLIST.md`), and reference reasoning plans (`EXAMPLES.md`) that transform generalist AI assistants into specialized engineering agents.

### Q2: Is Nexulyt-AI-OS a runtime framework like LangChain or AutoGen?
**A:** No. Nexulyt-AI-OS is a declarative knowledge base. It provides the system logic, blueprints, and schemas that guide AI assistants during development. It can be integrated into frameworks like LangChain, CrewAI, or used directly within AI-first IDEs like Cursor and Windsurf.

### Q3: Who is the author of this project?
**A:** The project is authored by Shivang Kesarwani.

### Q4: Under what license is Nexulyt-AI-OS released?
**A:** The project is released under the MIT License. You can review the details in the [LICENSE](file:///d:/projects/Nexulyt-AI-OS/LICENSE) file.

### Q5: Can I use this in proprietary commercial software?
**A:** Yes, the MIT License allows commercial use, modification, distribution, and private use, provided the copyright notice and permission notice are included.

---

## Orchestration Questions

### Q6: What is the role of the Full-Stack Orchestrator?
**A:** The Orchestrator is the central coordinator of Nexulyt-AI-OS. It decomposes user requests, selects the required specialist skills, sequences their execution, and validates outputs through defined quality gates.

### Q7: Does the Orchestrator write code directly?
**A:** No. The Orchestrator manages the workflow and delegates implementation tasks to specialists (e.g., Frontend/Backend Engineers). It does not write application code itself.

### Q8: What happens if two specialists disagree?
**A:** The Orchestrator applies the **Specialist Priority Matrix** to resolve conflicts deterministically. Security has the highest priority, followed by Data Integrity, Architecture, Compliance, and Performance.

### Q9: Can I skip the Software Architect phase?
**A:** No. The Software Architect must establish the system design before any code implementation begins.

### Q10: How does the Orchestrator determine if a project is complex?
**A:** It uses the **Complexity Classification Engine** to evaluate database requirements, multi-tenancy, real-time features, AI integrations, and compliance levels.

---

## Skills & Specialists

### Q11: How many core skills are in the repository?
**A:** There are 13 core skills: Orchestrator, Architect, UI/UX Designer, Frontend Engineer, Backend Engineer, Database Architect, AI Engineer, DevOps Engineer, Security Engineer, Performance Engineer, Code Reviewer, Debugging Expert, and Deployment Engineer.

### Q12: Why is the Database Architect skill folder named `database-architect`?
**A:** It was updated during the repository cleanup to correct a naming typo (`database-architec`) and restore link consistency across all files.

### Q13: Can I run skills in parallel?
**A:** Yes, the Orchestrator identifies parallel tracks (e.g., Database Architect and UI/UX Designer) that have no mutual dependencies.

### Q14: How does the Security Engineer audit RAG pipelines?
**A:** It checks for prompt injection, inputs verification, and ensures vector data queries are filtered by tenant context keys.

### Q15: What is the "Self Review Engine" pattern?
**A:** It is a cognitive self-critique loop defined in the skill files that forces the AI assistant to validate its output against constraints before delivering it to the user.

### Q16: How does the Debugging Expert investigate database deadlocks?
**A:** It enforces the collection of actual database lock logs (`pg_locks`) and queries execution stats (`EXPLAIN ANALYZE`), explicitly forbidding speculation.

### Q17: What does the Performance Engineer do to prevent N+1 query patterns?
**A:** It audits API endpoints to ensure list-based data fetches use batch queries instead of iterative loops.

### Q18: What is the deployment strategy of the Deployment Engineer?
**A:** It configures rolling, blue-green, and canary deployments with automated metric-based rollback gates.

### Q19: Why does the Security Engineer activate twice in compliance projects?
**A:** Once in Phase 0 to define compliance constraints, and once in Phase 3 to audit the completed codebase against those constraints.

### Q20: What database does the Database Architect prefer for relational transaction models?
**A:** PostgreSQL 16 due to its transactional safety, ACID compliance, and robust RLS implementation.

---

## Directory & File Structure

### Q21: What is the purpose of the `standards/` folder?
**A:** It houses global style guides, naming conventions, formatting configurations, and engineering rules.

### Q22: Why are standard markdown files capitalized?
**A:** It makes the core skill files (`SKILL.md`, `README.md`, `CHECKLIST.md`, `EXAMPLES.md`) stand out from generic files.

### Q23: What should go in the `templates/` folder?
**A:** General boilerplate files, secure Dockerfiles, pipeline configurations, and base manifests.

### Q24: What is the purpose of the `checklists/` folder?
**A:** It contains repository-wide, domain-agnostic production readiness validation lists.

### Q25: How do I create local file links in markdown?
**A:** Use the absolute `file:///` protocol with forward slashes (e.g., `[label](file:///d:/projects/Nexulyt-AI-OS/skills/software-architect)`).

---

## Tooling & IDE Integration

### Q26: Can I use this repository in Cursor?
**A:** Yes. Open the cloned folder in Cursor and reference the skills under your system prompt or `@` context variables.

### Q27: How does Antigravity IDE use this project?
**A:** Antigravity reads the skills recursively and uses the configurations to shape its pairing output.

### Q28: How do I lint markdown files in this repository?
**A:** We recommend using the "Markdown All in One" extension in VS Code.

### Q29: Does this repository require Python dependencies?
**A:** No core runtime dependencies are required, but Python 3.10+ is recommended for executing validation scripts.

### Q30: How do I resolve Git CRLF warnings on Windows?
**A:** Run `git config --global core.autocrlf true` in your shell.

---

## Advanced Usage

### Q31: How do I add a new skill to the repository?
**A:** Create a folder under `skills/`, add `SKILL.md`, `README.md`, `CHECKLIST.md`, and `EXAMPLES.md`, and register it in the Orchestrator's selection list.

### Q32: What compliance scopes are supported?
**A:** The skills contain frameworks for GDPR, HIPAA, and PCI-DSS compliance.

### Q33: How does the rollback strategy work in Kubernetes?
**A:** The Deployment Engineer configures `kubectl rollout undo` commands tied to automated CloudWatch or Prometheus alerts.

### Q34: What is the difference between liveness and readiness probes?
**A:** Liveness probes detect if a process needs to be restarted; readiness probes control if a process should receive traffic.

### Q35: How does the AI Engineer prevent RAG context leakage?
**A:** By enforcing tenant metadata filtering during the vector retrieval step.

### Q36: How do I configure Nginx for high security?
**A:** Enforce TLS 1.3, configure strict CORS headers, HSTS, and block standard proxy caching on endpoints.

### Q37: Why does the DevOps Engineer prefer OIDC over static keys?
**A:** OIDC uses short-lived, role-based tokens, eliminating the risk of exposed long-lived static credentials.

### Q38: How do I optimize Next.js rendering on Vercel?
**A:** Leverage Incremental Static Regeneration (ISR) and serve static assets through the Vercel Edge CDN.

### Q39: What is the threshold for a container memory exit code 137?
**A:** Exit code 137 indicates the container was terminated by the kernel's Out-Of-Memory (OOM) killer.

### Q40: How do I start contribution updates?
**A:** Fork the repository, create a `feat/` or `fix/` branch, run local validation checks, and open a PR.
