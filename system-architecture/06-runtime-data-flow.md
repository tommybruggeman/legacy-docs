# Chapter 6 — Runtime Data Flow

## Purpose

Runtime Data Flow defines how information moves through Legacy while users, services, events, evaluations, and AI workflows are executing.

It explains how raw requests become:

- Authorized operations
- Deterministic calculations
- Validated state changes
- Persisted records
- Auditable outcomes
- User-facing responses

The runtime flow should remain traceable from request to final result.

---

## Standard Request Flow

```text
User Request
      │
      ▼
Authentication
      │
      ▼
Authorization
      │
      ▼
Input Validation
      │
      ▼
Application Service
      │
      ▼
Domain Services
      │
      ▼
Business Validation
      │
      ▼
Persistence
      │
      ▼
Audit
      │
      ▼
Response
```

Every write operation should follow this general pattern.

---

## Read Request Flow

```text
User Request
      │
      ▼
Authentication
      │
      ▼
Authorization
      │
      ▼
Query Service
      │
      ▼
Repository
      │
      ▼
Canonical Records
      │
      ▼
Response Model
      │
      ▼
Presentation
```

Read models may aggregate data, but they should not become canonical sources of truth.

---

## Command and Query Separation

Legacy should distinguish between:

### Commands

Commands request a state change.

Examples:

- Sign player
- Release player
- Accept trade
- Start rollover
- Approve recovery

### Queries

Queries retrieve information.

Examples:

- Show team roster
- Calculate available cap
- List future picks
- Review recovery history

```text
Command

↓

Validation and Mutation

Query

↓

Retrieval and Projection
```

Separating these concerns reduces accidental writes and simplifies auditing.

---

## Transaction Flow

A transaction should follow a deterministic sequence.

```text
Transaction Request
        │
        ▼
Resolve Actor and League
        │
        ▼
Load Canonical State
        │
        ▼
Evaluate Rules
        │
        ▼
Build Transaction Plan
        │
        ▼
Validate Plan
        │
        ▼
Apply Atomically
        │
        ▼
Persist Audit Record
        │
        ▼
Publish Result
```

The transaction plan should make all intended mutations explicit before persistence.

---

## Trade Flow

```text
Trade Proposal
      │
      ▼
Resolve Teams and Assets
      │
      ▼
Verify Ownership
      │
      ▼
Evaluate Roster and Cap Effects
      │
      ▼
Validate League Rules
      │
      ▼
Create Transaction Plan
      │
      ▼
Apply Asset Transfers
      │
      ▼
Record Trade History
      │
      ▼
Publish Updated State
```

No asset should move until the complete trade is validated.

---

## Season Rollover Flow

```text
Rollover Request
      │
      ▼
Authorization
      │
      ▼
Pre-Rollover Validation
      │
      ▼
Initial Snapshot
      │
      ▼
Execution Context
      │
      ▼
Event Engine
      │
      ▼
Event Validation
      │
      ▼
Checkpoint Snapshot
      │
      ▼
Continue or Recover
      │
      ▼
Final Validation
      │
      ▼
Final Snapshot
      │
      ▼
Execution Complete
```

Every event should produce traceable inputs, outputs, and validation evidence.

---

## Failure Flow

```text
Failure Detected
      │
      ▼
Failure Classification
      │
      ▼
Freeze Execution
      │
      ▼
Build Recovery Context
      │
      ▼
Select Recovery Strategy
      │
      ▼
Restore Snapshot
      │
      ▼
Validate Restored State
      │
      ▼
Resume or Abort
```

Failure handling should never continue from uncertain state.

---

## AI Question Flow

```text
Owner Question
      │
      ▼
Identity and League Resolution
      │
      ▼
Query Understanding
      │
      ▼
Requirement Graph
      │
      ▼
Evidence Resolver
      │
      ▼
Deterministic Analysis
      │
      ▼
Answer Plan
      │
      ▼
OpenAI Explanation
      │
      ▼
Response Validation
      │
      ▼
Rendered Answer
```

The model should receive approved evidence and conclusions rather than unrestricted database access.

---

## Evidence Flow

An Evidence Packet should contain only the facts required for the current operation.

Example:

```python
EvidencePacket(
    league_id="league_123",
    team_id="team_007",
    question_type="TRADE_EVALUATION",
    facts=[
        "team_cap_space",
        "player_contracts",
        "draft_pick_ownership",
        "league_scoring_rules",
    ],
)
```

Evidence should be:

- Scoped
- Versioned
- Traceable
- Sufficient
- Free from unrelated sensitive data

---

## Evaluation Flow

```text
Canonical Facts
      │
      ▼
Player Evaluation
      │
      ▼
Team Evaluation
      │
      ▼
League Evaluation
      │
      ▼
Transaction Evaluation
      │
      ▼
Recommendation
```

Higher-level evaluations should reference the lower-level evidence used to construct them.

---

## External Data Flow

```text
External Provider
      │
      ▼
Provider Adapter
      │
      ▼
Schema Validation
      │
      ▼
Normalization
      │
      ▼
Canonical Mapping
      │
      ▼
Persistence or Evidence Store
```

External data should never bypass validation and normalization.

---

## Notification Flow

```text
Completed Business Operation
      │
      ▼
Notification Request
      │
      ▼
Notification Adapter
      │
      ▼
Provider Delivery
      │
      ▼
Delivery Status
```

Notification delivery should occur after canonical state changes unless delivery is an explicit precondition.

---

## Correlation and Traceability

Every runtime operation should carry identifiers such as:

- Request ID
- Execution ID
- Transaction ID
- Validation ID
- Recovery ID
- User ID
- League ID
- Team ID

These identifiers connect logs, audits, metrics, and user-visible results.

---

## Data Transformation Rules

Every transformation should identify:

- Input contract
- Output contract
- Owning subsystem
- Version
- Validation requirements
- Error behavior

Undocumented transformation chains should be avoided.

---

## Runtime Guarantees

Runtime Data Flow guarantees:

1. Every request is authenticated and authorized before protected access.
2. Every mutation is validated before persistence.
3. Complex operations are planned before being applied.
4. State changes are atomic where required.
5. Every important operation creates an audit record.
6. External data is normalized before use.
7. AI receives scoped evidence and approved conclusions.
8. Failures route into deterministic recovery.
9. Correlation identifiers preserve end-to-end traceability.

---

## Definition of Done

This chapter is complete when every major Legacy workflow has a documented path from request through authorization, domain logic, validation, persistence, auditing, and response rendering.
