# Performance Engineer Production Validation Checklist

A comprehensive quality assurance checklist for verifying performance standards across frontend, backend, database, API, AI, and infrastructure layers before production deployment.

---

## 1. Frontend Performance
- [ ] **Critical CSS Inlined:** Above-the-fold styles are inlined directly in `<head>` to prevent render-blocking requests.
- [ ] **Non-Critical CSS Deferred:** Stylesheets not required for initial render load asynchronously using `media="print"` and `onload` swap.
- [ ] **JavaScript Deferred:** Non-critical scripts use `defer` or `async` attributes to prevent parser blocking.
- [ ] **Render-Blocking Resources Eliminated:** Lighthouse audit reports zero render-blocking resources on the critical path.
- [ ] **Code Splitting Configured:** Application bundles are split by route entry points; each route loads its own dedicated chunk.
- [ ] **Tree Shaking Active:** Dead code elimination is confirmed via bundle analyzer; unused exports are absent from build outputs.
- [ ] **Bundle Size Within Budget:** All JavaScript entry bundles remain below 250KB (gzipped) per Webpack performance budget rules.
- [ ] **Unused Dependencies Removed:** `depcheck` or equivalent audit confirms no unused packages exist in `node_modules`.
- [ ] **Third-Party Scripts Audited:** All third-party scripts are loaded asynchronously and reviewed for performance cost.

---

## 2. Backend Performance
- [ ] **Event Loop Non-Blocking:** No synchronous, blocking operations (e.g., `fs.readFileSync`, CPU-heavy loops) exist on the Node.js main thread.
- [ ] **Connection Pooling Configured:** Database connection pools are configured with appropriate `min`, `max`, and `idleTimeoutMillis` parameters.
- [ ] **Async/Await Patterns Consistent:** All I/O operations use async/await or promise chains; callbacks are absent from critical paths.
- [ ] **Worker Threads Active:** CPU-intensive operations (e.g., image processing, data parsing) offload to dedicated worker threads.
- [ ] **Background Queues Configured:** Long-running tasks (e.g., email sends, report generation) publish to BullMQ or equivalent queues.
- [ ] **Payload Sizes Minimized:** API responses omit unnecessary fields; response bodies return only requested data columns.
- [ ] **HTTP Keep-Alive Enabled:** Persistent connections are configured on reverse proxies and HTTP clients to reduce connection overhead.
- [ ] **Graceful Shutdown Implemented:** Applications drain in-flight requests cleanly on SIGTERM before process termination.

---

## 3. Database Performance
- [ ] **Query Execution Plans Verified:** `EXPLAIN ANALYZE` output confirms all slow queries use index scans, not sequential scans.
- [ ] **Composite Indexes Created:** Indexes exist on common multi-column filter and join combinations used in production queries.
- [ ] **N+1 Queries Eliminated:** No database queries execute inside application loops; batch or join queries replace serial lookups.
- [ ] **Select Star Removed:** All production queries specify explicit column lists; `SELECT *` is absent from ORM and raw queries.
- [ ] **Read Replicas Configured:** Read-heavy endpoints route queries to read replicas, offloading the primary write instance.
- [ ] **Slow Query Log Active:** Database slow query logging is enabled with a threshold (e.g., queries exceeding 100ms are logged).
- [ ] **Connection Pool Limits Set:** Maximum pool size is configured below the database server's maximum connection limit.
- [ ] **Vacuum/Analyze Scheduled:** PostgreSQL autovacuum is active; manual ANALYZE runs after large bulk operations.
- [ ] **Pagination Enforced:** All list-returning queries implement cursor-based or offset pagination with enforced maximum page sizes.

---

## 4. API Performance
- [ ] **Response Compression Enabled:** Brotli or gzip compression is active on all text-format API responses (`application/json`, `text/plain`).
- [ ] **Pagination on All List Endpoints:** Every collection endpoint implements cursor-based pagination with enforced limits.
- [ ] **Rate Limiting Active:** Public API endpoints enforce IP-based rate limits (e.g., max 100 requests per 15 minutes).
- [ ] **Cache-Control Headers Set:** API endpoints return appropriate `Cache-Control` directives (e.g., `private, no-cache` for authenticated routes).
- [ ] **GraphQL Depth Limiting:** Query depth and complexity limits are configured to block resource-exhausting GraphQL queries.
- [ ] **DataLoaders Active:** All GraphQL resolvers use DataLoader instances to batch and deduplicate database calls.
- [ ] **HTTP/2 Enabled:** The reverse proxy (e.g., Nginx, Cloudflare) serves API responses over HTTP/2 for multiplexing.
- [ ] **Streaming for Large Payloads:** File downloads and large dataset exports use Node.js streams rather than buffering entire payloads in memory.

---

## 5. AI Performance
- [ ] **Token Counts Measured:** Prompt lengths are calculated using a tokenizer (e.g., `tiktoken`) before submitting to LLM APIs.
- [ ] **Prompt Compression Applied:** System and context prompts are compressed to remove redundant text when approaching token limits.
- [ ] **Embedding Cache Active:** Common query embeddings are cached in Redis or a vector database to avoid redundant API recalculations.
- [ ] **Batch Vector Queries Used:** Multiple similar vector search queries are batched into single bulk requests where possible.
- [ ] **LLM Response Streamed:** Streaming response modes are used for long-form AI outputs to reduce time-to-first-token.
- [ ] **Fallback Models Configured:** Lower-cost model fallbacks are available for non-critical, high-volume inference tasks.
- [ ] **AI API Cost Monitored:** LLM API spend is tracked per endpoint and alerted when daily thresholds are exceeded.
- [ ] **RAG Chunk Sizes Optimized:** Retrieval-Augmented Generation chunk sizes are tuned to balance context quality and token consumption.

---

## 6. Core Web Vitals
- [ ] **LCP Under 2.5s:** Largest Contentful Paint passes the "Good" threshold on real-device field data (CrUX).
- [ ] **CLS Under 0.1:** Cumulative Layout Shift passes the "Good" threshold; all images and embeds have explicit dimensions.
- [ ] **INP Under 200ms:** Interaction to Next Paint passes the "Good" threshold; long tasks are broken up using `yieldToMain`.
- [ ] **FCP Under 1.8s:** First Contentful Paint passes the "Good" threshold; render-blocking resources are eliminated.
- [ ] **TTFB Under 800ms:** Time to First Byte passes the "Good" threshold; server response times are within budget.
- [ ] **LCP Image Preloaded:** The hero/banner image is preloaded using `<link rel="preload">` with `fetchpriority="high"`.
- [ ] **Fonts Use `font-display: swap`:** Web font declarations use `font-display: swap` to prevent invisible text during font load.
- [ ] **No Lazy-Loading Above-the-Fold:** Images in the initial viewport do not carry the `loading="lazy"` attribute.
- [ ] **Lighthouse Score Above 90:** Production pages achieve a Lighthouse performance score of 90 or higher in CI audits.

---

## 7. Caching
- [ ] **Static Asset Cache Headers Set:** Versioned static files serve with `Cache-Control: max-age=31536000, immutable`.
- [ ] **Redis TTL Configured:** All Redis keys have explicit expiration times (TTL) set to prevent unbounded memory growth.
- [ ] **Read-Through Cache Pattern Active:** Database results for frequent, stable queries are cached in Redis on first fetch.
- [ ] **Cache Invalidation Strategy Defined:** A documented cache invalidation mechanism exists for all cached data types.
- [ ] **CDN Cache Rules Verified:** Cloudflare or equivalent CDN is configured to cache static assets at edge nodes.
- [ ] **API Response Caching Active:** Stable, public API responses (e.g., product listings, configuration endpoints) are cached at the gateway layer.
- [ ] **Session Store in Redis:** User session data is stored in Redis rather than in-memory to support horizontal scaling.
- [ ] **Cache Hit Rate Monitored:** Redis cache hit ratios are tracked in dashboards; low hit rates trigger optimization reviews.

---

## 8. Monitoring
- [ ] **OpenTelemetry SDK Instrumented:** Applications export traces, metrics, and logs using the OpenTelemetry SDK.
- [ ] **Golden Signals Dashboarded:** Latency, traffic, error rate, and saturation metrics are visible in Grafana or equivalent dashboards.
- [ ] **P95/P99 Latency Tracked:** API response time percentiles (P95, P99) are tracked and alerted when budgets are breached.
- [ ] **Error Rate Alerts Active:** Alerts fire when API error rates exceed 1% sustained over a 5-minute window.
- [ ] **Database Query Metrics Exported:** Slow query counts and connection pool utilization metrics are exported to the monitoring stack.
- [ ] **Node.js Heap Usage Tracked:** Application heap size metrics are exported via Prometheus or OTLP to detect memory leaks.
- [ ] **Event Loop Lag Monitored:** Node.js event loop delay is tracked; alerts fire when lag exceeds 100ms.
- [ ] **Anomaly Alerts Configured:** Automated anomaly detection alerts flag abnormal traffic spikes or error rate surges.

---

## 9. Load Testing
- [ ] **Staging Environment Matches Production:** The load test environment uses the same infrastructure specifications as production.
- [ ] **k6 Scripts Committed:** Load test scripts exist in the repository and are version-controlled alongside application code.
- [ ] **Baseline Tests Established:** Baseline performance metrics (RPS, P95 latency, error rate) are recorded before optimization.
- [ ] **Soak Tests Executed:** Sustained load tests run for at least 30 minutes to detect memory leaks and gradual degradation.
- [ ] **Stress Tests Run:** Load is ramped beyond expected peak traffic to identify breaking points and failure modes.
- [ ] **P95 Latency Threshold Met:** Under target load, 95% of requests complete within the defined latency budget (e.g., under 250ms).
- [ ] **Error Rate Under 1%:** Total error rates remain below 1% at all tested load levels.
- [ ] **Autoscaling Verified Under Load:** Kubernetes HPA or equivalent autoscaler is confirmed to scale replicas during load test ramp-up.

---

## 10. Scalability
- [ ] **Stateless Application Design:** Application instances carry no in-process session state; all state lives in Redis or a database.
- [ ] **HPA Min/Max Replicas Set:** Kubernetes HorizontalPodAutoscaler defines minimum replica floors and maximum replica ceilings.
- [ ] **Autoscaling Metric Configured:** HPA scales on CPU utilization target (e.g., 70%) or custom metrics (e.g., request queue depth).
- [ ] **Database Read Replicas Active:** Read replicas serve all non-transactional queries to distribute database load.
- [ ] **Connection Pool Sized for Scale:** Connection pool limits account for the maximum expected number of running application replicas.
- [ ] **Queue Workers Scale Independently:** Background task workers scale independently from API replicas based on queue depth metrics.
- [ ] **CDN Offloads Static Traffic:** Static file requests (JS, CSS, images) are fully served by CDN, removing load from origin servers.
- [ ] **Rate Limits Prevent Overload:** Per-IP and per-client rate limits protect backends from traffic spikes causing saturation.

---

## 11. Production Readiness
- [ ] **Performance Budgets Enforced in CI:** Webpack, Lighthouse CI, and k6 budget checks are integrated into the deployment pipeline.
- [ ] **Health Check Endpoints Active:** `/health/live` and `/health/ready` endpoints return correct status codes for load balancer probes.
- [ ] **Graceful Shutdown Verified:** Applications drain requests and close database connections cleanly within a 30-second SIGTERM window.
- [ ] **Resource Limits Set on Containers:** Kubernetes pods define CPU and memory `requests` and `limits` to prevent noisy neighbor issues.
- [ ] **Rollback Plan Defined:** A deployment rollback strategy is documented and verified to restore the previous stable release within 5 minutes.
- [ ] **Canary or Blue/Green Deployment Active:** New releases roll out incrementally to a subset of traffic before full promotion.
- [ ] **Runbooks Documented:** Performance incident runbooks exist for common degradation scenarios (e.g., database connection exhaustion, cache stampede).

---

## 12. Final Review
- [ ] **All Section Checkpoints Passed:** All checkpoints in sections 1–11 have been reviewed and confirmed.
- [ ] **Profiling Data Documented:** Profiling outputs (flame graphs, query plans) are archived alongside the optimization changes.
- [ ] **Metrics Validated Against Budgets:** Post-deployment monitoring confirms all Golden Signal metrics remain within defined budgets.
- [ ] **Load Test Results Archived:** Final load test reports are stored and linked in the deployment release notes.
- [ ] **Performance Regression Monitoring Active:** Continuous telemetry dashboards are live and alerting, monitoring for post-release regressions.
