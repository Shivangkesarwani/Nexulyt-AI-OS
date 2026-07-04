# Code Reviewer — Real-World Examples

This document demonstrates how the AI Code Reviewer thinks through production-grade pull request reviews across different application domains. Each example models the cognitive process, decision hierarchy, and structured feedback delivery of a Principal Engineer performing a review.

---

## 1. Portfolio Website — Authentication & Contact Form PR

### User Request
*"Review this pull request for a developer portfolio site. It adds a contact form with email submission and a simple admin login to view submitted messages."*

### Requirement Analysis
- **What changed:** A contact form POST endpoint, an email-sending integration, a login route, and an admin dashboard route that displays stored messages.
- **Risk profile:** Medium–High. The addition of authentication and user-submitted data introduces injection, enumeration, and authorization risks that must be evaluated before merge.
- **Audience:** Public internet users submitting contact messages; one admin user accessing the dashboard.

### Thinking Process
The reviewer begins by identifying the data flow: public user submits form → data stored in database → admin authenticates → admin views stored messages. Each transition is a potential vulnerability surface. The first question is whether user input is sanitized before storage and display. The second is whether the admin route verifies authentication on every request.

Next, the reviewer checks the email integration. Third-party email calls can fail. Are errors handled, or does the API silently time out? If the email fails, does the form submission still get saved to the database?

Finally, the reviewer evaluates the admin login. Is the password hashed? Does the login endpoint expose whether a username exists via different error messages?

### Engineering Decisions
- **Authorization gate:** Verify that the `/admin/messages` route checks session authentication on every request—not just on the login redirect.
- **Injection surface:** Contact form fields (name, email, message) must be stored via parameterized queries, never string-concatenated into SQL.
- **Stored XSS risk:** The admin dashboard renders submitted contact messages. If message content is rendered as raw HTML, a submitted `<script>` tag executes in the admin's browser.
- **Error handling:** Email send failures must be caught and logged without breaking the user-facing form submission response.
- **User enumeration:** Login failure messages must not distinguish between "user not found" and "wrong password."

### Workflow
1. Check admin route for missing authentication middleware.
2. Verify contact form fields use parameterized database inserts.
3. Confirm message content is escaped before rendering in the admin view.
4. Verify email errors are caught and the form submission is not dependent on email success.
5. Confirm password is stored as an Argon2id or bcrypt hash—not plaintext.
6. Check login error response is generic regardless of failure reason.

### Best Practices
- Wrap third-party API calls (email, SMS) in `try/catch` and decouple their success from the user-facing operation result.
- Apply a `Content-Security-Policy` header on the admin dashboard to mitigate XSS impact even if output encoding is missed.
- Use `HttpOnly`, `Secure`, and `SameSite=Strict` cookie flags on the admin session token.

### Common Mistakes
- Applying authentication only to the login page redirect without protecting the actual dashboard data route.
- Rendering submitted contact messages with `.innerHTML` instead of `.textContent`, enabling stored XSS.
- Returning a 500 error to the user if the email service is unavailable, even though the message was saved successfully.

### Final Recommendation
**Request Changes.** Two blocking issues must be resolved: (1) confirm the admin dashboard route verifies authentication on every request, and (2) confirm contact form message content is output-encoded before rendering in the admin view. These are `[CRITICAL]` findings. All remaining items are `[MAJOR]` or `[MINOR]` and may be addressed in the same revision.

---

## 2. SaaS Application — Multi-Tenant Billing Endpoint PR

### User Request
*"Review this PR that adds a billing summary endpoint to our SaaS platform. It returns the current plan, usage metrics, and invoice history for a workspace."*

### Requirement Analysis
- **What changed:** A new `GET /api/billing/summary` endpoint that fetches plan, usage, and invoice records for the requesting workspace.
- **Risk profile:** Critical. Billing data is sensitive. The primary risk is Broken Object Level Authorization (BOLA)—returning one workspace's data to a different workspace's authenticated user.
- **Audience:** Authenticated workspace members and admins. Each workspace must see only their own data.

### Thinking Process
In a multi-tenant SaaS system, the most dangerous billing endpoint bug is one that accepts a `workspaceId` parameter from the client and uses it directly in the database query without verifying that the authenticated user belongs to that workspace.

The reviewer traces the full request lifecycle: JWT decoded → `workspaceId` extracted from token or from request parameter? If it comes from a request parameter, the authorization gap is present. The correct pattern is to derive `workspaceId` exclusively from the authenticated token context.

Next, the reviewer checks what data is returned. Invoice history may contain customer names, billing addresses, or partial card numbers. Are fields scoped to the minimum necessary for the UI?

Finally, the reviewer checks error handling: what happens if the workspace has no billing record yet? Does the endpoint return a 500 or a graceful empty state?

### Engineering Decisions
- **Authorization source:** `workspaceId` must be derived from the verified JWT claims, not from a client-supplied query parameter.
- **Data minimization:** Return only fields required by the frontend. Exclude internal metadata, raw Stripe customer objects, or internal pricing tier IDs.
- **Pagination:** Invoice history must be paginated. Returning all invoices for an enterprise workspace with 5 years of history in a single response is unbounded.
- **Graceful empty state:** Return HTTP 200 with an empty invoices array for workspaces with no billing history—not a 404 or 500.

### Workflow
1. Locate where `workspaceId` is sourced. Flag if it comes from query params or request body instead of token context.
2. Verify database queries filter by the token-derived `workspaceId` on every data fetch.
3. Check response payload fields for unnecessary data exposure.
4. Confirm invoice list is paginated with a maximum page size.
5. Verify error handling for workspaces with no billing record.
6. Check for rate limiting on the billing endpoint to prevent scraping.

### Best Practices
- Isolate billing data access behind a dedicated `BillingRepository` class that always requires an authenticated workspace context parameter—never optional.
- Log all billing data access events to the audit log for compliance purposes.
- Return consistent error envelopes: `{ code, message }` for all failure cases.

### Common Mistakes
- Trusting a client-supplied `workspaceId` query parameter without verifying it against the authenticated user's workspace memberships.
- Returning the full Stripe invoice object including metadata fields that expose internal pricing configurations.
- Missing pagination, causing a single request to return hundreds of invoice records for enterprise accounts.

### Final Recommendation
**Request Changes.** Verify authorization source before approving. If `workspaceId` comes from any source other than the verified JWT, this is a `[CRITICAL]` BOLA vulnerability that must be fixed before merge. Add pagination on the invoice list as a `[MAJOR]` requirement. All other findings are `[MINOR]` or `[NIT]`.

---

## 3. AI SaaS — LLM Prompt Execution PR

### User Request
*"Review this PR that adds a feature allowing users to submit custom prompts to our LLM integration. The prompt is combined with a system context and sent to the OpenAI API."*

### Requirement Analysis
- **What changed:** A new endpoint accepts a user-supplied prompt string, appends it to a system prompt, and forwards the combined message to the OpenAI Chat Completions API.
- **Risk profile:** Critical. User-controlled input injected directly into LLM prompts is the primary attack surface for prompt injection—a class of vulnerability that can leak system prompt contents, manipulate AI behavior, or trigger unauthorized tool executions.
- **Audience:** Authenticated users of the AI SaaS platform.

### Thinking Process
The reviewer's first check is how the user's prompt is combined with the system prompt. Is the user input appended at the end of the system message, or is it placed in its own `user` role message? These have critically different injection characteristics.

Next: is `max_tokens` set? Without it, a malicious user can craft a prompt that generates an extremely long response, causing unbounded API cost.

Then: is the response validated before it is returned to the client or stored? LLM outputs are not trusted data. If the response is rendered in a web interface without output encoding, it introduces XSS risk.

Finally: are the LLM API credentials stored in environment variables, or are they hardcoded anywhere in the PR diff?

### Engineering Decisions
- **Role separation:** User input must be placed in a `user` role message—never concatenated into the `system` role message where it can override system instructions.
- **Max tokens:** Set an explicit `max_tokens` limit appropriate to the feature's use case to cap API cost per request.
- **Output validation:** LLM response content must be treated as untrusted user input when rendered in a browser.
- **Rate limiting:** The prompt endpoint must enforce per-user rate limits to prevent API cost abuse.
- **Credential handling:** OpenAI API key must be loaded from environment variables, not hardcoded or logged.

### Workflow
1. Locate how user prompt is combined with system prompt. Flag concatenation into the system role.
2. Verify `max_tokens` is set on the API call.
3. Confirm API response content is escaped before browser rendering.
4. Check for per-user rate limiting on the prompt endpoint.
5. Verify API key is sourced from environment variables.
6. Confirm errors from the OpenAI API are caught and returned as clean error messages, not raw API error objects.

### Best Practices
- Implement a prompt content filter (regex or classifier-based) that rejects prompts containing known injection pattern phrases before forwarding to the LLM.
- Set a `timeout` on the OpenAI API call to prevent the server from hanging on slow LLM responses.
- Audit LLM input and output in the application log at the `debug` level for anomaly detection.

### Common Mistakes
- Concatenating user input directly into the system prompt string, enabling users to override AI behavior with "Ignore all previous instructions..."
- Not setting `max_tokens`, allowing a crafted prompt to generate a 100,000-token response and exhaust the monthly API budget in minutes.
- Returning the raw OpenAI error response object to the client, exposing internal model names, API versioning, and usage data.

### Final Recommendation
**Request Changes.** Verify role separation of user input immediately—this is a `[CRITICAL]` prompt injection risk if user content is combined with system instructions in the same role message. Set `max_tokens` as a `[MAJOR]` requirement. Output encoding and rate limiting are also `[MAJOR]`. Do not approve until all four items are confirmed.

---

## 4. CRM — Contact Management Bulk Delete PR

### User Request
*"Review this PR that adds a bulk delete feature to our CRM. Admins can select multiple contacts and delete them all at once."*

### Requirement Analysis
- **What changed:** A new `DELETE /api/contacts/bulk` endpoint accepts an array of contact IDs and deletes them in a single database operation.
- **Risk profile:** High. Bulk delete is a destructive, irreversible operation. The primary risks are insufficient authorization (any user can delete any contact) and missing confirmation safeguards.
- **Audience:** CRM admins and workspace owners. Regular users must not have access to bulk delete operations.

### Thinking Process
The first question is role-based access: is this endpoint protected by an admin-role check, or is it accessible to any authenticated user? In CRMs, regular sales team members have no business deleting contacts—this must be restricted to admin roles.

Second: tenant isolation. In a multi-tenant CRM, the endpoint must verify that all contact IDs in the request array belong to the requesting workspace. A malicious actor could pass IDs from another tenant's contact database.

Third: soft delete vs. hard delete. Is this operation permanent? If records are hard-deleted, there is no recovery path if an admin accidentally selects the wrong contacts. Soft delete (marking records as `deleted_at` timestamp) is a safer default for bulk operations.

Fourth: input limits. What happens if a user submits an array of 50,000 contact IDs? There must be a maximum batch size enforced.

### Engineering Decisions
- **Role check:** Verify the endpoint middleware confirms the requesting user has the `admin` or `owner` role before executing.
- **Tenant ownership verification:** Before deletion, query the database to confirm all provided IDs belong to the authenticated workspace. Reject the entire batch if any ID is not owned by the requesting tenant.
- **Soft delete:** Implement soft delete using a `deleted_at` timestamp column rather than hard deletion, enabling recovery from accidental operations.
- **Batch size limit:** Enforce a maximum batch size (e.g., 500 contacts per request) to prevent single requests from overwhelming the database.
- **Audit log:** Record the bulk delete event—including the requesting user ID, timestamp, and list of deleted contact IDs—in the audit log.

### Workflow
1. Check for admin role middleware on the bulk delete route.
2. Verify that all contact IDs are validated against the requesting workspace before deletion.
3. Confirm soft delete is used instead of hard `DELETE FROM` statements.
4. Check for a maximum batch size constraint on the ID array.
5. Verify an audit log entry is created for every bulk delete operation.
6. Confirm the endpoint returns a meaningful response: count of deleted records, not just HTTP 200.

### Best Practices
- Use a database transaction to wrap the ownership verification and deletion query. If the verification fails, the transaction rolls back with no partial deletions.
- Return the count of successfully deleted records in the response body so the client can display confirmation to the user.
- Consider a `dryRun` parameter that returns which contacts would be deleted without executing the deletion—valuable for large operations.

### Common Mistakes
- Skipping tenant ownership verification and trusting client-supplied IDs, enabling cross-tenant data deletion.
- Hard-deleting records in a CRM context where sales data has compliance and audit retention requirements.
- No batch size limit, allowing a single request to lock database rows for an extended duration under high load.

### Final Recommendation
**Request Changes.** Tenant ownership verification is `[CRITICAL]` and must be implemented before merge. Role check enforcement is also `[CRITICAL]`. Soft delete vs. hard delete is `[MAJOR]`—confirm with the product owner whether permanent deletion is intended. Batch size limit and audit log are `[MAJOR]` requirements.

---

## 5. ERP — Inventory Stock Update PR

### User Request
*"Review this PR that adds a stock quantity update endpoint to our ERP system. Warehouse staff can submit stock adjustments after physical counts."*

### Requirement Analysis
- **What changed:** A new endpoint allows authenticated warehouse users to submit a quantity adjustment for a specific product SKU. The endpoint updates the `stock_quantity` field in the inventory table.
- **Risk profile:** High. Incorrect inventory data has direct financial and operational impact. Race conditions on concurrent stock updates can produce inaccurate counts. Missing audit trails obscure accountability.
- **Audience:** Warehouse staff and inventory managers. Direct access from untrusted clients must not be possible.

### Thinking Process
The reviewer immediately considers concurrency. If two warehouse staff submit adjustments for the same SKU simultaneously, and the endpoint reads the current value, adds the delta, and writes it back—the classic read-modify-write race condition causes one update to be silently overwritten.

Next: validation. The adjustment value could be negative (write-off) or positive (restocking). Are both accepted? Is there a lower bound preventing stock from going below zero? A stock quantity of -500 is semantically invalid.

Then: audit trail. Every stock adjustment in an ERP system is a regulated financial event. Who made the adjustment? When? What was the previous quantity? This record must be immutable.

Finally: authorization scope. Can any warehouse user adjust any product's stock, or are there warehouse-level or product-category-level access controls?

### Engineering Decisions
- **Atomic update:** Use an SQL atomic increment (`UPDATE inventory SET stock_quantity = stock_quantity + $delta WHERE sku = $sku`) instead of a read-modify-write pattern to prevent race conditions.
- **Non-negative constraint:** Enforce a database-level `CHECK (stock_quantity >= 0)` constraint and validate at the application layer before the database call.
- **Immutable audit log:** Insert a record into a `stock_adjustment_log` table for every update: `sku`, `delta`, `previous_quantity`, `new_quantity`, `user_id`, `timestamp`, `reason`.
- **Role scoping:** Confirm the endpoint validates that the requesting user has warehouse access rights for the product's storage location.

### Workflow
1. Locate the database update logic. Flag any read-modify-write pattern.
2. Verify non-negative stock constraint exists at both application and database layer.
3. Confirm an audit log entry is written on every adjustment.
4. Check role and warehouse scoping on the endpoint.
5. Verify the API accepts and validates a required `reason` field for the adjustment.
6. Confirm a `200` response returns the updated stock value so the UI can display confirmation.

### Best Practices
- Wrap stock updates and audit log inserts in a single database transaction. If the audit log insert fails, the stock update rolls back—preventing untracked inventory changes.
- Set a maximum absolute adjustment delta limit (e.g., ±10,000 units) to flag implausibly large adjustments for manual review.
- Expose a separate read endpoint for adjustment history so warehouse managers can audit changes without querying the primary inventory table.

### Common Mistakes
- Using a read-then-write pattern for stock quantity updates, causing silent data corruption under concurrent warehouse staff submissions.
- Not recording the `previous_quantity` in the audit log, making it impossible to reverse incorrect adjustments.
- Accepting stock adjustment submissions with no `reason` field, making the audit trail entries meaningless for compliance review.

### Final Recommendation
**Request Changes.** The read-modify-write race condition is a `[CRITICAL]` data integrity issue. Replace with an atomic SQL increment immediately. Missing audit log is `[MAJOR]`. Non-negative constraint enforcement is `[MAJOR]`. All findings must be resolved before merge given the financial audit implications of this feature.

---

## 6. Restaurant — Online Ordering & Menu Management PR

### User Request
*"Review this PR that adds a public menu endpoint and an authenticated endpoint for restaurant owners to update menu item prices and availability."*

### Requirement Analysis
- **What changed:** A public `GET /api/menu` endpoint and an authenticated `PATCH /api/menu/:itemId` endpoint for updating price and availability.
- **Risk profile:** Medium. The public endpoint must not expose data intended for internal use. The update endpoint must enforce that only the owning restaurant can modify its own menu items.
- **Audience:** Public customers browsing the menu; restaurant owner/staff managing items.

### Thinking Process
The public menu endpoint is read-only, so the primary concern is data over-exposure: does it return internal fields such as `cost_price`, `supplier_id`, or `internal_notes` that should never be visible to customers?

For the update endpoint, the reviewer checks: does the authorization logic verify that the `itemId` belongs to the restaurant owned by the authenticated user? A restaurant owner from Restaurant A should not be able to modify Restaurant B's menu items by guessing an item ID.

Input validation on price updates is critical. A price field accepting negative values or non-numeric strings without validation leads to corrupted pricing data displayed to customers.

Caching is also a consideration: if the public menu endpoint is cached at the CDN or application layer, price and availability changes must invalidate the cache immediately so customers see current data.

### Engineering Decisions
- **Data projection:** The public menu endpoint query must explicitly select only public-facing fields. Use a DTO or select projection—never `SELECT *` from the menu items table.
- **Ownership check:** The PATCH endpoint must verify `menu_item.restaurant_id === authenticated_user.restaurant_id` before applying the update.
- **Input validation:** Price must be validated as a positive numeric value with a maximum decimal precision (e.g., 2 decimal places). Availability must be a boolean.
- **Cache invalidation:** If the public menu is cached, the PATCH endpoint must invalidate the relevant cache key on successful update.

### Workflow
1. Review public endpoint response payload for internal fields.
2. Check PATCH endpoint for restaurant ownership verification.
3. Validate price and availability input schemas.
4. Confirm cache invalidation logic is triggered on menu updates.
5. Verify error handling returns clean messages for invalid item IDs or unauthorized access.
6. Check rate limiting on the public menu endpoint to prevent scraping.

### Best Practices
- Return availability status as a boolean, not an internal database enum string that exposes internal state machine values.
- Log price change events (previous price, new price, user, timestamp) for financial audit and dispute resolution.
- Apply a short (30–60 second) TTL on public menu caching to balance freshness with server load.

### Common Mistakes
- Returning `cost_price` or `margin_percentage` fields in the public menu API response, exposing business-sensitive pricing data to competitors.
- Skipping the restaurant ownership check on the PATCH endpoint, enabling cross-restaurant menu manipulation.
- Not invalidating the menu cache after price updates, causing customers to see stale prices during peak ordering periods.

### Final Recommendation
**Request Changes.** The ownership check on the PATCH endpoint is `[CRITICAL]`—confirm it is present and tested. Review public menu field projection for data over-exposure as a `[MAJOR]` item. Cache invalidation and input validation are `[MAJOR]` requirements for a production menu system.

---

## 7. E-commerce — Checkout & Payment Flow PR

### User Request
*"Review this PR that implements the checkout flow. It creates an order, processes payment via Stripe, and sends a confirmation email."*

### Requirement Analysis
- **What changed:** A `POST /api/checkout` endpoint that creates an order record, calls the Stripe Payments API, and triggers a confirmation email on success.
- **Risk profile:** Critical. This is a financial transaction flow. Incorrect ordering of operations, missing error handling, or webhook validation failures can result in revenue loss, duplicate charges, or order fulfillment without payment.
- **Audience:** Authenticated and guest customers completing a purchase.

### Thinking Process
The reviewer immediately identifies the dangerous operation sequence: create order → charge Stripe → send email. What happens if the Stripe charge fails after the order is created? Is the order left in a `pending` state, or is it incorrectly marked as `confirmed`?

The correct pattern is: create order in `pending` state → initiate payment intent → listen for Stripe webhook `payment_intent.succeeded` → mark order `confirmed` and send email. The reviewer checks whether the PR follows this event-driven pattern or attempts synchronous payment confirmation.

Next: webhook signature verification. Stripe sends webhooks to confirm payment events. If the endpoint does not verify the Stripe webhook signature, a malicious actor can POST a fake `payment_intent.succeeded` event and receive order confirmation without paying.

Finally: idempotency. What happens if the checkout endpoint is called twice with the same cart? Does it create two orders and two charges?

### Engineering Decisions
- **Async confirmation pattern:** Payment should be confirmed via Stripe webhook, not by trusting the synchronous payment intent response.
- **Order state machine:** Orders must transition through defined states: `pending` → `payment_processing` → `confirmed` → `fulfilled`. Never jump from `pending` to `confirmed` synchronously.
- **Webhook signature verification:** Every incoming Stripe webhook must be verified using the Stripe SDK's signature verification function against the webhook signing secret.
- **Idempotency key:** Include a client-generated idempotency key on the Stripe API call to prevent duplicate charges on retry.

### Workflow
1. Trace the operation sequence. Flag synchronous order confirmation without webhook.
2. Locate the Stripe webhook handler. Verify signature validation is present.
3. Check order state transitions for illegal state jumps.
4. Verify idempotency key is passed on Stripe API calls.
5. Confirm email is sent only after webhook confirms payment success—not after synchronous API response.
6. Check error handling: what state is the order left in if the Stripe API call fails?

### Best Practices
- Store the Stripe `payment_intent_id` on the order record to enable reconciliation and support dispute resolution.
- Implement a dead-letter queue for failed webhook processing so events are not silently dropped if the handler throws.
- Cap retry attempts on confirmation email sends with exponential backoff to avoid email spam on transient failures.

### Common Mistakes
- Marking an order as `confirmed` based on the synchronous Stripe API response instead of waiting for the `payment_intent.succeeded` webhook event.
- Not verifying Stripe webhook signatures, allowing spoofed payment success events to fulfill unpaid orders.
- Sending the confirmation email before the order is committed to the database, then failing to roll back the email if the database write fails.

### Final Recommendation
**Request Changes.** Webhook signature verification is `[CRITICAL]`. Synchronous order confirmation without webhook is `[CRITICAL]`. Both must be resolved before merge. Idempotency key and order state machine are `[MAJOR]`. This PR should not ship in its current form.

---

## 8. Analytics Dashboard — Data Visualization PR

### User Request
*"Review this PR that adds a new analytics dashboard showing user retention, revenue trends, and funnel conversion rates."*

### Requirement Analysis
- **What changed:** New dashboard views backed by three new API endpoints that query aggregated analytics data. Charts are rendered client-side using a visualization library.
- **Risk profile:** Medium. Primary risks are query performance on large datasets, data access authorization, and cross-tenant data leakage in multi-tenant analytics.
- **Audience:** Workspace admins and team leads with analytics access permissions.

### Thinking Process
The reviewer begins by checking who can access this dashboard. Is there a role check confirming the requesting user has analytics access? In many SaaS systems, analytics is a paid tier feature—the endpoint must verify entitlement, not just authentication.

Next: query performance. Analytics aggregations on large event tables are expensive. Are these queries running raw aggregations on every page load, or are they backed by pre-computed materialized views or summary tables?

Then: tenant isolation. Do all three queries filter by `workspace_id`? Missing a tenant filter on a `GROUP BY` aggregation returns aggregate data across all tenants—a severe data leakage issue.

Finally: chart rendering. If any chart label or tooltip renders user-generated content (e.g., campaign names, custom event names), that content must be escaped before display.

### Engineering Decisions
- **Entitlement check:** Verify the dashboard route checks both authentication and the workspace's analytics feature entitlement flag.
- **Tenant filter:** All three aggregation queries must include `WHERE workspace_id = $workspaceId` at the top level—not in a subquery that could be optimized away.
- **Query performance:** Flag any query performing a full table scan on an events table exceeding 1 million rows. Require either an index or a pre-aggregated summary table.
- **Output encoding:** All chart labels derived from user-configurable names must be rendered as text, not HTML, to prevent stored XSS.

### Workflow
1. Check analytics dashboard route for authentication and entitlement middleware.
2. Trace all three endpoint queries for `workspace_id` filter presence.
3. Run `EXPLAIN` on the aggregation queries to identify missing indexes.
4. Review chart rendering for user-generated content that could carry XSS payloads.
5. Verify API response is paginated or date-range scoped to prevent returning unbounded data volumes.
6. Confirm loading, error, and empty states are all handled in the UI components.

### Best Practices
- Cache analytics aggregation results in Redis with a 5-minute TTL—analytics dashboards do not require real-time accuracy.
- Accept `start_date` and `end_date` query parameters and enforce a maximum range (e.g., 90 days) to prevent runaway queries.
- Log dashboard view events for usage analytics to understand which charts are most valuable to users.

### Common Mistakes
- Missing `workspace_id` filter on analytics aggregations, returning cross-tenant data rolled up into a single workspace's dashboard.
- Aggregating against raw event tables with billions of rows on every request without caching or pre-aggregation.
- Rendering user-defined campaign names or event names as HTML in chart tooltips, enabling stored XSS via the analytics data pipeline.

### Final Recommendation
**Request Changes.** Tenant filter verification is `[CRITICAL]`. All three queries must be reviewed for workspace isolation before merge. Query performance on the aggregation endpoints is `[MAJOR]`—require evidence of index coverage or pre-aggregation before approval. Entitlement check is `[MAJOR]`. XSS risk from user-generated chart labels is `[MAJOR]`.

---

## 9. Landing Page — Marketing Page SEO & Performance PR

### User Request
*"Review this PR that rebuilds our SaaS marketing landing page. It includes animations, a pricing table, a features section, and a CTA form."*

### Requirement Analysis
- **What changed:** A new Next.js landing page with CSS animations, a dynamic pricing table with toggle between monthly/annual, a features grid, and an email capture form.
- **Risk profile:** Low–Medium. No authentication or database interaction. Primary concerns are SEO correctness, Core Web Vitals, accessibility, and form submission security.
- **Audience:** Public internet users. Search engine crawlers.

### Thinking Process
Landing pages are judged by two primary metrics: conversion rate and search ranking. Both are directly impacted by performance and SEO. The reviewer checks Core Web Vitals first: LCP (hero image preloaded?), CLS (pricing toggle causes layout shift?), INP (animations blocking main thread?).

Next, SEO fundamentals: is there exactly one `<h1>`? Is the title tag unique and descriptive? Is the meta description present and within 155 characters?

For the email capture form, the reviewer checks: is there basic email format validation on the client? Does the submission endpoint validate server-side? Is there rate limiting to prevent spam submissions?

For animations: are they gated by `prefers-reduced-motion` media queries for users with vestibular disorders?

### Engineering Decisions
- **LCP:** The hero section image or headline text must load within 2.5 seconds. If the hero uses an image, it must be preloaded and served in WebP format.
- **CLS:** The pricing table toggle animation must not cause layout shifts. Components must have stable heights or use `overflow: hidden` with fixed containers.
- **Heading hierarchy:** Exactly one `<h1>` per page. Features section headings use `<h2>`. Feature card headings use `<h3>`.
- **Animation accessibility:** All CSS animations must be wrapped in `@media (prefers-reduced-motion: no-preference)` to disable motion for users who configure reduced motion.
- **Form protection:** The email capture endpoint must enforce rate limiting (e.g., max 3 submissions per IP per hour) and validate email format server-side.

### Workflow
1. Check `<title>` tag uniqueness and `<meta name="description">` presence.
2. Verify heading hierarchy: one `<h1>`, proper `<h2>`/`<h3>` nesting.
3. Check hero image for `<link rel="preload">` and WebP format.
4. Review pricing toggle for CLS risk.
5. Verify animation CSS uses `prefers-reduced-motion` media query.
6. Check email form for client-side validation and server-side rate limiting.
7. Verify all CTA buttons have descriptive accessible labels.

### Best Practices
- Run Lighthouse in CI against the PR preview deployment to catch performance regressions before merge.
- Add `og:title`, `og:description`, and `og:image` Open Graph meta tags for social sharing previews.
- Serve the landing page statically (SSG) rather than server-side rendered to achieve maximum CDN cacheability.

### Common Mistakes
- Using multiple `<h1>` tags (one per feature card) rather than a single page-level heading, confusing search engine crawlers.
- Loading the hero video or animation as an auto-playing `<video>` without checking `prefers-reduced-motion`, causing accessibility violations.
- Missing `meta name="description"` and relying solely on page content for search result snippets.

### Final Recommendation
**Approve with Comments.** No blocking security or correctness issues are expected for a public marketing page with no sensitive data. Flag heading hierarchy, `prefers-reduced-motion` compliance, and email form rate limiting as `[MAJOR]` items to resolve in this PR or immediately following. LCP and CLS items are `[MAJOR]` if Lighthouse scores drop below 90.

---

## 10. Enterprise Project — Microservices Authorization Refactor PR

### User Request
*"Review this large PR that refactors how authorization is handled across our microservices. It introduces a shared authorization middleware library that all services will import."*

### Requirement Analysis
- **What changed:** A new shared `@company/auth-middleware` internal library replaces 6 individual service-level authorization implementations. All 6 services update to import and use the shared library.
- **Risk profile:** Critical. This is a cross-cutting change affecting every service's authorization layer simultaneously. A bug in the shared library creates a vulnerability across the entire platform in a single deployment.
- **Audience:** Internal engineering team. Every service in the platform depends on this library's correctness.

### Thinking Process
The reviewer recognizes this is the highest-risk category of PR: a shared infrastructure change with platform-wide blast radius. The first instinct is to check the testing strategy. Does the shared library have its own comprehensive test suite? Are the service-level integration tests updated to exercise the new middleware?

Next: the reviewer checks whether the refactor preserves all existing authorization behaviors exactly. The safest refactor is one where the new library produces identical decisions to all six previous implementations for every tested input. Any behavioral divergence is a potential authorization gap.

Then: deployment strategy. Rolling out this change to all 6 services simultaneously maximizes risk. Can the library be rolled out service by service with the ability to roll back per-service?

Finally: does the new shared library introduce new dependencies that expand the attack surface? If it pulls in a new JWT library that replaces the vetted library currently in use, that is a supply chain risk requiring separate review.

### Engineering Decisions
- **Test coverage requirement:** The shared library must have ≥90% branch coverage with tests specifically for: valid token acceptance, expired token rejection, invalid signature rejection, missing token rejection, insufficient scope rejection.
- **Behavioral parity validation:** Require evidence (test comparison or integration test matrix) that the new library produces identical authorization decisions as the previous per-service implementations for all current test cases.
- **Incremental rollout:** Request that the PR be scoped to a single service first. Full platform rollout should follow after the first service validates behavior in production.
- **Dependency audit:** Review all new transitive dependencies introduced by the shared library for known CVEs using a dependency scanner.

### Workflow
1. Review the shared library's own test suite for coverage of all token validation scenarios.
2. Verify each service's existing authorization tests are updated to use the new middleware.
3. Request evidence of behavioral parity between old and new implementations.
4. Check for new transitive dependencies and CVE status.
5. Evaluate whether the PR is scoped for incremental rollout or simultaneous platform-wide deployment.
6. Confirm the library exports a clear, stable API contract with TypeScript types.
7. Verify the shared library version is pinned (not a floating semver range) in each service's dependency file.

### Best Practices
- Publish the shared library to the internal package registry with semantic versioning. Services should lock to an exact version, not a range.
- Include a migration guide in the library's README documenting the behavioral differences from each previous implementation.
- Require approval from two senior engineers and a security reviewer for platform-wide authorization changes.

### Common Mistakes
- Merging a platform-wide authorization refactor in a single PR without incremental rollout validation, creating a single deployment that can break all services simultaneously.
- Missing test cases for edge conditions: expired tokens near the boundary, tokens with extra unexpected claims, tokens signed with the wrong algorithm.
- Floating semver version pins (`^1.0.0`) on the shared auth library, allowing minor version updates to silently introduce behavioral changes across all services.

### Final Recommendation
**Request Changes.** This PR requires two conditions before approval: (1) the shared library must have a test suite with complete coverage of all rejection scenarios, and (2) the deployment plan must specify incremental per-service rollout rather than simultaneous platform deployment. Both are `[MAJOR]` requirements. Additionally, request a security review sign-off from the Security Engineer skill before merging any change that modifies platform-wide authorization behavior.
