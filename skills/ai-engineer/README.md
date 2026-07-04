# AI Engineer AI Skill

A production-grade AI Skill designed to teach AI assistants to build secure, scalable, and cost-efficient AI applications that align with Staff/Principal-level AI Architects.

---

## 1. Overview
The AI Engineer skill defines the prompt engineering paradigms, context optimizations, memory buffers, RAG pipelines, vector search configurations, multi-agent frameworks, tool integrations, and security guardrails required to build intelligent systems. It prevents AI from producing insecure prompts, raw JSON parsing crashes, or runaway API costs.

---

## 2. Purpose
- **Cost & Token Efficiency:** Enforce prompt caching, token truncation, and model configurations to minimize API expenses.
- **Data Validation:** Enforce native structured outputs and schema validations (Zod) to ensure predictable model behavior.
- **Reliable Architectures:** Standardize RAG retrieval steps, sliding history memory buffers, and multi-agent coordination frameworks.

---

## 3. Responsibilities
- Implement cognitive intelligence pipelines designed by the Software Architect skill.
- Formulate prompts, XML context models, and few-shot examples.
- Select optimal models based on latency (TTFT), reasoning needs, and costs.
- Implement security guardrails and hallucination validations.

---

## 4. Features
- **Structured JSON Parsing:** Native structured output integrations and schema validations.
- **Advanced RAG Retrieval:** Hybrid vector searches (Vector + Keyword) and ranker sorting algorithms.
- **Modular Prompt Chains:** Task workflows split into distinct, validated steps to reduce errors.
- **Self Review Engine:** An automated prompt structure, key validation, and security check loop.

---

## 5. AI Technologies
- **Models:** GPT-4o, Claude 3.5 Sonnet, Llama 3, Gemini 1.5 Pro
- **Vector Databases:** pgvector (PostgreSQL), Pinecone, Qdrant
- **Protocols:** Model Context Protocol (MCP)
- **Validation:** Zod Schema Validation

---

## 6. Workflow
1. **Deconstruct Tasks:** Identify requirements, latency limits, and model parameters.
2. **Setup Prompt & Context:** Write system instructions, XML structures, and few-shot examples.
3. **Build Agents & Tools:** Configure tools, register function schemas, and set up memory buffers.
4. **Audit Security & Budgets:** Run safety guardrail checks and optimize token configurations.

---

## 7. Folder Structure
```
skills/ai-engineer/
├── SKILL.md      # Core AI Identity, prompt paradigms, and agent rules
├── CHECKLIST.md  # Professional QA audit checklists for prompt and RAG reviews
└── EXAMPLES.md   # Step-by-step agent setups and validation examples
```

---

## 8. Compatible Skills
- **Software Architect:** Designs the overall system boundaries and logical API schemas.
- **Backend Engineer:** Integrates LLM tool calls and database queries into service frameworks.
- **Database Architect:** Sets up vector database tables and metadata queries.

---

## 9. Expected Outputs
When activated, this skill generates:
- Structured prompt templates and XML context models.
- Tool schemas and server integrations.
- Structured output parsers and schema validation code.
- Agent loops, memory buffers, and RAG retrieval pipelines.

---

## 10. Best Practices
- **Validate Outputs:** Always validate model outputs using Zod schemas at the application boundary.
- **Minimize Temp:** Set model temperature to `0.0` for tasks requiring factual accuracy.
- **Structured Prompts:** Separate system rules from dynamic user inputs using explicit variables.

---

## 11. Example Requests
- *Request 1:* "Configure a structured output parser for an AI email writer using Zod."
- *Request 2:* "Set up a pgvector schema and metadata search query for a custom documentation database."
- *Request 3:* "Configure a memory summary buffer for a customer chat session using Redis."

---

## 12. License
Licensed under the [MIT License](file:///d:/projects/Nexulyt-AI-OS/LICENSE).
