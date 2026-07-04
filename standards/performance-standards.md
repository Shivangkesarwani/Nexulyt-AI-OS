# Performance Standards

This document establishes the latency metrics, database optimization targets, and load requirements for all applications built using **Nexulyt-AI-OS**.

---

## 1. Latency & Core Web Vitals Budgets

All services must target the following performance metrics:

| Metric | Target (P50) | Target (P95) | Target (P99) |
|---|---|---|---|
| API Endpoint Response | < 100ms | < 200ms | < 500ms |
| Page load (Lighthouse) | > 90 | - | - |
| Time to First Byte (TTFB) | < 300ms | < 600ms | - |

---

## 2. Database Optimization Guidelines

- **Composite Indexing:** Ensure indexes cover all standard `WHERE`, `JOIN`, and `ORDER BY` query columns.
- **Explain Analyze Verification:** Run `EXPLAIN ANALYZE` on every slow query (>100ms execution time) to eliminate sequential scans.
- **Connection Pools:** Configure active database connection thresholds matching expected replica counts to avoid pool exhaustion.
- **No loops in queries:** Never execute database queries inside code loops (loops result in N+1 query performance degradation).

---

## 3. Caching & Static Asset Delivery

- **CDN Caching:** Configure immutable headers on static files (javascript, css, pictures) for edge delivery.
- **API Cache Policies:** Stale-while-revalidate policies must be applied on cacheable GET resources.
- **Queue long jobs:** Offload compute-intensive operations (image resizing, video transcoding, report building) to worker queues instead of blocking request handlers.
