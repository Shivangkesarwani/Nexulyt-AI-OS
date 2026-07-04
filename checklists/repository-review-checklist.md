# Repository Review Checklist

This document provides a quality gate checklist to audit the entire repository structure, verify directory layouts, check for missing files, and validate cross-references prior to final releases, in alignment with [Folder Structure Standards](file:///d:/projects/Nexulyt-AI-OS/standards/folder-structure.md).

---

## 1. Overview

* **Objective**: Ensure workspace consistency, prevent project drift, verify file naming patterns, and validate all relative documentation links.
* **When to Use**: Prior to major releases, during codebase migrations, or during routine repository health checks.
* **Prerequisites**: Access to the complete repository directory layout, standard rules context, and file list outputs.

---

## 2. Checklist Items

### Directory Layout & File Audits
* [ ] **Folder Structure Compliance**: Verify directories align with [Folder Structure Standards](file:///d:/projects/Nexulyt-AI-OS/standards/folder-structure.md) (no unauthorized root folders).
* [ ] **Required Root Files**: Confirm the presence of root files: `README.md`, `LICENSE`, `.gitignore`, and lockfiles.
* [ ] **File Naming Schema**: Ensure all files conform to naming patterns (e.g. kebab-case for code, config formats matching runtimes).
* [ ] **No Git Leakage**: Verify `.gitignore` excludes node_modules, temp files, local `.env` values, and IDE caches.

### Documentation & Link Integrity
* [ ] **Absolute Link Scans**: Check that documentation does not contain hardcoded local machine paths (e.g. `C:\Users\www\projects\...`).
* [ ] **Relative Links Check**: Audit all relative links in `.md` files to confirm they point to existing targets.
* [ ] **Active Code References**: Verify that documented classes, types, or configuration settings align with current implementations.
* [ ] **No Backticks on Links**: Verify that markdown links do not wrap the link text inside backticks, which breaks IDE hyperlink formatting.

### Standards & Skills Syncing
* [ ] **Standards Mapping**: Verify directories (e.g. `standards/`, `skills/`, `prompts/`, `workflows/`, `examples/`) align with the repository map.
* [ ] **ADR Status Verification**: Ensure all proposed Architectural Decision Records (ADRs) have been updated to `Approved` or `Deprecated`.
* [ ] **Skills Schema checks**: Verify that workspace skills carry valid YAML frontmatter instructions.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Run automated workspace linters to verify file and folder structures.
* Run relative link checking tools across all directories to confirm zero broken links.
* Check that git status reports zero untracked runtime logs or cache folders.

### Exit Criteria
* **Folder Consistency Verified**: Workspace naming conventions show zero deviations.
* **Documentation Links Active**: All markdown relative references resolve correctly.
* **Git Status Clean**: Build artifacts, logs, and caches are excluded from source control.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Leaving stale, unused code files or deleted configs in repository directories.
* Uploading untracked debug logs or local test databases due to incomplete `.gitignore` rules.
* Allowing documentation paths to break during structural file movements.

### Professional Recommendations
1. **Automate Repository Linters**: Configure pre-commit rules or CI workflows to block merges if file naming patterns or folder structures violate guidelines.
2. **Conduct Regular Audits**: Schedule routine repository-wide reviews to clean up unused code and prune outdated documentation.
3. **Verify Links in CI**: Include markdown link-checking tools in CI pipelines to automatically reject broken relative references.
