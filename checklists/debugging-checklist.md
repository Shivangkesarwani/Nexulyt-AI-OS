# Debugging Checklist

This document provides a quality gate checklist to guide bug investigation, root cause analysis (RCA), log parsing, and resolution verification in alignment with repository debugging workflows.

---

## 1. Overview

* **Objective**: Enforce systematic bug isolation, find the logical root cause, prevent regression bugs, and document corrective actions.
* **When to Use**: When troubleshooting runtime errors, analyzing crash logs, resolving performance issues, or writing post-incident reports.
* **Prerequisites**: Error stack traces, system logs, environment variables context, and user reproduction details.

---

## 2. Checklist Items

### Bug Isolation & Reproduction
* [ ] **Error Details Capture**: Extract main error logs, error messages, and identify the throwing files/lines.
* [ ] **Deterministic Verification**: Determine if the bug is consistently reproducible (deterministic) or random (non-deterministic).
* [ ] **Minimal Reproducible Example (MRE)**: Build a minimal, isolated test script replicating the bug state.
* [ ] **Environment Mapping**: Check if the bug is specific to particular browsers, node runtimes, database loads, or operating systems.

### Root Cause Analysis (5 Whys)
* [ ] **RCA Timeline Construction**: Map out a chronological timeline of events leading up to the failure event.
* [ ] **Five Whys Analysis**: Ask "Why?" sequentially to trace from the immediate trigger down to the systemic root cause.
* [ ] **State Drift Analysis**: Identify where the database or application memory drift occurred (e.g. race conditions, unhandled null values).
* [ ] **Incident Impact Assessment**: Measure data corruption scales, user downtime durations, and security risks.

### Resolution Quality & Verification
* [ ] **Automated Regression Test**: Write an automated test replicating the bug state, confirming it fails without the fix and passes with it.
* [ ] **Minimal Risk Diff**: Implement the simplest code fix that addresses the root cause, avoiding large, unrelated code edits.
* [ ] **Rollback and Recovery**: Define rollback instructions and validation steps for the hotfix deployment.
* [ ] **Documentation Updates**: Document the bug context in the troubleshooting guides or FAQs.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Run the regression test locally, verifying it fails on the old code and passes on the modified code.
* Verify that the fix does not break existing unit and integration test suites.
* Inspect application logs to confirm the fix resolves the target issue.

### Exit Criteria
* **Regression Test Committed**: Automated test added to the test suite.
* **RCA Report Completed**: Technical post-mortem documented for high-priority incidents.
* **Production Hotfix Deployed**: Target code successfully updated and verified.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Writing quick hotfixes that mask symptoms (e.g., catching exceptions without resolving the core issue) instead of fixing the root cause.
* Making multiple unsystematic changes concurrently, making it difficult to identify the actual fix.
* Deploying emergency hotfixes directly to production without running automated regression test checks.

### Professional Recommendations
1. **Always Write a Regression Test**: Enforce a rule that no bug ticket is closed without a regression test in the codebase to prevent future regressions.
2. **Log Systematically**: If a bug is non-reproducible, add structured debug logging to the target file to capture runtime diagnostics.
3. **Trace Dependencies**: Check recent dependency updates, deployment logs, or system changes when debugging sudden environment issues.
