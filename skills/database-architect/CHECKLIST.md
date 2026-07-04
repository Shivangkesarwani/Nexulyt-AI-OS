# Database Architect Review & QA Checklist

Use this checklist to perform a comprehensive schema, index, and query audit of databases before staging commits.

---

## 1. REQUIREMENTS
- [ ] **Architectural Boundary:** The database engine and logical models match the requirements defined by the Software Architect.
- [ ] **Workload Fit:** The database choice (e.g., PostgreSQL for relations, Redis for caching) is validated for target query volumes.
- [ ] **Compliance Alignment:** Table data configurations satisfy compliance standards (e.g., GDPR data deletion flows, PCI payment tokenization).

---

## 2. SCHEMA DESIGN
- [ ] **Type Minimization:** Columns use the minimal required storage types (e.g., `VARCHAR(255)` instead of `TEXT`, `SMALLINT` for small ranges).
- [ ] **Primary Key Presence:** Every table defines a surrogate primary key (e.g., UUIDv4 or BIGINT).
- [ ] **Normalization Integrity:** Schemas are structured to Third Normal Form (3NF) to eliminate redundant storage columns.
- [ ] **Selective Denormalization:** Duplicated fields are restricted to high-read paths, with triggered updates configured.
- [ ] **Audit Columns:** Tables include standard `created_at` and `updated_at` timestamps.

---

## 3. ER DIAGRAM
- [ ] **Fidelity check:** The generated entity relation diagrams match active schema migrations.
- [ ] **Clear Entities:** Tables represent singular resources (e.g., `user`, `invoice`) with clear bounds.
- [ ] **Cardinality Symbols:** Connector notation (e.g., one-to-many, many-to-many) is correct.

---

## 4. RELATIONSHIPS
- [ ] **Foreign Keys Constraint:** All relationships are enforced at the engine layer using foreign keys.
- [ ] **Deletions Cascade:** Delete cascade options (`ON DELETE RESTRICT` or `ON DELETE CASCADE`) are explicitly declared.
- [ ] **Join Indexes:** Indexes are defined on all foreign key columns.
- [ ] **Join Tables composite:** Many-to-many join tables configure composite primary keys instead of surrogate integer IDs.

---

## 5. INDEXES
- [ ] **Filter Indexes:** Index structures (B-Tree) are set on columns used in `WHERE`, `JOIN`, or `ORDER BY` clauses.
- [ ] **Partial Indexes:** High-volume tables use partial indexes (`WHERE status = 'active'`) to keep index sizes small.
- [ ] **Index cardinality:** Low cardinality columns (e.g., booleans, simple status fields) are not indexed alone.
- [ ] **Over-indexing checks:** Duplicate indexes (e.g., single columns that are already the prefix of a compound index) are dropped.

---

## 6. CONSTRAINTS
- [ ] **Nullability rules:** Columns are marked `NOT NULL` by default, allowing `NULL` only when explicitly necessary.
- [ ] **Check constraints:** `CHECK` constraints validate numeric bounds, format constraints, and status lists.
- [ ] **Unique indexes:** Candidate keys (e.g., usernames, emails) have unique indexes.

---

## 7. QUERY OPTIMIZATION
- [ ] **Explicit SELECT:** Wildcard selects (`SELECT *`) are removed from production queries.
- [ ] **Pagination limits:** All `SELECT` list queries enforce strict `LIMIT` and `OFFSET` parameters.
- [ ] **Execution plan checks:** Queries are checked using `EXPLAIN (ANALYZE, BUFFERS)` to verify they use indexes and avoid full table scans.
- [ ] **No N+1 Queries:** Database fetches avoid looping selects, using explicit joins or relation batches.

---

## 8. TRANSACTIONS
- [ ] **Atomicity check:** Multiple related database updates are wrapped in transaction blocks.
- [ ] **Short Lock times:** External API requests and slow CPU operations are kept outside active transaction blocks.
- [ ] **Row locking locks:** Concurrent updates use explicit row-level locking (`SELECT ... FOR UPDATE`).
- [ ] **Isolation settings:** Transactions use the default `Read Committed` isolation level, switching to higher isolation levels only when necessary.

---

## 9. SECURITY
- [ ] **Port Security:** Database network ports are closed to the public internet, restricting queries to local VPCs.
- [ ] **SSL Encryption:** Client connections enforce SSL/TLS encryption in transit.
- [ ] **Storage Encryption:** Disk storage arrays and backups are encrypted at rest using AES-256.
- [ ] **Parameterization:** Queries use parameterized values to prevent SQL injection vulnerabilities.

---

## 10. BACKUP
- [ ] **Automated runs:** Daily backup runs are configured and monitored.
- [ ] **Offsite storage:** Backup files are encrypted and stored in offsite, geographically separate cloud buckets.
- [ ] **Recovery checks:** Backup restoration flows are tested weekly to verify recovery viability.

---

## 11. MIGRATION
- [ ] **No Manual edits:** Schema changes are applied via versioned migration files checked into git.
- [ ] **Zero-downtime flow:** Migrations are backward-compatible, supporting active API operations during deploys.
- [ ] **Rollback support:** Migrations run within transactions to support automatic rollbacks on failure.

---

## 12. SCALING
- [ ] **Read replicas:** Read queries are routed to read replica pools, separating them from write queries on the primary database.
- [ ] **Connection limits:** Connection poolers (e.g., PgBouncer) manage connection limits.
- [ ] **Table partitioning:** High-volume transactional logs are partitioned by date ranges.

---

## 13. MONITORING
- [ ] **Health Endpoint:** A `/healthz` API endpoint checks database connectivity states.
- [ ] **Slow Query Alerts:** pg_stat_statements is enabled to identify and alert teams about slow queries.
- [ ] **Resource Limits:** Alerts warn operators if disk space, memory usage, or connection counts near capacity.

---

## 14. FINAL REVIEW
- [ ] **Audit logs append:** System audit tables use append-only write permissions.
- [ ] **Access Credentials:** Decryption keys are stored in secure environments, separate from the database.
- [ ] **Lint checks:** Schemas match Prisma/Drizzle type checks.
