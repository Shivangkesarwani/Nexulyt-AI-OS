# Repository Glossary

This glossary defines technical terms, structural systems, and design concepts used throughout **Nexulyt-AI-OS**.

---

## Technical Terms

### Active-Active Multi-Region
A deployment architecture where multiple data centers in different geographical regions actively serve traffic simultaneously, with data synchronized dynamically between them.

### ADR (Architecture Decision Record)
A document that captures a key architectural decision made for the project, including context, options considered, and the chosen solution with trade-offs.

### Blue-Green Deployment
A release strategy that runs two identical production environments (Blue and Green) concurrently. Only one environment serves user traffic. Deployments are updated on the standby environment, tested, and cut over atomically.

### Canary Deployment
A rollout strategy where a new software version is released to a small, controlled subset of users (e.g. 5%) before propagating to 100% of traffic.

### C4 Model
A hierarchical visualization framework for software architecture, modeling systems at four levels: Context, Containers, Components, and Code.

### CRDT (Conflict-free Replicated Data Type)
A data structure that can be replicated across multiple computers in a network, where updates can be made independently and concurrently without coordination, guaranteed to converge mathematically.

### DREAD
A risk evaluation model used to grade security vulnerabilities across five categories: Damage, Reproducibility, Exploitability, Affected Users, and Discoverability.

### HPA (Horizontal Pod Autoscaler)
A Kubernetes controller that automatically scales the number of pods in a deployment based on CPU utilization or custom metrics.

### ISR (Incremental Static Regeneration)
A rendering pattern in Next.js that enables static page generation to be updated dynamically in the background after deployment.

### OIDC (OpenID Connect)
An authentication layer built on OAuth 2.0 that allows applications to verify client identities using temporary, role-based assertion tokens instead of static credentials.

### RAG (Retrieval-Augmented Generation)
An architecture that optimizes LLM outputs by querying external vector databases for context before generating responses.

### RLS (Row-Level Security)
A database security control (native to PostgreSQL) that restricts which data rows are returned or mutated based on the query user's context.

### RPO (Recovery Point Objective)
The maximum acceptable age of data that can be lost from a data store during a disaster event.

### RTO (Recovery Time Objective)
The maximum target duration of downtime acceptable before a service must be fully restored after a disaster.

### STRIDE
A security threat modeling framework classifying threats across six categories: Spoofing, Tampering, Repudiation, Information Disclosure, Denial of Service, and Elevation of Privilege.
