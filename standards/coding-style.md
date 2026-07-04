# Coding Style Standards

This document establishes the official coding standards for code files and configurations within the **Nexulyt-AI-OS** repository.

---

## 1. General Principles

- **Readability Over Shortness:** Code must be clear, descriptive, and self-documenting.
- **Type Safety:** Always enforce strict typing. Avoid using `any` or untyped parameters.
- **No Unused Imports/Variables:** Keep code clean of dead segments.
- **Strict Linting:** All code must pass automated linting checks (ESLint for TypeScript, Flake8/Black for Python).

---

## 2. TypeScript / JavaScript Standards

- **Strict Type Checking:** Configure `"strict": true` in `tsconfig.json`.
- **Use Interfaces for Shapes:** Prefer `interface` over `type` for object models.
- **Explicit Returns:** Define explicit return types on all functions.
- **Async/Await Correctness:** Always `await` async operations; avoid unhandled promises.
- **No Globals:** Do not pollute the global namespace.

```typescript
// Correct
export interface UserWorkspace {
  id: string;
  tenantId: string;
  name: string;
}

export async function getWorkspace(workspaceId: string): Promise<UserWorkspace> {
  const result = await db.query<UserWorkspace>("SELECT id, tenant_id, name FROM workspaces WHERE id = $1", [workspaceId]);
  if (!result) {
    throw new Error(`Workspace with id ${workspaceId} not found`);
  }
  return result;
}
```

---

## 3. Python Standards

- **Type Hinting:** Use type hints for all function arguments and return variables.
- **Follow PEP 8:** Format code with Black and sort imports with `isort`.
- **Exception Handling:** Never use bare `except:` blocks; always catch specific exceptions.
- **Virtual Environments:** Always declare dependencies in a `requirements.txt` or `pyproject.toml` with pinned versions.

```python
# Correct
from typing import Dict, Any

def process_payload(data: Dict[str, Any]) -> bool:
    try:
        user_id: str = data["user_id"]
        return True
    except KeyError as e:
        logger.error(f"Missing required key in payload: {e}")
        return False
```

---

## 4. Configuration Manifests (YAML/JSON)

- **Indentation:** Use 2 spaces for indentation in YAML files.
- **Schema Mapping:** Always validate configuration structures against official JSON schemas where available.
- **Pin Versions:** Pin Docker base images, Github Actions tags, and package versions to specific releases (never use `latest` or floating tag boundaries).
