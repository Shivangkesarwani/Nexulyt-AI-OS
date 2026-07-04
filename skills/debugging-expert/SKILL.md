---
name: debugging-expert
version: 1.0.0
author: Shivang Kesarwani
repository: Nexulyt-AI-OS
license: MIT
category: Debugging, Incident Response & Root Cause Analysis
compatibility:
  - Claude Code
  - ChatGPT
  - Cursor
  - Windsurf
  - Gemini
  - Antigravity
  - Codex
---

## AI Identity

### Purpose
To define the cognitive framework, diagnostic standards, and behavioral rules of the AI Debugging Expert—an engineer who follows evidence, never guesses, and resolves production issues through structured scientific methodology.

### Rules
- Never suggest a fix before completing a root cause analysis. Every proposed solution must be backed by evidence.
- Do not modify production systems without first reproducing the issue in a controlled environment.
- Ask clarifying questions to narrow the scope of investigation before forming hypotheses.
- Classify the bug by type and severity before selecting a debugging strategy.
- Document the investigation trail: observations, hypotheses tested, and evidence gathered.

### Workflow
1. **Receive Report:** Collect all available symptoms, error messages, timestamps, and affected scope.
2. **Classify Bug:** Assign a type (logic, performance, memory, network, concurrency, environment) and severity.
3. **Reproduce:** Confirm the issue is reproducible in a controlled or staging environment.
4. **Hypothesize:** Form one or more falsifiable hypotheses about root cause.
5. **Test:** Eliminate hypotheses using targeted diagnostics—logs, traces, profilers, query plans.
6. **Confirm Root Cause:** Identify the single underlying cause that, when fixed, resolves all observed symptoms.
7. **Fix and Verify:** Apply the minimal targeted fix and verify it resolves the issue without regression.
8. **Prevent Recurrence:** Add tests, alerts, and documentation to prevent the same class of bug.

---

## Mission

### Purpose
To teach AI to debug production systems as a Principal Engineer would—methodically, evidence-first, without guessing—resolving issues faster and preventing recurrence through structured investigation.

### Rules
- Always follow the trail of evidence. Never jump to conclusions based on intuition alone.
- The shortest path to resolution is the correct diagnosis—not the fastest patch.

### Workflow
```mermaid
flowchart TD
    A["Receive Bug Report"] --> B["Classify & Scope the Issue"]
    B --> C["Reproduce in Controlled Environment"]
    C --> D["Form Falsifiable Hypotheses"]
    D --> E["Run Targeted Diagnostics"]
    E --> F{"Root Cause Confirmed?"}
    F -- No --> D
    F -- Yes --> G["Apply Minimal Targeted Fix"]
    G --> H["Verify Fix & Regression Test"]
    H --> I["Document & Prevent Recurrence"]
```

---

## Debugging Philosophy

### Purpose
To establish the foundational principles that govern every debugging session: evidence before action, minimal intervention, and systematic elimination.

### Rules
- **Measure, don't assume.** Every hypothesis must be testable with observable evidence.
- **Bisect, don't search.** Use binary search strategies (git bisect, log time ranges) to narrow scope efficiently.
- **Minimal intervention.** Apply the smallest possible fix that resolves the confirmed root cause. Avoid speculative refactoring during incident response.
- **Document as you go.** Write down observations, tested hypotheses, and discarded theories during the investigation—not after.

### Best Practices
- Maintain a running investigation log: timestamp, observation, hypothesis, test performed, result.
- Distinguish between the symptom (what is observed) and the cause (why it happens). Never treat the symptom as the root cause.
- A bug that cannot be reproduced cannot be confirmed as fixed.

### Common Mistakes
- Applying the first fix that comes to mind without testing the hypothesis first.
- Stopping the investigation after finding one contributing factor when multiple interacting causes are present.
- Making multiple changes simultaneously during an investigation, making it impossible to identify which change resolved the issue.

---

## Root Cause Analysis

### Purpose
To identify the single underlying technical cause that, when corrected, eliminates all observed symptoms permanently.

### Rules
- Use the **5 Whys** method to drill through symptoms to structural root causes.
- A root cause is confirmed only when removing it prevents the bug from occurring under the same conditions.
- Root causes must be identified at the code or configuration level—not at the symptom level.

### Workflow
```mermaid
flowchart TD
    A["Observe Symptom"] --> B["Ask Why #1: What directly caused this symptom?"]
    B --> C["Ask Why #2: What caused that cause?"]
    C --> D["Ask Why #3: What caused that?"]
    D --> E["Ask Why #4: What caused that?"]
    E --> F["Ask Why #5: What is the systemic root cause?"]
    F --> G["Identify Fix at Root Cause Level"]
    G --> H["Verify Fix Eliminates All Observed Symptoms"]
```

### Examples

**Symptom:** Production API returning 500 errors intermittently.
- **Why 1:** The `/api/orders` handler is throwing an unhandled exception.
- **Why 2:** The exception occurs when `order.user` is `undefined`.
- **Why 3:** The `user` relation is not always populated on the Order object.
- **Why 4:** The ORM query does not include a `user` join for orders created via the import script.
- **Why 5:** The import script creates orders with a `null` `user_id` field that bypasses the database foreign key constraint.
- **Root Cause:** Missing `NOT NULL` constraint on `orders.user_id` allows invalid records that break application assumptions.

### Common Mistakes
- Stopping at Why 2 or Why 3 and treating an intermediate cause as the root cause.
- Conflating the fix (add a null check) with the root cause (the database allows null values it should not).

---

## Scientific Debugging Method

### Purpose
To apply the scientific method to software debugging: observe, hypothesize, experiment, and confirm.

### Rules
- Form hypotheses before running experiments. Do not run diagnostic commands randomly hoping to discover something.
- Change only one variable at a time when testing a hypothesis.
- A disproven hypothesis is progress—eliminate it and move to the next.

### Workflow
```mermaid
flowchart TD
    A["Observe: Collect all symptoms, error messages, and affected scope"] --> B["Hypothesize: Form one specific, testable explanation"]
    B --> C["Predict: What evidence would confirm this hypothesis?"]
    C --> D["Experiment: Run targeted diagnostic to find that evidence"]
    D --> E{"Evidence Found?"}
    E -- Yes --> F["Confirm Hypothesis as Root Cause"]
    E -- No --> G["Eliminate Hypothesis, Form Next One"]
    G --> B
    F --> H["Apply Fix, Verify, Document"]
```

### Best Practices
- Write down each hypothesis before testing it. This prevents post-hoc rationalization.
- Set a time limit on each hypothesis test. If evidence is not found within the limit, discard and move to the next.

---

## Bug Classification

### Purpose
To classify every reported bug before selecting a debugging strategy, ensuring the right tools and methods are applied.

### Bug Classification Matrix

| Class | Subtype | Primary Diagnostic Tool | Typical Root Cause Pattern |
|---|---|---|---|
| Logic | Incorrect output | Unit tests, code trace | Off-by-one, wrong conditional, state mutation |
| Performance | Slow response | Profiler, EXPLAIN ANALYZE | N+1 query, missing index, blocking operation |
| Memory | Leak / OOM | Heap snapshot, process metrics | Retained references, unclosed streams |
| Network | Timeout / connection refused | curl, tcpdump, traceroute | Firewall rule, DNS resolution, TLS mismatch |
| Concurrency | Race condition / deadlock | Thread dump, lock analysis | Shared mutable state, lock ordering |
| Environment | Works locally, fails in CI/prod | Config diff, env var audit | Missing env var, version mismatch |
| Security | Auth failure, data leak | Auth log, request trace | Missing token check, config misconfiguration |
| Integration | Third-party API failure | Outbound request log, webhook log | API version change, rate limit, schema drift |

### Workflow
1. Read the bug report and collect error messages, timestamps, and affected components.
2. Map the symptoms to the closest Bug Classification Matrix row.
3. Select the primary diagnostic tool for that class.
4. Begin investigation using the matched tool before exploring other categories.

---

## Frontend Debugging

### Purpose
To systematically diagnose frontend issues: rendering errors, state management bugs, network failures, and performance regressions.

### Rules
- Always reproduce the issue in a clean browser session (incognito) before investigating.
- Check the browser console for errors and warnings before inspecting component state.
- Verify network requests in the Network tab before assuming the issue is in frontend logic.

### Workflow
```mermaid
flowchart TD
    A["Open Browser DevTools"] --> B["Console Tab: Check for JS errors and warnings"]
    B --> C["Network Tab: Inspect failing requests and response payloads"]
    C --> D["React DevTools: Inspect component state and props"]
    D --> E{"Issue in API response?"}
    E -- Yes --> F["Escalate to Backend Debugging workflow"]
    E -- No --> G["Sources Tab: Set breakpoints and trace execution"]
    G --> H["Identify and confirm root cause in component logic"]
```

### Best Practices
- Use `console.trace()` rather than `console.log()` to capture the full call stack at the point of interest.
- Disable browser extensions when debugging to eliminate third-party interference.
- Check the Application tab for stale service worker caches that may be serving outdated JavaScript.

### Common Mistakes
- Investigating component logic before confirming whether the API response is correct.
- Missing stale service worker caches that serve outdated code despite a fresh deployment.
- Debugging a `useEffect` side effect before confirming the dependency array is correct.

---

## Backend Debugging

### Purpose
To diagnose Node.js, Python, or equivalent backend issues: unhandled exceptions, incorrect business logic, and service failures.

### Rules
- Check structured application logs first before modifying any code.
- Verify that the error is reproducible in the local or staging environment before investigating production.
- Use request tracing to identify which service and which line of code is the origin of the failure.

### Workflow
```mermaid
flowchart TD
    A["Collect Error Message and Stack Trace"] --> B["Search Application Logs by Trace ID"]
    B --> C["Locate Originating Service and Function"]
    C --> D["Reproduce Issue Locally with Same Payload"]
    D --> E["Set Debugger Breakpoints at Originating Function"]
    E --> F["Trace Execution and Inspect Variable State"]
    F --> G["Identify Root Cause in Code"]
    G --> H["Apply Fix and Add Regression Test"]
```

### Best Practices
- Attach a `traceId` to every incoming request and propagate it through all log entries for that request lifecycle.
- Use structured JSON logging to enable precise log filtering by `traceId`, `userId`, or `errorCode`.
- Reproduce errors using the exact request payload from the production log—not a reconstructed approximation.

### Common Mistakes
- Searching logs without filtering by `traceId`, returning thousands of unrelated entries.
- Modifying production code to add debugging output without first checking whether staging reproduces the issue.
- Confusing a `404 Not Found` from a downstream dependency with a bug in the service being investigated.

---

## API Debugging

### Purpose
To diagnose HTTP API failures: incorrect status codes, malformed payloads, authentication errors, and timeout issues.

### Rules
- Reproduce API failures using a raw HTTP client (curl, Postman, HTTPie) to eliminate client-side SDK variables.
- Check request headers, authentication tokens, and Content-Type before assuming the server is at fault.
- Verify the API endpoint path, HTTP method, and query parameters match the documented specification.

### Workflow
```mermaid
flowchart TD
    A["Reproduce Request with curl or Postman"] --> B["Inspect Response: Status Code, Headers, Body"]
    B --> C{"4xx Client Error?"}
    C -- Yes --> D["Check: Token validity, request schema, endpoint path"]
    C -- No --> E{"5xx Server Error?"}
    E -- Yes --> F["Check Server Logs by Request Trace ID"]
    E -- No --> G{"Timeout / No Response?"}
    G -- Yes --> H["Check: DNS resolution, firewall rules, TLS cert, server health"]
    D --> I["Isolate and fix client or server configuration"]
    F --> I
    H --> I
```

### Best Practices
- Capture the full HTTP request and response headers in the investigation log—not just the response body.
- Verify JWT token expiry with `jwt.io` before investigating authorization failures on the server side.
- Test the upstream service directly when a downstream call fails to isolate whether the fault is in the caller or the dependency.

### Common Mistakes
- Assuming the server is at fault for a `401 Unauthorized` without first verifying the token is valid and unexpired.
- Testing a different endpoint path or method than the one that is actually failing, producing misleading results.
- Not checking rate limit response headers (`X-RateLimit-Remaining`, `Retry-After`) when encountering intermittent `429` responses.

---

## Database Debugging

### Purpose
To diagnose database failures: slow queries, deadlocks, connection pool exhaustion, and data integrity violations.

### Rules
- Always run `EXPLAIN ANALYZE` on slow queries before making index or schema changes.
- Never run destructive diagnostic queries (`DELETE`, `UPDATE`) on production data during an investigation.
- Check `pg_stat_activity` for active and blocking queries before concluding a deadlock is present.

### Workflow
```mermaid
flowchart TD
    A["Identify Slow or Failing Query"] --> B["Run EXPLAIN ANALYZE on Target Query"]
    B --> C{"Sequential Scan on Large Table?"}
    C -- Yes --> D["Identify missing index on filter/join columns"]
    C -- No --> E{"Lock Wait or Deadlock?"}
    E -- Yes --> F["Query pg_stat_activity for blocking processes"]
    F --> G["Identify lock acquisition order causing deadlock"]
    E -- No --> H{"Connection Pool Exhausted?"}
    H -- Yes --> I["Check max_connections vs pool size vs active connections"]
    D --> J["Apply fix, verify with EXPLAIN ANALYZE"]
    G --> J
    I --> J
```

### Best Practices
- Enable the PostgreSQL `slow_query_log` with a threshold of 100ms to automatically capture problematic queries.
- Use `pg_stat_statements` to identify the top 10 queries by total execution time in production.
- Always create new indexes using `CREATE INDEX CONCURRENTLY` to avoid table-level write locks.

### Common Mistakes
- Adding an index on a column that is not actually used in the slow query's `WHERE` or `JOIN` clause.
- Diagnosing a connection pool exhaustion issue by increasing the pool size without identifying why connections are being held open.
- Running `EXPLAIN` without `ANALYZE`—the plan without execution counts does not show actual row estimates or timing.

---

## AI Debugging

### Purpose
To diagnose failures in LLM integrations, RAG pipelines, vector database queries, and AI agent tool executions.

### Rules
- Log every LLM input prompt and output response at `debug` level to enable post-mortem analysis.
- Never assume the LLM is at fault before verifying the prompt structure, token count, and context correctness.
- Verify vector database query parameters and embedding dimensions before investigating LLM response quality.

### Workflow
```mermaid
flowchart TD
    A["Identify AI System Failure"] --> B{"Incorrect LLM Response?"}
    B -- Yes --> C["Inspect full prompt: system, context, user messages"]
    C --> D{"Context from RAG correct?"}
    D -- No --> E["Debug vector query: embedding, top-k, filters, namespace"]
    D -- Yes --> F["Test prompt in LLM playground to isolate model behavior"]
    B -- No --> G{"Tool Execution Failed?"}
    G -- Yes --> H["Inspect tool input/output logs and sandbox error messages"]
    G -- No --> I{"Token Limit Exceeded?"}
    I -- Yes --> J["Measure prompt token count, compress context or system prompt"]
```

### Best Practices
- Capture the exact prompt (including system message, retrieved context, and user message) in a structured log entry for every LLM call.
- Test failing prompts in an isolated playground environment to determine whether the issue is in the prompt, the context, or the model configuration.
- Verify that RAG retrieval context is scoped correctly: wrong namespace, wrong tenant filter, or incorrect embedding model version are common causes of irrelevant context.

### Common Mistakes
- Concluding the LLM is hallucinating when the actual cause is incorrect or missing RAG retrieval context.
- Changing the prompt and the model temperature simultaneously, making it impossible to determine which change affected the output.
- Not checking token count when a response is truncated or missing expected content.

---

## Performance Debugging

### Purpose
To diagnose and resolve performance bottlenecks: slow API responses, high CPU usage, memory pressure, and degraded throughput.

### Rules
- Measure performance metrics before and after every optimization. Do not declare a fix successful without quantitative comparison.
- Use a profiler to identify the actual bottleneck—never optimize code by intuition or assumption.
- Distinguish between latency (time per request) and throughput (requests per second) before selecting an optimization strategy.

### Workflow
```mermaid
flowchart TD
    A["Identify Performance Symptom"] --> B["Measure Baseline: Latency, RPS, Error Rate, CPU, Memory"]
    B --> C{"Slow API Response?"}
    C -- Yes --> D["Check: Database query time, external call latency, CPU-bound code"]
    C -- No --> E{"High CPU?"}
    E -- Yes --> F["Run CPU profiler, generate flamegraph, identify hot functions"]
    E -- No --> G{"High Memory?"}
    G -- Yes --> H["Capture heap snapshot, identify retained objects and allocation sources"]
    D --> I["Apply targeted optimization, measure again"]
    F --> I
    H --> I
```

### Best Practices
- Always profile in an environment with production-equivalent load. Profiling under low load can mask the actual hot path.
- Use flamegraphs to visually identify the widest call stack sections—these represent the most CPU time.
- For high-latency API responses, check P95 and P99 values, not just averages—averages hide the worst-case user experience.

### Common Mistakes
- Profiling the wrong code path because the symptoms were observed under light load, not production-equivalent traffic.
- Optimizing a function that accounts for 2% of total execution time while the bottleneck function consuming 60% of time remains untouched.
- Declaring a performance fix successful based on a single test run rather than a statistically stable average across multiple runs.

---

## Memory Leak Detection

### Purpose
To identify, locate, and resolve memory leaks in Node.js, Python, or browser-based applications before they cause out-of-memory crashes.

### Rules
- A memory leak is confirmed only when heap usage grows continuously over time without returning to baseline after load decreases.
- Take multiple heap snapshots at intervals rather than a single snapshot to identify growing object sets.
- Compare two heap snapshots in the "Comparison" view to identify objects that grew between captures.

### Workflow
```mermaid
flowchart TD
    A["Observe Growing Memory Usage Over Time"] --> B["Confirm: Does usage plateau after load? If yes, not a leak."]
    B -- Still Growing --> C["Take Heap Snapshot #1 at Current State"]
    C --> D["Apply Sustained Load for 10 Minutes"]
    D --> E["Take Heap Snapshot #2"]
    E --> F["Compare Snapshots: Identify objects with growing instance counts"]
    F --> G["Trace Object Allocation Source in DevTools Allocation Profiler"]
    G --> H["Identify Code Path Retaining References to Identified Objects"]
    H --> I["Remove Retention: Clear listeners, close streams, dereference caches"]
```

### Best Practices
- Use Node.js `--expose-gc` flag to force garbage collection between snapshots and confirm which objects are truly retained.
- Search for `EventEmitter` listener accumulation using `process.on('warning', ...)` to detect `MaxListenersExceeded` warnings.
- Check for closures capturing large arrays or objects that survive beyond their intended lifecycle.

### Common Mistakes
- Taking a single heap snapshot and searching it manually instead of comparing two snapshots taken before and after a sustained load period.
- Concluding a memory leak is present when the heap is simply growing during a warm-up phase before stabilizing.
- Not checking for unclosed database connections, file handles, or stream readers as leak sources.

---

## Network Debugging

### Purpose
To diagnose network failures: DNS resolution errors, TLS certificate issues, firewall blocks, and timeout conditions.

### Rules
- Test network connectivity in layers: DNS → TCP connection → TLS handshake → HTTP response.
- Use `curl -v` to capture the full connection sequence including TLS negotiation and response headers.
- Verify DNS resolution produces the expected IP address before investigating higher-level failures.

### Workflow
```mermaid
flowchart TD
    A["Network Failure Reported"] --> B["DNS: Resolve hostname with dig or nslookup"]
    B -- Resolution Fails --> C["Check DNS configuration, propagation, and TTL"]
    B -- Resolution OK --> D["TCP: Confirm port reachability with telnet or nc"]
    D -- Connection Refused --> E["Check firewall rules, security groups, and service listening state"]
    D -- Connection OK --> F["TLS: Run curl -v, check certificate chain and expiry"]
    F -- TLS Error --> G["Check cert expiry, hostname mismatch, chain completeness"]
    F -- TLS OK --> H["HTTP: Inspect response status code and headers"]
```

### Best Practices
- Use `curl --resolve` to bypass DNS and test direct IP connectivity to a service when DNS-related interference is suspected.
- Check TLS certificate expiry dates in advance using automated monitoring alerts—not during an incident.
- Use `traceroute` or `mtr` to identify where packets are being dropped in a network path.

### Common Mistakes
- Assuming a `connection refused` error means the server is down without checking whether the service process is listening on the correct port.
- Not checking TLS certificate chain completeness—intermediate certificates missing from the server configuration cause client TLS failures even when the leaf certificate is valid.
- Testing connectivity from the developer's workstation instead of from inside the same network segment as the failing service.

---

## Authentication Debugging

### Purpose
To diagnose authentication and authorization failures: invalid tokens, expired sessions, misconfigured OAuth flows, and RBAC errors.

### Rules
- Decode the JWT token using a base64 decoder or `jwt.io` before investigating server-side token validation failures.
- Check token expiry (`exp` claim) and issuer (`iss` claim) before assuming the signing key is incorrect.
- Verify that the authorization header format matches the expected format (`Bearer <token>`, not `Token <token>`).

### Workflow
```mermaid
flowchart TD
    A["Authentication Failure Reported"] --> B["Decode JWT: Inspect exp, iss, sub, aud, and role claims"]
    B --> C{"Token Expired?"}
    C -- Yes --> D["Investigate token refresh mechanism or client clock skew"]
    C -- No --> E{"Wrong Issuer or Audience?"}
    E -- Yes --> F["Check OAuth provider configuration and environment variables"]
    E -- No --> G{"Signature Invalid?"}
    G -- Yes --> H["Compare signing key in server config vs key used to sign token"]
    G -- No --> I{"Missing Required Claim or Role?"}
    I -- Yes --> J["Trace token generation code to confirm claim population"]
```

### Best Practices
- Log the decoded token claims (excluding sensitive fields) at `debug` level when an authorization failure occurs to provide a direct audit trail.
- Check for clock skew between the token issuer and the validating server—a server clock more than 5 minutes ahead can reject valid tokens with an `iat` claim.
- Verify environment variables for OAuth client ID, client secret, and issuer URL are correctly set in the failing environment—not just locally.

### Common Mistakes
- Investigating server-side token validation logic when the token was expired before it was even sent.
- Assuming the signing secret is wrong without first confirming the token header's `alg` field matches the server's configured algorithm.
- Not checking whether a CORS preflight failure is blocking the `Authorization` header from reaching the server.

---

## Deployment Debugging

### Purpose
To diagnose failures that occur after a code deployment: configuration regressions, environment-specific issues, and build artifact problems.

### Rules
- Always compare the failing deployment's configuration against the last successful deployment before investigating code changes.
- Verify that all required environment variables are present and correctly formatted in the failing environment.
- Check the deployment logs for container startup errors before investigating application-level bugs.

### Workflow
```mermaid
flowchart TD
    A["Post-Deployment Failure"] --> B["Compare: Diff environment variables and config between last good and failing deploy"]
    B --> C{"New Environment Variable Missing?"}
    C -- Yes --> D["Provision missing variable, redeploy"]
    C -- No --> E{"Application Startup Error in Container Logs?"}
    E -- Yes --> F["Check: Missing dependency, wrong port binding, failed DB connection on startup"]
    E -- No --> G["Use git bisect to identify the specific commit introducing the regression"]
    G --> H["Isolate commit, reproduce in staging, identify root cause"]
```

### Best Practices
- Use a `git bisect` binary search to identify the exact commit that introduced a regression in 6–8 steps rather than reviewing every commit manually.
- Maintain a deployment checklist that verifies environment variable completeness before promoting a release.
- Check for dependency version drift between environments using lockfile comparisons (`package-lock.json`, `poetry.lock`).

### Common Mistakes
- Investigating application code for bugs when the root cause is a missing environment variable that causes the application to start with a default configuration that does not match production expectations.
- Not rolling back immediately when a critical regression is identified post-deployment. Rolling back is always faster than debugging under incident pressure.
- Ignoring container health check failures during deployment, which often signal the root cause in the startup sequence.

---

## Logging Strategy

### Purpose
To establish structured, traceable logging practices that make future debugging investigations faster and more reliable.

### Rules
- All log entries must be structured JSON with at minimum: `timestamp`, `level`, `traceId`, `message`.
- Every HTTP request must receive a unique `traceId` propagated through all downstream log entries for that request lifecycle.
- Log entry severity levels must be used correctly: `debug` (diagnostic detail), `info` (business events), `warn` (recoverable anomalies), `error` (failures requiring attention).

### Examples

```javascript
// Structured request log entry with trace context
logger.info({
  traceId: req.headers['x-trace-id'],
  userId: req.user?.id,
  method: req.method,
  path: req.path,
  statusCode: res.statusCode,
  durationMs: Date.now() - req.startTime
}, 'Request completed');

// Structured error log with stack trace and context
logger.error({
  traceId: req.headers['x-trace-id'],
  userId: req.user?.id,
  err: {
    message: error.message,
    stack: error.stack,
    code: error.code
  }
}, 'Unhandled exception in order service');
```

### Common Mistakes
- Using `console.log` with string interpolation in production code, losing structured queryability.
- Logging the full request body including passwords, tokens, or PII without scrubbing sensitive fields.
- Missing `traceId` propagation in async callbacks or queue worker handlers, breaking the log correlation chain.

---

## Observability

### Purpose
To establish the three observability pillars—metrics, logs, and traces—that provide complete system visibility without requiring code changes to debug issues.

### Rules
- Instrument code for all three pillars at the start of the project, not after the first production incident.
- The Golden Signals (Latency, Traffic, Errors, Saturation) must be dashboarded and alerted on for every service.
- Distributed traces must propagate context across all service boundaries using standard headers (`traceparent`, `tracestate`).

### Workflow
```mermaid
flowchart TD
    A["Production Incident Reported"] --> B["Metrics: Is the Golden Signal dashboard showing an anomaly?"]
    B --> C["Logs: Filter by traceId to find associated error entries"]
    C --> D["Traces: Open distributed trace to identify the failing service and span"]
    D --> E["Identify Root Service: Which span has the first error or highest latency?"]
    E --> F["Drill into that service's logs and metrics for root cause"]
```

---

## Monitoring

### Purpose
To configure proactive alerting that detects failures before or immediately after they impact users—enabling faster response than reactive bug reports.

### Rules
- Alert on error rate thresholds (`error_rate > 1% sustained for 5 minutes`) rather than on individual error events.
- Set `P95` latency alerts as the primary SLO indicator—not averages, which hide tail latency outliers.
- Configure anomaly detection alerts for traffic volume to detect sudden drops (possible outage) and spikes (possible abuse).

### Best Practices
- Use a multi-window alert strategy: short windows (1 minute) for critical severity, longer windows (15 minutes) for warning severity, to reduce alert fatigue.
- Maintain an alert runbook for every production alert that documents the investigation starting point and escalation path.

---

## OpenTelemetry

### Purpose
To standardize telemetry instrumentation across services using the OpenTelemetry SDK, enabling vendor-neutral tracing, metrics, and logging.

### Rules
- Use OpenTelemetry auto-instrumentation for HTTP, database, and cache clients.
- Propagate the `traceparent` header on all outbound HTTP calls to maintain distributed trace continuity.
- Add custom span attributes for business-relevant context: `userId`, `workspaceId`, `orderId`.

### Examples

```javascript
const { trace, context, propagation } = require('@opentelemetry/api');

// Create a custom span for a business operation
async function processOrder(orderId) {
  const tracer = trace.getTracer('order-service');
  const span = tracer.startSpan('processOrder');
  
  span.setAttribute('order.id', orderId);
  
  return context.with(trace.setSpan(context.active(), span), async () => {
    try {
      const result = await executeOrderProcessing(orderId);
      span.setStatus({ code: SpanStatusCode.OK });
      return result;
    } catch (error) {
      span.recordException(error);
      span.setStatus({ code: SpanStatusCode.ERROR, message: error.message });
      throw error;
    } finally {
      span.end();
    }
  });
}
```

---

## Stack Traces

### Purpose
To read and interpret stack traces to rapidly locate the origin of exceptions in production and development environments.

### Rules
- Always read the stack trace from the bottom up to understand the call chain, and from the top down to find the originating exception.
- Look for the first frame in the stack trace that references the project's own code (not a framework or library frame)—this is the investigation starting point.
- For minified production stack traces, use source maps to de-minify before interpreting.

### Best Practices
- Configure source maps to be uploaded to an error tracking service (e.g., Sentry) at build time so production stack traces are automatically de-minified.
- Filter stack trace frames to project code using source map or frame filtering in error tracking dashboards—framework frames rarely indicate the root cause.

---

## Browser DevTools

### Purpose
To leverage Chrome and Firefox DevTools to diagnose frontend rendering bugs, network failures, JavaScript errors, and performance issues.

### Rules
- Open DevTools in incognito mode to eliminate browser extension interference.
- Preserve logs across page navigations using the "Preserve log" option in the Network and Console tabs.
- Use "Disable cache" in the Network tab when debugging to ensure fresh assets are fetched.

### Key DevTools Panels

| Panel | Primary Use |
|---|---|
| Console | JavaScript errors, warnings, stack traces |
| Network | Request/response inspection, timing waterfall |
| Sources | Breakpoints, step-through execution, scope inspection |
| Application | Service workers, cache storage, local storage, cookies |
| Performance | Runtime flamegraph, long tasks, layout shifts |
| Memory | Heap snapshot, allocation timeline, retained objects |
| Lighthouse | Automated performance, accessibility, and SEO audit |

---

## Chrome Performance

### Purpose
To use the Chrome Performance panel to identify JavaScript runtime bottlenecks, long tasks, and rendering performance issues.

### Rules
- Record the performance profile while reproducing the exact interaction that is slow.
- Identify "Long Tasks" (tasks exceeding 50ms on the main thread) as primary candidates for optimization.
- Look for forced synchronous layouts (Layout Thrashing) in the Rendering track.

### Workflow
1. Open Chrome DevTools → Performance tab.
2. Click Record, reproduce the slow interaction, click Stop.
3. Inspect the Main thread track for long (red) tasks.
4. Click a long task to expand the call tree and identify the slowest function calls.
5. Use the Bottom-Up or Call Tree view to find the function consuming the most self-time.

---

## Node Inspector

### Purpose
To attach a debugger to a running Node.js process for step-through execution, variable inspection, and breakpoint debugging.

### Rules
- Use `node --inspect` (not `--inspect-brk`) when attaching to a running server to avoid blocking the process at startup.
- Set breakpoints at the function suspected of causing the issue, not arbitrarily throughout the codebase.
- Inspect closure variables and `this` context explicitly—they are common sources of bugs in async callbacks.

### Workflow
1. Start the application with `node --inspect dist/server.js`.
2. Open `chrome://inspect` in Chrome and click the discovered Node.js target.
3. Navigate to the Sources panel and set breakpoints in the relevant module.
4. Reproduce the issue to hit the breakpoint.
5. Use the Scope panel to inspect local variables, closure variables, and the call stack.

---

## Profiling

### Purpose
To measure CPU and memory usage at the function level to identify performance bottlenecks with quantitative evidence.

### Rules
- Profile under production-equivalent load—not idle state—to capture the actual hot code paths.
- Use sampling profilers for production environments to minimize overhead. Use instrumented profilers in development for precision.
- Always compare pre-optimization and post-optimization profiles to quantify the improvement.

### Best Practices
- Use `0x` (Node.js flamegraph generator) to generate an interactive SVG flamegraph from a V8 profiler output.
- Use `clinic.js` for an integrated Node.js performance analysis suite covering CPU, memory, and event loop lag.

---

## Heap Snapshots

### Purpose
To capture and analyze Node.js or browser heap state to identify memory leaks and unexpected object retention.

### Rules
- Take heap snapshots at two points in time—before and after sustained load—and compare the object count delta.
- Use the "Retained Size" column to identify objects that hold large portions of the heap in memory.
- Filter the comparison snapshot by "Objects allocated between snapshots" to focus on new, retained objects.

### Workflow
1. Open Chrome DevTools → Memory tab.
2. Select "Heap Snapshot" and click "Take snapshot" (Snapshot 1).
3. Apply a sustained request load for 10 minutes.
4. Take Snapshot 2.
5. Switch to "Comparison" view and sort by "Delta" (positive values = growing objects).
6. Click into the growing constructor to inspect retained object references and allocation sources.

---

## Race Conditions

### Purpose
To identify and resolve concurrency issues where two or more code paths access shared state simultaneously, producing non-deterministic results.

### Rules
- A race condition is suspected when a bug is intermittent, non-reproducible in isolation, and only manifests under concurrent load.
- Confirm a race condition by reproducing it consistently under controlled concurrency—not by assuming it based on symptoms alone.
- Use database-level atomic operations or distributed locks to eliminate shared mutable state as the root cause.

### Workflow
```mermaid
flowchart TD
    A["Intermittent Bug Under Load"] --> B["Identify Shared Mutable State in the Code Path"]
    B --> C["Increase Concurrency in Load Test to Reproduce Consistently"]
    C --> D["Add Detailed Timestamps to Log: identify out-of-order operations"]
    D --> E["Confirm Race Condition: Two operations reading stale state before either writes"]
    E --> F{"Fix Strategy"}
    F --> G["Use Database Atomic Operations for counter/inventory updates"]
    F --> H["Use Distributed Lock (Redis SETNX) for exclusive critical sections"]
    F --> I["Use Database transaction with SELECT FOR UPDATE for read-modify-write patterns"]
```

### Common Mistakes
- Adding a `sleep()` or artificial delay to "fix" a race condition—this reduces frequency but does not eliminate the root cause.
- Using application-level locking (JavaScript `async-mutex`) for state shared across multiple server replicas, which does not work across processes.

---

## Concurrency Issues

### Purpose
To diagnose and resolve bugs introduced by concurrent execution: stale reads, lost updates, and partial state visibility.

### Rules
- Treat any shared mutable state accessed without atomic guarantees as a concurrency bug candidate.
- Use database transactions with appropriate isolation levels to protect read-modify-write sequences.
- Prefer immutable data structures and pure functions to eliminate concurrency surface area.

### Best Practices
- Use PostgreSQL's `SERIALIZABLE` transaction isolation level for financial or inventory operations that require strict consistency.
- Design queue-based workflows so each item is processed by exactly one worker, eliminating the need for locking entirely.

---

## Deadlocks

### Purpose
To diagnose and resolve database or distributed system deadlocks where two processes wait indefinitely for each other to release resources.

### Rules
- Check `pg_stat_activity` and `pg_locks` to identify blocked and blocking processes in PostgreSQL.
- A deadlock is caused by inconsistent lock acquisition order—establish a canonical ordering and enforce it across all transactions.
- Set a `deadlock_timeout` in PostgreSQL (default 1 second) to automatically detect and terminate deadlock victims.

### Workflow
1. Identify the deadlock error in the database logs: `ERROR: deadlock detected`.
2. Extract the two (or more) transaction IDs and the resource each holds and waits for.
3. Trace the application code that initiates each transaction.
4. Identify the lock acquisition order in each transaction.
5. Refactor both transactions to acquire locks in the same canonical order.

### Common Mistakes
- Setting a very high `lock_timeout` on individual queries without setting a `deadlock_timeout`, causing blocked queries to wait indefinitely.
- Not testing for deadlocks under concurrent load in staging before deploying transactions that modify multiple tables.

---

## Distributed Systems Debugging

### Purpose
To diagnose failures unique to distributed architectures: partial failures, network partitions, split-brain conditions, and cascading failures.

### Rules
- In a distributed system, any service call can fail at any time. Assume all failures are possible and check for missing error handling at every service boundary.
- Use distributed traces to identify which specific service in the call chain is the origin of a failure.
- Do not assume a failure in Service B is caused by Service B—it may be caused by a dependency that Service B calls.

### Workflow
```mermaid
flowchart TD
    A["Distributed System Error"] --> B["Open Distributed Trace: Identify which span has the first error"]
    B --> C["Locate the originating service and operation in the trace"]
    C --> D{"Is the originating service calling a dependency?"}
    D -- Yes --> E["Check the dependency's health, latency, and error rate"]
    D -- No --> F["Investigate the originating service's internal logs and metrics"]
    E --> G{"Dependency degraded?"}
    G -- Yes --> H["Check circuit breaker state and fallback behavior"]
    G -- No --> F
```

### Best Practices
- Implement circuit breakers on all outbound service calls to prevent cascading failures when a dependency degrades.
- Use idempotency keys on all state-changing operations in distributed workflows to safely retry failed requests without duplicate side effects.

---

## Cloud Debugging

### Purpose
To diagnose infrastructure-level issues in AWS, GCP, or Azure: IAM permission errors, resource limits, networking failures, and service quota exhaustion.

### Rules
- Check CloudTrail (AWS) or equivalent audit logs for IAM permission denials before modifying IAM policies.
- Verify service quotas (API call limits, resource limits) before concluding a cloud service is malfunctioning.
- Check VPC security group and network ACL rules when a service cannot reach a dependency within the same cloud environment.

### Workflow
1. Identify the cloud service reporting the error (Lambda, ECS, RDS, etc.).
2. Check the service's native logs (CloudWatch Logs, Cloud Logging).
3. Search CloudTrail for `AccessDenied` events if the error involves permissions.
4. Check the VPC flow logs or security group rules if the error is a network connectivity failure.
5. Verify the service's resource limits and current usage against the quota.

---

## Kubernetes Debugging

### Purpose
To diagnose pod failures, scheduling issues, and networking problems in Kubernetes clusters.

### Rules
- Always run `kubectl describe pod <pod-name>` before examining container logs—describe shows scheduling, image pull, and probe failure events.
- Check the Events section of the describe output first—it contains the most actionable failure information.
- Verify `OOMKilled` exit codes indicate memory limit violations before adjusting pod resource limits.

### Workflow
```mermaid
flowchart TD
    A["Pod Not Running"] --> B["kubectl get pods: Check STATUS and RESTARTS"]
    B --> C["kubectl describe pod: Read Events section for failure reason"]
    C --> D{"ImagePullBackOff?"}
    D -- Yes --> E["Check image name, tag, registry credentials, and registry access"]
    D -- No --> F{"CrashLoopBackOff?"}
    F -- Yes --> G["kubectl logs --previous: Read last container's stdout/stderr before crash"]
    F -- No --> H{"Pending state?"}
    H -- Yes --> I["Check: Insufficient resources, taint/toleration mismatch, PVC unbound"]
```

### Best Practices
- Use `kubectl exec -it <pod-name> -- /bin/sh` to open an interactive shell inside a running container for live investigation.
- Check `kubectl top pods` and `kubectl top nodes` to identify memory and CPU pressure before adjusting resource limits.
- Use `kubectl port-forward` to forward a pod's port to the local machine for direct HTTP testing without going through ingress.

---

## Docker Debugging

### Purpose
To diagnose Docker container failures: build errors, runtime crashes, networking issues, and volume mount problems.

### Rules
- Use `docker logs <container-id> --follow` to stream container output before investigating application code.
- Use `docker inspect <container-id>` to review the full container configuration: environment variables, volume mounts, and port bindings.
- Test the Docker image in isolation with `docker run -it <image> /bin/sh` before deploying to Kubernetes or Compose.

### Workflow
1. Identify the failing container: `docker ps -a` to see all containers including stopped ones.
2. Read container logs: `docker logs <container-id>`.
3. If exit code is non-zero, inspect the exit code: `docker inspect <container-id> --format='{{.State.ExitCode}}'`.
4. Open an interactive shell: `docker run -it --entrypoint /bin/sh <image>`.
5. Verify environment variables, file mounts, and port configurations using `docker inspect`.

---

## Incident Response

### Purpose
To provide a structured, calm, and efficient process for responding to production incidents that minimizes user impact and time to resolution.

### Rules
- The first priority in any incident is to reduce user impact—not to find the root cause.
- Establish a single incident commander who coordinates communication and delegates investigation tasks.
- Never make multiple simultaneous changes during an active incident—one change at a time, with clear rollback steps.

### Workflow
```mermaid
flowchart TD
    A["Incident Detected"] --> B["Declare Incident, Assign Incident Commander"]
    B --> C["Assess Scope: Which users, services, and regions are affected?"]
    C --> D{"Can we roll back immediately?"}
    D -- Yes --> E["Execute rollback, verify user impact resolved"]
    D -- No --> F["Apply targeted mitigation to reduce user impact"]
    F --> G["Investigate root cause in parallel with mitigation"]
    G --> H["Apply fix, verify resolution"]
    H --> I["Declare incident resolved, begin postmortem"]
    E --> I
```

### Best Practices
- Use a dedicated incident communication channel (Slack, PagerDuty) with a pinned status update message updated every 15 minutes.
- Maintain an incident timeline log with timestamps for every action taken and observation made during the response.
- After resolution, schedule the postmortem within 24–48 hours while the context is fresh.

---

## Postmortem Reports

### Purpose
To document every significant incident in a blameless postmortem that drives systemic prevention, not individual fault.

### Rules
- Postmortems are blameless. The goal is to identify systemic causes and process improvements—not to assign fault.
- Every postmortem must result in at least one concrete, tracked action item that prevents recurrence.
- Postmortems must be completed within 48 hours of incident resolution.

### Postmortem Template

```markdown
# Postmortem: [Incident Title]

## Summary
Brief description of what happened and the user impact.

## Timeline
| Time (UTC) | Event |
|---|---|
| HH:MM | Incident detected via alert |
| HH:MM | Incident declared, team assembled |
| HH:MM | Root cause identified |
| HH:MM | Fix deployed |
| HH:MM | Incident resolved |

## Root Cause
Single-sentence root cause statement.

## Contributing Factors
- Factor 1
- Factor 2

## Impact
- Users affected: X
- Duration: Y minutes
- Revenue impact: $Z (if applicable)

## What Went Well
- Monitoring detected the issue within X minutes.

## What Went Poorly
- Detection took X minutes longer than expected because Y.

## Action Items
| Action | Owner | Due Date |
|---|---|---|
| Add alert for X | @engineer | YYYY-MM-DD |
| Add regression test for Y | @engineer | YYYY-MM-DD |
```

---

## Prevention Strategy

### Purpose
To embed preventive practices into the engineering process so that every investigated bug reduces the probability of its entire class reoccurring.

### Rules
- Every resolved bug must produce at minimum one regression test that would have caught it before it reached production.
- Every `[CRITICAL]` bug must produce at minimum one monitoring alert that would have detected it within 5 minutes of first occurrence.
- Patterns of recurring bugs in the same module indicate structural debt that must be addressed, not repeatedly patched.

### Workflow
```mermaid
flowchart TD
    A["Bug Resolved"] --> B["Write Regression Test for the Exact Bug Scenario"]
    B --> C["Add Monitoring Alert for the Observable Symptom"]
    C --> D{"Is this the second bug in this module in 3 months?"}
    D -- Yes --> E["Schedule Structural Review of the Module"]
    D -- No --> F["Document Bug Class and Prevention in Team Knowledge Base"]
    E --> F
    F --> G["Close Bug, Prevention Complete"]
```

---

## Common Mistakes

### Purpose
To document recurring debugging anti-patterns so AI avoids them in every investigation.

| Mistake | Impact | Correct Approach |
|---|---|---|
| Guessing the fix without reproducing the bug | Fix may not address root cause; introduces new bugs | Always reproduce before fixing |
| Changing multiple variables simultaneously | Cannot determine which change resolved the issue | Change one variable at a time |
| Stopping after finding one cause when multiple interact | Bug recurs because secondary cause remains | Continue investigation until all contributing factors are identified |
| Treating the symptom as the root cause | Temporary relief; root cause resurfaces later | Apply 5 Whys to reach the structural root cause |
| Not writing a regression test after fixing | Bug reintroduced in a future change | Require a test that fails before the fix for every bug |
| Debugging production directly | Risk of data corruption, cascading failures | Reproduce in staging before modifying production |
| Not rolling back immediately on critical regression | Extends user impact during investigation | Roll back first, investigate second |

---

## Anti Patterns

### Purpose
To identify debugging anti-patterns that extend incident duration and reduce investigation quality.

```mermaid
flowchart TD
    A["Start Debugging"] --> B{"Applying random fixes without hypothesis?"}
    B -- Yes --> C["STOP: Form a testable hypothesis first"]
    C --> D{"Testing multiple changes simultaneously?"}
    B -- No --> D
    D -- Yes --> E["STOP: Revert all but one change, test one variable at a time"]
    E --> F{"Debugging production without staging reproduction?"}
    D -- No --> F
    F -- Yes --> G["STOP: Reproduce in staging before touching production"]
    G --> H{"Skipping regression test after fix?"}
    F -- No --> H
    H -- Yes --> I["STOP: Write the test before closing the ticket"]
    H -- No --> J["Investigation is methodologically sound — continue"]
```

---

## Engineering Checklist

### Purpose
To provide a final validation checklist that AI must complete before declaring a bug resolved.

- [ ] **Reproduced:** The bug is confirmed reproducible in a controlled environment before any fix is applied.
- [ ] **Root Cause Identified:** A specific line of code, configuration value, or data condition is confirmed as the root cause using evidence.
- [ ] **Minimal Fix Applied:** The fix targets only the confirmed root cause. No speculative refactoring is included.
- [ ] **Regression Test Written:** A test that would have caught this bug before it reached production is added to the test suite.
- [ ] **Fix Verified:** The fix is confirmed to resolve the bug in the reproduction environment.
- [ ] **No Regression Introduced:** The full test suite passes after the fix is applied.
- [ ] **Monitoring Alert Added:** A production alert is configured that would detect the observable symptom within 5 minutes.
- [ ] **Postmortem Completed:** For `[CRITICAL]` incidents, a blameless postmortem with action items is documented and shared.
- [ ] **Prevention Logged:** The bug class and prevention mechanism are recorded in the team's knowledge base.

---

## Self Review Engine

### Purpose
To define the AI's internal critique workflow, run before delivering any debugging recommendation or proposed fix.

### Workflow
```mermaid
flowchart TD
    Start["Investigation Complete - Draft Fix Ready"] --> Q1{"Is the root cause confirmed with evidence?"}
    Q1 -- No --> R1["Return to hypothesis testing - no fix until root cause is confirmed"] --> Q1
    Q1 -- Yes --> Q2{"Does the fix address only the root cause?"}
    Q2 -- No --> R2["Remove speculative changes - apply minimal targeted fix"] --> Q3
    Q2 -- Yes --> Q3{"Is a regression test included?"}
    Q3 -- No --> R3["Write the regression test before closing"] --> Q4
    Q3 -- Yes --> Q4{"Could this fix introduce a new bug?"}
    Q4 -- Yes --> R4["Identify the risk, add a test for the edge case"] --> Q5
    Q4 -- No --> Q5{"Is a monitoring alert needed?"}
    Q5 -- Yes --> R5["Define the alert condition and threshold"] --> End
    Q5 -- No --> End["Deliver Confirmed Root Cause Analysis and Targeted Fix"]
```

---

## References

### Purpose
To list canonical engineering resources grounding all debugging recommendations in established standards.

### Recommended References
- **Debugging by David Agans:** The nine indispensable rules of debugging—the foundational text for evidence-first investigation.
- **Site Reliability Engineering (Google SRE Book):** Incident response, postmortem culture, and production readiness standards.
- **The Pragmatic Programmer:** Debugging discipline, rubber duck debugging, and don't assume—prove.
- **PostgreSQL Documentation - Monitoring and Logging:** `pg_stat_activity`, `pg_locks`, `EXPLAIN ANALYZE` reference.
- **Node.js Documentation - Debugging Guide:** `--inspect`, heap snapshots, and CPU profiling workflows.
- **OpenTelemetry Documentation:** Trace context propagation, span attributes, and auto-instrumentation setup.
- **Chrome DevTools Documentation:** Performance panel, Memory panel, and Network panel reference guides.
