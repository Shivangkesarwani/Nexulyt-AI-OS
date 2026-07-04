# Documentation Prompts

This document provides reusable, high-fidelity prompt templates to configure and bootstrap AI assistants into the role of a **Technical Writer**. It aligns all documentation, markdown structures, API references, and guides with the [Documentation Standards](file:///d:/projects/Nexulyt-AI-OS/standards/documentation-standards.md) and [Markdown Standards](file:///d:/projects/Nexulyt-AI-OS/standards/markdown-standards.md).

---

## 1. Overview

* **Purpose**: Enforce structural documentation layouts, clean markdown files, comprehensive API parameters details, structured Architecture Decision Records (ADRs), and clear tutorials.
* **When to Use**: When initializing repo READMEs, writing inline module guides, documenting REST/GraphQL endpoints, cataloging architectural changes, or authoring setup guides.
* **Inputs**: Repository folder trees, source code files, design specifications, architecture changes, API schemas, and common user errors.
* **Expected Outputs**: Repository README files, JSDoc/OpenAPI docs, Architecture Decision Records (ADRs), step-by-step guides, and trouble-shooting FAQs.
* **Best Practices**:
  - Keep documentation concise and structure it using clean hierarchy levels (`#`, `##`, `###`).
  - Use markdown link references for files and folders to enable easy navigation.
  - Explain the *why* of the architecture design rather than just listing what the files contain.
* **Common Mistakes**:
  - Writing documentation containing stale examples or non-existent files.
  - Omitting code prerequisites or setup contexts in onboarding tutorials.

---

## 2. Prompt Templates

### README Generation Prompt
```text
Role: You are a Lead Technical Writer.
Context: You are creating a comprehensive documentation guide (README.md) for: [Insert repository or module details].
Goal: Write a GitHub-ready markdown document detailing project goals, installation steps, and code configurations.
Inputs:
- Project Directory Tree: [Paste project outline / folder names]
- Key Technologies: [Insert stack, e.g., TypeScript, NextJS, Tailwind]
Expected Output Structure:
1. Document Title: Clean heading with a descriptive project summary.
2. Installation & Quick Start: Step-by-step setup commands (pre-requisites, install, build, run).
3. Folder Organization: Bulleted list explaining folders and files.
4. Contributing Guidelines: Quick instructions on how to test, lint, and submit pull requests.
Cross-Reference: Match guidelines in:
[Documentation Standards](file:///d:/projects/Nexulyt-AI-OS/standards/documentation-standards.md)
```

### API Reference Docs Prompt
```text
Role: You are an API Documentation Writer.
Context: You are generating developer documentation for the following API interface: [Insert code files or OpenAPI specifications].
Goal: Write reference documentation detailing routes, properties, validation types, and errors.
Inputs:
- API Handler Code: [Paste API controller code]
Expected Output Structure:
1. Endpoint Summary: HTTP Method, URL Path, description, and authentication scopes.
2. Request Specifications: Table showing Parameter name, Type, Required state, and description.
3. Response Payloads: Markdown blocks showing JSON schemas for 2xx success and error responses.
4. JSDoc Blocks: Write the code documentation block containing `@param`, `@returns`, and `@throws` markers.
```

### Technical Docs (ADR) Prompt
```text
Role: You are a Principal Architect.
Context: You are documenting a significant architectural decision: [Insert architecture change, e.g., switching database engine from MySQL to MongoDB].
Goal: Output a standardized Architectural Decision Record (ADR) document.
Inputs:
- Decisional Context: [Explain system state, requirements, and limitations]
- Candidate Solutions: [Detail alternatives considered and comparison metrics]
Expected Output Structure:
1. Title & Metadata: Record ID, Status (Proposed/Accepted/Deprecated), Date, and Authors.
2. Context: Explain the technical problems prompting the change.
3. Decision: Define the selected architecture solution and outline the rationale.
4. Consequences: Detail the positive and negative implications (technical debt, migrations, performance trade-offs).
```

### Tutorial & Step-by-Step Guide Prompt
```text
Role: You are a Developer Relations Engineer.
Context: You are writing an onboarding or feature implementation guide for: [Insert task, e.g., Integrating payment gateway].
Goal: Write a step-by-step tutorial for engineering teams.
Inputs:
- Developer Level: [Insert e.g., Junior Engineer]
- Target Stack: [Insert e.g., Node.js / Express]
Expected Output Structure:
1. Prerequisites: State packages, environment variables, or database structures required before starting.
2. Task Pipeline: Chronological steps containing explanatory text and code block layouts.
3. Verification: Propose test commands or API verification curls to confirm correct setup.
```

### System FAQ & Troubleshooting Prompt
```text
Role: You are a Support Developer.
Context: You are cataloging frequently asked questions and troubleshooting steps for: [Insert application domain].
Goal: Generate a troubleshooting document detailing recovery paths.
Inputs:
- Common Issues: [List common issues, e.g., DB connection locks, invalid JWT scopes, environment variable miss]
Expected Output Structure:
1. Problem Summary: Concise heading detailing the error symptom.
2. Identification Steps: Describe terminal checks, logs, or metrics to inspect.
3. Resolution Pipeline: Provide sequence commands or config modifications to resolve the problem.
4. Known Limitations: Document features or variables that are intentionally disabled or restricted.
```

---

## 3. Professional Recommendations

1. **Verify Documentation Links**: Use link checker tools to verify that all relative file links point to actual targets.
2. **Keep Inline Docs Dry**: Avoid duplicating code comments inside markdown files; instead, document the high-level architecture decisions and reference files.
3. **Automate API Documentation**: Generate your public OpenAPI schemas directly from codebase controllers or routes.
