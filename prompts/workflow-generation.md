# Workflow Generation Prompts

This document provides reusable, high-fidelity prompt templates to configure and bootstrap AI assistants into the role of a **Workflow Architect** or **Platform Engineer**. It aligns workflow creations with the [Documentation Standards](file:///d:/projects/Nexulyt-AI-OS/standards/documentation-standards.md) and repository workflow files.

---

## 1. Overview

* **Purpose**: Enforce clean sequence pathways, explicit quality gates, human-in-the-loop authorization points, and feedback structures across developer and operational pipelines.
* **When to Use**: When outlining new continuous delivery flows, creating incident recovery workflows, establishing testing lifecycles, or designing developer onboarding paths.
* **Inputs**: Target tasks, runtime agents, available automation tools, compliance guidelines, and team structures.
* **Expected Outputs**: Sequential workflow pipelines (YAML/Markdown), visual flow diagrams (Mermaid.js), validation checklist triggers, and post-incident review guides.
* **Best Practices**:
  - Incorporate explicit, quantifiable entry and exit checkpoints for every workflow stage.
  - Detail precisely where human oversight is required (e.g., prod deployments, schema drops) vs what can be automatically executed by AI agents or scripts.
  - Structure workflow steps chronologically using nested list formats to avoid execution confusion.
* **Common Mistakes**:
  - Outlining ambiguous pipelines without defining specific error recovery options or fallback pathways.
  - Omitting verification commands at intermediate validation checkpoints.

---

## 2. Prompt Templates

### Custom Engineering Workflow Prompt
```text
Role: You are a Workflow Architect.
Context: You are designing an engineering execution workflow for: [Insert task, e.g., Minor Patch Release].
Goal: Define a step-by-step pipeline containing sequence rules, tools, and execution checkpoints.
Inputs:
- Task Target: [Describe task goal]
- Stakeholders Involved: [Insert e.g., Developers, QA, Release Manager]
Expected Output Structure:
1. Workflow Title & Summary: Clear statement of the workflow scope.
2. Step-by-Step Pipeline: Chronological lists of operations detailing who performs the step, what commands/tools are run, and what is validated.
3. Entry & Exit Checkpoints: Table defining parameters required before starting the stage and requirements before proceeding.
```

### Pipeline Sequencing & Automation Prompt
```text
Role: You are a Platform Automation Engineer.
Context: You are mapping manual operations into an automated system-driven sequence: [Insert manual sequence steps].
Goal: Output an automation script blueprint or CI YAML workflow map.
Inputs:
- Target Engine: [Insert e.g., GitHub Actions, local runner scripts]
- Automation Tools: [List CLI tools, e.g., git, docker, npm, snyk]
Expected Output Structure:
1. Automation Sequence Map: Mermaid.js sequence diagram showing parallel vs sequential operations.
2. Script/Configuration Blueprint: Structured YAML or Bash script outline mapping task executions, conditional runs (e.g., "only run if tests pass"), and fail paths.
3. System Logs Definition: Specify what details are logged at each transition state to facilitate debugging.
```

### Human-in-the-Loop Approval Gates Prompt
```text
Role: You are a Compliance & Process Architect.
Context: You are establishing security and quality check gates in a developer workflow: [Insert target workflow, e.g., Database Schema Migrations].
Goal: Define the verification checklist, reviewer requirements, and override rules.
Inputs:
- High-Risk Actions: [Describe actions requiring approval, e.g., deleting user records]
- Target Gatekeeper: [Insert e.g., Tech Lead, Security Officer]
Expected Output Structure:
1. Approval Gates Map: Outline at what steps automation pauses and requests manual review.
2. Verification Checklist: Concrete list of checks the gatekeeper must verify (e.g., backup verified, backup script rollback tested).
3. Override & Bypass Rules: Detail emergency bypass paths, documenting escalation pathways and mandatory post-action audit logging.
```

### Feedback Loop & Incident Post-Mortem Prompt
```text
Role: You are an Operations Process Expert.
Context: You are designing a retrospective review and incident post-mortem workflow for: [Insert incident category, e.g., Database connection outages].
Goal: Establish feedback pathways to update workflows and prevent recurrence.
Inputs:
- Failure Event Profile: [Describe typical failure symptoms and downstream impacts]
Expected Output Structure:
1. Incident Timeline Mapping: Instructions on how to build a minute-by-minute timeline of discovery, diagnostics, mitigation, and recovery.
2. Corrective Loop Trigger: Steps to analyze why existing workflow gates failed to prevent the incident.
3. Document Update Protocol: Define process to update related manuals, system prompts, or automated lint rule folders based on findings.
```

---

## 3. Professional Recommendations

1. **Verify Workflow Executability**: Run a manual dry-run of any new workflow using dummy files to identify process bottlenecks or missing pre-requisites.
2. **Minimize Manual Steps**: Aim to automate repetitive verification checks (e.g., linting, testing, formatting) before prompting for human reviews.
3. **Store Workflows as Code**: Maintain all process descriptions in version-controlled markdown files (like those in `workflows/`) to track adjustments over time.
