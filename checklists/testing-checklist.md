# Testing Checklist

This document provides a quality gate checklist to validate unit testing coverage, integration testing pathways, End-to-End (E2E) workflows, regression tests, and User Acceptance Testing (UAT) in compliance with the [Testing Standards](file:///d:/projects/Nexulyt-AI-OS/standards/testing-standards.md).

---

## 1. Overview

* **Objective**: Ensure code correctness, catch regression errors before merge, validate user workflows, and maintain target coverage goals.
* **When to Use**: During active coding, pre-pull request submissions, or release staging audits.
* **Prerequisites**: Functional specifications, test datasets, mock environments configurations, and coverage metrics guidelines.

---

## 2. Checklist Items

### Unit Testing Coverage
* [ ] **Target Coverage Gates**: Verify that unit tests cover at least 80% of application logic (services, utility modules).
* [ ] **Isolated Executions**: Enforce complete isolation in unit tests (all database calls, network APIs, and heavy file system operations are mocked).
* [ ] **Assertion Completeness**: Ensure tests validate both success responses and error paths (handling exception throwing logic).
* [ ] **Boundary Conditions**: Test limits (e.g. empty lists, overflow bounds, invalid format strings).

### Integration Testing Pathways
* [ ] **Real Database Connectivity**: Run integration tests against local Docker-backed databases (e.g., PostgreSQL, Qdrant) to verify actual SQL/vector queries.
* [ ] **REST/tRPC Endpoints checks**: Verify route status responses, header parameters, and validate JSON payloads against schema targets.
* [ ] **Clean State Isolation**: Database entries are populated and purged clean before and after every individual test execution to prevent state spillover.

### End-to-End (E2E) & User Workflows
* [ ] **Critical User Path verification**: E2E test suites (e.g. Playwright, Cypress) run complete workflows (e.g., sign up -> choose item -> check out -> verify success).
* [ ] **Axe Accessibility Auditing**: Run automated WCAG checks inside browser tests.
* [ ] **Visual Regressions**: Check for layout offsets or container overlaps on viewport changes using snapshot testing tools.

### Regression & User Acceptance (UAT)
* [ ] **Regression Reproducer tests**: Every bug fix includes a corresponding regression test reproducing the error state (failing without the fix, passing with it).
* [ ] **Gherkin Acceptance Checks**: Verify that final implementations pass UAT checks mapped out in requirements specifications.
* [ ] **Production-Like Validation**: Test applications under production configurations using staging subnets before final approvals.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Run the test suites command locally (`npm run test` or equivalent) to confirm all tests pass successfully.
* Inspect coverage reports to confirm that target coverage thresholds are achieved.
* Verify that test migrations setup and tear down clean databases without errors.

### Exit Criteria
* **All Tests Pass**: Unit, integration, and E2E test suites report 100% success.
* **Coverage Target Achieved**: Code coverage reports meet or exceed the 80% threshold.
* **Zero Regression Violations**: Previously fixed bugs remain resolved under automated checks.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Writing unit tests that execute actual database queries, causing slow test runs and random failures due to shared state.
* Skipping UAT validation checks, leading to successful deployments of features that do not align with user goals.
* Leaving staging environment configurations un-tested, discovering missing env parameters only in production.

### Professional Recommendations
1. **Mock External Networks Always**: Never let automated test suites call live external third-party APIs (e.g. Stripe, SendGrid); use mock wrappers to guarantee test speed and stability.
2. **Fail CI on Test Dropouts**: Configure repository gates to block pull requests if test coverages drop below target percentages or if any individual test fails.
3. **Keep Tests Dry**: Avoid duplicate setup logic; utilize hooks (`beforeEach`, `afterEach`) to keep test code clean and maintainable.
