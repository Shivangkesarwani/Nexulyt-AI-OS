# Naming Conventions

Consistent naming conventions are critical for making codebases readable and search operations predictable. All files, folders, code structures, and database entities must adhere to these guidelines.

---

## 1. Directory & File Naming

- **Directories:** Always use `kebab-case` (e.g., `devops-engineer`, `full-stack-orchestrator`).
- **Standard Markdown Files:** Always use uppercase for standard files (e.g., `README.md`, `SKILL.md`, `CHECKLIST.md`, `EXAMPLES.md`).
- **Code Files:**
  - TypeScript/JavaScript: `camelCase` for utilities (`authHelper.ts`), `PascalCase` for React components/classes (`DashboardCard.tsx`).
  - Python: `snake_case` for files and modules (`db_adapter.py`).

---

## 2. Code Structures

- **Classes / Types / Interfaces:** Always use `PascalCase` (e.g., `class DatabaseConnection`, `interface TenantSettings`).
- **Variables / Functions / Properties:** Always use `camelCase` (e.g., `const userEmail`, `function computePayload()`).
- **Constants:** Always use `UPPER_SNAKE_CASE` (e.g., `const MAX_RETRY_LIMIT = 5`).
- **Python Variables & Functions:** Always use `snake_case` (e.g., `def fetch_user_record()`).

---

## 3. Database Entities

- **Tables:** Always use lowercase plural `snake_case` (e.g., `tenants`, `workspace_members`).
- **Columns:** Always use lowercase singular `snake_case` (e.g., `tenant_id`, `created_at`).
- **Foreign Keys:** Use `table_singular_id` (e.g., `tenant_id` referencing `tenants.id`).
- **Indexes:** Prefix indexes with table and column details (e.g., `idx_workspaces_tenant_id`).

---

## 4. Ingress & API Endpoints

- **Paths:** Always use lowercase `kebab-case` for URL paths (e.g., `/api/v1/user-profiles`).
- **Query Parameters:** Always use `camelCase` (e.g., `/api/v1/search?userId=123&pageLimit=10`).

---

## 5. Git & Configuration

- **Environment Variables:** Always use `UPPER_SNAKE_CASE` (e.g., `DATABASE_URL`, `JWT_SECRET`).
- **Git Branches:** Follow standard prefixes:
  - `feat/feature-name`
  - `fix/bug-fix`
  - `docs/doc-update`
  - `refactor/code-cleanup`
