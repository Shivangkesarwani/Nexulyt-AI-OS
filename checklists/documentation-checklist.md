# Documentation Checklist

This document provides a quality gate checklist to validate the completeness, accuracy, links resolution, formatting standards, and structural guidelines of repository documentation in compliance with the [Documentation Standards](file:///d:/projects/Nexulyt-AI-OS/standards/documentation-standards.md) and [Markdown Standards](file:///d:/projects/Nexulyt-AI-OS/standards/markdown-standards.md).

---

## 1. Overview

* **Objective**: Ensure that all repository documentation is technically accurate, consistently formatted, easy to navigate, and contains zero broken links.
* **When to Use**: During content authoring sprint tasks, pre-merge pull request reviews, or during repository-wide auditing checks.
* **Prerequisites**: Codebase files map, documentation guidelines, and markdown configurations.

---

## 2. Checklist Items

### Document Structure & Completeness
* [ ] **Descriptive H1 Header**: Verify each document features a single, clear `#` title.
* [ ] **Target Summaries**: Ensure documentation files open with a concise summary block detailing the page's purpose and scope.
* [ ] **Prerequisites & Setup Contexts**: Onboarding guides and tutorials explicitly define packages and environment parameters needed.
* [ ] **No Placeholder Text**: Verify that documents are free of generic placeholders (e.g. `TODO`, `Insert text here`).

### Formatting & Style Compliance
* [ ] **Markdown Hierarchy**: Headers use descending levels (`#`, `##`, `###`) without skipping steps.
* [ ] **Code Block Syntax**: All code blocks declare explicit syntax highlighters (e.g. ` ```typescript `) to enable visual formatting.
* [ ] **Alert Syntax Conformance**: Strategic alerts (NOTE, TIP, IMPORTANT, WARNING, CAUTION) are correctly formatted using GitHub blockquote syntax.
* [ ] **Line Length and Wrapping**: Ensure lists and bullets remain short and readable.

### Links & Code References
* [ ] **Relative Links Resolution**: Verify all internal links point to active, existing repository paths.
* [ ] **URI Scheme Compliance**: File references match standard URI schemes (`file:///`) using forward slashes.
* [ ] **Symbol Consistency**: Documented class names, database properties, and method calls match active code names.
* [ ] **No Backticks on Links**: Verify that markdown links do not wrap the link text inside backticks, which breaks IDE hyperlink formatting.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Run link-checking utilities on markdown directories to verify all references resolve to valid files.
* Review formatting in a markdown viewer to check readability, alert layouts, and code block styles.
* Audit documented code examples against active APIs to check for documentation drift.

### Exit Criteria
* **No Broken Links**: Markdown link checkers report zero broken relative link targets.
* **Format Compliant**: Markdown files validate against style guidelines.
* **Examples Synchronized**: Code snippets match current implementation structures.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Copying absolute file paths from local environments (e.g. `C:\Users\...`) into repository documentation, causing broken links for other team members.
* Wrapping links in code blocks (e.g., [`README.md`](file:///path/to/readme.md)), which prevents IDEs from parsing the hyperlink context correctly.
* Allowing documentation files to drift from the active codebase after major refactors.

### Professional Recommendations
1. **Automate Link Checking**: Run markdown link validation tools (e.g. markdown-link-check) in CI workflows to automatically reject broken references.
2. **Keep Docs Near Code**: Keep module-specific documentation in markdown files within the same directory as the source code to encourage synchronous updates.
3. **Use WORM Decollators**: Version documentation files alongside project source branches to ensure historical versions remain accessible.
