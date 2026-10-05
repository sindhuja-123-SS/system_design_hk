# SALESTORM Engineering Workflow & Collaboration Protocol

This protocol defines the standard engineering operating procedure for the team during the **SALESTORM SYSCRAFTERS 2026** Hackathon.

---

## 🔁 The 8-Step Engineering Cycle

All architectural milestones, design additions, and technical decisions must strictly progress through the following sequential workflow:

```text
 1. Understand ──► 2. Discuss ──► 3. Decide ──► 4. Document
                                                    │
 8. Record Evidence ◄── 7. Validate ◄── 6. Implement ◄── 5. Review
```

### 1. Understand
- Extract core requirements directly from the hackathon brief.
- Unpack constraints, identify domain boundaries, and surface all unstated assumptions.
- Formulate mathematical and domain invariants (e.g., Inventory $\ge 0$).

### 2. Discuss
- Identify architectural bottlenecks (e.g., 10k concurrent lock contentions).
- Propose at least two viable technical alternatives (e.g., Optimistic vs. Pessimistic vs. In-Memory Atomic Lua).
- Analyze trade-offs (latency vs. consistency vs. complexity).

### 3. Decide
- Select the optimal approach based on engineering rationale, not popularity.
- Document the decision using an Architecture Decision Record (ADR).

### 4. Document
- Create or update the relevant artifacts in the official submission structure (`01_Requirements` through `10_ADR`).
- Maintain one primary source of truth (C4 in Structurizr, UML in PlantUML, supporting flows in Mermaid).
- Synchronize `TRACEABILITY_MATRIX.md`.

### 5. Review
- Conduct a formal Requirement Gate check against acceptance criteria.
- Present the design to the lead architect / user for sign-off.
- **GATE RULE**: Do not move to implementation or next-stage design without review and approval.

### 6. Implement (Scaffolding / Proofs / Specs)
- Once approved, generate the formal specifications, schemas, interfaces, or verification models.
- Maintain clean code craftsmanship adhering strictly to SOLID principles and selected design patterns.

### 7. Validate
- Subject the design to chaos / stress scenarios (e.g., simulated 10k concurrent requests, payment timeout, Order Service crash).
- Verify invariant preservation with mathematical or programmatic proofs.

### 8. Record Evidence
- Log simulation outputs, stress test results, and audit traces in `11_AI_Assisted_Validation/`.
- Prepare talking points and defensive proofs for the jury deck (`12_Presentation/`).

---

## 🛑 The Non-Negotiable Rule

> **"No team member should implement a major architectural change that is not documented and reviewed."**

Any change altering service boundaries, data ownership, concurrency semantics, or failure recovery must originate as an updated ADR and pass review before artifact modification.
