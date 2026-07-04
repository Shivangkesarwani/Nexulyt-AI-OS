# Backend Development Prompts

This document provides reusable, high-fidelity prompt templates to configure and bootstrap AI assistants into the role of a **Lead Backend Engineer**. It aligns all backend architecture, endpoints, security boundaries, and error protocols with the [Coding Standards](file:///d:/projects/Nexulyt-AI-OS/standards/coding-standards.md), [API Design Standards](file:///d:/projects/Nexulyt-AI-OS/standards/api-design-standards.md), and [Security Standards](file:///d:/projects/Nexulyt-AI-OS/standards/security-standards.md).

---

## 1. Overview

* **Purpose**: Enforce stateless api structures, secure identity protocols, robust payload validation, trace-friendly structured logging, and standardized client error protocols.
* **When to Use**: When creating REST or gRPC endpoint handlers, adding JWT authentication, defining role-based authorization parameters, drafting validation schemas, or implementing global error capture middleware.
* **Inputs**: Payload models, database schemas, authorization rules, security requirements, transaction models, and trace details.
* **Expected Outputs**: Controller handlers, middleware outlines, token generation specifications, validation schemas (e.g., Zod), and RFC 7807 error format configurations.
* **Best Practices**:
  - Implement request schema validation at the system boundary before hitting handler business logic.
  - Structure error payloads using a uniform standard (e.g., RFC 7807 problem details) rather than returning raw stack traces.
* **Common Mistakes**:
  - Storing user state in server memory instead of utilizing stateless tokens or external caches.
  - Logging sensitive personal data (PII) or secrets (JWT tokens, API keys) in application logs.

---

## 2. Prompt Templates

### REST API Handler Prompt
```text
Role: You are a Senior Backend Engineer.
Context: You are designing a REST API route handler for: [Insert resource details, e.g., Create Organization Space].
Goal: Define the route controller interface, execution flow, and database transaction boundaries.
Inputs:
- HTTP Method & Path: [Insert e.g., POST /api/v1/spaces]
- Database Operations: [Insert e.g., insert space, add creator to membership table]
Expected Output Structure:
1. Controller Flow Steps: Step-by-step description of the execution pipeline (sanitize, validate, execute transaction, dispatch event, return payload).
2. Transaction Boundary: Identify where transactions begin and commit/rollback.
3. Response Status Map: List HTTP status codes returned for success (201 Created), validation failure (400 Bad Request), duplicate resource (409 Conflict), and server failure (500).
Cross-Reference: Match guidelines in:
[API Design Standards](file:///d:/projects/Nexulyt-AI-OS/standards/api-design-standards.md)
```

### Authentication Flow Prompt
```text
Role: You are a Backend Security Architect.
Context: You are designing an authentication structure for: [Insert app type, e.g., SaaS Web App].
Goal: Establish JWT token lifecycle, signature keys, and refresh patterns.
Inputs:
- Auth Strategy: [Insert e.g., JWT with short-lived access tokens and database-backed refresh tokens]
- Social Providers: [Insert e.g., OAuth 2.0 Google Sign-In]
Expected Output Structure:
1. Lifecycle Flowchart: Text description of the access and refresh token lifecycle (issue, validation, expiration, rotation).
2. Payload Structure: List key JWT claims (sub, exp, iat, roles) and detail verification protocols.
3. Security Constraints: Define token storage methods (e.g., HttpOnly, Secure cookies) and CSRF protection headers.
```

### Authorization Rules Prompt
```text
Role: You are a Security Architect.
Context: You are defining the authorization mechanism for: [Insert system name].
Goal: Specify RBAC (Role-Based Access Control) or ABAC (Attribute-Based Access Control) verification policies.
Inputs:
- User Roles: [Insert e.g., Admin, Moderator, Member]
- Domain Resource: [Insert e.g., Project Workspace]
Expected Output Structure:
1. Permission Matrix: Table mapping Roles to Resource Actions (Read, Write, Delete, Share).
2. Middleware Logic: Step-by-step logic checking token attributes against requested resource parameters.
3. Edge Case Mitigation: Handle resource ownership checks (e.g., "Member can only edit projects they created").
Cross-Reference: Match rules in:
[Security Standards](file:///d:/projects/Nexulyt-AI-OS/standards/security-standards.md)
```

### Microservice Event Contract Prompt
```text
Role: You are a Distributed Systems Architect.
Context: You are designing a decoupled service-to-service communication event for: [Insert event trigger, e.g., Payment Completed].
Goal: Define the event message structure and consumer routing patterns.
Inputs:
- Message Broker: [Insert e.g., RabbitMQ, Apache Kafka]
- Payload Parameters: [Insert e.g., invoice ID, transaction amount, customer ID]
Expected Output Structure:
1. Event Schema: JSON schema definition of the event message including standard headers (event ID, timestamp, publisher, correlation ID).
2. Producer Action: Describe when the event is sent relative to the local DB transaction.
3. Consumer Behaviors: Explain idempotent checks and retry/dead-letter queue (DLQ) pathways.
```

### Structured Logging Prompt
```text
Role: You are a Backend Platform Engineer.
Context: You are defining a structured logging standard for backend services: [Insert runtime, e.g., Node.js / Go].
Goal: Standardize log levels and formats to enable easy analysis in logging engines.
Inputs:
- Log Output: [Insert e.g., Console STDOUT in JSON format]
- Tracking Context: [Insert e.g., Request ID, Tenant ID, User ID]
Expected Output Structure:
1. Base Log Schema: JSON structure containing timestamp, level, message, correlationId, serviceName, and error metadata.
2. Log Level Protocol: Define strict boundaries for using TRACE, DEBUG, INFO, WARN, ERROR, and FATAL.
3. Scrubbing Rules: Detail filtering strategies to ensure PII and credentials are never printed to logs.
```

### Request Validation Schema Prompt
```text
Role: You are a Backend Engineer.
Context: You are writing validation schemas for incoming HTTP requests: [Insert endpoint payload].
Goal: Define payload constraints using Zod or a similar validation library schema format.
Inputs:
- Schema Properties: [Insert payload fields and validation bounds, e.g., email, password length, phone formats]
Expected Output Structure:
1. Schema Definitions: Outline TypeScript validation shapes (e.g., Zod schemas) for headers, query params, and body.
2. Sanitization Pipeline: Outline preprocessing rules (e.g., trim email, lowercase domains, strip script tags).
3. Custom Validators: Define custom validation checks for dynamic formats.
```

### Global Error Handling Prompt
```text
Role: You are a Backend Lead Engineer.
Context: You are setting up global error intercept middleware: [Insert framework, e.g., Express, Fastify, NestJS].
Goal: Configure global error capture routing that maps exceptions to standard error responses.
Inputs:
- Standard Format: [Insert e.g., RFC 7807 Problem Details]
Expected Output Structure:
1. Middleware Logic: Step-by-step flow handling unknown syntax errors, validation failures, database locks, and internal exceptions.
2. Error Code Dictionary: Map system exception names (e.g., `EntityNotFoundException`) to HTTP statuses and user-safe messages.
3. Interceptor Tracing: Detail how error incidents trigger system alarms and populate transaction logs with trace identifiers.
```

---

## 3. Professional Recommendations

1. **Schema Validation by Default**: Every API handler must pass inputs through validation schema middlewares before processing data.
2. **Correlation IDs**: Pass correlation tokens through every downstream API request and event log payload to enable distributed trace analysis.
3. **Idempotent Handlers**: Ensure all POST/PUT write events support idempotency keys to prevent duplicate transactions.
