# Debugging Expert — Production Debugging Examples

This document models the engineering reasoning process used by a Principal Debugging Engineer to investigate and resolve production issues. Each example demonstrates structured, evidence-first investigation rather than trial-and-error guessing.

---

## 1. React Bug — Infinite Re-render Loop

### User Request
*"Our React dashboard is freezing the browser tab after navigating to the analytics page. CPU usage spikes to 100% within 2 seconds of loading the page."*

### Symptoms
- Browser tab becomes unresponsive within 2 seconds of page load.
- CPU usage spikes to 100% and remains there until the tab is force-closed.
- The issue is consistent and reproducible on every visit to the analytics page.
- No JavaScript errors appear in the Console tab.
- React DevTools Profiler shows component re-rendering continuously without stopping.

### Initial Hypothesis
A component is trapped in an infinite re-render loop. The most common cause in React is a `useEffect` hook that updates state on every execution, triggering a re-render, which re-executes the effect—forming a cycle. The secondary hypothesis is that a parent component is passing a new object or array reference as a prop on every render, causing a child component using `useEffect` with that prop in its dependency array to re-execute infinitely.

### Investigation Process
1. **Open React DevTools → Profiler tab.** Record a 3-second window after navigating to the analytics page. The Profiler immediately shows one component re-rendering hundreds of times per second. This confirms the infinite loop hypothesis.
2. **Identify the component.** The Profiler highlights `<AnalyticsChart>` as the component re-rendering continuously.
3. **Inspect the component's `useEffect` dependency array.** The component has a `useEffect` that calls a data transformation function and stores the result in local state. The dependency array includes a `filters` object prop.
4. **Trace where `filters` is defined.** In the parent `<AnalyticsDashboard>` component, `filters` is defined as an inline object literal inside the render function body—not memoized with `useMemo`.
5. **Confirm the root cause.** Every time `<AnalyticsDashboard>` renders, it creates a new object reference for `filters`. React's `useEffect` performs a shallow reference comparison on dependencies. Since `filters` is a new reference on every render, the effect fires on every render. The effect updates local state, which triggers a re-render of the parent, which creates a new `filters` reference again—completing the infinite loop.

### Root Cause
The parent component passes an inline object literal as a prop to a child component. The child's `useEffect` includes this prop in its dependency array. Since a new object reference is created on every parent render, `useEffect` fires on every render cycle, updates state, triggers a re-render, and repeats indefinitely. The root cause is the absence of `useMemo` on the `filters` object in the parent component.

### Solution Strategy
- Wrap the `filters` object in `useMemo` in the parent component, with the actual filter values (primitive fields) in its dependency array. This ensures `filters` only produces a new reference when the filter values genuinely change.
- As a secondary defensive measure, flatten the `filters` object into individual primitive props passed to `<AnalyticsChart>`, since primitive values are compared by value rather than by reference in `useEffect` dependency checks.

### Prevention Strategy
- Add an ESLint rule (`react-hooks/exhaustive-deps`) to the project configuration to warn when object or array values are used in `useEffect` dependency arrays without memoization.
- Add a performance test using React Testing Library that asserts the component renders fewer than 3 times during initial mount.
- Document the team convention: never pass non-primitive values as `useEffect` dependencies without memoization.

### Lessons Learned
React's shallow reference comparison for `useEffect` dependencies is a common source of infinite loops. The symptom (100% CPU, frozen tab) is severe but the root cause (missing `useMemo`) is a single-line fix. The key diagnostic step is using the React DevTools Profiler to confirm the loop before inspecting code—it immediately reveals which component is cycling and at what rate.

---

## 2. Next.js Error — Hydration Mismatch in Production

### User Request
*"Our Next.js e-commerce site is showing a blank white screen for approximately 15% of page loads in production. It works perfectly in development. The browser console shows an error: 'Error: Hydration failed because the initial UI does not match what was rendered on the server.'"*

### Symptoms
- Blank white screen on approximately 15% of production page loads.
- The hydration error appears in the browser console on affected loads.
- The issue does not reproduce in the local development environment.
- The affected pages are product detail pages rendered with Next.js SSR.
- The error does not occur on statically generated pages.

### Initial Hypothesis
A hydration mismatch occurs when the HTML rendered by the server does not match what React expects to render on the client during hydration. The inconsistency between development (where it works) and production (where it fails) strongly suggests the mismatch is caused by dynamic content that differs between server and client environments—the primary candidates being: `Date.now()` or `Math.random()` called during render, browser-only APIs (like `window` or `localStorage`) accessed during server rendering, or content that varies based on the user's locale or timezone.

### Investigation Process
1. **Read the full hydration error message.** The error includes a diff of the mismatched content: the server rendered a price formatted as `$1,234.56` while the client rendered `$1,234.56` with a different currency symbol for some locales.
2. **Identify the component.** The error trace points to a `<PriceDisplay>` component that formats a price using `Intl.NumberFormat`.
3. **Check the `Intl.NumberFormat` configuration.** The component uses `Intl.NumberFormat` without specifying a `locale` parameter, allowing it to default to the environment's locale. On the server (running in a Linux container with a `en-US` locale), it renders `$1,234.56`. On some users' browsers (running with their system locale, e.g., `de-DE`), it renders `1.234,56 $` using German formatting conventions.
4. **Confirm with production server locale vs. client locale.** Reproduce by temporarily setting the browser locale to `de-DE`. The blank screen and hydration error appear consistently.
5. **Confirm the development environment difference.** The developer's machine uses `en-US` locale, matching the server. This is why development never reproduces the issue.

### Root Cause
The `<PriceDisplay>` component uses `Intl.NumberFormat` without an explicit `locale` argument, causing it to inherit the environment locale. The server container uses `en-US`. Users in locales with different number formatting conventions receive a different format on the client, causing a React hydration mismatch that results in a blank white screen.

### Solution Strategy
- Pass an explicit `locale` string to every `Intl.NumberFormat` and `Intl.DateTimeFormat` call in the application. The locale should be derived from the user's preferred locale stored in a cookie or HTTP `Accept-Language` header, processed server-side, and passed as a prop to all formatting components.
- Use `suppressHydrationWarning` only as a last resort on specific elements known to have benign client/server differences (e.g., a timestamp showing "just now")—never globally.
- Use Next.js internationalized routing (`i18n` configuration in `next.config.js`) to normalize locale handling across the application.

### Prevention Strategy
- Add a CI step that runs the Next.js build and renders pages with a non-default locale environment variable to catch locale-sensitive hydration mismatches before deployment.
- Add a lint rule that flags `Intl.NumberFormat` and `Intl.DateTimeFormat` calls missing an explicit `locale` argument.
- Document the project standard: all number and date formatting utilities must accept a `locale` parameter derived from server-side context.

### Lessons Learned
Hydration mismatches that only appear in production (not development) are almost always caused by environment-specific differences: locale, timezone, environment variables, or browser-only APIs accessed during render. The first step is always to read the hydration error diff carefully—it directly shows the server-rendered value versus the client-expected value, which points immediately to the source of the divergence.

---

## 3. Node.js Crash — Out of Memory in Production

### User Request
*"Our Node.js API server crashes every 6–8 hours in production with an 'out of memory' error. The process restarts automatically, but we are getting 2–3 minutes of downtime with each restart. Heap usage climbs linearly over the 6-hour period."*

### Symptoms
- Node.js process terminates with `FATAL ERROR: Reached heap limit Allocation failed - JavaScript heap out of memory`.
- Process monitoring shows heap memory growing linearly from ~150MB at startup to ~2GB over 6 hours.
- Memory usage does not decrease during low-traffic periods—it only grows.
- The crash occurs regardless of traffic volume, even overnight with minimal load.
- The API handles background job processing via a BullMQ worker running in the same process.

### Initial Hypothesis
The linear, unrecoverable heap growth regardless of traffic volume is a strong indicator of a memory leak—not a traffic-driven resource demand issue. Two primary suspects: (1) an `EventEmitter` accumulating listeners over time without removal, which is a common Node.js leak pattern; (2) a cache or data structure that grows unbounded without eviction, accumulating references that prevent garbage collection. The BullMQ worker running in the same process is flagged for investigation since queue systems frequently accumulate job objects if not properly cleaned up.

### Investigation Process
1. **Enable Node.js memory usage logging.** Add `process.memoryUsage()` to a periodic logger that emits heap usage every 5 minutes. Confirm heap grows at approximately 5MB per minute even during off-peak hours.
2. **Check for `MaxListenersExceededWarning`.** Run the application with `process.on('warning', console.warn)`. Within 10 minutes, warnings appear: `MaxListenersExceededWarning: Possible EventEmitter memory leak detected. 11 listeners added to [EventEmitter]`.
3. **Identify the EventEmitter.** The warning includes the emitter type. It points to a custom `JobEventEmitter` class used inside the BullMQ worker to track job progress events.
4. **Trace the listener registration code.** Inside the worker, each job processed registers a new `progress` event listener on the shared `JobEventEmitter` instance. The listeners are never removed after job completion—neither on success, failure, nor timeout.
5. **Confirm the leak.** The worker processes approximately 600 jobs per hour. Each job adds one listener that is never removed. After 6 hours, the emitter has ~3,600 accumulated listeners, each holding a closure reference to the job's data object. These closures prevent the job data (sometimes several KB of payload per job) from being garbage collected.

### Root Cause
A shared `EventEmitter` instance inside the BullMQ worker accumulates `progress` event listeners without ever removing them. Each processed job adds a listener; no cleanup code removes it after job completion. Over 6 hours of continuous operation, thousands of listeners accumulate, each retaining a closure reference to job payload data, causing linear heap growth until the process exhausts available memory.

### Solution Strategy
- Add `emitter.removeListener(event, handler)` or `emitter.off(event, handler)` in the job completion, failure, and timeout handlers to remove the listener as soon as the job lifecycle ends.
- Alternatively, use `emitter.once()` instead of `emitter.on()` for per-job listeners that should fire at most once, since `once` automatically removes the listener after the first emission.
- Set `emitter.setMaxListeners(20)` explicitly on the shared emitter to ensure the `MaxListenersExceededWarning` fires at a low threshold and is caught in monitoring early.

### Prevention Strategy
- Add a memory usage alert that fires when the Node.js process heap exceeds 70% of its configured limit (`--max-old-space-size`), providing advance warning before the OOM crash.
- Add a listener count health check metric: emit `emitter.listenerCount(event)` as a gauge metric to Prometheus or OTLP every minute. Alert if listener count exceeds 50 on any single event.
- Establish a team rule: every `emitter.on()` call in a lifecycle function (job handler, request handler) must have a corresponding removal call in the cleanup path.

### Lessons Learned
Out-of-memory crashes with linear heap growth over a predictable time period are almost always memory leaks, not resource demand issues. The `MaxListenersExceededWarning` is a low-cost, high-signal indicator of EventEmitter leaks that Node.js provides built-in—but it requires a warning handler to be surfaced. Enabling `process.on('warning', ...)` logging in production is a zero-cost leak detector that should be standard in every Node.js application.

---

## 4. API Failure — Intermittent 502 Bad Gateway

### User Request
*"We're seeing intermittent 502 Bad Gateway errors on our payment API. It happens roughly 3–5 times per hour during peak traffic. Users see a payment failure screen. The errors last about 10–15 seconds and then resolve on their own."*

### Symptoms
- 502 Bad Gateway responses from the Nginx load balancer, not from the application itself.
- Error rate: approximately 3–5 occurrences per hour, each lasting 10–15 seconds.
- Errors correlate with peak traffic windows (12:00–14:00 and 18:00–21:00).
- Application logs show no corresponding errors during the 502 windows.
- Nginx error logs show `upstream connect() failed` and `no live upstreams while connecting to upstream`.

### Initial Hypothesis
A 502 from Nginx with "no live upstreams" means Nginx cannot reach any upstream application server instance. Since the application logs show nothing during these windows, the application process itself is not logging the failure—suggesting the application process is alive but not accepting connections during these intervals. The traffic-correlation points to connection exhaustion: at peak load, the application server's connection queue fills, causing new TCP connections to be refused, which Nginx interprets as "no live upstreams."

### Investigation Process
1. **Check Nginx upstream configuration.** The upstream block is configured with `keepalive 32`, meaning Nginx maintains up to 32 persistent connections per upstream worker. During peak traffic, this is exhausted.
2. **Check application server connection limits.** The Node.js application uses Express with no explicit connection limit configuration. The default Node.js HTTP server `maxConnections` is not set.
3. **Check server resource metrics during the 502 windows.** CloudWatch metrics for the EC2 instance show CPU at 95% and TCP connections in `TIME_WAIT` state reaching 16,000 during the 502 windows.
4. **Identify the `TIME_WAIT` accumulation cause.** The Node.js application is not reusing connections to the PostgreSQL database—it opens a new connection per request during peak load rather than using the connection pool efficiently. Each short-lived connection transitions to `TIME_WAIT` and occupies a local port for 60 seconds (the TCP `TIME_WAIT` default), eventually exhausting available ephemeral ports.
5. **Confirm port exhaustion.** During a peak window, run `ss -s` on the server: the output shows 28,000 sockets in `TIME_WAIT` state, exhausting the OS ephemeral port range (typically 28,000–60,000).

### Root Cause
TCP port exhaustion caused by ephemeral port depletion. The Node.js application opens a new database connection per request during peak load instead of properly reusing connections from the pool. Each closed connection enters a 60-second `TIME_WAIT` state. Under peak traffic, the 60-second accumulation of `TIME_WAIT` sockets exhausts the OS ephemeral port range (~28,000 ports), preventing new outbound connections. Nginx receives connection refusals from the upstream and returns 502 to clients.

### Solution Strategy
- Verify the database connection pool is correctly configured with a minimum pool size that prevents connection creation per request. Set `pool.min` to match steady-state concurrent request volume.
- Enable `SO_REUSEADDR` and reduce `TIME_WAIT` reuse interval using Linux kernel parameters (`net.ipv4.tcp_tw_reuse = 1`) to allow faster reuse of `TIME_WAIT` sockets.
- Add the `keep-alive` header to upstream HTTP connections in Nginx to prevent frequent TCP connection teardown and recreation between Nginx and the Node.js upstream.

### Prevention Strategy
- Add a `TIME_WAIT` socket count gauge metric to production monitoring with an alert threshold at 20,000 sockets—providing advance warning before port exhaustion occurs.
- Add connection pool utilization metrics (active connections, idle connections, pool size) to the monitoring dashboard.
- Load test the application to the expected peak RPS in staging, with connection pool metrics visible, before each major release.

### Lessons Learned
A 502 from Nginx with "no live upstreams" is not always an application crash—it can be caused by TCP-level resource exhaustion that prevents new connections without terminating the process. The key diagnostic insight is that application logs show nothing during the outage windows, which rules out application-level errors and directs investigation to the infrastructure layer. OS-level socket statistics (`ss -s`) are the correct tool for confirming port exhaustion.

---

## 5. Database Deadlock — Checkout Service

### User Request
*"Our checkout service is logging frequent deadlock errors in the database during flash sale events. Orders are failing, and customers are seeing 'transaction failed' errors. The errors appear in bursts lasting 30–60 seconds and then resolve."*

### Symptoms
- PostgreSQL logs contain `ERROR: deadlock detected` entries, peaking during flash sale traffic spikes.
- The deadlock error includes two transaction IDs and the specific rows each transaction holds and waits for.
- Order failure rate reaches 8% during peak flash sale minutes.
- The issue is not present during normal traffic; only during high-concurrency checkout events.
- Both transactions involved in each deadlock are in the `processCheckout` code path.

### Initial Hypothesis
A deadlock requires two transactions, each holding a lock the other needs, resulting in a circular wait. Since both transactions are in the same `processCheckout` code path, the deadlock must be caused by two concurrent checkout transactions acquiring locks on the same rows in different orders. The most common pattern in checkout systems is: Transaction A locks `inventory` row for Product X, then attempts to lock `orders` row. Transaction B locks `orders` row, then attempts to lock `inventory` row for Product X. This crossing lock order produces a circular wait.

### Investigation Process
1. **Extract the deadlock log entries.** PostgreSQL's deadlock error log includes the full transaction details: which transaction holds which lock, and which lock it is waiting for. Extract 5 deadlock events from the logs.
2. **Identify the lock order in each transaction.** All 5 deadlock events show the same pattern: Transaction A holds a row lock on `inventory` (acquired via `SELECT ... FOR UPDATE` when decrementing stock) and waits for a lock on `orders`. Transaction B holds a row lock on `orders` (acquired when inserting the order record) and waits for a lock on `inventory`.
3. **Trace the application code.** The `processCheckout` function executes two database operations: first it inserts the order record, then it decrements the inventory. However, the inventory decrement uses a `SELECT ... FOR UPDATE` inside a nested helper function that also reads the order for validation—creating the crossing lock acquisition order between concurrent executions.
4. **Confirm the crossing lock order.** Under sequential execution, no deadlock occurs. Under concurrent execution with two simultaneous checkouts for the same product, the crossing order creates a guaranteed deadlock.

### Root Cause
Two concurrent `processCheckout` transactions acquire row-level locks on `inventory` and `orders` tables in opposite orders. Transaction A acquires the `inventory` lock first; Transaction B acquires the `orders` lock first. Each then waits for the lock held by the other, creating a circular dependency that PostgreSQL detects and resolves by terminating one transaction (the deadlock victim), causing a checkout failure.

### Solution Strategy
- Establish a canonical lock acquisition order across all database operations in the checkout flow: always lock `inventory` before `orders`—never the reverse. Refactor the `processCheckout` function to perform the inventory decrement first (acquiring its lock first), then insert the order record.
- Replace the `SELECT ... FOR UPDATE` inventory pattern with an atomic `UPDATE inventory SET quantity = quantity - 1 WHERE id = $1 AND quantity > 0 RETURNING quantity` statement. This acquires and releases the lock in a single statement, dramatically reducing the lock hold duration.
- Wrap the entire checkout operation in a single database transaction with a defined timeout (`SET LOCAL lock_timeout = '500ms'`) to fail fast and return an error to the client rather than waiting indefinitely.

### Prevention Strategy
- Add a load test scenario that simulates 100 concurrent checkouts for the same product SKU. Run this in staging before every major release and confirm zero deadlocks.
- Create a PostgreSQL alert that fires when deadlock detection rate exceeds 5 per minute, providing early warning before customer-facing order failure rate climbs.
- Document the canonical lock acquisition order in the project's database conventions guide and add it to the code review checklist.

### Lessons Learned
Deadlocks caused by circular lock acquisition are deterministic under concurrent load—they will always occur when the lock order crosses. The PostgreSQL deadlock log is highly diagnostic: it explicitly names the transactions and the locks they hold and wait for, making the crossing order immediately visible without needing to reproduce the issue. The fix is always structural (enforce a canonical lock order) rather than symptomatic (retry on deadlock), because retrying without fixing the order simply delays the next deadlock.

---

## 6. Redis Issue — Cache Stampede During Deployment

### User Request
*"Every time we deploy our application, our database CPU spikes to 100% for about 90 seconds immediately after the new pods start. During this window, API latency jumps from 80ms to 4.5 seconds and some requests time out entirely."*

### Symptoms
- Immediate post-deployment database CPU spike to 100% lasting 60–90 seconds.
- API P95 latency jumps from 80ms to 4.5 seconds during the spike window.
- Redis cache hit rate drops to near 0% at the moment new pods start, then recovers to 85% within 2 minutes.
- The pattern is consistent across every deployment, regardless of the change being deployed.
- Database connection pool shows maximum connections saturated during the spike window.

### Initial Hypothesis
The Redis cache hit rate dropping to 0% at deployment time is the key signal. When new pods start, their local cache state is cold. If the application also flushes or invalidates the shared Redis cache during startup (or if the Redis TTL expires during the deployment window), all new requests go directly to the database simultaneously—a "cache stampede" or "thundering herd" problem. The database, previously handling only 15% of requests (the cache misses), suddenly receives 100% of requests from all pods simultaneously, causing saturation.

### Investigation Process
1. **Examine the application startup sequence.** Review the deployment script and application initialization code. Confirm that during startup, the application calls `redis.flushdb()` to clear the entire cache. The intention was to clear potentially stale data from the old application version.
2. **Confirm the stampede timing.** Correlate the Redis `flushdb` event timestamp in the Redis logs with the database CPU spike onset. They occur within 1 second of each other—confirming `flushdb` as the trigger.
3. **Measure the cache rebuild duration.** Monitor Redis keyspace size after startup. The cache returns to its steady-state key count after approximately 90 seconds—matching the database spike duration exactly. This confirms the database is absorbing all traffic while the cache rebuilds.
4. **Quantify the impact.** At steady state, the database handles approximately 200 queries per second. During the 90-second stampede, it handles 1,400 queries per second—7× its normal load—causing CPU saturation and connection pool exhaustion.

### Root Cause
The application's startup sequence calls `redis.flushdb()` to prevent stale cache data after deployments. This simultaneously invalidates all cached data across all cache keys. When all new pod instances start concurrently, they all encounter cache misses for every request and simultaneously query the database, overwhelming it with 7× its normal query volume for the 90-second cache warm-up period.

### Solution Strategy
- Remove the `redis.flushdb()` call from the startup sequence. Instead, use versioned cache keys (e.g., `v2:user:profile:123`) in the new application version. Old version keys remain in Redis but are never requested by the new version. They expire naturally via their existing TTL.
- Implement a cache warm-up routine that pre-populates the top 500 most-requested cache keys during the pod's readiness probe delay—before the pod begins receiving traffic from the load balancer.
- Add a mutex lock pattern (probabilistic early expiration) on cache reads: when a key is within 30 seconds of expiration, one request regenerates it while others continue serving the slightly stale cached value, preventing simultaneous expiration stampedes.

### Prevention Strategy
- Add a post-deployment canary metric that alerts if the Redis cache hit rate drops below 50% in the 5 minutes following a deployment—detecting stampedes before they cause user-facing impact.
- Add a database CPU alert at 70% sustained for 2 minutes to detect future stampede events regardless of their cause.
- Document and test the deployment procedure in staging by simulating a rolling restart with cache monitoring enabled.

### Lessons Learned
Cache stampedes are one of the most dangerous failure modes in production because they are self-inflicted and occur precisely when the team's attention is on the deployment. The Redis hit rate metric is the earliest and clearest indicator. The root cause is almost always an overly aggressive cache invalidation strategy—clearing everything at once—when a selective, versioned approach eliminates the risk entirely.

---

## 7. Docker Problem — Container Exits with Code 137

### User Request
*"Our background worker container keeps exiting unexpectedly with exit code 137. It runs for 20–40 minutes, processes a batch of jobs, then exits. Docker restart policy brings it back, but it cycles repeatedly."*

### Symptoms
- Container exits with code 137 (SIGKILL signal) after 20–40 minutes of operation.
- Docker event logs show `OOMKill` as the stop reason.
- No application error logs are written before the exit—the process is killed without warning.
- The worker processes image resizing jobs, each converting a large image into multiple resolution variants.
- Memory usage reported by `docker stats` climbs linearly during each batch before the container is killed.

### Initial Hypothesis
Exit code 137 means the process received a SIGKILL signal. In Docker, the primary cause of SIGKILL being sent to a container is the Docker daemon enforcing the container's memory limit (`--memory` flag). The symptom pattern—linear memory growth during image processing, eventual kill—strongly suggests that the image resizing operations accumulate memory faster than garbage collection reclaims it, ultimately hitting the container memory limit. The secondary hypothesis is that the image resizing library is leaking native memory (C-level heap, not tracked by the Node.js/Python heap).

### Investigation Process
1. **Confirm the OOMKill.** Run `docker inspect <container-id> --format='{{json .State}}'` on the killed container's record. The output shows `"OOMKilled": true`, confirming the Docker daemon killed the container for exceeding its memory limit.
2. **Identify the memory limit.** Check the Docker Compose or run configuration: the worker is configured with `--memory=512m`. Running `docker stats` during processing shows memory climbing to 510MB before the kill.
3. **Profile memory usage during a single job.** Add memory sampling to the worker that logs `process.memoryUsage()` before and after each image job. Memory increases by approximately 80–120MB per job and is not released after job completion.
4. **Identify the library causing retention.** The worker uses `sharp` (a Node.js image processing library backed by `libvips`, a C library) for resizing. Research reveals that `sharp` allocates image buffers in native C memory not tracked by the Node.js V8 heap. `sharp` provides an explicit `gc()` function to release native memory between jobs.
5. **Confirm the fix.** Add `sharp.cache(false)` and call `sharp.cleanup()` between jobs in the test environment. Memory no longer accumulates; each job's native memory is released before the next begins.

### Root Cause
The `sharp` image processing library allocates large image buffers in native C memory (outside the V8 JavaScript heap). This memory is not automatically released by Node.js garbage collection. Without explicit cache clearing between jobs, native memory accumulates linearly across the batch, eventually hitting the container's 512MB memory limit and triggering an OOMKill by the Docker daemon.

### Solution Strategy
- Call `sharp.cache(false)` to disable `sharp`'s internal tile cache, and call `sharp.cleanup()` between each job to explicitly release native memory allocations.
- Increase the container memory limit to `--memory=1g` as a short-term mitigation while the cleanup calls are tested and deployed.
- Monitor native memory separately from the V8 heap by tracking RSS (Resident Set Size) from `process.memoryUsage().rss`, which includes native allocations.

### Prevention Strategy
- Add a container memory usage alert at 80% of the configured limit to provide advance warning before OOMKill.
- Add a per-job memory usage log (RSS before and after each job) to detect memory retention patterns before they cause crashes.
- Add the `sharp.cleanup()` call to the worker's standard job completion handler so it cannot be omitted by future contributors.

### Lessons Learned
Exit code 137 means SIGKILL—always check `OOMKilled` in Docker inspect before investigating application code. Native memory leaks (in C libraries called from Node.js or Python) do not appear in the V8 heap snapshot and are not caught by JavaScript-level memory profiling. RSS (Resident Set Size) is the correct metric for total process memory consumption including native allocations.

---

## 8. Kubernetes Pod Crash — CrashLoopBackOff on Startup

### User Request
*"We deployed a new version of our auth service to Kubernetes and now the pod is in CrashLoopBackOff. The previous version ran fine. Requests to the auth service are failing across the platform."*

### Symptoms
- Pod status: `CrashLoopBackOff` immediately after deployment.
- `kubectl describe pod` shows the container exits with exit code 1.
- Restart count is climbing (7 restarts in 10 minutes).
- Platform-wide auth failures because all other services depend on the auth service.
- The deployment uses a new Docker image tagged `v2.1.0`; the previous `v2.0.9` was stable.

### Initial Hypothesis
A pod entering CrashLoopBackOff immediately after startup typically means the application process is failing during initialization—before it begins serving requests. Common causes: (1) a missing or misconfigured environment variable that the application requires at startup; (2) the application cannot connect to a required dependency (database, Redis) during startup; (3) a code bug introduced in the new version causes an unhandled exception during module initialization.

### Investigation Process
1. **Read the container logs for the crashed pod.** `kubectl logs <pod-name> --previous` (the `--previous` flag retrieves logs from the last terminated container before restart). The logs show: `Error: Cannot find module '@company/auth-lib'`. The process exits immediately after this error.
2. **Identify the missing module.** The new `v2.1.0` image imports `@company/auth-lib` version `2.0.0`. Check the `Dockerfile` and build logs.
3. **Check the Docker build log.** The build log shows that `npm ci` completed successfully, but with a warning: `npm warn saveError ENOENT: no such file or directory, open '/app/node_modules/@company/auth-lib/package.json'`. The build used a cached layer from the previous build that did not include the new dependency.
4. **Identify the Docker layer caching issue.** The `Dockerfile` copies `package.json` and `package-lock.json`, then runs `npm ci`. In the CI build, the `package.json` was modified to add `@company/auth-lib`, but the `package-lock.json` was not committed with the updated lockfile. Docker's layer cache matched on the unchanged `package-lock.json` and served the cached `npm ci` result—which did not include the new dependency.
5. **Confirm by rebuilding without cache.** Trigger a new build with `--no-cache`. The build correctly installs all dependencies. The new image starts successfully in a test environment.

### Root Cause
The `package-lock.json` was not updated and committed when `@company/auth-lib` was added to `package.json`. Docker's build layer cache matched on the unchanged `package-lock.json` and reused the cached `node_modules` layer—which did not contain the new dependency. The resulting Docker image was missing the required module, causing an immediate startup crash.

### Solution Strategy
- Roll back to the `v2.0.9` image immediately to restore auth service availability.
- Rebuild the `v2.1.0` image with `--no-cache` after committing the correct `package-lock.json` (regenerated by running `npm install` locally with the new dependency).
- Add a startup validation step to the CI pipeline that runs the application's health check endpoint against the built image before pushing to the registry.

### Prevention Strategy
- Add a CI pre-check that compares `package.json` and `package-lock.json` modification timestamps. If `package.json` was modified in the commit but `package-lock.json` was not, fail the build with an explicit error.
- Implement Kubernetes readiness probes that verify the application is fully initialized before routing traffic. If the app crashes at startup, the readiness probe ensures no traffic is routed to the failing pod—preventing the platform-wide impact.
- Add a `--no-cache` flag to the CI Docker build for release image builds (as opposed to feature branch builds where layer caching is acceptable for speed).

### Lessons Learned
`kubectl logs --previous` is the single most important command for diagnosing CrashLoopBackOff—it retrieves the logs from the terminated container before the restart timer cleared them. The root cause was not in the application code but in the Docker build process—a stale layer cache serving an incomplete `node_modules`. Always investigate the Docker build artifact before assuming a code regression when a deployment suddenly fails to start.

---

## 9. AI Agent Failure — Tool Execution Returning Wrong Results

### User Request
*"Our AI agent is supposed to look up customer account details and then draft a support response. It's returning correct responses for 80% of queries but for some customers it returns account details that belong to a completely different customer. This is a serious data privacy issue."*

### Symptoms
- The AI agent returns the correct customer name and account details for 80% of queries.
- For approximately 20% of queries, the agent returns account details belonging to a different customer than the one specified in the conversation.
- The wrong customer details are always real customer records—not hallucinated content.
- The issue is not reproducible on demand; it appears to correlate with certain customer names or ID formats.
- The agent uses a RAG pipeline to retrieve customer records from a vector database before composing the response.

### Initial Hypothesis
If the agent is returning real but wrong customer data, the error is almost certainly in the retrieval step—not in the LLM generation step. The LLM cannot invent real customer records; it can only use what is retrieved and provided to it as context. The primary hypothesis is that the vector similarity search is returning the wrong customer's embedding as the top result for certain queries. The 20% failure rate and correlation with specific names suggests an embedding collision: customers whose names or account descriptions are semantically similar in the embedding space are being conflated.

### Investigation Process
1. **Log the full RAG retrieval context.** Enable `debug`-level logging that captures the full vector search query and the retrieved documents (customer records) passed to the LLM. Reproduce a failing query.
2. **Inspect the retrieved context for a failing query.** The debug log for a query about customer "Thomas Anderson" shows the retrieved context contains the account record for "Thomas Andrews"—a different customer with a semantically similar name.
3. **Check the vector search query.** The agent embeds only the customer's name as the search query. Customer name embeddings for "Thomas Anderson" and "Thomas Andrews" are extremely close in the vector space (cosine similarity: 0.97).
4. **Identify the missing filter.** The vector database query uses only semantic similarity ranking. It does not filter by an exact customer ID or account number. The query is: "find the customer most similar to this name"—when it should be: "find the customer with this exact account ID, then use embedding to retrieve their support history."
5. **Confirm the architectural flaw.** Customer identity lookup should be performed with an exact match on a unique identifier (account ID, email), not semantic vector similarity. Vector search is appropriate for retrieving related support tickets or knowledge base articles—not for identifying which customer the query refers to.

### Root Cause
The agent uses vector similarity search to identify which customer the conversation is about, using the customer's name as the embedding query. For customers with similar names, the cosine similarity scores are nearly identical, causing the vector database to return the wrong customer's record as the top result. The fundamental design error is using semantic similarity for identity resolution—a task that requires exact matching on a unique identifier.

### Solution Strategy
- Restructure the agent workflow: customer identity resolution must use an exact database lookup by account ID or email address (provided by the support ticket system or the customer authentication session)—not a vector similarity search on the customer's name.
- Reserve vector search for retrieving semantically relevant content within the confirmed customer's own data: their past support tickets, account notes, and product usage history.
- Add a tenant/customer namespace filter on all vector database queries so that even if similarity search is used for content retrieval, it can only return documents belonging to the already-confirmed customer.

### Prevention Strategy
- Add an integration test that runs the agent with two customers whose names are intentionally similar and verifies that each query returns data for the correct customer exclusively.
- Add a logging assertion: after every retrieval step, log the retrieved customer ID and compare it against the expected customer ID from the session context. Alert if they do not match.
- Document the agent architecture rule: customer identity is always established via exact unique identifier lookup before any vector retrieval is performed.

### Lessons Learned
When an AI agent returns real but wrong data, the error is always in the data pipeline feeding the LLM—not in the LLM itself. The retrieval layer is the trust boundary: if incorrect data enters the LLM context, the LLM will use it confidently. Vector similarity is a powerful tool for semantic content retrieval but is fundamentally unsuitable for identity resolution, which requires exact-match guarantees on unique identifiers.

---

## 10. Production Outage — Complete API Unavailability

### User Request
*"Complete API outage. All endpoints returning 503. Started 8 minutes ago. All services are affected. Engineering team is assembled. What is the investigation and resolution process?"*

### Symptoms
- All API endpoints returning HTTP 503 Service Unavailable.
- Outage started approximately 8 minutes ago (correlation with a deployment that completed 10 minutes ago).
- All microservices affected simultaneously—not isolated to a single service.
- Monitoring dashboards show all services showing zero healthy instances.
- Load balancer health checks failing for all upstream targets.
- No application error logs are being generated (logging pipeline may also be affected).

### Initial Hypothesis
A simultaneous failure across all services immediately following a deployment points to a shared infrastructure change—not an application code bug. Application code bugs affect individual services; infrastructure bugs affect all services simultaneously. The most likely candidates: (1) a misconfigured Kubernetes change that affected all deployment namespaces (wrong resource limits, a broken network policy, or a misconfigured admission webhook rejecting all pod creations); (2) a DNS resolution failure preventing services from communicating; (3) a shared secret or environment variable change that caused all services to fail health checks.

### Investigation Process
1. **Establish the incident timeline.** Confirm the deployment completed 10 minutes ago. Review what was deployed: the deployment was a change to the shared `infra/kubernetes/base` Helm chart—a chart applied to all services.
2. **Check pod status across all namespaces.** `kubectl get pods --all-namespaces`. Output shows: all pods in `Terminating` or `Pending` state. No pods are in `Running` state across the entire cluster.
3. **Check recent Kubernetes events.** `kubectl get events --all-namespaces --sort-by='.lastTimestamp'`. Recent events show: `FailedScheduling` for all pods. Message: `0/3 nodes are available: 3 nodes have taint {dedicated: system-only}, pod does not tolerate`.
4. **Identify the cause.** The Helm chart change added a `tolerations` configuration to the base deployment template. However, the taint `dedicated: system-only` is the taint applied to the control plane nodes—not the worker nodes. The tolerations change inadvertently removed the existing worker node tolerations from all deployments, preventing all pods from being scheduled on worker nodes.
5. **Confirm the fix.** Review the Helm chart diff. The change accidentally replaced the worker node toleration block instead of adding to it. Reverting the Helm chart to the previous version restores the correct tolerations.

### Solution Strategy
- **Immediate mitigation (T+2 minutes):** Roll back the Helm chart to the previous version using `helm rollback <release> <revision>`. This triggers pod rescheduling with the correct tolerations.
- **Verify recovery:** Monitor `kubectl get pods --all-namespaces` as pods transition from `Pending` to `Running`. Monitor load balancer health check success rate.
- **Confirm resolution:** Verify API endpoints return 200 responses via synthetic monitoring within 5 minutes of rollback.

### Prevention Strategy
- Add a CI/CD stage that runs `helm template` against the changed chart and validates the rendered YAML against a policy engine (e.g., Kyverno or OPA) before deploying. A policy rule requiring that all deployments specify valid worker node tolerations would have caught this change.
- Implement a staged rollout for infrastructure Helm chart changes: deploy to one non-production namespace first and verify pod scheduling before promoting to production namespaces.
- Add a post-deployment health check gate: after applying any Helm chart change, wait 2 minutes and verify that the pod count across all namespaces matches the expected replica count before declaring the deployment successful.

### Lessons Learned
Cross-service simultaneous outages following a shared infrastructure change are always caused by the shared infrastructure—not by application code. The fastest path to recovery is always to roll back the most recent shared change, not to investigate application logs. `kubectl get events` sorted by timestamp is the fastest way to identify what Kubernetes is attempting and failing to do. The investigation validated the rollback hypothesis within 3 minutes—faster than any attempt to debug the application layer would have been. In a production outage, reducing user impact (rollback first) must always precede root cause analysis.
