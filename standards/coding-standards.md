# Coding Standards

This document establishes the official coding standards for code files and configurations within the **Nexulyt-AI-OS** repository.

---

## 1. Overview & Core Principles

The goal of this standard is to ensure that all code in the repository is clean, type-safe, maintainable, and aligned with industry-standard engineering paradigms.

- **KISS (Keep It Simple, Stupid):** Prefer simple, readable logic over complex, clever implementation.
- **DRY (Don't Repeat Yourself):** Consolidate duplicated logic into reusable functions, modules, or services.
- **YAGNI (You Aren't Gonna Need It):** Do not write code for speculative future requirements. Only build what is requested now.
- **SOLID Principles:** Enforce Single Responsibility, Open/Closed, Liskov Substitution, Interface Segregation, and Dependency Inversion.

---

## 2. Code Organization

- **Separate Concerns:** Keep business logic (services), database access (repositories), and routing (controllers) in distinct files.
- **Single Responsibility:** A function or class must do exactly one thing. If a function is longer than 50 lines, it is a candidate for decomposition.
- **Imports Sorting:** Group imports in this order: standard libraries, external dependencies, and local module imports.

---

## 3. Type Safety

- Enforce strict type check configs globally.
- Avoid using `any` or disabling type checks via `ts-ignore` unless explicitly justified in comments.
- Define return types for all functions and service methods.

---

## 4. Error Handling & Logging

- **No Empty Catch Blocks:** All errors must be handled, logged with context, or rethrown.
- **Context-Aware Logging:** Emit logs in structured JSON format including: `timestamp`, `level`, `traceId`, and a descriptive message.
- **Fail Fast:** Validate inputs at the boundary of a function or endpoint and exit early with descriptive error structures.

---

## 5. Comments & Readability

- **Document the "Why", Not the "How":** Clean code should explain *how* it works by variable naming. Use comments to explain the business reason or non-obvious constraints.
- **Docstrings:** Provide JSDoc / Docstring formatting for public API functions and exported modules.

---

## 6. Refactoring Guidelines

- **Leave it Better:** Follow the Boy Scout Rule — always leave code cleaner than you found it.
- **Write Regression Tests First:** Before refactoring any complex function, ensure it has test coverage to verify that behavior remains identical.
- **Automated Formatting:** Run formatter checks (Black for Python, ESLint/Prettier for JS/TS) before finalizing code modifications.
