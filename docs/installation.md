# Installation Guide

This document describes the system requirements, environment setups, and configuration steps required to install and run the **Nexulyt-AI-OS** repository.

---

## 1. System Requirements

Ensure your machine satisfies these requirements before continuing:
- **Operating System:** Windows 10/11, macOS 12+, or modern Linux (Ubuntu 20.04+).
- **Git:** Version 2.30.0 or higher.
- **Node.js:** Node.js 18.x or 20.x LTS.
- **Python:** Python 3.10 or higher.
- **IDE:** Visual Studio Code (recommended) or any AI-first development environment (e.g., Cursor, Windsurf).

---

## 2. Git Installation

1. Download Git from the [official website](https://git-scm.com/).
2. Run the installer and configure your user details in terminal:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```
3. Set default branch naming conventions:
   ```bash
   git config --global init.defaultBranch main
   ```

---

## 3. IDE Setup: Visual Studio Code

We recommend using Visual Studio Code or an AI-enabled IDE like Cursor to work with this repository:
1. Install [VS Code](https://code.visualstudio.com/).
2. Install recommended extensions:
   - **Markdown All in One:** For linting and navigating Markdown files.
   - **Docker:** For validating container configurations.
   - **YAML:** For validating Kubernetes manifests and pipeline configurations.
   - **GitLens:** For tracking file change histories and commit references.

---

## 4. Antigravity IDE Integration

If you are using the **Antigravity** agentic assistant:
1. Ensure the workspace path `d:\projects\Nexulyt-AI-OS` is registered in your Antigravity workspaces settings.
2. The agent automatically discovers skills under `skills/` using standard root configurations. You do not need to register directories manually unless they are located outside the workspace.

---

## 5. GitHub Configuration & Cloning

1. Authenticate with GitHub using SSH keys or personal access tokens:
   ```bash
   ssh -T git@github.com
   ```
2. Clone the repository into your local projects directory:
   ```bash
   git clone https://github.com/Shivangkesarwani/Nexulyt-AI-OS.git
   ```
3. Verify directory contents match:
   ```bash
   cd Nexulyt-AI-OS
   ls
   ```

---

## 6. Verification Steps

1. Run the local markdown validation script (if available) or execute a basic syntax check:
   ```bash
   # Check if files exist and links match
   python -c "import os; print('Database architect exists:', os.path.exists('skills/database-architect'))"
   ```
2. Output should print:
   ```text
   Database architect exists: True
   ```

---

## 7. Troubleshooting

- **Permissions Errors:** If Git clone fails with permission errors, verify your SSH key is added to your GitHub account under settings.
- **Line Endings Warning (Windows):** If git prints CRLF warning messages, configure core autocrlf:
  ```bash
  git config --global core.autocrlf true
  ```
