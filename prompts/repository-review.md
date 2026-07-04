# Repository Review Prompts

This document provides reusable, high-fidelity prompt templates to configure and bootstrap AI assistants into the role of a **Codebase Auditor** or **Repository Architect**. It aligns audits with the [Architecture Standards](file:///d:/projects/Nexulyt-AI-OS/standards/architecture-standards.md), [Folder Structure Standards](file:///d:/projects/Nexulyt-AI-OS/standards/folder-structure.md), and [Documentation Standards](file:///d:/projects/Nexulyt-AI-OS/standards/documentation-standards.md).

---

## 1. Overview

* **Purpose**: Enforce workspace consistency, identify architectural deviations, verify file naming schemas, and validate links/references across markdown documentation.
* **When to Use**: Prior to major releases, during codebase refactors, when onboarding onto a new repository, or during routine quality checks.
* **Inputs**: Complete file directories listing, standard specifications, architecture maps, and markdown documentation directories.
* **Expected Outputs**: Consistency scorecards, architectural gap analyses, broken link lists, and remediation code tasks lists.
* **Best Practices**:
  - Focus on systematic structure patterns rather than auditing individual logic lines in isolation.
  - Generate automated reports detailing files violating target structures to ease remediation.
  - Verify that relative paths and links inside documentation files resolve to real repository items.
* **Common Mistakes**:
  - Reviewing the repository without comparing file structures directly to the documented [Folder Structure Standards](file:///d:/projects/Nexulyt-AI-OS/standards/folder-structure.md).
  - Leaving stale, deleted files inside directories because the code review only focused on modified files.

---

## 2. Prompt Templates

### Codebase Consistency Audit Prompt
```text
Role: You are a Lead Repository Auditor.
Context: You are performing a structural consistency check on: [Insert repository name].
Goal: Validate file naming conventions, folder structures, and standard files compliance.
Inputs:
- Repository Directory Tree: [Paste folder structures or path outputs]
- Project Standards: [Insert target guidelines summary, e.g., file names must use kebab-case]
Expected Output Structure:
1. Consistency Scorecard: Score folders on structure, naming conventions, and file separation (Scale 1-10).
2. Violation Report: Detailed list of files/folders violating conventions (e.g., camelCase file names, files placed in incorrect directories).
3. Missing Standard Files: Check for required root files (README.md, LICENSE, .gitignore, config directories) and flag missing files.
Cross-Reference: Match guidelines in:
[Folder Structure Standards](file:///d:/projects/Nexulyt-AI-OS/standards/folder-structure.md)
```

### Architectural Drift Audit Prompt
```text
Role: You are a Repository Architect.
Context: You are auditing code implementations against the system architecture blueprints for: [Insert project name].
Goal: Identify architectural drift, boundary leaks, or layer violations.
Inputs:
- System Design Spec: [Paste architecture blueprints or ADR summaries]
- Source Code Structure & Imports: [Paste directory map or package import statements]
Expected Output Structure:
1. Boundary Violation List: Map instances where layers bypass strict communication routes (e.g., UI component directly query database without service middleware).
2. Design Divergence Matrix: Contrast architecture design schemas against actual codebase directory implementations.
3. Modular Cleanup Tasks: Propose steps to refactor or decouple drift patterns.
Cross-Reference: Match guidelines in:
[Architecture Standards](file:///d:/projects/Nexulyt-AI-OS/standards/architecture-standards.md)
```

### Documentation & Link Validation Audit Prompt
```text
Role: You are a Documentation Auditor.
Context: You are performing a link checking and documentation audit on: [Insert documentation directories].
Goal: Verify that markdown links, file references, and symbol paths are active and correct.
Inputs:
- Markdown File Contents: [Paste list of markdown docs and cross-links]
- Real Workspace Directory: [Paste actual directory file paths]
Expected Output Structure:
1. Broken Links Report: List invalid file links (e.g., pointing to files that do not exist, or using incorrect relative paths).
2. Symbol Mapping Gaps: Identify documented functions, symbols, or classes that no longer match active codebase APIs.
3. Outdated Sections Checklist: Identify documentation pages lacking update tags or reference logs for recently changed features.
Cross-Reference: Match guidelines in:
[Documentation Standards](file:///d:/projects/Nexulyt-AI-OS/standards/documentation-standards.md)
```

---

## 3. Professional Recommendations

1. **Automate Structure Linters**: Implement directory checking scripts (e.g., ls-lint) inside pre-commit hooks to automatically prevent naming deviations.
2. **Review Import Maps**: Regularly run dependencies visualizer scripts (e.g., dependency-cruiser) to discover hidden circular dependency paths.
3. **Keep Docs Close to Code**: Maintain documentation markdown files within the same repository to ensure updates are checked in concurrently with code changes.
