# Database Design Checklist

This document provides a quality gate checklist to validate database schema designs, indexing strategies, data models consistency, migration configurations, and backup policies in alignment with the [Architecture Standards](file:///d:/projects/Nexulyt-AI-OS/standards/architecture-standards.md).

---

## 1. Overview

* **Objective**: Enforce transactional safety, correct data types, optimal lookup paths, database scale options, and reliable backups.
* **When to Use**: During database schema design, migration planning phases, query optimization runs, or pre-release verification checks.
* **Prerequisites**: Domain models specifications, access pattern parameters, volumetric estimations, and data lifecycle requirements.

---

## 2. Checklist Items

### Relational Schema Design (PostgreSQL)
* [ ] **Normalization Level**: Verify relational schemas conform to 3NF standards (document intentional denormalizations with clear trade-offs).
* [ ] **Foreign Key Constraints**: Ensure all relational entities map explicit foreign keys with cascade/restrict rules.
* [ ] **Nullable Configuration**: Check columns nullable status; enforce `NOT NULL` constraints on columns that require values.
* [ ] **Exact Numeric Types**: Enforce exact numerical mapping types (e.g. `numeric(18,4)`) for currency fields (never use floating floats).
* [ ] **Audit Column Timestamps**: Ensure all tables include `created_at` and `updated_at` columns, managed by DB triggers.

### Indexing & Query Optimizations
* [ ] **Foreign Key Indexing**: Enforce B-tree indices on all columns targeted in foreign keys relationships.
* [ ] **Composite Index Mapping**: Build composite indices on fields filtered and sorted concurrently (order matches filtering sequence).
* [ ] **Scan Auditing**: Run `EXPLAIN ANALYZE` on primary query paths, verifying they perform Index Scans rather than Full Table Scans.
* [ ] **Avoid Over-Indexing**: Check that tables do not exceed 5 indices to maintain write throughput performance.

### NoSQL & Vector Data Models
* [ ] **Access Pattern Alignment**: NoSQL document layouts are modeled around query access patterns (read frequencies) rather than relational normalizations.
* [ ] **Partition Keys Selection**: Enforce high-cardinality partition keys to prevent hot partitions.
* [ ] **Embedding Dimension Integrity**: Verify that vector collection index scopes match target embedding dimensions (e.g., 1536 for OpenAI models).

### Migrations, Backups & Disaster Recovery
* [ ] **Versioned Migrations DDL**: All schema modifications are written in versioned, declarative migration script files (committed to git).
* [ ] **Migration Rollback Blueprint**: Ensure migration scripts feature matching down/rollback steps.
* [ ] **Automated Backup Policy**: Configure daily snapshots and point-in-time recovery (PITR) parameters.
* [ ] **Backup Encryption**: Verify backups are encrypted using keys managed outside the database environment.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Run migration scripts on clean local test databases to verify syntax and consistency.
* Verify that index structures are actively utilized by database query planners under simulated data loads.
* Conduct a restore dry-run of a database backup to confirm retrieval parameters.

### Exit Criteria
* **Declarative Migration committed**: Schema delta DDL scripts added to the codebase.
* **Explain Plan Approved**: Main query paths demonstrate index-only execution scans.
* **Backup verification log status**: Successful restoration test verified.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Changing database tables manually in production environments without using versioned migration files.
* Omitting indices on foreign keys, leading to database locking conflicts during cascades.
* Storing large raw binary blobs (images, PDFs) inside SQL tables, causing disk thrashing and slow table scans.

### Professional Recommendations
1. **Index Foreign Keys Always**: Ensure database validation scripts reject migration pull requests if new foreign keys lack matching index definitions.
2. **Review Execution Plans in Staging**: SQL query plans can vary under dataset sizes. Run query plans on staging datasets matching production scales.
3. **Automate Schema Testing**: Run database migration tests (up and down pathways) inside CI pipelines.
