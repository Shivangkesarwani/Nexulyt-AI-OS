# AI Engineer Review & QA Checklist

Use this checklist to perform a comprehensive prompt, context, RAG, and agent code audit before staging commits.

---

## 1. AI PLANNING
- [ ] **Functional Alignment:** The selected AI architecture maps accurately to system capability specifications.
- [ ] **Workload Fit:** The design splits cognitive workloads into prompt chains rather than relying on single monolithic LLM queries.
- [ ] **Performance Bounds:** Target user session latency, TTFT, and API costs have documented limits.

---

## 2. MODEL SELECTION
- [ ] **Task Matching:** Simple text parsing and classification steps use fast open-weights models (e.g., Llama 3 8B).
- [ ] **Frontier Choice:** Proprietary frontier models (e.g., Claude 3.5 Sonnet) are used only for complex reasoning or coding tasks.
- [ ] **Model Configuration:** Temperature and top_p values are configured based on the task (e.g., `temperature = 0.0` for structured outputs).
- [ ] **Multi-Provider Fallback:** The codebase defines fallback models and API routes to ensure availability during outages.

---

## 3. PROMPT DESIGN
- [ ] **XML Structuring:** Prompt segments (instructions, context, variables) are separated using XML tags (e.g., `<rules>`).
- [ ] **Input Segregation:** Dynamic user inputs are isolated to prevent prompt injection attempts.
- [ ] **Few-Shot Examples:** Complex output formatting prompts include mock input/output examples.
- [ ] **Positive Assertions:** Rules are stated as positive assertions (what the model *should* do) rather than negative limits.

---

## 4. RAG
- [ ] **Hybrid Search:** Search queries combine dense vector lookups and keyword indexing (BM25).
- [ ] **Reranker Integration:** Chunks are ranked by semantic relevance using ranker models (e.g., Cohere Rerank) before prompt injection.
- [ ] **Citation Enforcement:** Prompt instructions require the model to cite document IDs next to all factual assertions.
- [ ] **Chunk Size Match:** Document chunk sizes are optimized for content (e.g., 256 tokens for detailed logs).

---

## 5. AGENT DESIGN
- [ ] **Iteration Limit:** All agent loops enforce maximum step limits to prevent infinite execution loops.
- [ ] **Schema Validation:** Tool argument variables are validated using Zod schemas before running tool code.
- [ ] **Low Privileges:** Tool scripts run with minimal system privileges to protect host servers.
- [ ] **Reasoning Telemetry:** Agent reasoning steps and execution times are logged for debugging.

---

## 6. SECURITY
- [ ] **Input Sanitization:** User inputs are sanitized before being injected into prompt templates.
- [ ] **Jailbreak Filters:** Guardrail models (e.g., Llama Guard) filter input prompts and output responses.
- [ ] **PII Masking:** Output guardrails mask private data (e.g., emails, phone numbers) before returning responses.
- [ ] **Secrets Security:** API keys and connection secrets are loaded from environment variables and never committed to git.

---

## 7. EVALUATION
- [ ] **Golden Test Suite:** Prompts are evaluated against golden datasets containing target test cases.
- [ ] **Metrics Tracking:** Automated metrics track retrieval accuracy (precision, recall) and output correctness.
- [ ] **Judge Pattern:** Qualitative outputs are evaluated using automated LLM-as-a-judge patterns.

---

## 8. COST OPTIMIZATION
- [ ] **Prompt Caching:** Caching is enabled for static context segments (e.g., system instructions, documentation).
- [ ] **History Truncation:** Sliding window memory systems summarize and truncate chat logs to limit token bloat.
- [ ] **Max Tokens Limit:** All API completion requests specify `max_tokens` to prevent runaway generation costs.

---

## 9. MONITORING
- [ ] **APM Dashboard:** Telemetry tracking tools (e.g., Langfuse) log request latencies, costs, and token usage in real-time.
- [ ] **Structured Logs:** Logging systems write structured JSON outputs, avoiding raw text format.
- [ ] **Error Alerts:** Automated alert thresholds warn teams if error rates, latency (TTFT), or API costs spike.

---

## 10. TESTING
- [ ] **Mocked Unit Tests:** Unit tests mock API call actions to keep runs fast and cost-free.
- [ ] **Integration Checks:** Integration tests check connections against real models using deterministic variables (`temperature = 0.0`).
- [ ] **Timeout Handling:** Network mocks check that the codebase handles rate limits (HTTP 429) and timeouts gracefully.

---

## 11. DEPLOYMENT
- [ ] **Versioned Prompts:** Prompt configurations are version-controlled in the repository.
- [ ] **Non-root Container:** Deployment Dockerfiles use multi-stage builds and run with non-root privileges.
- [ ] **Environment Setup:** Production API keys and host variables are validated before deploys.

---

## 12. FINAL REVIEW
- [ ] **No Dead Code:** console logs, debuggers, and unused prompt templates are removed from branches.
- [ ] **Ethics Audit:** The user interface informs users that the service utilizes AI and may generate incorrect information.
- [ ] **Schema Compliance:** LLM responses parse cleanly against Zod schemas without throws.
