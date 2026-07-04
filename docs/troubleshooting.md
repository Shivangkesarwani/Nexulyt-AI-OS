# Troubleshooting Guide

This document describes common issues and solutions encountered when using the **Nexulyt-AI-OS** repository.

---

## 1. Git & GitHub Issues

### Issue: Permission Denied (publickey) on Clone
- **Symptom:** Git clone fails with: `Permission denied (publickey). fatal: Could not read from remote repository.`
- **Solution:** Verify your SSH key is added to GitHub. Run:
  ```bash
  ssh -T git@github.com
  ```
  If this fails, generate a new key and add it to your profile.

### Issue: CRLF Line Endings Warning on Windows
- **Symptom:** Git prints warnings regarding CRLF line endings.
- **Solution:** Enforce checkout conversions:
  ```bash
  git config --global core.autocrlf true
  ```

---

## 2. Editor & IDE Issues (VS Code / Antigravity)

### Issue: Markdown Links Not Resolving in Editor
- **Symptom:** Clicking `file:///` links in VS Code does not open the file.
- **Solution:** VS Code handles absolute paths relative to your local filesystem. Ensure your workspace folder is mapped to `d:\projects\Nexulyt-AI-OS`.
- **Note:** Avoid using relative links (e.g. `../`) as they do not resolve consistently across different editor configurations.

### Issue: Antigravity IDE Skills Auto-Discovery Fails
- **Symptom:** The Antigravity agent cannot find a newly added skill directory.
- **Solution:** Verify that the skill folder name is in `kebab-case` and contains the mandatory files (`SKILL.md`, `README.md`, `CHECKLIST.md`, `EXAMPLES.md`).

---

## 3. AI Model Issues (Claude / Gemini)

### Issue: Assistant Ignores Skill Guidelines
- **Symptom:** The AI assistant writes code without planning or ignores security rules.
- **Solution:** Explicitly prompt the assistant to load the skill at the start of your conversation:
  ```text
  Context: Read skills/software-architect/SKILL.md before answering.
  ```

### Issue: Model Output Truncated
- **Symptom:** Large generated codebase files stop printing mid-operation.
- **Solution:** Instruct the model to yield execution and output in blocks, or partition the task into distinct components.

---

## 4. Skills & Repository Issues

### Issue: Broken Link Verification Error
- **Symptom:** Verification scripts fail on absolute directory paths.
- **Solution:** Check the spelling of directories.
- **Note:** The database architect skill must refer to the directory `skills/database-architect/` (with a "t" at the end).
