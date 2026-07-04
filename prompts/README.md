# Nexulyt-AI-OS Prompt Library

Welcome to the **Nexulyt-AI-OS Prompt Library**. This directory contains a production-grade, highly structured repository of reusable prompts designed to align and configure AI coding assistants (such as Claude Code, Cursor, Windsurf, ChatGPT, and GitHub Copilot) with the engineering standards, architecture designs, and workflows of the Nexulyt-AI-OS project.

---

## 1. Purpose

The purpose of this prompt library is to eliminate ad-hoc, inconsistent interactions with AI assistants and instead enforce structured, high-fidelity engineering behaviors. By using these prompts, developers can:
1. **Enforce Compliance**: Automatically align AI code generation with repository-specific standards (e.g., [Coding Standards](file:///d:/projects/Nexulyt-AI-OS/standards/coding-standards.md), [Security Standards](file:///d:/projects/Nexulyt-AI-OS/standards/security-standards.md)).
2. **Accelerate Bootstrapping**: Transition raw AI agents instantly into specialized software engineering roles (e.g., Database Architect, Security Expert, Frontend Engineer).
3. **Enhance Quality**: Maximize code correctness, accessibility compliance, and performance optimization through rigorous formatting constraints and validation checks.

---

## 2. Prompt Engineering Philosophy

Our prompt design follows three key principles:
* **Role-Based Framing**: Every prompt starts by explicitly establishing a professional persona (e.g., Principal Software Architect, DevSecOps Engineer) to set appropriate context and response standards.
* **Context Injection**: Prompts are designed to receive explicit, real-world inputs (such as codebases, logs, database schemas, or specifications) to ensure they operate on ground truth, not assumptions.
* **Strict Constraints & Output Schemas**: We eliminate LLM hallucinations and low-grade boilerplate by specifying strict formatting structures (JSON, YAML, Mermaid.js, structured Markdown) and rules of engagement (e.g., "Do not write implementation code during architectural planning").

---

## 3. How to Use the Prompts

To use a prompt from this library:
1. **Select the category** matching your current task.
2. **Copy the desired template** from the corresponding markdown file.
3. **Replace the placeholder variables** denoted in square brackets (e.g., `[Insert resource details]` or `[Paste code here]`).
4. **Execute the prompt** within your AI interface or configure it inside your agent's system profile (such as `.cursorrules` or Claude's custom instructions).

---

## 4. Integration with Skills

The prompts in this library are designed to work in synergy with the workspace skills located in the [skills/](file:///d:/projects/Nexulyt-AI-OS/skills/) folder.
* **Prompt Templates** act as the **user-facing interfaces or agent starter scripts** to kick off tasks.
* **Skills (e.g., [software-architect](file:///d:/projects/Nexulyt-AI-OS/skills/software-architect/SKILL.md))** provide the **underlying capabilities, constraints, and execution guides** that the agent carries out autonomously.

---

## 5. Directory Map

Explore the specialized prompts in the following files:

* **[Software Architecture Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/software-architecture.md)**: System design, folder organization, database selection, API design, scalability planning, and architecture reviews.
* **[UI/UX Design Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/ui-ux-design.md)**: User flows, design systems, visual audits, wireframe layouts, accessibility, mobile-first design, and dashboards.
* **[Frontend Development Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/frontend-development.md)**: Component architectures, Next.js routing, React hooks, Tailwind styles, responsive design, animations, and SEO.
* **[Backend Development Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/backend-development.md)**: REST handlers, JWT auth, RBAC authorization, microservice events, structured logging, validation schemas, and global error handling.
* **[Database Design Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/database-design.md)**: Relational schema plans, ER diagrams, query optimizations, NoSQL mapping, indexing, and scaling partitions.
* **[AI Engineering Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/ai-engineering.md)**: Meta-prompts, semantic chunking, multi-agent coordination, Model Context Protocol (MCP) server definitions, and context window optimizations.
* **[Debugging Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/debugging.md)**: Isolating bug patterns, Root Cause Analysis, tracing logs, hotfixing, memory leak inspection, and rendering performance audits.
* **[Code Review Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/code-review.md)**: Classifying issue severities, linting, pattern alignment, and pre-merge checklist evaluations.
* **[Deployment Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/deployment.md)**: Dockerfiles, Kubernetes resources, CI/CD pipelines, IaC setups, rollbacks, and metrics.
* **[Security Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/security.md)**: STRIDE modeling, secure inputs, OWASP compliance, token configurations, and AI protection.
* **[Performance Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/performance.md)**: CPU profiling, Core Web Vitals, index tuning, response compression, and token budget optimizations.
* **[Documentation Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/documentation.md)**: Technical specifications, API reference models, walkthrough logs, tutorials, and system FAQs.
* **[Project Planning Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/project-planning.md)**: Requirement mapping, milestones, technical sizing, risk registers, and sprint planning.
* **[Prompt Writing Guide](file:///d:/projects/Nexulyt-AI-OS/prompts/prompt-writing-guide.md)**: Detailed framework and methods to create custom production-grade prompts.
* **[Repository Review Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/repository-review.md)**: Full codebase consistency checks, architecture drift detection, and workspace audits.
* **[Workflow Generation Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/workflow-generation.md)**: Designing automated workflows, checklist structures, and feedback loops.
* **[Template Generation Prompts](file:///d:/projects/Nexulyt-AI-OS/prompts/template-generation.md)**: Bootstrapping skeleton code, boilerplate components, and folder structures.

---

## 6. General Best Practices

1. **Be Specific**: Always supply exact technical stack details, version numbers, and file contents when using prompts.
2. **Validate Outputs**: Cross-check AI-generated code and designs against the [Standards Directory](file:///d:/projects/Nexulyt-AI-OS/standards/README.md).
3. **Iterate Incrementally**: When executing multi-step architecture changes, break down the prompts into sequential turns rather than expecting a single large output.
