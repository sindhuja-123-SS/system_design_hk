# SALESTORM Requirements Register

This register is the formal catalog of all functional, non-functional, and invariant requirements derived directly from the SALESTORM Hackathon Brief and core problem specifications.

---

## Requirements Catalog

| Requirement ID | Requirement Description | Type | Priority | Source in Brief | Strict Guarantee or Target | Related Architecture Area | Validation Method | Status |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **REQ-INV-001** | Support 10,000 concurrent purchase requests during flash sale surge | NFR | P0 | Primary Challenge | Target: 10,000 req/s with p99 latency < 500ms | Ingress / API Gateway / Virtual Queue / Cache | Concurrency load testing & bottleneck analysis | 📋 Identified |
| **REQ-INV-002** | Exactly 100 available units for sale; strict zero overselling | Invariant | P0 | Primary Challenge | Strict Guarantee: Total confirmed reservations $\le 100$; Inventory $\ge 0$ | Inventory Service / Atomic Lock / Data Store | Concurrency race condition simulation / Lua proof | 📋 Identified |
| **REQ-INV-003** | No duplicate reservations per user/request | Invariant | P0 | Primary Challenge | Strict Guarantee: Single active reservation per customer per flash-sale event | Ingress / Inventory Service / Idempotency Key | Automated replay request simulation | 📋 Identified |
| **REQ-INV-004** | Automatic reservation expiration and inventory release | Functional | P1 | Primary Challenge | Strict Guarantee: Expired reservations release stock back within deterministic TTL | Inventory Service / Delay Queue / Reaper Worker | Time-travel TTL expiration test | 📋 Identified |
| **REQ-PAY-001** | Zero duplicate payments on repeated user clicks or network retries | Invariant | P0 | Primary Challenge | Strict Guarantee: Exactly-once payment execution per order attempt | Payment Service / Idempotency Registry | Idempotency token collision test | 📋 Identified |
| **REQ-PAY-002** | Resilient payment gateway timeout handling | Reliability | P0 | Primary Challenge | Strict Guarantee: Timeout $\ne$ Failure; asynchronous reconciliation or query before cancel | Payment Service / Gateway Adapter / Polling Worker | Fault injection: simulated gateway timeout | 📋 Identified |
| **REQ-ORD-001** | Zero duplicate orders generated from successful payment | Invariant | P0 | Primary Challenge | Strict Guarantee: Exactly one order per successful payment transaction | Order Service / Transactional Outbox / Message Consumer | Message duplicate delivery injection | 📋 Identified |
| **REQ-ORD-002** | Reliable order state recovery when Order Service crashes post-payment | Reliability | P0 | Primary Challenge | Strict Guarantee: Recoverable state via durable events / outbox replay; no lost paid orders | Message Broker / Outbox Pattern / Order Service | Fault injection: Order Service pod crash post-payment | 📋 Identified |
| **REQ-SYS-001** | Clean service boundary separation with minimal synchronous chaining | Architecture | P1 | Core Governance | Target: Asynchronous event-driven decoupling for order fulfillment | System Architecture / Broker / Event Bus | Architecture review & container verification | 📋 Identified |
| **REQ-SYS-002** | Bot mitigation and fair-access protection | Security | P2 | Core Governance | Target: Rate-limit bot farms, ensure equitable user participation | Edge CDN / WAF / Virtual Waiting Room | Token validation and abuse pattern testing | 📋 Identified |

---

## Status Legend
- 📋 **Identified**: Requirement extracted and verified against brief.
- 📐 **Designed**: Architecture / Component design produced and mapped.
- 🔍 **Reviewed**: Reviewed and approved by lead architect / user.
- 🧪 **Validated**: Verified via simulation, proof, or test scenario.
