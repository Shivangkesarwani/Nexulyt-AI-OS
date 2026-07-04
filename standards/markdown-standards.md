# Markdown Standards

This document establishes the formatting rules for Markdown files within the **Nexulyt-AI-OS** repository.

---

## 1. Overview & Principles

Consistent Markdown syntax ensures that all documentation remains clean, legible, and compatible across various editors, browsers, and terminal viewports.

---

## 2. Formatting Rules

### Headings
- Use a single H1 (`#`) per document for the main title.
- Follow logical nesting: H1 (`#`) → H2 (`##`) → H3 (`###`). Do not skip levels.
- Separate headings from surrounding text with blank lines.

### Lists
- Use hyphens (`-`) for unordered lists.
- For checklists, use `- [ ]` (uncompleted) or `- [x]` (completed) with single spaces.

### Tables
- Align table headers and column boundaries cleanly.
- Keep content concise. Avoid massive text lines inside cells.

### Code Blocks
- Always declare the target programming language on the opening block boundary (e.g. ````typescript`, ````python`, ````yaml`).
- Never leave placeholders (`// TODO`) in code examples.

### Mermaid Diagrams
- Use Mermaid blocks (````mermaid`) to illustrate flows, dependencies, or architectures.
- Verify diagrams render correctly without syntax errors.

### Local Link Integrity
- File links must be absolute local paths using the `file:///` protocol and forward slashes:
  - **Correct:** `[Link Text](file:///d:/projects/Nexulyt-AI-OS/skills/software-architect)`
  - **Incorrect:** `[Link Text]` `(./skills/software-architect)` (relative links are not allowed)
- Avoid relative links as they do not resolve consistently across user environments.

### Images & Assets
- Store visual assets under the `assets/` directory in optimized WebP format.
- Link using absolute local references.
