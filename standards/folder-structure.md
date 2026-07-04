# Folder Structure Standards

This document establishes the repository structure conventions and defines the rules and purposes of every top-level folder within the **Nexulyt-AI-OS** repository.

---

## 1. Directory Structure

The root directory is structured as follows:

```text
Nexulyt-AI-OS/
├── skills/              # The "brains" — cognitive definitions for each agent
├── prompts/             # System prompts for bootstrapping LLMs into roles
├── templates/           # Configuration files, Dockerfiles, and templates
├── workflows/           # Declarative multi-agent execution pipeline configurations
├── checklists/          # Domain-agnostic production readiness validation sheets
├── standards/           # Engineering guidelines for code, styling, security, and performance
├── docs/                # Comprehensive technical guides and architectural concepts
├── examples/            # Reference implementation files showing correct structures
├── assets/              # Architecture diagrams, visual elements, and graphics
└── README.md            # Repository landing page and onboarding documentation
```

---

## 2. Directory Explanations

### `skills/`
- **Purpose:** The core cognitive assets of the repository. Every folder represents a specialized AI agent (e.g. `database-architect`, `security-engineer`).
- **Rules:** Every folder must be named in `kebab-case` and contain exactly `SKILL.md`, `README.md`, `CHECKLIST.md`, and `EXAMPLES.md`.

### `docs/`
- **Purpose:** Central technical documentation library for human developers and system integrators.
- **Rules:** Files must cover setup instructions, release details, architectures, guides, and troubleshooting steps.

### `prompts/`
- **Purpose:** Houses static system prompt templates and runtime context injection instructions to configure raw APIs.
- **Rules:** Formatted as modular markdown files separating roles, actions, and safety boundaries.

### `templates/`
- **Purpose:** Holds configuration templates (Dockerfiles, kubernetes manifests, yaml workflows) that can be easily injected.
- **Rules:** Must use pinned versions and follow security-hardened patterns.

### `workflows/`
- **Purpose:** Declarative orchestrator pipeline configurations.
- **Rules:** Formats execution sequences and phase gates.

### `standards/`
- **Purpose:** Defines the rules for variables, files, folders, commit messages, and coding practices.
- **Rules:** Changes to standards require an ADR and validation.

### `checklists/`
- **Purpose:** Domain-agnostic checklists for validating production readiness.
- **Rules:** Checklist items must be structured with standard markdown check indicators (`- [ ]`).

### `examples/`
- **Purpose:** Reference implementations.
- **Rules:** Code snippets must be complete, tested, and follow all standards.

### `assets/`
- **Purpose:** Media folder for diagrams, pictures, and graphics.
- **Rules:** Do not commit raw heavy assets. Use optimized WebP formats for visuals.
