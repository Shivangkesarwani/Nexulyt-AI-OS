# Folder Structure Reference

This document explains the organization of directories and files in the **Nexulyt-AI-OS** repository.

---

## 1. Directory Structure

The root folder contains the following subdirectories:

```text
Nexulyt-AI-OS/
├── skills/              # Specialized cognitive roles for AI agents
├── prompts/             # System prompts for bootstrapping models
├── templates/           # Configuration files, Dockerfiles, and templates
├── workflows/           # Declarative multi-agent execution pipeline configurations
├── checklists/          # Domain-agnostic production readiness validation sheets
├── standards/           # Engineering guidelines for code, styling, security, and performance
├── docs/                # Comprehensive technical guides and architectural concepts
├── examples/            # Reference implementation files showing correct structures
├── assets/              # Architecture diagrams, visual elements, and graphics
└── README.md            # Public repo guide
```

---

## 2. Directory Explanations

### `skills/`
The core logic of the system. Contains folders for each of the 13 specialized engineering roles (e.g., `software-architect`, `security-engineer`). Each folder contains `SKILL.md`, `README.md`, `CHECKLIST.md`, and `EXAMPLES.md`.

### `docs/`
Houses all markdown-based technical guides, installation walk-throughs, release details, workflows, and FAQs. It serves as the developer portal for understanding the repository.

### `prompts/`
Contains reusable system prompts and context injection templates used to configure raw LLM APIs or CLI scripts into specific engineering roles.

### `templates/`
Boilerplate setups, including multi-stage rootless `Dockerfile` templates, secure `docker-compose.yml` local orchestration parameters, and deployment YAML configs.

### `workflows/`
Contains declarative pipelines (in YAML or JSON) mapping the sequence of skill executions, dependencies, and phase gates for different project scopes.

### `standards/`
Houses the official guidelines for coding styles, naming conventions, directory setups, formatting limits, and general engineering rules.

### `checklists/`
Contains system-wide validation sheets, including general Production Readiness audits and CI/CD promotion gates.

### `examples/`
Contains reference code files and configuration snippets showcasing correct implementations (e.g., parameterized backend queries, sanitization components, security headers).

### `assets/`
Storage for visuals, including architecture diagrams, Mermaid PNG files, banners, and logos.
