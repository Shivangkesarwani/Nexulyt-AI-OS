# Contributing Guide

Welcome! This guide outlines the standards and workflows for contributing to **Nexulyt-AI-OS**.

---

## 1. Contribution Workflow

All contributions must follow a structured pipeline:
1.  **Open an Issue:** Discuss the proposed change or skill addition before writing code.
2.  **Create a Branch:** Create a branch following naming rules.
3.  **Implement changes:** Respect coding and folder standards.
4.  **Verify local files:** Ensure all links are correct and files are formatted.
5.  **Submit a Pull Request (PR):** Target the `main` branch.

---

## 2. Branch Naming Conventions

Use the following prefixes for branch names:
- **`feat/`**: For new skills or workflows (e.g., `feat/new-designer-skill`).
- **`fix/`**: For fixing typos, broken links, or configuration bugs (e.g., `fix/broken-links`).
- **`docs/`**: For documentation updates (e.g., `docs/faq-update`).
- **`refactor/`**: For cleanups without functional changes (e.g., `refactor/clean-templates`).

---

## 3. Commit Message Standards

We follow the **Conventional Commits** specification:
- **`feat: add software-architect skill`**
- **`fix: resolve syntax error in deployment yaml`**
- **`docs: update quick-start guide`**
- **`style: format markdown headers`**

---

## 4. Code & Documentation Standards

- **No Placeholders:** Never submit files with `// TODO` or empty templates.
- **Strict Linting:** Markdown files must have consistent headers and valid references.
- **Link Integrity:** All file links must be local and reference active workspace targets:
  - **Correct:** `[Software Architect](file:///d:/projects/Nexulyt-AI-OS/skills/software-architect)`
  - **Incorrect:** `[Software Architect]` `(./skills/software-architect)`

---

## 5. PR Review Process

1.  **Checklist validation:** The PR must pass all standard repository checks.
2.  **Code Reviewer Sign-off:** A maintainer will run the `code-reviewer` checklist against your branch diff.
3.  **Merge:** Once approved, the PR is merged into `main`.
