# Prompt Writing Guide

This document provides reusable, high-fidelity prompt templates, frameworks, and guidelines on how to author and configure production-grade, deterministic AI prompts. It aligns prompt designs with the [AI Engineering Standards](file:///d:/projects/Nexulyt-AI-OS/standards/ai-engineering-standards.md).

---

## 1. Overview

* **Purpose**: Provide developers with a systematic methodology to write instructions that minimize LLM hallucinations, enforce output formats, optimize system boundaries, and regulate context usage.
* **When to Use**: When creating new prompt configurations, writing agent system profiles, constructing prompt-based templates, or refining LLM interaction loops.
* **Inputs**: Target tasks, framework preferences (e.g., CO-STAR), output schema definitions, variables configurations, and context scopes.
* **Expected Outputs**: Production-grade system prompts, few-shot examples layouts, context boundary structures, and instruction filters.
* **Best Practices**:
  - Leverage structured XML tags (`<identity>`, `<instructions>`, `<constraints>`) to separate prompts sections.
  - Define explicit output models (JSON schemas, YAML) to enable automated downstream parsing.
  - Run prompts through systematic evaluations using representative benchmark sets before committing to production.
* **Common Mistakes**:
  - Utilizing vague descriptors like "be helpful" or "avoid bad quality code" instead of concrete, actionable rules.
  - Hardcoding variables in the prompt instead of using standard templating variables (e.g., `{{VARIABLE_NAME}}`).

---

## 2. Prompt Engineering Frameworks

### CO-STAR Prompting Framework
Use the CO-STAR structure to construct comprehensive prompts:
* **Context (C)**: Provide background on the task or scenario.
* **Objective (O)**: State the exact task the model must perform.
* **Style (S)**: Define the tone and writing persona (e.g., technical analyst).
* **Tone (T)**: Establish the execution vibe (e.g., direct, objective).
* **Audience (A)**: Detail who will consume the output (e.g., backend developers).
* **Response (R)**: Enforce the precise format (e.g., JSON schema, Markdown tables).

### CREATE Prompting Framework
Use the CREATE structure for task-oriented outputs:
* **Character (C)**: Define the role or expert persona.
* **Request (R)**: Clearly state the goal or request.
* **Examples (E)**: Embed few-shot input-output pairs to guide the model.
* **Adjustment (A)**: Outline negative constraints (what *not* to do).
* **Type (T)**: Enforce the output structure (e.g., code diff, checklist).
* **Execution (E)**: Guide the logical path the model should take.

---

## 3. Prompt Templates

### System Prompt & Meta-Prompt Blueprint
```text
Role: You are a Meta-Prompt Engineer.
Context: You are designing a system instruction prompt to configure an LLM for: [Insert task summary, e.g., Code Reviewer Agent].
Goal: Write a structured, production-grade system instruction document.
Inputs:
- Agent Role: [Insert expert persona]
- Primary Task: [Insert details of action required]
- Output Format: [Insert e.g., JSON schema / Markdown table]
Expected Output Structure:
1. Meta-Prompt Structure: System instruction output using structured XML tags:
   - `<identity>`: Define role, credentials, and tone.
   - `<instructions>`: Clear step-by-step logic path.
   - `<constraints>`: Negative constraints and forbidden behaviors.
   - `<output_schema>`: The required formatting specifications.
   - `<few_shot_examples>`: Variables mappings and draft examples placeholders.
```

### CO-STAR Prompt Generator Prompt
```text
Role: You are a Prompt Architect.
Context: You are constructing a user prompt for: [Insert goal, e.g., SQL query generator].
Goal: Generate a CO-STAR aligned user prompt template.
Inputs:
- Task Context: [Describe system data sources]
- Objective: [Insert target output goal]
Expected Output Structure:
1. CO-STAR Prompt Payload: Output the structured CO-STAR prompt containing Context, Objective, Style, Tone, Audience, and Response blocks.
2. Variable Map: Outline variables (denoted as `{{VARIABLE_NAME}}`) and define details of what to inject at runtime.
```

### Few-Shot Example Design Prompt
```text
Role: You are a Prompt Alignment Engineer.
Context: You are designing few-shot examples to stabilize LLM outputs for: [Insert task].
Goal: Create realistic few-shot input-output pairs.
Inputs:
- Input Payload Shape: [Describe typical user query inputs]
- Expected Output Shape: [Describe target format, e.g., JSON schemas]
Expected Output Structure:
1. Few-Shot Examples: Outline 2-3 distinct input-output pairs showing edge cases (e.g., error inputs vs success inputs).
2. Boundary Rules: Document why each example was selected and how it guides the model's logic boundaries.
```

### Chain-of-Thought (CoT) Prompting template
```text
Role: You are a Logical Reasoning Expert.
Context: You are designing a prompt for complex calculations or reasoning steps: [Insert problem area].
Goal: Define a Chain-of-Thought (CoT) prompt structure that forces step-by-step reasoning.
Inputs:
- Problem details: [Describe algorithmic or logical problem]
Expected Output Structure:
1. CoT Instruction: Write instructions forcing the LLM to write out its intermediate thinking process using a `<thinking>` XML block before outputting the final result.
2. Thinking Step Guidelines: Define what logical checks the model must perform within the thinking block (e.g., check variables constraints, calculate space complexity).
```

### ReAct (Reasoning and Acting) Loop Prompt
```text
Role: You are an Agent Loop Architect.
Context: You are designing a prompt structure to execute a ReAct (Reasoning + Acting) loop for a tool-enabled agent: [Insert agent task].
Goal: Generate system instructions enforcing ReAct structures.
Inputs:
- System Tools: [List tools, e.g., read_file, exec_sql]
Expected Output Structure:
1. ReAct System Prompt: Generate prompt forcing the cycle:
   - **Thought**: The model reasons about the current state.
   - **Action**: The model selects and calls a tool.
   - **Observation**: The model observes tool results.
   - (Loop until completion criteria are met).
2. Exit Criteria: Define how the model detects it has reached the target state and outputs the final answer.
```

---

## 4. Professional Recommendations

1. **Prune Prompt Redundancies**: Routinely scan system prompts to remove soft modifiers, saving token cost and improving response determinism.
2. **Handle Context Drift**: In conversational interfaces, ensure system prompts are injected as system instructions rather than raw user history to prevent context drift.
3. **Use Markdown to Structure Input Data**: Format runtime context (source code files, variables, schemas) in markdown blocks (e.g. ` ```typescript `) to help the model parse boundaries.
