# Git Standards

This document defines the official Git, branching, and pull request workflows for the **Nexulyt-AI-OS** repository.

---

## 1. Branching Strategy

We use a feature branch workflow branching off the `main` branch.
- **`main`**: The active stable production-ready branch. Direct commits to `main` are blocked. All updates must go through pull requests.
- **`feat/feature-name`**: Used for new skills, documents, or templates.
- **`fix/bug-name`**: Used for fixing typos, broken links, or syntax errors.
- **`docs/doc-name`**: Used for non-functional documentation updates.
- **`refactor/refactor-name`**: Used for cleaning templates without altering functionality.

---

## 2. Pull Request (PR) Policy

- **Single Concern:** A PR must address exactly one feature or bug. Large, multi-concern PRs will be rejected.
- **Validation check:** A PR must pass all local checks (link validation, syntax audits).
- **Code Review Sign-off:** Requires approval from at least one repository maintainer.
- **Checklist Verification:** The author must confirm they have run the target skill's `CHECKLIST.md` before requesting review.

---

## 3. Merge & Rebasing Strategy

- **Rebase to Resolve Conflicts:** Merge commits are discouraged. Keep history clean by rebasing feature branches against `main`:
  ```bash
  git checkout feat/my-feature
  git fetch origin
  git rebase origin/main
  ```
- **Squash and Merge:** When merging PRs into `main`, squash commits to keep the repository history clean.

---

## 4. Conflict Resolution

When a conflict occurs during a rebase:
1. Identify the conflicting files using `git status`.
2. Open files and resolve markers (`<<<<<<<`, `=======`, `>>>>>>>`).
3. Enforce the **Specialist Priority Matrix** if the conflict involves architecture or security.
4. Stage resolved files:
   ```bash
   git add resolved_file.md
   ```
5. Continue the rebase:
   ```bash
   git rebase --continue
   ```
6. Force-push to your feature branch:
   ```bash
   git push --force-with-lease origin feat/my-feature
   ```
