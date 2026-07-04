# Commit Conventions

This document defines the commit message standards for the **Nexulyt-AI-OS** repository.

---

## 1. Conventional Commits Specification

Every commit message must follow this structure:

```text
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Commit Types
- **`feat`**: A new skill, template, workflow, or feature.
- **`fix`**: A bug fix (e.g. fixing broken links, typos).
- **`docs`**: Documentation changes only.
- **`style`**: Changes that do not affect the meaning of the code (white-space, formatting).
- **`refactor`**: A code change that neither fixes a bug nor adds a feature.
- **`perf`**: A code change that improves performance.
- **`test`**: Adding missing tests or correcting existing tests.
- **`chore`**: Updates to build tasks, package manager configs, or auxiliary tools.

---

## 2. Examples

### Adding a new skill
```text
feat(skills): add ui-ux-designer skill configurations

Creates the SKILL.md, README.md, and checklists for the UI/UX Designer agent.
```

### Fixing a link typo
```text
fix(docs): resolve database-architec link references in architecture.md

Renames incorrect links to database-architect to align with the folder name.
```

### Updating changelog during release
```text
docs(changelog): update changelog history for version 1.0.0
```
