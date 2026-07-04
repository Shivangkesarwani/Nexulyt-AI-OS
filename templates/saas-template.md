# SaaS Template

This document provides the systems architecture and specifications for building a multi-tenant subscription-based SaaS platform.

---

## 1. Overview & Purpose
- **Objective:** Deploy a multi-tenant product with stripe billing integrations, workspace separation, and Role-Based Access Control (RBAC).
- **Target Users:** Small to Medium Businesses (SMBs) and corporate groups.
- **Recommended Stack:** Next.js, Node.js + NestJS, PostgreSQL, Redis, Stripe, Docker.

---

## 2. Directory Layout

```text
saas-app/
├── src/
│   ├── components/      # Workspace components (WorkspaceSwitcher, BillingCard)
│   ├── database/        # Migrations, schemas, seed scripts
│   ├── routes/          # API route definitions
│   └── services/        # Logic handlers (billing, auth, notifications)
├── Dockerfile           # Multi-stage container runner
├── docker-compose.yml   # Multi-service local setup
└── package.json         # Pinned packages
```

---

## 3. Core Requirements

- **Database:** PostgreSQL. Enforce Row-Level Security (RLS) policies to isolate tenant data logically.
- **API Requirements:** REST endpoints for workspace CRUD, team invites management, and billing portal redirect hooks.
- **Authentication Strategy:** JWT with RS256 signing, short-lived tokens, and HTTP-only cookie session storage.
- **UI Components:** Sidebar workspace selection controls, user avatar components, data list tables with sorting.
- **Pages:** Landing page, Authentication login, Workspace dashboard, Settings (Billing, Team permissions).

---

## 4. Performance & Security

- **Performance:** Enforce API rate-limiting. Apply stale-while-revalidate caching patterns using Redis for tenant configurations.
- **Security:** Strict CORS limits, SQL injection parameterization, CSRF token validation on cookies.
- **Deployment:** Containerized GKE or AWS ECS Fargate deploy with load-balanced proxy endpoints.

---

## 5. Development Roadmap
- **Phase 1:** Configure Auth, multi-tenancy database structures, and RLS rules.
- **Phase 2:** Integrate Stripe webhooks and plan feature gates.
- **Phase 3:** Build frontend workspace screens and release via CI pipelines.
