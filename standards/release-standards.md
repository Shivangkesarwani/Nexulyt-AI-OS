# Release Standards

This document establishes the release verification, semantic versioning, git tagging, and changelog requirements for the **Nexulyt-AI-OS** repository.

---

## 1. Semantic Versioning Specification

We adhere to Semantic Versioning (`MAJOR.MINOR.PATCH`):
- **`MAJOR`**: Breaking architectural changes.
- **`MINOR`**: Non-breaking feature additions.
- **`PATCH`**: Non-breaking bug fixes.

---

## 2. Release Validation Gates

No release is permitted to proceed unless the following conditions are met:
- **No Empty Files:** Check that no files are 0-byte size.
- **No Broken Links:** The automated local link checker must run and report zero failures.
- **Commit History Check:** Ensure all commit messages on the branch follow the commit conventions.
- **Code Review Approval:** A maintainer has reviewed the release diff.

---

## 3. Git Tagging & GitHub Releases

- **Annotated Tags:** Releases must be tagged using annotated tags including author and release message.
  ```bash
  git tag -a v1.0.0 -m "Release v1.0.0"
  ```
- **Changelog Sync:** The release notes on GitHub must match the release logs in the changelog file.
