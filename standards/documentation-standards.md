# Documentation Standards

This document establishes the quality, structure, and validation requirements for all technical documentation within the **Nexulyt-AI-OS** repository.

---

## 1. Overview & Core Principles

Documentation is a first-class engineering asset. It must be maintained with the same rigor as runtime code.
- **Accurate & Updated:** Documentation must reflect the current state of the repository. Outdated docs must be refactored or deleted.
- **Comprehensive:** Avoid leaving incomplete sections or placeholders.
- **Traceable:** Link design choices to architectural principles or user constraints.

---

## 2. Document Template Requirements

Every technical guide or reference file must include the following sections:
1.  **Title:** A clean, clear H1 heading.
2.  **Overview:** A blockquote summarizing the document's context and purpose.
3.  **Core Content:** Well-structured sections separating concerns with horizontal rules.
4.  **Reference links:** Clickable local `file:///` links to related guides or standard documents.

---

## 3. Link Verification Policy

- All Markdown links are parsed by verification scripts. A pull request containing a broken link is blocked from merging.
- Verify links are absolute local paths to avoid directory resolution errors on client systems.
- Do not commit example links that execute active calls; escape bracket patterns if you are showing incorrect formats.
