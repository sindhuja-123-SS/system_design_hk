# SALESTORM Project Engineering Plan & Execution Roadmap

This document serves as the master execution roadmap for the **SALESTORM SYSCRAFTERS 2026** Hackathon.
Every phase is bound by a strict **Requirement Gate** and explicit **Acceptance Criteria**. Moving between phases requires explicit user review and sign-off.

---

## 🚦 Project Phase Overview

```text
  Phase I: Requirement Analysis & HLD
     │
     ▼
  Phase II: High Concurrency & Inventory Reservation
     │
     ▼
  Phase III: Payment Reliability & Order Management
     │
     ▼
  Phase IV: LLD, SOLID & Design Patterns
     │
     ▼
  Phase V: Stress Scenarios & Jury Defense
```

---

## Phase I — Requirement Analysis & High-Level Design (HLD)

### 1. What the Hackathon Asks For
- Comprehensive decomposition of the flash-sale problem statement.
- System boundary identification, high-level architecture, external integrations, and component interactions.
- Structurizr / C4 Model diagrams (System Context and Container level).

### 2. Why It Exists
Establishes the macro-architecture, defines service boundaries, and determines synchronous vs. asynchronous data pathways before designing granular components.

### 3. What Engineering Concepts We Learn
- C4 architectural modeling principles.
- Macro service boundary sizing (Domain-Driven Design bounded contexts).
- Traffic decoupling and backpressure isolation at system ingress.

### 4. Inputs Required
- Official SALESTORM hackathon brief and constraints (10k req/s, 100 units).
- Identified external actor personas (buyers, payment gateways, warehouse/fulfillment).

### 5. Artifacts to Produce
- `01_Requirements/Requirements_Register.md` (initial baseline)
- `01_Requirements/Scope_and_Assumptions.md`
- `02_HLD/C4_System_Context.dsl` & visual representation
- `02_HLD/C4_Container_Diagram.dsl` & visual representation
- `02_HLD/High_Level_Architecture_Specification.md`

### 6. Decisions to Make
- Microservices vs. Modular Monolith vs. Event-Driven Core.
- Ingress strategy (API Gateway, Rate Limiting, Waiting Room / Virtual Queue).
- Synchronous vs. Asynchronous boundary separation for order processing.

### 7. Risks
- Over-engineering into dozens of microservices prematurely.
- Vague service ownership leading to split-brain inventory data.

### 8. Acceptance Criteria
- Complete C4 System Context and Container architecture documented.
- All 10k concurrent request ingress paths mapped without unbounded synchronous fanout.
- No ambiguities regarding which service owns the inventory truth.

### 9. What Must Be Reviewed Before Proceeding
- Architecture topology, service boundaries, and ingress rate-limiting strategy.

---

## Phase II — High Concurrency & Inventory Reservation

### 1. What the Hackathon Asks For
- Architectural and algorithmic guarantee preventing overselling (10,000 requests competing for 100 units).
- Reservation lifecycle management (reservation creation, expiration/TTL, release).
- Zero negative inventory, zero lost updates under extreme contention.

### 2. Why It Exists
Flash sales fail primarily at the inventory lock point. Hot-spot row locking on traditional relational databases leads to connection pool exhaustion, deadlocks, and latency spikes.

### 3. What Engineering Concepts We Learn
- Highly contended resource concurrency models (Optimistic vs. Pessimistic locking vs. In-Memory Atomic Decoupling / Redis Lua).
- Two-phase reservation patterns (Soft Reserve with TTL vs. Hard Commit).
- Safe cache eviction and cache stampede protection.

### 4. Inputs Required
- Approved Phase I Container Architecture.
- Invariant definitions from `AGENTS.md`.

### 5. Artifacts to Produce
- `10_ADR/ADR-002-Inventory-Concurrency-Strategy.md`
- `03_LLD/Reservation_State_Machine.puml`
- `04_Database/Inventory_Data_Model_and_Isolation.md`
- `08_Scalability_Reliability/Flash_Sale_Traffic_Absorption.md`

### 6. Decisions to Make
- Concurrency control: Database-level row lock (`SELECT FOR UPDATE`), Redis atomic Lua script (`DECRBY`), or distributed memory queue.
- Reservation TTL duration and release worker architecture (Delayed message queue vs. Redis keyspace notifications vs. periodic scanner).

### 7. Risks
- Race conditions causing 101 units sold (instant disqualification).
- Reservation abandonment deadlocking stock permanently.

### 8. Acceptance Criteria
- Mathematical proof / concurrency trace demonstrating zero overselling under 10k concurrent requests.
- Deterministic expiration mechanism releasing abandoned stock back to the pool.

### 9. What Must Be Reviewed Before Proceeding
- Selected concurrency strategy, race condition proof, and reservation TTL state machine.

---

## Phase III — Payment Reliability & Order Management

### 1. What the Hackathon Asks For
- Guaranteed idempotent payment handling.
- Graceful handling of payment gateway timeouts and webhook out-of-order delivery.
- Reliable order fulfillment state progression even when Order Service crashes post-payment.

### 2. Why It Exists
Real-world payment gateways are distributed third parties subject to network partitions, retries, and variable latencies. Money must never be deducted without either confirmed order placement or guaranteed automated reversal.

### 3. What Engineering Concepts We Learn
- Distributed idempotency keys with distributed lock acquisition.
- Transactional Outbox Pattern and at-least-once messaging semantics.
- Distributed Saga / Orchestration vs. Choreography for multi-service consistency.

### 4. Inputs Required
- Phase II Reservation State Machine and inventory release triggers.
- Payment gateway interface assumptions and retry models.

### 5. Artifacts to Produce
- `10_ADR/ADR-003-Payment-Idempotency-and-Reconciliation.md`
- `03_LLD/Payment_and_Order_Sequence.puml`
- `04_Database/Payment_Order_Schema_and_Idempotency.md`
- `05_API/Payment_and_Order_Contracts.md`

### 6. Decisions to Make
- Outbox pattern vs. two-phase commit vs. event choreographing.
- Handling payment timeouts: Synchronous status query vs. webhook reconciliation vs. reversal saga.

### 7. Risks
- Double charges due to user retry clicks.
- "Paid but order not created" ghost state if order service fails.

### 8. Acceptance Criteria
- Provable idempotency protocol preventing duplicate deductions.
- State-recovery sequence showing deterministic order creation or automated refund upon downstream failure.

### 9. What Must Be Reviewed Before Proceeding
- Idempotency key lifecycle, payment webhook handler design, and failure recovery sequence.

---

## Phase IV — Low-Level Design (LLD), SOLID & Design Patterns

### 1. What the Hackathon Asks For
- Rigorous object-oriented and component architecture.
- Explicit demonstrations of SOLID principles in system design.
- Concrete design patterns solving explicit flash-sale bottlenecks.

### 2. Why It Exists
Translates macro architectures into clean, extensible, maintainable code-level structures that jury members can evaluate for software craftsmanship.

### 3. What Engineering Concepts We Learn
- Domain-Driven Design (Aggregates, Value Objects, Domain Services).
- Design patterns in action: Strategy (gateways), State (order lifecycle), Observer/Outbox (events), Circuit Breaker (fault tolerance).
- Strict interface boundaries adhering to SOLID.

### 4. Inputs Required
- Approved Phase I-III HLD, Concurrency, and Payment/Order designs.

### 5. Artifacts to Produce
- `03_LLD/Core_Domain_Class_Diagrams.puml`
- `06_SOLID/SOLID_Analysis_and_Mappings.md`
- `07_Design_Patterns/Design_Patterns_Catalog.md`
- `05_API/OpenAPI_and_Event_Schemas.md`

### 6. Decisions to Make
- Class structural boundaries and domain aggregate roots.
- Exception hierarchy and standardized error handling contracts.

### 7. Risks
- Superficial design pattern inclusion without practical engineering rationale.
- Violations of Dependency Inversion Principle coupling core domain to infrastructure.

### 8. Acceptance Criteria
- PlantUML class diagrams exhibiting low coupling and high cohesion.
- Concrete code/class examples directly mapping to each of the 5 SOLID principles and chosen design patterns.

### 9. What Must Be Reviewed Before Proceeding
- Class diagrams, domain aggregate boundaries, and pattern justifications.

---

## Phase V — Stress Scenarios, AI Validation & Jury Defense

### 1. What the Hackathon Asks For
- Validation of the system under catastrophic failure scenarios.
- AI-assisted verification of concurrency, consistency, and invariant preservation.
- Comprehensive presentation deck, defense scripts, and architecture trade-off justifications.

### 2. Why It Exists
Hackathon juries evaluate solutions based on how well they handle edge cases, recover from failure, explain trade-offs, and defend architectural choices under questioning.

### 3. What Engineering Concepts We Learn
- Chaos engineering principles & failure-mode analysis (FMEA).
- Formal invariant verification.
- Executive and technical architectural storytelling.

### 4. Inputs Required
- All artifacts from Phases I through IV.

### 5. Artifacts to Produce
- `11_AI_Assisted_Validation/Concurrency_and_Failure_Injections.md`
- `11_AI_Assisted_Validation/Invariant_Verification_Report.md`
- `09_Security_Observability/Bot_Mitigation_and_Observability.md`
- `12_Presentation/Jury_Defense_Deck.md`
- `12_Presentation/Technical_Defense_QandA.md`
- Final updated `TRACEABILITY_MATRIX.md`

### 6. Decisions to Make
- Key narrative anchors for jury presentation (e.g., zero overselling mathematical proof, resilient payment saga).

### 7. Risks
- Unconvincing answers to edge cases during jury cross-examination.
- Inconsistencies between HLD, LLD, and Database schemas.

### 8. Acceptance Criteria
- 100% traceability across all requirements in `TRACEABILITY_MATRIX.md`.
- End-to-end defense deck ready with clear trade-off defensibility.

### 9. What Must Be Reviewed Before Proceeding
- Final jury presentation and complete repository consistency audit.
