# Database Architect AI Skill

A production-grade AI Skill designed to teach AI assistants to build secure, scalable, and high-performance database architectures that align with Staff/Principal-level Database Architects.

---

## 1. Overview
The Database Architect skill defines the data modeling paradigms, indexing models, transaction rules, partitioning conventions, and security audits required to design databases. It prevents AI from producing tables lacking primary keys, unindexed queries, or transactional write anomalies.

---

## 2. Purpose
- **Data Integrity:** Enforce referential bounds, check constraints, and ACID properties at the database engine layer.
- **Query Performance:** Guide the selection of indexing structures (B-Tree, GIN) and check query plans using EXPLAIN ANALYZE.
- **Enterprise Scaling:** Plan read replicas, horizontal partitioning, caching layers, and sharding routing policies.

---

## 3. Responsibilities
- Implement storage designs designed by the Software Architect skill.
- Formulate database migrations, entity relationship maps (ERD), and schemas.
- Optimize index selections, slow-running queries, and lock tables.
- Verify migration script rollbacks and validate queries against test databases.

---

## 4. Features
- **Strict Modeling:** Schema definitions built on third normal form (3NF) structures and strict key bindings.
- **Transaction Optimizations:** Explicit row-level locking configurations (`SELECT ... FOR UPDATE`) and isolation level checks.
- **Database Partitioning:** Range and hash table partition models for high-volume logs databases.
- **Self Review Engine:** An automated query plan, index configuration, and constraint check loop.

---

## 5. Supported Databases
- PostgreSQL
- MySQL
- MongoDB
- Redis

---

## 6. Workflow
1. **Deconstruct Models:** Deconstruct specifications, identifying query volumes, relations, and limits.
2. **Setup Schema & Keys:** Configure database tables, key constraints, and nullability settings.
3. **Optimize Index paths:** Build indexing structures, write query paginations, and analyze plans.
4. **Audit Transactions & Security:** Check lock limits, write transactions, and set encryption.

---

## 7. Folder Structure
```
skills/database-architect/
├── SKILL.md      # Core AI Identity, database philosophy, and modeling rules
├── CHECKLIST.md  # Professional QA audit checklists for migrations and indexes
└── EXAMPLES.md   # Step-by-step schema designs and transactional examples
```

---

## 8. Expected Outputs
When activated, this skill generates:
- Database schema files mapped to ORM templates (Prisma / Drizzle ORM).
- Version-controlled SQL migration scripts.
- Query optimizations and index definitions.
- Triggers, constraints, and audit log procedures.

---

## 9. Compatible Skills
- **Software Architect:** Designs the overall system boundaries and logical API schemas.
- **Backend Engineer:** Integrates database queries and transaction boundaries into service frameworks.
- **Frontend Engineer:** Maps UI forms to schema-validated API endpoints.

---

## 10. Best Practices
- **Define Primary Keys:** Every table must have a surrogate primary key (e.g., UUID or BIGINT) by default.
- **Index Join Columns:** Always index foreign keys and columns used in query filters.
- **Short Transactions:** Keep locks short and avoid third-party API calls inside database transactions.

---

## 11. Example User Requests
- *Request 1:* "Generate a PostgreSQL schema for a multi-tenant subscription model using Prisma."
- *Request 2:* "Optimize a slow-running JOIN query and create partial indexes."
- *Request 3:* "Set up a monthly date partition table for invoice activity logs."

---

## 12. License
Licensed under the [MIT License](file:///d:/projects/Nexulyt-AI-OS/LICENSE).
