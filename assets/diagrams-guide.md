# Diagrams Guide

This document defines the coding practices, styling guidelines, and structural patterns for all technical architecture diagrams rendered using **Mermaid.js** within the **Nexulyt-AI-OS** repository.

---

## 1. Mermaid.js Coding Standards

Mermaid.js allows rendering dynamic, version-controlled diagrams inside Markdown files. To prevent syntax errors and maintain visual clarity, follow these coding rules:

* **Use Fenced Code Blocks**: Always wrap Mermaid code blocks in three backticks followed by the `mermaid` language identifier.
* **Double-Quotes on Text Labels**: Always wrap label strings in double-quotes to prevent parser syntax errors when using special characters (e.g. parentheses or brackets).
  * **Correct**: `nodeA["Label (Metadata)"]`
  * **Incorrect**: `nodeA[Label (Metadata)]`
* **Clean Indentations**: Use 4-space indentations for sub-graphs and relationship lists to maintain readability.

---

## 2. Diagram Type Guidelines

### Flowcharts (`graph TD` / `graph LR`)
* Use Flowcharts to illustrate logic sequences and execution pipelines (such as workflows).
* Use top-down layouts (`graph TD`) for sequential pipeline steps, and left-to-right (`graph LR`) for quick step-by-step progressions.
* Style decision diamonds clearly, labeling yes/no paths explicitly.

### Architecture Diagrams (C4 model style)
* Model C4 container diagrams using nested sub-graphs representing system boundaries.
* Clearly isolate frontend clients, API gateways, database clusters, and background queue workers into distinct containers.

### Sequence Diagrams (`sequenceDiagram`)
* Use Sequence diagrams to illustrate chronological process flows, API handshakes, and microservices event loops.
* Explicitly label client requests, middleware validations, database writes, and client notifications.
* Use `activate` and `deactivate` tags on lifelines to show execution durations.

### Entity Relationship Diagrams (`erDiagram`)
* Use ER diagrams to document database tables, field attributes, primary/foreign keys, and cardinalities.
* Declare key attributes (PK, FK) explicitly.
* Map cardinalities using standard notation symbols (`||--||`, `||--|{`, `||--o{`).

---

## 3. Style & Color Customizations

To align Mermaid diagram outputs with our dark theme branding:
* **Background Canvas**: Use dark slate background settings.
* **Border Colors**: Use thin borders in slate-700.
* **Node Colors**: Style active nodes using neon violet backgrounds (`#8B5CF6`) and white text. Secondary database or queue nodes utilize slate gray backgrounds (`#475569`).

---

## 4. Professional Recommendations

1. **Test Rendering**: Verify Mermaid codes render correctly in standard markdown viewers (e.g. GitHub preview, VS Code markdown extensions) before committing.
2. **Avoid HTML in Labels**: Do not inject HTML tags inside node text labels; use plaintext strings to prevent compiler parsing failures.
3. **Limit Complexity**: Keep diagrams focused on specific modules. If a diagram becomes too cluttered, split it into multiple nested sub-diagrams.
