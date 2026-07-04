# Debugging Expert Production Validation Checklist

A comprehensive, evidence-first debugging validation checklist for systematically investigating and resolving production issues across frontend, backend, database, infrastructure, and AI systems.

---

## 1. Problem Understanding
- [ ] The bug report includes a clear description of the observed behavior and the expected behavior.
- [ ] The first occurrence timestamp and the frequency of recurrence are documented.
- [ ] The affected scope is identified: which users, endpoints, services, or regions are impacted.
- [ ] The severity is classified: data loss, service unavailability, degraded performance, or cosmetic.
- [ ] A correlation is confirmed or ruled out between the first occurrence and recent deployments, configuration changes, or traffic spikes.
- [ ] The bug is classified by type: logic, performance, memory, network, concurrency, environment, security, or integration.
- [ ] The primary diagnostic tool appropriate for the classified bug type has been identified before investigation begins.
- [ ] No fix has been proposed or applied before completing the investigation checklist.

---

## 2. Reproduce the Issue
- [ ] The bug is confirmed reproducible in a controlled environment (local, staging, or production replica).
- [ ] The exact reproduction steps are documented: request payload, URL, user account, and browser/environment details.
- [ ] The reproduction environment matches the production environment in: runtime version, dependency versions, and environment variables.
- [ ] The bug reproduces consistently under the documented steps—not just intermittently during a one-time attempt.
- [ ] Intermittent bugs are reproduced under elevated concurrency or specific timing conditions before proceeding.
- [ ] A "clean state" reproduction is verified: the bug appears in a fresh session without cached state or browser extensions.
- [ ] The minimum reproduction case is established: the simplest input or sequence that triggers the bug.

---

## 3. Environment Verification
- [ ] Runtime version in the failing environment matches the expected version (Node.js, Python, Go version checked).
- [ ] Dependency versions match between the failing environment and the last known working environment (lockfile diff reviewed).
- [ ] All required environment variables are present and non-empty in the failing environment.
- [ ] Environment variable values are correctly formatted (no trailing spaces, no missing quotes for special characters).
- [ ] The application configuration file loaded in the failing environment is confirmed (not a cached or default config).
- [ ] The deployed artifact (Docker image tag, release SHA) is confirmed to match the intended deployment version.
- [ ] If the bug is environment-specific (e.g., works locally, fails in staging), a diff of environment variables and config values between the two environments has been produced.

---

## 4. Logs Review
- [ ] Logs from the time window surrounding the first bug occurrence are collected (minimum 5 minutes before and after).
- [ ] Log entries are filtered by `traceId` or `requestId` to isolate the specific failed request lifecycle.
- [ ] The log level is set to include `debug` entries in the reproduction environment for the investigation session.
- [ ] All log entries in the trace are read in chronological order before forming hypotheses.
- [ ] The originating service and the originating function are identified from the log sequence.
- [ ] Logs from all services involved in the request chain (not just the first-reported service) are reviewed.
- [ ] Sensitive fields (passwords, tokens, PII) are confirmed to be absent from the collected log entries before sharing.
- [ ] Log entries from dependent services (database slow query log, Redis log, third-party API webhook log) are checked if the application log does not contain the error.

---

## 5. Stack Trace Analysis
- [ ] The full stack trace is captured — not a truncated excerpt.
- [ ] The stack trace is read top-to-bottom to identify the exception type and message, then bottom-to-top to understand the call chain.
- [ ] The first stack frame that belongs to project code (not a framework or library frame) is identified as the investigation starting point.
- [ ] If the stack trace is minified, source maps are applied to de-minify before interpretation.
- [ ] The exception type is looked up in the relevant runtime documentation to confirm its standard meaning.
- [ ] The exact file path, function name, and line number from the first project-owned frame are recorded in the investigation log.
- [ ] Secondary exceptions or "caused by" chained exceptions are read in full — they often contain the actual root error.

---

## 6. API Debugging
- [ ] The failing request is reproduced using a raw HTTP client (`curl` or Postman) to eliminate SDK or client-side variables.
- [ ] The full request is captured: method, URL, all headers (including `Authorization`, `Content-Type`, `Accept`), and request body.
- [ ] The full response is captured: status code, all response headers, and response body.
- [ ] The response status code semantics are confirmed: `4xx` indicates a client error; `5xx` indicates a server error.
- [ ] For `401 Unauthorized`: the token is decoded (e.g., using `jwt.io`) and expiry (`exp`) and issuer (`iss`) claims are verified before investigating server-side validation.
- [ ] For `429 Too Many Requests`: `X-RateLimit-Remaining` and `Retry-After` response headers are checked.
- [ ] For `503 Service Unavailable`: the upstream load balancer logs are checked for `no live upstreams` or connection refusal errors.
- [ ] The upstream service's own health endpoint is tested directly when a downstream call fails, to isolate whether the fault is in the caller or the dependency.
- [ ] Rate limiting, CORS preflight failures, or missing headers are ruled out before concluding the issue is in application logic.

---

## 7. Frontend Debugging
- [ ] Chrome DevTools is opened in an Incognito window to eliminate browser extension interference.
- [ ] The Console tab is checked for JavaScript errors and warnings before any other investigation step.
- [ ] The Network tab is inspected for failing requests, incorrect status codes, and unexpected response payloads.
- [ ] "Preserve log" is enabled in the Network and Console tabs to retain entries across page navigations.
- [ ] "Disable cache" is enabled in the Network tab to ensure fresh assets are loaded during investigation.
- [ ] The Application tab is checked for stale Service Worker caches that may be serving outdated JavaScript.
- [ ] React DevTools Profiler is used to confirm whether a component is re-rendering unexpectedly before inspecting `useEffect` dependency arrays.
- [ ] For hydration mismatch errors: the error diff (server-rendered value vs. client-expected value) is read in full to identify the divergence source.
- [ ] Browser console `console.trace()` is used instead of `console.log()` to capture the full call stack at the point of interest.
- [ ] All four UI states are verified for the affected component: loading, success, error, and empty.

---

## 8. Backend Debugging
- [ ] The Node.js debugger is attached using `--inspect` and breakpoints are set at the originating function identified from the stack trace.
- [ ] The exact request payload from the production log is used to reproduce the error locally — not a reconstructed approximation.
- [ ] All `catch` blocks in the code path are confirmed to log and re-throw errors — no empty `catch` blocks are silently swallowing exceptions.
- [ ] The return value of every async call in the failing code path is confirmed to be `await`-ed correctly.
- [ ] Variable state (parameters, return values, intermediate results) is inspected at the breakpoint before and after the failing operation.
- [ ] External service calls (database, Redis, third-party API) within the code path are individually tested with isolated inputs to confirm which dependency is failing.
- [ ] The failing function is tested with the minimum input that reproduces the error to isolate the exact condition.
- [ ] Unhandled promise rejections are confirmed to be surfaced by `process.on('unhandledRejection', ...)` logging.

---

## 9. Database Debugging
- [ ] `EXPLAIN ANALYZE` is run on the slow or failing query — never `EXPLAIN` alone.
- [ ] The `EXPLAIN ANALYZE` output is inspected for: sequential scans on large tables, high actual row counts vs. estimated row counts, and nested loop joins on large result sets.
- [ ] `pg_stat_statements` is queried to identify the top queries by total execution time in the current investigation window.
- [ ] `pg_stat_activity` is queried to check for currently blocked or blocking processes before concluding a deadlock is present.
- [ ] `pg_locks` is joined with `pg_stat_activity` to identify which transactions hold which locks and what they are waiting for.
- [ ] The PostgreSQL slow query log is checked for entries with duration exceeding the configured threshold during the failure window.
- [ ] Connection pool metrics (active connections, idle connections, pool wait queue depth) are reviewed during the failure window.
- [ ] Migrations that run `NOT NULL` column additions on large tables are confirmed to include a `DEFAULT` value to prevent table-level locks.
- [ ] New indexes are confirmed to have been created with `CONCURRENTLY` in production to avoid write lock contention.
- [ ] For deadlock errors: the exact crossing lock acquisition order between the two transactions is identified from the PostgreSQL error log.

---

## 10. Performance Debugging
- [ ] Baseline performance metrics are recorded before any optimization is applied: P50, P95, P99 latency, RPS, and error rate.
- [ ] A CPU profiler (Node.js `--prof` or Python `cProfile`) is run under production-equivalent load — not idle load.
- [ ] The flamegraph is inspected to identify the widest call stacks: functions consuming the most CPU time.
- [ ] A database query `EXPLAIN ANALYZE` is run for every query in the slow code path.
- [ ] N+1 query patterns are confirmed to be absent by checking that list-based data fetches use batch queries, not per-item loops.
- [ ] All list-returning API endpoints are confirmed to enforce pagination with a maximum page size.
- [ ] CPU-intensive operations are confirmed to be offloaded to worker threads or background queues, not executing on the main Node.js thread.
- [ ] Redis cache hit rate is checked for the affected endpoints during the performance degradation window.
- [ ] Performance metrics are re-measured after every optimization to quantify the improvement — not declared successful based on intuition.
- [ ] P95 and P99 latency values are used as the primary performance signal — not averages, which mask tail latency outliers.

---

## 11. Memory Leak Investigation
- [ ] Heap usage is confirmed to grow continuously over time without returning to baseline after load decreases (distinguishing a leak from expected warm-up growth).
- [ ] `process.memoryUsage().rss` is monitored alongside `heapUsed` to detect native memory leaks not visible in the V8 heap.
- [ ] Node.js is started with `--expose-gc` to allow manual garbage collection triggering between heap snapshots.
- [ ] Heap Snapshot 1 is taken at a known stable point in time.
- [ ] Sustained load is applied for a minimum of 10 minutes.
- [ ] Heap Snapshot 2 is taken and compared against Snapshot 1 in the Chrome DevTools Memory panel "Comparison" view.
- [ ] The comparison view is sorted by "Delta" (positive values) to identify constructor types with growing instance counts.
- [ ] The Allocation Profiler is used to trace the allocation source of the growing object type to its originating code path.
- [ ] `EventEmitter` listener counts are checked using `emitter.listenerCount(event)` and the `MaxListenersExceededWarning` warning is confirmed to be surfaced via `process.on('warning', ...)`.
- [ ] Unclosed database connections, file handles, and stream readers are confirmed to be absent in the identified code path.
- [ ] `EventEmitter.off()` or `emitter.once()` is confirmed to be used wherever `emitter.on()` is called inside a lifecycle function.

---

## 12. Network Investigation
- [ ] DNS resolution is verified first using `dig` or `nslookup` before investigating higher-level failures.
- [ ] TCP port reachability is confirmed using `telnet <host> <port>` or `nc -zv <host> <port>`.
- [ ] TLS handshake is verified using `curl -v` to inspect the full certificate chain, expiry date, and hostname match.
- [ ] TLS certificate intermediate chain completeness is confirmed — not just the leaf certificate validity.
- [ ] `traceroute` or `mtr` is used to identify where packets are dropped along the network path.
- [ ] Connectivity is tested from inside the same network segment as the failing service — not from a developer workstation.
- [ ] VPC security group rules and network ACL rules are reviewed for the service's ingress and egress ports.
- [ ] For `connection refused` errors: the service process is confirmed to be listening on the expected port using `ss -tlnp` or `netstat -tlnp`.
- [ ] `TIME_WAIT` socket accumulation is checked using `ss -s` when intermittent connection failures correlate with high request volume.
- [ ] Firewall logs (AWS Security Group flow logs, VPC flow logs) are reviewed for dropped packets if TCP connectivity fails.

---

## 13. Authentication Issues
- [ ] The JWT token is decoded using a base64 decoder or `jwt.io` before any server-side investigation.
- [ ] The `exp` claim (token expiry) is checked in UTC — the most common cause of `401 Unauthorized` is an expired token.
- [ ] The `iss` (issuer) and `aud` (audience) claims match the server's configured expected values.
- [ ] The `alg` header field in the JWT matches the algorithm the server is configured to accept (e.g., `RS256` not `HS256`).
- [ ] The signing key used to verify the token matches the key currently configured in the failing environment.
- [ ] Server clock skew is checked: a server clock more than 5 minutes ahead of the token issuer will reject valid tokens based on `iat` (issued at) validation.
- [ ] The `Authorization` header format is confirmed: `Bearer <token>` with correct capitalization and spacing.
- [ ] CORS preflight failure is ruled out: confirm the `Authorization` header is not being blocked by a missing `Access-Control-Allow-Headers` response header.
- [ ] OAuth callback redirect URI matches exactly between the application configuration and the OAuth provider registration.
- [ ] Session cookie attributes are verified: `HttpOnly`, `Secure`, and `SameSite` are all set and the cookie is being transmitted on the failing request.

---

## 14. Infrastructure Issues
- [ ] Container exit code is checked using `docker inspect <container-id> --format='{{.State}}'`—exit code 137 means OOMKill, exit code 1 means application error.
- [ ] `docker logs <container-id>` is read in full before any other investigation step for container failures.
- [ ] `kubectl describe pod <pod-name>` is run and the Events section is read before checking container logs for Kubernetes pod failures.
- [ ] `kubectl logs <pod-name> --previous` is used to retrieve logs from the terminated container for CrashLoopBackOff investigation.
- [ ] `kubectl get events --all-namespaces --sort-by='.lastTimestamp'` is run to identify recent cluster-level scheduling or resource failures.
- [ ] Pod resource requests and limits are confirmed to be set and appropriate for the workload's actual CPU and memory consumption.
- [ ] For `Pending` pod state: insufficient cluster resources, taint/toleration mismatches, and unbound PersistentVolumeClaims are all checked.
- [ ] For cloud IAM permission errors: CloudTrail (AWS) or equivalent audit logs are checked for `AccessDenied` events before modifying IAM policies.
- [ ] Deployment environment variable completeness is verified: a diff of environment variables between the last successful deployment and the failing deployment is produced.
- [ ] For post-deployment regressions: `git bisect` is used to identify the specific commit introducing the regression rather than reviewing all commits manually.

---

## 15. Root Cause Analysis
- [ ] The 5 Whys method has been applied: each "why" answer leads to a deeper cause until the structural root cause is reached.
- [ ] The identified root cause is expressed as a specific line of code, configuration value, or data condition — not as a symptom or category.
- [ ] The root cause explains all observed symptoms — not just the most visible one.
- [ ] The root cause is verified: removing or correcting it prevents the bug from occurring under the same reproduction conditions.
- [ ] All contributing factors (secondary causes that amplified or enabled the root cause) are documented alongside the primary root cause.
- [ ] The fix targets the root cause — not a symptom. Adding a null check is a symptom fix; fixing the database constraint that allows the null value is the root cause fix.
- [ ] No changes other than the targeted fix are included in the resolution. Speculative refactoring during bug resolution is not permitted.

---

## 16. Regression Testing
- [ ] A regression test is written that fails against the unfixed codebase and passes after the fix is applied.
- [ ] The regression test reproduces the minimum conditions that triggered the bug (the input, state, or concurrency condition).
- [ ] The regression test is added to the automated test suite and will run on every future CI build.
- [ ] For concurrency bugs: a concurrent load test scenario is added that confirms the fix holds under parallel execution.
- [ ] For database bugs: a test that inserts the data condition that triggered the bug is included and verifies correct handling.
- [ ] The full existing test suite passes after the fix is applied — no regressions introduced by the fix itself.
- [ ] For `[CRITICAL]` bugs: an integration test is added in addition to the unit-level regression test.

---

## 17. Documentation
- [ ] The investigation trail is documented: symptoms, hypotheses tested, evidence gathered, and hypotheses eliminated in sequence.
- [ ] The root cause is stated clearly in one sentence in the bug ticket or postmortem.
- [ ] For `[CRITICAL]` incidents: a blameless postmortem is written within 48 hours of resolution, covering timeline, impact, root cause, contributing factors, and action items.
- [ ] Action items from the postmortem are assigned to named owners with defined due dates and tracked in the team's project management system.
- [ ] The bug class and prevention mechanism are added to the team's internal knowledge base or runbook library.
- [ ] The monitoring alert added as part of prevention is documented with its threshold, firing condition, and escalation path.
- [ ] If the bug was caused by a missing or incorrect convention, that convention is added to the relevant engineering standards document.

---

## 18. Final Validation
- [ ] The root cause is confirmed with evidence — not assumed from symptoms.
- [ ] The fix is applied only to the confirmed root cause — no speculative changes are included.
- [ ] The bug is verified as resolved in the reproduction environment after the fix is applied.
- [ ] The regression test passes and is committed to the test suite.
- [ ] The full test suite passes without new failures introduced by the fix.
- [ ] A monitoring alert is configured that would detect this failure class within 5 minutes of onset in production.
- [ ] For `[CRITICAL]` incidents: a postmortem with at least one tracked action item is completed.
- [ ] The fix has been reviewed by the Code Reviewer skill before merging to the main branch.
- [ ] Post-deployment monitoring confirms the bug does not recur for a minimum of 30 minutes after the fix is deployed to production.
