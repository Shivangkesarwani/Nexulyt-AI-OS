# AI Engineering Checklist

This document provides a quality gate checklist to validate prompt layouts, RAG ingestion pipelines, single and multi-agent loops, evaluation datasets, security shields, and token cost controls in compliance with the [AI Engineering Standards](file:///d:/projects/Nexulyt-AI-OS/standards/ai-engineering-standards.md).

---

## 1. Overview

* **Objective**: Enforce systematic prompt layouts, RAG chunking boundaries, agent termination rules, evaluation rubrics, prompt injection shields, and prompt caching hits.
* **When to Use**: During active LLM application engineering, prompt design runs, agent tools creation, or model benchmarking phases.
* **Prerequisites**: Defined business task, model provider configurations, evaluation data matrices, and latency budgets.

---

## 2. Checklist Items

### Prompt Layout & Structuring
* [ ] **XML Tag Separation**: Enforce structured XML tags (`<identity>`, `<instructions>`, `<constraints>`) inside system prompts.
* [ ] **Few-Shot Representative Pairs**: Include 2-3 input-output examples representing typical edge cases and success paths inside prompt blocks.
* [ ] **Variable Declarations**: Dynamic values are formatted using consistent template syntax (e.g. `{{USER_INPUT}}`) and isolated from instructions.
* [ ] **Instruction Clarity**: Verify prompts contain objective boundaries, negative constraints, and precise formatting rules rather than vague instructions.

### RAG & Context Optimization
* [ ] **Chunk Overlap Boundaries**: Ingestion pipelines use recursive character splitters with documented chunk sizes and overlaps (e.g. 500 characters, 50-character overlap).
* [ ] **Context Re-ranking Filter**: Implement scoring filters (e.g. Cohere Rerank) to restrict injected context chunks to the top K records.
* [ ] **Metadata Propagation**: Document chunks carry parent section metadata tags to maintain global context during semantic queries.
* [ ] **Prompt Caching Organization**: Position static system instructions at the start of prompts, keeping dynamic user histories at the end to trigger prompt caching hit rules.

### Agent Loops & Tool Routing
* [ ] **Loop Execution Limits**: Enforce strict iteration counts (e.g. max 5 loops) and time limits on ReAct agent loops to prevent runaway token costs.
* [ ] **Pydantic Tool Validations**: Expose tools using explicit schema classes (e.g. Pydantic / JSON Schema) to prevent malformed model inputs.
* [ ] **Human-in-the-Loop Approval Gates**: High-risk actions (e.g. database deletes, outbound payment captures) pause execution, requesting manual approval triggers.

### AI Safety, Evaluation & Monitoring
* [ ] **Prompt Injection Shield**: Install guardrail classifiers to scan user-supplied prompts for instruction-override attempts *before* they enter agents context.
* [ ] **Output PII Filter**: Run post-generation filters (e.g., regex, NER classifiers) to scan and scrub credentials or PII before returning payloads.
* [ ] **LLM-as-a-Judge Rubrics**: Implement evaluation datasets containing baseline scenarios and auto-judge prompts scoring output coverage and hallucinations.
* [ ] **Token Usage Telemetry**: Track token count budgets per user/session, writing usage statistics to database ledgers.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Confirm that prompt designs achieve similarity cache matches using test suites.
* Test agent error recovery by returning mock failures from MCP tools, verifying agents retry or log errors safely.
* Run prompt injection classifiers against test jailbreak strings to confirm correct filtering behavior.

### Exit Criteria
* **Evaluation Target Reached**: Prompt configuration passes the evaluation benchmarks target thresholds.
* **Safety Verification Log**: Guardrails validated against test datasets with zero security bypass events.
* **Token Caching Validated**: Logs verify that static instructions trigger prompt cache hit signals.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Injecting entire database logs or raw document files directly into prompts, causing context window exhaustion.
* Allowing agents to query tools in loops without set iteration limits, causing high API billing spikes.
* Trusting LLM output structures blindly without validating JSON properties at the API parser level.

### Professional Recommendations
1. **Enforce Static Instruction Caching**: Organize system variables carefully. If system prompts change dynamically on every request, providers cannot cache them, increasing latency and cost.
2. **Scrub PII at boundaries**: Run input/output filters on separate, lightweight pipelines to minimize latency impacts.
3. **Establish Regression Test Sets**: Maintain a suite of at least 20 static query cases, testing system prompts against these sets before deployments.
