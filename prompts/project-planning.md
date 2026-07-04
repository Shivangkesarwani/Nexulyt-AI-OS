# Project Planning Prompts

This document provides reusable, high-fidelity prompt templates to configure and bootstrap AI assistants into the role of a **Product Owner** or **Agile Project Planner**. It aligns project breakdowns and schedules with the [Architecture Standards](file:///d:/projects/Nexulyt-AI-OS/standards/architecture-standards.md) and project workflow models.

---

## 1. Overview

* **Purpose**: Enforce structured requirement capture, dependency mapping, velocity estimation, and task scoping.
* **When to Use**: During product discovery, prior to initiating development phases, during release scheduling, or during sprint planning cycles.
* **Inputs**: Target feature ideas, raw requirements documents, team capabilities, timeline limits, and resource capacities.
* **Expected Outputs**: Structured software requirements specification (SRS), project roadmap timelines, work breakdown structure (WBS), and sprint task estimates.
* **Best Practices**:
  - Break complex user stories down into small, decoupled engineering tasks.
  - Define explicit acceptance criteria (Gherkin style) before starting implementation planning.
  - Factor in developer velocity buffers (typically 20%) to account for code review and testing processes.
* **Common Mistakes**:
  - Sizing tasks without consulting security, testing, or documentation standards.
  - Mapping roadmap dependencies without identifying critical path bottleneck tasks.

---

## 2. Prompt Templates

### Requirement Analysis (User Story to Specs) Prompt
```text
Role: You are a Lead Product Owner.
Context: You are translating a feature idea into an engineering requirement specification: [Insert raw feature description].
Goal: Generate a clear Software Requirements Specification (SRS) with explicit acceptance criteria.
Inputs:
- User Audience: [Insert target user details]
- Core Objective: [Insert e.g., enable customers to apply discount codes to checkout]
Expected Output Structure:
1. Feature Description: Summarize the feature goal from the user's perspective.
2. Functional Requirements: List system capabilities (e.g., validate code exists, apply discount to total, verify expiration date).
3. Acceptance Criteria: Write criteria in Gherkin syntax (Given, When, Then) for success, failure, and expired discount codes.
4. Non-Functional Specifications: List requirements for performance (e.g., validation response < 200ms) and security.
```

### Roadmap & Milestone Generation Prompt
```text
Role: You are a Project Management Lead.
Context: You are planning the milestone timeline for a new product phase: [Insert product details].
Goal: Define development phases, key milestones, and critical dependencies.
Inputs:
- Timeline limit: [Insert e.g., 3 months]
- Team size: [Insert e.g., 3 Developers, 1 QA Engineer]
Expected Output Structure:
1. Phase Breakdown: List sequence of phases (e.g., Discovery, Foundation, Feature MVP, QA, Deploy).
2. Milestones Table: Grid showing Milestone Name, Expected Completion Date, and Deliverables.
3. Dependency Mapping: Highlight blocker tasks and outline the critical path to release.
```

### Architecture Estimation & Planning Prompt
```text
Role: You are a Technical Architect.
Context: You are sizing and planning the implementation of a new system component: [Insert system details].
Goal: Propose a work breakdown structure (WBS) with sequence paths and sizing.
Inputs:
- Component Scope: [Describe component interface, database tables, and external integrations]
Expected Output Structure:
1. Work Breakdown Structure (WBS): Hierarchical list of tasks (e.g., Database migrations, Route handlers, UI Views, Integration tests).
2. Technical Sequence: Chronological execution list highlighting which tasks can be completed in parallel and which require sequential blocking.
3. Sizing Estimations: Provide estimations in story points (Fibonacci sequence: 1, 2, 3, 5, 8) with technical rationale.
```

### Sprint Planning & Calibration Prompt
```text
Role: You are an Agile Scrum Master.
Context: You are organizing a sprint backlog for a 2-week cycle: [Insert target sprint goals].
Goal: Size tasks, calibrate velocity limits, and select backlog items.
Inputs:
- Team Velocity Limit: [Insert e.g., 30 story points]
- Backlog Tasks: [Paste list of pending tasks and current sizes]
Expected Output Structure:
1. Sprint Backlog Selection: Select tasks matching velocity limit, prioritizing blocker tasks.
2. Capacity Calibration: Table showing team member allocations, daily capacity, and out-of-office schedules.
3. Sprint Risk Analysis: Detail potential risks (e.g., complex API integration delay) and define mitigation steps.
```

---

## 3. Professional Recommendations

1. **Use Gherkin Syntax for Testing**: Write acceptance criteria in Given-When-Then format to enable QA engineers to write automated tests.
2. **Track Critical Paths**: Monitor the critical path daily to identify early delays and adjust resource allocation.
3. **Include Validation Checkpoints**: Embed code review and integration testing checkpoints directly into task definitions.
