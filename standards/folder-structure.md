# Folder Structure Standards

All features, skills, templates, and configurations within the **Nexulyt-AI-OS** repository must follow a standardized directory layout. This document defines the structural patterns that must be maintained.

---

## 1. Skill Structure

Each specialized AI skill resides under `skills/<skill-name>/` and must contain the following core files:

```text
skills/<skill-name>/
├── SKILL.md          # Core AI Identity, cognitive paradigm, and rules
├── README.md         # Public overview, workflow, and dependencies
├── CHECKLIST.md      # Domain-specific validation checklists
└── EXAMPLES.md       # High-density, real-world examples (no source code)
```

### Naming Conventions for Skills
- Skill directories must be in `kebab-case` (e.g., `full-stack-orchestrator`, `software-architect`).
- Standard file names (`SKILL.md`, `README.md`, `CHECKLIST.md`, `EXAMPLES.md`) must be in uppercase.

---

## 2. Standard Repository Directories

The root of the repository is structured as follows:

```text
Nexulyt-AI-OS/
├── skills/              # Cognitive definitions and domain logic for each agent
├── prompts/             # System prompts for bootstrapping LLMs into roles
├── templates/           # Configuration files, Dockerfiles, and templates
├── workflows/           # Declarative multi-agent execution pipeline configurations
├── checklists/          # Domain-agnostic production readiness validation sheets
├── standards/           # Engineering guidelines for code, styling, security, and performance
├── docs/                # Comprehensive technical guides and architectural concepts
├── examples/            # Reference implementation files showing correct structures
├── assets/              # Architecture diagrams, visual elements, and graphics
├── LICENSE              # Repository license configuration
└── README.md            # Repository landing page and onboarding documentation
```

---

## 3. Custom Application Boilerplate Structure

When generating application templates under `templates/` or creating examples in `examples/`, follow the standard microservices-ready project layout:

```text
app-template/
├── src/
│   ├── components/      # Shared presentational UI elements
│   ├── services/        # Business logic and external API integrations
│   ├── database/        # Migrations, schemas, and seeding configs
│   ├── routes/          # API endpoint route declarations
│   ├── utils/           # Shared helper functions
│   └── index.ts         # Application entry point
├── tests/               # Unit, integration, and E2E test suites
├── Dockerfile           # Multi-stage, non-root runner build specification
├── docker-compose.yml   # Multi-service local orchestrator configurations
├── .dockerignore        # Context exclusion guidelines
├── .env.example         # Blank template for required environment variables
└── package.json         # Dependency configuration files with pinned versions
```
