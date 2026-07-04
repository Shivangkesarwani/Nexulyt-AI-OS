# AI Engineer Design Examples

This document details production-grade AI system architectures, prompt strategies, model selections, and optimization pathways across multiple real-world scenarios.

---

## 1. AI Chatbot

### User Request
Create a customer-facing support chatbot that answers queries using our company FAQ database.

### Requirement Analysis
- **Core Needs:** Low latency (TTFT), conversational history memory, high retrieval accuracy, and fallback safety options.
- **Constraints:** Prevent prompt injections and ensure token usage remains within constraints over long conversations.

### AI Architecture
- A chat controller integrated with a sliding-window memory buffer (stored in Redis) and a RAG semantic search step.

### Prompt Strategy
- System prompt defines the support role, links context using explicit XML tags (`<faq_context>`), and defines fallbacks for out-of-scope queries.

### Model Selection
- Primary Model: GPT-4o-mini (low latency and API cost).
- Guardrail Model: Llama Guard (input/output filtering).

### RAG Strategy
- Query embeddings generated using `text-embedding-3-small` and run against `pgvector` indexes.

### Security
- User queries are sanitized and checked against injection patterns before model calls.
- Output guardrails check responses to ensure no client PII is leaked.

### Cost Optimization
- Enable prompt caching for static FAQ document context.
- Keep the sliding history window capped to the most recent 10 messages.

### Final Recommendation
Standardize RAG retrievals on PostgreSQL `pgvector` and use `gpt-4o-mini` with prompt caching enabled to balance latency and cost.

---

## 2. AI SaaS

### User Request
Design a copywriting SaaS platform that generates marketing posts matching brand tones.

### Requirement Analysis
- **Core Needs:** Structured output formatting, brand copy templates, credit wallet integration, and audit logs.
- **Constraints:** Guarantee JSON outputs match data models without schema failures.

### AI Architecture
- An Express server that uses structured output options (`response_format: { type: "json_object" }`) to generate copy.

### Prompt Strategy
- System instructions define target copywriting formulas, brand guidelines, and examples.

### Model Selection
- Primary Model: Claude 3.5 Sonnet (complex copywriting accuracy).
- Secondary Model: GPT-4o-mini (basic classification and SEO tag generation).

### RAG Strategy
- RAG is not required; brand copy examples are injected directly into system prompt guidelines.

### Security
- Validate user brand templates against Zod schemas before compiling prompts.
- Force rate-limiting configurations on generation endpoints.

### Cost Optimization
- Use `gpt-4o-mini` to classify user inputs and select copy routes before calling heavier reasoning models.
- Set strict limits on the generated output size (`max_tokens`).

### Final Recommendation
Utilize Claude 3.5 Sonnet's native structured JSON capabilities to deliver reliable marketing copy outputs.

---

## 3. Personal AI Assistant

### User Request
Design a personal AI assistant to manage calendars, track tasks, and execute actions based on user commands.

### Requirement Analysis
- **Core Needs:** Real-time tool execution, structured function schemas, state tracking, and fast response streaming.
- **Constraints:** Prevent unintended tool actions and handle API timeouts gracefully.

### AI Architecture
- An agent loop system connected to tool integration routes and a Redis memory session cache.

### Prompt Strategy
- System prompt details available tools, parameters, and step-by-step thinking processes (Reasoning paths).

### Model Selection
- Primary Model: GPT-4o (fast tool calling capabilities).
- Agent Executor: Local Node.js controller that parses and executes tool calls.

### RAG Strategy
- RAG is not required; user data is fetched dynamically using backend database tools when requested.

### Security
- User confirmation is required before running write-action tools (e.g., deleting tasks).
- Tool parameters are validated against Zod schemas before running tool code.

### Cost Optimization
- Limit the maximum number of agent execution steps to 5 to prevent infinite loops.
- Cache user profile data locally in the API server context.

### Final Recommendation
Use GPT-4o's native function calling interface to ensure reliable tool selection and execution.

---

## 4. RAG Application

### User Request
Build an internal query tool to search and summarize thousands of pages of technical documentation.

### Requirement Analysis
- **Core Needs:** Multi-document chunking, hybrid search options, document reranking, and citation accuracy.
- **Constraints:** Prevent hallucinations and manage token costs for large search outputs.

### AI Architecture
- A hybrid search system (Vector + BM25) paired with a reranker model (e.g., Cohere Rerank) and an LLM compilation pipeline.

### Prompt Strategy
- System prompt instructs the model to rely only on the provided context and say "I don't know" if facts are missing.

### Model Selection
- Primary Model: Claude 3.5 Sonnet (large context window and reasoning capability).
- Embedding Model: `text-embedding-3-large` (3072 dimensions).

### RAG Strategy
- Chunk documents into 512-token segments with a 10% overlap.
- Rerank search results using Cohere Rerank, selecting only the top 10 chunks for the LLM prompt.

### Security
- Restrict document access based on user authorization roles (RBAC) at the database retrieval layer.
- Mask database file paths in final text outputs.

### Cost Optimization
- Enable prompt caching on Claude 3.5 Sonnet for static context blocks.
- Filter out duplicate document chunks during the retrieval step.

### Final Recommendation
Implement hybrid vector search with Cohere reranking, and enforce strict citation guidelines in prompts to prevent hallucinations.

---

## 5. Multi-Agent Workflow

### User Request
Build an automated newsletter writing workflow that researches topics, writes copy, and audits formatting.

### Requirement Analysis
- **Core Needs:** Task delegation, structured data schemas, multi-agent coordination, and output validation.
- **Constraints:** Prevent infinite loops and coordinate communications between agents.

### AI Architecture
- A multi-agent framework with a central Coordinator Agent and specialist agents (Researcher, Writer, Auditor).

### Prompt Strategy
- System prompts define focused specialties and output guidelines for each agent.

### Model Selection
- Coordinator & Writer: Claude 3.5 Sonnet (complex reasoning and writing).
- Researcher & Auditor: GPT-4o-mini (low latency and API cost).

### RAG Strategy
- The Researcher agent uses custom web search tools to gather context dynamically.

### Security
- Enforce strict Zod schemas on data shared between agents.
- Run Researcher tool calls in a secure sandbox environment.

### Cost Optimization
- Set strict limits on the maximum number of steps for the agent workflow.
- Use smaller models (GPT-4o-mini) for research and auditing steps.

### Final Recommendation
Use Claude 3.5 Sonnet as the Coordinator agent, and validate all data shared between agents using Zod schemas.

---

## 6. AI Coding Assistant

### User Request
Create an in-editor AI coding assistant that generates code, refactors functions, and writes tests.

### Requirement Analysis
- **Core Needs:** Code generation accuracy, repository context parsing, structured output schemas, and low latency.
- **Constraints:** Prevent syntax errors and manage context windows for large codebases.

### AI Architecture
- An editor plugin integrated with an AST parser to extract code context, paired with an LLM generation pipeline.

### Prompt Strategy
- System prompt defines coding rules, formatting styles, and separates file context using XML tags (e.g., `<file_content>`).

### Model Selection
- Primary Model: Claude 3.5 Sonnet (coding accuracy).
- Refactoring Model: Llama 3 70B (fast open-weights serving).

### RAG Strategy
- Index repository code structures using tree-sitter, mapping symbols to a vector database for semantic retrieval.

### Security
- Sanitize user file paths to prevent directory traversal attacks.
- Scan generated code for common security vulnerabilities (e.g., hardcoded keys) before output.

### Cost Optimization
- Enable prompt caching for static project files and system instructions.
- Limit code generation output sizes using `max_tokens` limits.

### Final Recommendation
Standardize repository parsing on tree-sitter AST maps, and use Claude 3.5 Sonnet with prompt caching enabled for accurate code generation.

---

## 7. AI Customer Support

### User Request
Build an automated support system that answers client emails, drafts refunds, and escalates tickets.

### Requirement Analysis
- **Core Needs:** Sentiment analysis, automated tool routing, database updates, and email draft generation.
- **Constraints:** Prevent unauthorized refunds and handle customer escalations gracefully.

### AI Architecture
- An event-driven API server that triggers agent loops based on incoming email webhooks.

### Prompt Strategy
- System prompts define escalation guidelines, support policies, and email formatting constraints.

### Model Selection
- Primary Model: GPT-4o (fast tool routing and classification).
- Writer Model: Claude 3.5 Sonnet (empathetic writing).

### RAG Strategy
- Query internal user history logs and help documentation databases to gather ticket context.

### Security
- Enforce manual approval steps for all write actions (e.g., issuing refunds, sending emails).
- Validate all incoming webhook signatures before processing tickets.

### Cost Optimization
- Use GPT-4o-mini to classify emails and route simple queries, reserving heavier models for escalations.
- Cache common email templates to avoid redundant generation calls.

### Final Recommendation
Implement a human-in-the-loop review workflow for high-risk actions (e.g., refunds) while automating common FAQ email replies.

---

## 8. AI Document Analyzer

### User Request
Create a document analyzer that extracts financial metrics, contract clauses, and key dates from PDFs.

### Requirement Analysis
- **Core Needs:** PDF parsing, structured data extraction, schema validation, and high accuracy.
- **Constraints:** Prevent data extraction omissions and handle large documents.

### AI Architecture
- A document processing pipeline that extracts text from PDFs, chunks content, and calls LLM extraction endpoints.

### Prompt Strategy
- System prompts define target extraction schemas, formatting rules, and list examples.

### Model Selection
- Primary Model: Claude 3.5 Sonnet (large context window and extraction accuracy).
- Extraction validator: GPT-4o-mini (schema verification).

### RAG Strategy
- Chunk large PDFs into 1000-token segments with overlap, using keyword index mapping to locate targets.

### Security
- Enforce secure file upload validations (e.g., file size limits, PDF malware scans).
- Encrypt extracted financial records in the database.

### Cost Optimization
- Use regex and text search tools to locate target document sections before calling the LLM.
- Compress extracted output formats using compact JSON schemas.

### Final Recommendation
Use Claude 3.5 Sonnet for document parsing, and validate all extracted fields against Zod schemas to ensure accuracy.
