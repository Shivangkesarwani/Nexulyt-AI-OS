# Backend Engineer AI Skill

A production-grade AI Skill designed to teach AI assistants to build secure, scalable, and maintainable backend systems that align with Staff/Principal-level Backend Architects.

---

## 1. Overview
The Backend Engineer skill defines the database integrations, API architectures, authentication patterns, background queuing setups, and security layers required to implement backend services. It prevents AI from producing insecure configurations, plain SQL injection holes, or unhandled process crashes.

---

## 2. Purpose
- **Production-Ready Systems:** Deliver fully functional, TypeScript-validated, and structured backend modules.
- **Enterprise Security:** Enforce password hashing (Argon2), secure cookie storage, rate-limiting, and RBAC authorization protocols.
- **Scaling Excellence:** Enforce database transaction isolations, connection pooling, and background job queues.

---

## 3. Responsibilities
- Implement APIs designed by the Software Architect skill.
- Fetch, validate (via Zod), and modify database records securely.
- Setup background worker nodes to handle long-running tasks asynchronously.
- Verify API health using Vitest unit tests and Supertest integration tests.

---

## 4. Features
- **Clean Architecture:** Domain entities, use cases, and interfaces decoupled from external frameworks.
- **Caching & Caches:** Redis cache integrations for fast lookups, session management, and rate-limiting.
- **Job Queues:** BullMQ background job processing with retry policies and backoff limits.
- **Self Review Engine:** An automated database, security, and API check loop verifying code safety.

---

## 5. Tech Stack
- **Core:** Node.js, TypeScript
- **Frameworks:** Express, NestJS
- **ORM & DB:** Prisma, PostgreSQL
- **Caching & Queues:** Redis, BullMQ
- **Authentication:** JWT, OAuth, Passport, Session cookies
- **APIs:** OpenAPI, Swagger, WebSockets (Socket.IO)
- **Testing & Deployment:** Vitest, Supertest, Docker, GitHub Actions CI/CD

---

## 6. Folder Structure
```
skills/backend-engineer/
├── SKILL.md      # Core AI Identity, principles, and backend engineering rules
├── CHECKLIST.md  # Professional QA audit checklists for database and security reviews
└── EXAMPLES.md   # Step-by-step API setup and transactional examples
```

---

## 7. Workflow
1. **Analyze Specifications:** Deconstruct API designs, database schemas, and permission rules.
2. **Setup Schema & Types:** Configure Prisma models, Zod validators, and TypeScript interfaces.
3. **Build Core Logic:** Implement repositories, use cases/services, and controllers.
4. **Audit & Package:** Run integration tests, check query lock times, and build Docker containers.

---

## 8. Expected Outputs
When activated, this skill generates:
- Clean, modular NestJS modules or Express route handlers.
- Secure database models, migrations, and transactions.
- Fully configured environment configuration files.
- Automated testing files (Vitest / Supertest).

---

## 9. Compatible Skills
- **Software Architect:** Designs the data schemas, APIs, and overall system boundaries.
- **UI/UX Designer:** Designs visual styles and page interaction flows.
- **Frontend Engineer:** Integrates backend endpoints into web user interfaces.

---

## 10. Best Practices
- **Validate Everything:** Validate all request payloads using Zod at the route boundary.
- **Use Transactions:** Run related writes inside database transactions to ensure consistency.
- **Non-root Docker:** Configure Docker files to run with low-privilege users.

---

## 11. Example User Requests
- *Request 1:* "Create a NestJS service mapping invoice database records to Prisma."
- *Request 2:* "Set up a BullMQ worker to generate PDF reports in the background."
- *Request 3:* "Configure JWT cookie authentication with refresh token rotations."

---

## 12. License
Licensed under the [MIT License](file:///d:/projects/Nexulyt-AI-OS/LICENSE).
