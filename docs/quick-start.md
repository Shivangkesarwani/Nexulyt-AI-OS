# Quick Start Guide

Get up and running with **Nexulyt-AI-OS** in under 5 minutes.

---

## 1. Five-Minute Setup

To start building software with Nexulyt-AI-OS:
1. Clone the repository locally:
   ```bash
   git clone https://github.com/Shivangkesarwani/Nexulyt-AI-OS.git
   ```
2. Point your AI agent (Cursor, Windsurf, Claude, or Antigravity) to the `skills/` directory.
3. Ensure the agent has read-access to `skills/full-stack-orchestrator/SKILL.md`.

---

## 2. Bootstrapping Your First Project

When starting a project, execute these steps with your AI assistant:

### Step 1: Initialize the Orchestrator
Send a prompt to your assistant specifying that it must follow the `full-stack-orchestrator` rules:
```text
Role: You are the Full-Stack Orchestrator of Nexulyt-AI-OS.
Request: Design a subscription-based document processing API.
Enforce standard response formats.
```

### Step 2: Review the Generated Plan
The orchestrator will return a plan structured under the required engineering output format:
1. Requirements Analysis
2. Business Analysis
3. Complexity Assessment
4. Required Specialists
5. Execution Order
6. Parallel Tasks
7. Dependencies
8. Risks
9. Quality Gates
10. Development Roadmap
11. Validation
12. Final Engineering Recommendation

### Step 3: Sequential Execution
Let the assistant run through the execution order. It will activate:
- **Software Architect** to design the components.
- **Database Architect** to design database schemas.
- **Backend Engineer** to build APIs.
- **Security Engineer** to run threat assessments.

---

## 3. Common Beginner Mistakes

- **Coding Before Planning:** Modifying or creating source files without first generating the `full-stack-orchestrator` plan.
- **Skipping Quality Gates:** Proceeding to implementation before the Software Architect's schema/API contracts are approved.
- **Using Floating Docker Tags:** Deploying containers using `:latest` or general tags. Always pin versions as specified in `standards/engineering-rules.md`.
