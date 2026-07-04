# Performance Checklist

This document provides a quality gate checklist to validate application performance, Core Web Vitals metrics, caching strategies, SQL execution plans, API payload compressions, and AI token budgets in compliance with the [Performance Standards](file:///d:/projects/Nexulyt-AI-OS/standards/performance-standards.md).

---

## 1. Overview

* **Objective**: Enforce sub-second latency targets, minimize CPU consumption bottlenecks, optimize database query profiles, and implement efficient caching.
* **When to Use**: During active development reviews, post-build validation gates, database index planning runs, or pre-release load audits.
* **Prerequisites**: Latency SLA metrics, database volumetric profiles, traffic expectations, and Core Web Vitals target scores.

---

## 2. Checklist Items

### Core Web Vitals Targets
* [ ] **Largest Contentful Paint (LCP)**: Ensure LCP is under 1.5 seconds globally (optimized images, preloaded critical assets).
* [ ] **Cumulative Layout Shift (CLS)**: Verify CLS is under 0.05 (explicit image dimensions, font swap rules).
* [ ] **Interaction to Next Paint (INP)**: Ensure INP remains under 100ms (defer long tasks, eliminate heavy javascript animation loops).

### Caching Strategy Compliance
* [ ] **Edge CDN Configurations**: Cache public static files, images, and pre-compiled pages at edge nodes with explicit Cache-Control headers.
* [ ] **Redis Write Buffering**: Asynchronous, high-frequency updates (e.g. video progress ticks) write to Redis cache buffers before SQL bulk commits.
* [ ] **Cache Invalidation Policy**: Define explicit cache-invalidation hooks and Time-To-Live (TTL) parameters to prevent stale read data.

### Database Query Performance
* [ ] **Index Scan Verification**: Run `EXPLAIN ANALYZE` on SQL routes, confirming that joins and filters utilize indexes (no Sequential scans allowed on large tables).
* [ ] **Connection Pooling**: Configure PgBouncer or equivalent utilities to prevent database thread starvation under peak concurrent connections.
* [ ] **N+1 Query Auditing**: Verify ORM fetch relations execute with joined loads (`JOIN FETCH`) rather than nested lazy-load queries.

### API Gateway & Network Optimizations
* [ ] **Brotli/Gzip Compression**: Enable response payload compression (Brotli preferred) on all REST/GraphQL gateways.
* [ ] **Payload Pruning**: Strip redundant metadata fields and duplicate keys from JSON models to minimize network payload weights.
* [ ] **HTTP Keep-Alive**: Enforce keep-alive connection headers to reuse TCP paths for downstream requests.

### AI Cost & Token Optimizations
* [ ] **Prompt Caching Organization**: Position static instructions at the start of LLM prompts to maximize prompt cache hits.
* [ ] **Sliding Chat History Offsets**: Truncate active chat buffers to the last 10 messages, summarizing older items into unified system tags.
* [ ] **Chunk Size Pruning**: Limit RAG context vector injections to the top K chunks.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Run Lighthouse or similar Core Web Vitals tests to ensure scores exceed 95.
* Test Redis write configurations under concurrency simulators.
* Verify that DB explain outputs show Index Scans on primary filters.

### Exit Criteria
* **Lighthouse Targets Met**: Target LCP, CLS, and INP metrics achieved.
* **No Sequential Table Scans**: DB queries execute via indexes.
* **Payload Size Compliance**: Consolidated API payloads conform to maximum bandwidth limitations.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Omitting height and width boundaries on media tags, causing Cumulative Layout Shift (CLS) errors during loading.
* Running SQL aggregations (`SUM`, `COUNT`) across millions of active rows in live HTTP threads instead of using materialized views.
* Storing dynamic, changing parameters (e.g., local timestamps) at the start of prompts, breaking LLM prompt caching mechanisms.

### Professional Recommendations
1. **Enforce Budgets Early**: Set concrete performance bounds (e.g., page bundle size < 200kb, API response < 100ms) inside build checks.
2. **Profile Under Production Scales**: Never assume staging database metrics represent live performance; run query execution tests on representative data volumes.
3. **Use Materialized Views for Analytics**: Move heavy reports queries to pre-compiled ClickHouse columns or Postgres Materialized Views updated out-of-band.
