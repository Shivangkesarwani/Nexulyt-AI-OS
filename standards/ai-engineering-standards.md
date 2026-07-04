# AI Engineering Standards

This document establishes the prompt engineering, LLM integration, retrieval-augmented generation (RAG), agent workflows, and safety standards for all AI-enabled modules in **Nexulyt-AI-OS**.

---

## 1. Prompt Engineering & Context Management

- **Role Separation:** Explicitly separate instructions (system prompt) from runtime user variables. User input must never be directly concatenated into system prompts.
- **Context Pinning:** Selectively slice and filter context data before passing it to the prompt. Do not overload context windows with unneeded data.
- **Structured Output:** Always enforce structured output syntax (e.g. JSON schema, tRPC types) on LLM response endpoints.

---

## 2. RAG & Vector Search

- **Tenant Isolation:** Filter vector database queries using tenant IDs during the retrieval step. Cross-tenant retrieval is a critical security breach.
- **Hybrid Search:** Combine semantic vector search (cosine similarity) with lexical keyword search (BM25) for optimized retrieval accuracy.
- **Chunking Strategy:** Define explicit, document-type based chunking sizes (e.g. 500 characters with 10% overlap).

---

## 3. Agent Workflows & Safety

- **Sandboxed Execution:** AI agents capable of executing dynamic code must run inside isolated sandbox containers (e.g. Docker, gVisor) with network egress blocked.
- **Confirm Tool Calls:** Custom tool executions (especially write, delete, or run operations) must require human confirmation before execution.
- **Log all LLM events:** Log all raw API requests and responses at the debug level for audit trails and threat trace tracking.
