# API Design Standards

This document establishes the REST API design conventions for all services in the **Nexulyt-AI-OS** repository.

---

## 1. REST Conventions

- **Path Directories:** Use plural lowercase nouns for resource collections (e.g. `/api/v1/workspaces`).
- **HTTP Methods:** Enforce standard CRUD operations:
  - **`GET`**: Retrieve resources. Safe and idempotent.
  - **`POST`**: Create resources. Non-idempotent.
  - **`PUT`**: Replace resources. Idempotent.
  - **`PATCH`**: Partial update. Non-idempotent.
  - **`DELETE`**: Remove resources. Idempotent.

---

## 2. API Versioning

- Enforce URL path versioning (e.g. `/api/v1/users`).
- Break compatibility only with major version increments (e.g. v1 to v2).

---

## 3. Error Responses (RFC 7807)

All error payloads must be formatted according to the RFC 7807 problem details specification:

```json
{
  "type": "https://api.nexulyt.com/errors/invalid-payment",
  "title": "Invalid Payment Method",
  "status": 400,
  "detail": "The card provided has expired. Please check expiration dates.",
  "instance": "/api/v1/payments/pay_12345"
}
```

---

## 4. Input & Output Formatting

- **JSON Format:** All API request bodies and response payloads must be valid JSON format.
- **CamelCase:** Fields must use `camelCase` format (e.g. `userId`, `createdDate`).
- **Standard HTTP codes:**
  - `200 OK` / `201 Created` / `204 No Content` for successful requests.
  - `400 Bad Request` / `401 Unauthorized` / `403 Forbidden` / `404 Not Found` for client errors.
  - `500 Internal Server Error` for unhandled runtime failures.
