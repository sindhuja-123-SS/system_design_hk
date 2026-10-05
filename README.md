# SALESTORM — SYSCRAFTERS 2026 Hackathon

## ⚡ High-Scale E-Commerce Flash-Sale Platform

This repository contains the architecture, low-level design, data modeling, reliability specifications, and jury defense for the **SALESTORM SYSCRAFTERS 2026** system design hackathon.

---

### 🎯 Primary Engineering Challenge
- **10,000 Concurrent Purchase Requests** hitting a flash-sale item at $T_0$.
- **100 Available Units** in inventory.
- **Strict Invariants**:
  - Zero overselling (Inventory $\ge 0$).
  - Zero duplicate reservations, payments, or orders.
  - Reliable end-to-end payment and order state progression under partial failures.
  - Provable resilience and deterministic recovery from downstream service outages.

---

### 🏛️ Engineering Governance & Operating Model
All engineering activities in this workspace strictly adhere to:
- [`AGENTS.md`](./AGENTS.md): Engineering governance, invariant definitions, and phase gating rules.
- [`PROJECT_PLAN.md`](./PROJECT_PLAN.md): Multi-phase hackathon execution roadmap and review gates.
- [`TRACEABILITY_MATRIX.md`](./TRACEABILITY_MATRIX.md): Traceability from requirements to architecture, data, and validation.


---

### 📁 Official Submission Structure
```text
SALESTORM_TEAM_NAME/
├── 01_Requirements/              # Requirements register, constraints, invariants
├── 02_HLD/                       # C4 architecture, end-to-end traffic topology
├── 03_LLD/                       # UML class diagrams, state machines, sequence flows
├── 04_Database/                  # Schemas, indexing, concurrency controls, ER models
├── 05_API/                       # REST/gRPC contracts, async event schemas, idempotency
├── 06_SOLID/                     # Detailed SOLID design principle mappings
├── 07_Design_Patterns/           # Distributed & OO design patterns (Saga, Outbox, etc.)
├── 08_Scalability_Reliability/   # Traffic surge handling, caching, backpressure, DR
├── 09_Security_Observability/    # Bot defense, auth, metrics, tracing, alerting
├── 10_ADR/                       # Architecture Decision Records (ADRs)
├── 11_AI_Assisted_Validation/    # Concurrency verification, failure injection proofs
├── 12_Presentation/              # Jury pitch deck, defense script, one-pager
├── AGENTS.md                     # Engineering governance
├── PROJECT_PLAN.md               # Hackathon engineering roadmap
├── TRACEABILITY_MATRIX.md        # End-to-end requirement traceability                  
└── README.md                     # Root project overview
```
