# AI Engineering Prompts

This document provides reusable, high-fidelity prompt templates to configure and bootstrap AI assistants into the role of an **AI Systems Architect** or **AI Engineer**. It aligns all prompt frameworks, agent systems, retrieval topologies, and evaluations with the [AI Engineering Standards](file:///d:/projects/Nexulyt-AI-OS/standards/ai-engineering-standards.md).

---

## 1. Overview

* **Purpose**: Enforce clean meta-prompting, robust vector search structures, efficient agent loops, scalable multi-agent topologies, Model Context Protocol (MCP) compliance, and strict context budgets.
* **When to Use**: When designing LLM-based features, configuring system prompts for agents, structuring RAG ingestion pipelines, writing MCP schemas, or benchmarking AI output qualities.
* **Inputs**: Target tasks, database contents, API definitions, performance parameters, and evaluation datasets.
* **Expected Outputs**: Meta-prompts, semantic chunking layouts, agent execution steps, supervisor-worker topologies, MCP server definitions, and evaluation metrics.
* **Best Practices**:
  - Keep prompts clear of ambiguous modifiers; use exact parameters, boundaries, and concrete instruction schemas.
  - Utilize few-shot examples inside templates to constrain output variations.
  - Minimize context sizes to leverage prompt caching systems.
* **Common Mistakes**:
  - Overloading context windows with redundant documents instead of using targeted semantic slicing.
  - Allowing agents to execute loops without terminating constraints (maximum iterations, timeouts).

---

## 2. Prompt Templates

### Meta-Prompt Design Prompt
```text
Role: You are a Principal Prompt Engineer.
Context: You are writing a system instruction prompt to configure a raw LLM for: [Insert target application task, e.g., SQL Translation Tool].
Goal: Write a production-grade system instruction schema using structured XML tags.
Inputs:
- Task Scope: [Insert e.g., converts natural language into PostgreSQL queries]
- Constraints: [Insert e.g., must only output valid SQL, no text explanation]
Expected Output Structure:
1. System Instruction Blueprint: Generate the raw system prompt containing `<identity>`, `<instructions>`, `<constraints>`, `<formatting>`, and `<examples>` blocks.
2. Few-Shot Examples: Write representative, realistic example pairs inside the template.
3. Variable Mapping: Specify input parameters required at execution time (e.g., `{{USER_INPUT}}`, `{{SCHEMA_METADATA}}`).
Cross-Reference: Match guides in:
[AI Engineering Standards](file:///d:/projects/Nexulyt-AI-OS/standards/ai-engineering-standards.md)
```

### RAG Chunking & Context Design Prompt
```text
Role: You are a RAG Architect.
Context: You are designing an ingestion and semantic retrieval pipeline for: [Insert document types, e.g., Enterprise PDF contracts].
Goal: Design chunking boundaries and context injection strategies.
Inputs:
- Document Format: [Insert e.g., scanned PDF tables and long text]
- LLM Context Limit: [Insert e.g., 8k context window]
Expected Output Structure:
1. Chunking Strategy: Specify chunking methods (e.g., recursive character splitter, semantic chunking) and overlap values (bytes/tokens).
2. Metadata Tagging Schema: Design tags appended to chunks (e.g., document ID, section title, parent chunk hash) to preserve global context.
3. Context Re-ranking Spec: Design the scoring filter (e.g., Cohere Rerank, Cohere Rerank + BM25) to map the top K chunks into the LLM context.
```

### AI Agent Loop Specification Prompt
```text
Role: You are an Agent Systems Architect.
Context: You are designing a single-agent system equipped with tools to handle: [Insert task, e.g., Automated Dependency Updater].
Goal: Define the agent loop cycle, tools list, and termination boundaries.
Inputs:
- Agent Tools: [List tool parameters, e.g., check_version, write_file, run_tests]
- Goal State: [Insert e.g., update all security patches without breaking tests]
Expected Output Structure:
1. Tool Definition Schema: Define schema shapes (functions, descriptions, arguments) in JSON Schema format.
2. Agent Loop Protocol: Design execution stages (Think -> Select Tool -> Execute Tool -> Observe Result -> Re-evaluate).
3. Safeguards & Constraints: Specify maximum iteration depth, token limits, exception handling paths, and human-in-the-loop approvals.
```

### Multi-Agent Supervisor-Worker Topology Prompt
```text
Role: You are a Multi-Agent Architect.
Context: You are designing a multi-agent network to process: [Insert workflow, e.g., Full-Stack Code Generation].
Goal: Define agent roles, communication protocols, and supervision flow.
Inputs:
- Target Agents: [List roles, e.g., Architect, Frontend Writer, Tester, Reviewer]
- Supervisor Style: [Insert e.g., Centralized Planner vs Decentralized Choreography]
Expected Output Structure:
1. Topology Map: Text/Mermaid.js diagram showing agent communication lines and message queues.
2. Agent Specs: Define the system role, input cues, and output outputs for each agent.
3. Coordination Protocol: Define message payload envelopes and handoff criteria (e.g., "Reviewer accepts code only if Tester returns zero errors").
```

### Model Context Protocol (MCP) Server Schema Prompt
```text
Role: You are an MCP Integration Architect.
Context: You are designing a custom MCP Server to expose local capabilities to an external LLM client: [Insert capability details, e.g., Local Git Log Analyzer].
Goal: Define the server tools, resources, and prompt templates following the MCP specification.
Inputs:
- Exposed Tools: [List tools, e.g., get_commits, compare_branches]
- Local Resources: [List resources, e.g., file paths, database connections]
Expected Output Structure:
1. Server Tool Registry Schema: Output JSON Schema declarations for all tools, parameters, and return types.
2. Resource URI Schema: Define the path format for exposing static data (e.g., `git://{repo}/commits/{hash}`).
3. Security Boundaries: Define access restrictions to prevent LLM client directory traversal or arbitrary execution commands.
```

### Context Engineering & Cache Optimizer Prompt
```text
Role: You are an LLM Performance Engineer.
Context: You are optimizing context consumption for a high-frequency agent: [Insert agent type].
Goal: Define context slicing rules to maximize prompt cache hits and minimize token costs.
Inputs:
- Base Context Composition: [Insert e.g., System prompt + schema metadata + chat history + search results]
Expected Output Structure:
1. Cache-Friendly Ordering: Detail how to order the context (static variables first, dynamic user messages last) to trigger caching mechanism.
2. Slicing Thresholds: Specify sliding window sizes for chat histories and prune rules for old logs.
3. Token Savings Matrix: Compare expected token usage and response times with and without optimization.
```

### AI Evaluation Benchmark Prompt
```text
Role: You are an AI Quality Assurance Engineer.
Context: You are setting up an evaluation benchmark for an LLM feature: [Insert feature, e.g., Code Explainer].
Goal: Structure an evaluation matrix, test dataset, and validation metric checks.
Inputs:
- Expected Output Style: [Insert e.g., short summary + markdown code blocks]
- Metrics: [Insert e.g., Accuracy, Syntactic correctness, Hallucination checks]
Expected Output Structure:
1. Benchmark Cases: Define a set of 5 distinct test payloads (Inputs and Expected Ground Truths).
2. Grading Rubric: Establish structured grading rules (e.g., 1-5 scale) for Semantic Coverage, Format Adherence, and Executability.
3. Automated Evaluation Prompts: Create LLM-as-a-Judge prompt templates to automatically evaluate model outputs.
```

---

## 3. Professional Recommendations

1. **Verify Token Densities**: Use token counters (e.g., tiktoken) inside your agents to inspect exact token weights before sending payloads.
2. **Explicit Fallbacks**: Design agent loops to fall back gracefully to secondary LLMs or request human support if confidence thresholds drop below a set level.
3. **Structured JSON Mode**: When parsing model outputs, configure agents to enforce strict JSON schemas or tool calls to bypass parser failures.
