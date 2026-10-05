# SALESTORM Traceability Matrix

This matrix establishes rigorous, bidirectional traceability from high-level hackathon requirements to architectural components, low-level designs, database schemas, API contracts, reliability patterns, and validation proofs.

---

## Master Traceability Mapping

| Requirement ID | Summary | HLD Component (`02_HLD`) | LLD Component (`03_LLD`) | Database Artifact (`04_Database`) | API / Event (`05_API`) | Reliability Behavior (`08_Scalability_Reliability`) | Validation / Test (`11_AI_Assisted_Validation`) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **REQ-INV-001** | 10k Concurrent Requests | API Gateway & Ingress Virtual Queue | RateLimiter, TrafficThrottler | Session Cache Table / Redis Counter | `POST /api/v1/sale/queue/join` | Backpressure shed, Token Bucket rate limiting | Locust / k6 surge load test (10k VU) |
| **REQ-INV-002** | 100 Units, Zero Overselling | Inventory Service | InventoryManager, ReservationEngine | `inventory_items`, `stock_ledger` (ACID constraints) | `POST /api/v1/reservations` | Atomic decrement, Lua script isolation | Multi-threaded race condition stress test |
| **REQ-INV-003** | No Duplicate Reservations | Inventory Service / Ingress | ReservationRequestValidator | Unique Constraint (`sale_id`, `user_id`) | Idempotency Header `Idempotency-Key` | Idempotent rejection on duplicate attempt | Concurrent replay attack test |
| **REQ-INV-004** | Auto Reservation Expiry | Inventory Service / Reaper Worker | ReservationTTLManager, StockReleaser | `reservations` table (status: `PENDING`, `EXPIRED`, `CONFIRMED`) | Event: `InventoryReservationExpired` | Dead-letter / delay queue deterministic trigger | Simulated TTL clock-advance test |
| **REQ-PAY-001** | Zero Duplicate Payments | Payment Gateway Adapter | PaymentProcessor, IdempotencyGuard | `payment_transactions` (unique `idempotency_key`) | `POST /api/v1/payments` | Mutex lock on transaction execution | Double-click payment payload injection |
| **REQ-PAY-002** | Resilient Payment Timeouts | Payment Reconciler | GatewayHealthChecker, StatusPoller | `payment_transactions` (status: `IN_PROGRESS`, `SUCCESS`, `TIMEOUT`) | Webhook / `GET /payments/verify/{id}` | Exponential backoff retry & asynchronous webhook polling | Network partition & timeout injection |
| **REQ-ORD-001** | Zero Duplicate Orders | Order Service | OrderCreationHandler | `orders` table (unique `payment_id`) | Event: `PaymentSucceededEvent` | Deduplication filter at consumer level | Repeated event publication injection |
| **REQ-ORD-002** | Order Crash Recovery Post-Payment | Transactional Outbox & Order Worker | OutboxPublisher, OrderRecoverySaga | `outbox_events` table (relational commit with payment) | Event: `OrderCreatedEvent` | Guaranteed at-least-once replay from outbox | Process kill injection during order creation |
| **REQ-SYS-001** | Decoupled Async Communication | Message Broker (Kafka / RabbitMQ) | EventPublisher, EventSubscriber | Event store / offset checkpointing | Async topic contracts | Partitioning, buffer queues, consumer lag isolation | Broker latency & consumer outage simulation |
| **REQ-SYS-002** | Bot Mitigation & Fair Access | Edge CDN & Security WAF | DeviceFingerprinter, CaptchaVerifier | Token bucket / rate limit keys in Redis | WAF Challenge Token Header | IP reputation filtering, PoW / CAPTCHA on anomaly | Scalper bot script burst test |

---

*Note: Component details in this matrix will be progressively updated as formal decisions are made and approved in their respective phases.*
