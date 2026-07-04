# Architecture Checklist

This document provides a quality gate checklist to validate software architecture designs, ensuring system modularity, C4 container isolation, scalability mapping, and protocol alignment comply with the [Architecture Standards](file:///d:/projects/Nexulyt-AI-OS/standards/architecture-standards.md).

---

## 1. Overview

* **Objective**: Validate the system topology, component boundaries, database strategies, and performance scaling vectors before implementation.
* **When to Use**: During system discovery, feature architectural design sessions, or when reviewing system integrations maps.
* **Prerequisites**: Functional and non-functional requirements specifications, technology stack selections, and domain boundary mappings.

---

## 2. Checklist Items

### System Design & Topology
* [ ] **C4 Container Blueprint**: Draw and approve C4 Model Container level topologies mapping dependencies.
* [ ] **Service Decoupling**: Validate that services communicate asynchronously using brokers (e.g. RabbitMQ, Kafka) where operations can be deferred.
* [ ] **Monorepo / Monolith Boundary**: Ensure Monorepo layouts isolate domain logic boundaries securely, preventing cross-module imports.
* [ ] **High Availability Routing**: Map load balancing, multi-AZ deployment zones, and DNS failover pathways.
* [ ] **Single Point of Failure (SPOF)**: Identify all infrastructure components representing SPOFs, creating failover backups.

### Data & State Strategy
* [ ] **Database Partitioning Model**: Define database scale boundaries (logical multi-tenancy, physical database separation, or sharding partitions).
* [ ] **Transactional vs Analytical Split**: Decouple transactional databases (PostgreSQL) from analytical click streams (ClickHouse, Elasticsearch).
* [ ] **Caching Architecture**: Map cache layers (Redis, CDN Edge caches) and specify eviction strategies.
* [ ] **State Preservation**: Enforce stateless API services, storing session state in distributed caches.

### API & Protocols
* [ ] **API Contract Definition**: Publish OpenAPI, tRPC, or GraphQL specs mapping path resources, verbs, and HTTP responses.
* [ ] **Error Formatting Standards**: Configure RFC 7807 Problem Details error schemas for all endpoints.
* [ ] **API Versioning Strategy**: Establish clear version parameters in URL paths (e.g. `/api/v1/`) or request headers.
* [ ] **Transport Protocols Selection**: Ensure protocol choices align with latency budgets (e.g., SSE for one-way streams, WebSockets for two-way interactions, REST for CRUD).

### Folder Structure Compliance
* [ ] **Standard Folder Layout**: Validate directory hierarchies match [Folder Structure Standards](file:///d:/projects/Nexulyt-AI-OS/standards/folder-structure.md).
* [ ] **Separation of Concerns**: Confirm business logic layers (services, entities) remain isolated from HTTP frame frameworks.
* [ ] **Circular Dependency Auditing**: Run static imports checks, verifying no circular references exist between folders.

---

## 3. Validation & Exit Criteria

### Validation Criteria
* Verify that the proposed topology has been dry-run evaluated for major traffic flows.
* Confirm that database write operations use explicit pessimistic or optimistic locking paradigms.
* Verify that all service endpoints define strict request validation schemas (e.g. Zod).

### Exit Criteria
* **System Design Spec (SDS)**: Approved architecture design document published.
* **Architecture Decision Records (ADRs)**: Significant technology decisions recorded.
* **OpenAPI / tRPC Schemas**: API specifications committed.

---

## 4. Risks, Best Practices, and Recommendations

### Common Mistakes
* Over-engineering microservice boundaries before domain relationships are validated.
* Allowing direct cross-database queries between decoupled services, breaking logical isolation layers.
* Storing state inside server nodes, preventing horizontal scaling.

### Professional Recommendations
1. **Model Topologies Using C4**: Using C4 structures (System, Container, Component, Code) makes designs readable for developers and AI agents.
2. **Prioritize Stateless Routing**: Ensure backend services remain completely stateless. Store all transaction state in database layers or Redis clusters.
3. **Run Circular Import Scans**: Set up automated import check scripts to reject commits containing circular file references.
