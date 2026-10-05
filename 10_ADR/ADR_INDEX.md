# Architecture Decision Records (ADR) Index

This index tracks all major architectural decisions across the lifecycle of the **SALESTORM** platform.
Decisions are maintained as formal records under `10_ADR/` and adhere to the **MADR (Markdown Any Decision Record)** format.

> ⚠️ **Discipline Rule**: Decisions listed below as `⏳ PROPOSED / NOT YET DECIDED` are deliberately left open until their corresponding design phase and requirement gate are initiated and reviewed.

---

## Master Decision Registry

| ADR ID | Decision Topic | Target Phase | Status | Summary of Options Under Evaluation | Impacted Area |
| :--- | :--- | :---: | :---: | :--- | :--- |
| **ADR-001** | Service Boundaries & Architecture Topology | Phase I | ⏳ Not Yet Decided | Modular Monolith vs. Microservices vs. Event-Driven Services | Macro Architecture / 02_HLD |
| **ADR-002** | Flash-Sale Ingress & Traffic Smoothing Strategy | Phase I | ⏳ Not Yet Decided | Direct API Gateway vs. Virtual Waiting Room vs. Token Leaky Bucket | Scalability / Ingress / 08_Scalability |
| **ADR-003** | Inventory Concurrency & Overselling Prevention | Phase II | ⏳ Not Yet Decided | RDBMS Pessimistic Locking (`FOR UPDATE`) vs. Optimistic Locking (versioning) vs. Redis In-Memory Atomic Lua (`DECRBY`) | Inventory Service / 04_Database |
| **ADR-004** | Reservation TTL & Inventory Expiry Strategy | Phase II | ⏳ Not Yet Decided | Redis Keyspace Notifications vs. Delayed Message Queue (RabbitMQ/Kafka) vs. Scheduled DB Reaper Worker | Concurrency / 03_LLD |
| **ADR-005** | Primary Data Persistence Technology (SQL vs. NoSQL) | Phase II / III | ⏳ Not Yet Decided | PostgreSQL (ACID relational) vs. Distributed NoSQL (Cassandra/DynamoDB) vs. Polyglot Persistence | Database / 04_Database |
| **ADR-006** | Service-to-Service Communication (Sync vs. Async) | Phase I / III | ⏳ Not Yet Decided | Synchronous REST/gRPC vs. Asynchronous Messaging (Kafka/RabbitMQ) vs. Hybrid | Communication / 05_API |
| **ADR-007** | Message Broker & Distributed Log Selection | Phase III | ⏳ Not Yet Decided | Apache Kafka (Partitioned log) vs. RabbitMQ (AMQP message broker) vs. AWS SQS/SNS | Messaging / 02_HLD |
| **ADR-008** | Multi-Tier Caching & Invalidation Architecture | Phase I / II | ⏳ Not Yet Decided | Read-Through Redis vs. Cache-Aside with Write-Invalidate vs. Edge CDN Caching | Caching / 08_Scalability |
| **ADR-009** | Distributed Idempotency Strategy | Phase III | ⏳ Not Yet Decided | Client-Generated Idempotency Keys + Redis Lock vs. DB Unique Constraints + State Check | Payment & Order / 05_API |
| **ADR-010** | Payment Failure & Order State Recovery Strategy | Phase III | ⏳ Not Yet Decided | Transactional Outbox Pattern + Poller vs. Saga Orchestrator (Temporal/Step Functions) vs. Event Choreography | Reliability / 07_Design_Patterns |

---

## Decision Status Definitions
- ⏳ **Not Yet Decided**: Under research/analysis; options defined, waiting for phase requirement gate.
- 🟡 **Under Review**: Documented with alternatives and trade-offs; awaiting user/architect review.
- ✅ **Accepted**: Formally approved and incorporated into baseline design.
- 🚫 **Superseded**: Replaced by a subsequent ADR.
