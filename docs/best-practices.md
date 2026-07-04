# Engineering Best Practices

This document outlines the best practices that all developers and AI assistants must follow when working in the **Nexulyt-AI-OS** ecosystem.

---

## 1. Planning Best Practices
- **Plan First:** Always generate an implementation plan before writing code.
- **Decompose Tasks:** Break complex updates into component-level steps.
- **Lock API Contracts:** Lock the API interfaces after architecture review to allow frontend and backend tasks to run in parallel.

---

## 2. Coding Best Practices
- **Enforce Type Safety:** Write strictly typed code. Avoid untyped variables or `any`.
- **Pin Dependency Versions:** Always specify exact package and runtime versions in dependency files.
- **No Globals:** Do not pollute namespaces. Ensure variables are scoped appropriately.

---

## 3. Review Best Practices
- **Verify Checklists:** Run the target skill's `CHECKLIST.md` before committing.
- **Zero Tolerances for Criticals:** Never merge code with unresolved `[CRITICAL]` or `[MAJOR]` issues.
- **Verify Test Success:** Confirm the test suite runs and passes cleanly on new features.

---

## 4. Deployment Best Practices
- **Never Deploy From Local:** All deployments must originate from a verified CI/CD pipeline.
- **Tested Rollbacks:** Ensure a rollback command is documented and verified in staging before deploying to production.
- **Canary Rollouts:** Shift user traffic incrementally (e.g. 5% → 25% → 100%) while monitoring error logs.

---

## 5. Documentation Best Practices
- **Absolute Local Links:** Use the `file:///` protocol with forward slashes for cross-file links.
- **Visual Flowcharts:** Use Mermaid diagrams to describe workflows and service relations.
- **No Placeholders:** Ensure files are written in full. Do not use `// TODO` or empty templates.

---

## 6. AI Integration Best Practices
- **Secure System Prompts:** Explicitly partition system rules from dynamic user inputs to prevent injection.
- **Limit Token Budgets:** Set strict `max_tokens` boundaries on API calls.
- **Filter Vector Data:** Enforce tenant-context filters during RAG vector retrieval steps.

---

## 7. Security Best Practices
- **Zero Trust:** Enforce parameterized queries for database operations.
- **Run Containers Rootless:** Configure non-root users in container runtimes.
- **Sanitize Outputs:** Contextually escape variable values before displaying them in UI views.

---

## 8. Performance Best Practices
- **Enforce Pagination:** Ensure list-returning endpoints require page parameters.
- **Eliminate N+1 Queries:** Use batch querying instead of executing database calls in loops.
- **Measure Latency:** Record latency baselines under load rather than relying on idle performance.
