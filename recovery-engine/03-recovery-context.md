# Chapter 3 — Recovery Context

## Purpose

The Recovery Context captures all information required to plan and execute a recovery operation.

It provides every Recovery Strategy with the same immutable view of the failed execution.

---

## Responsibilities

The Recovery Context stores:

- Recovery ID
- Execution ID
- League ID
- Failure ID
- Failed Event
- Recovery Strategy
- Candidate Snapshots
- Validation Results
- Execution History
- Timestamp

---

## Example

```python
RecoveryContext(
    recovery_id="recovery_001",
    execution_id="rollover_2026_2027",
    league_id="league_123",
    failure_id="failure_014",
    failed_event="season.contracts.expire",
)
```

---

## Lifecycle

```text
Failure
    │
    ▼
Build Recovery Context
    │
    ▼
Freeze Context
    │
    ▼
Recovery Planning
    │
    ▼
Execute Recovery
```

---

## Immutability

After planning begins:

- Recovery ID
- Execution ID
- Failure ID
- Snapshot references
- Execution history

must never change.

---

## Consumers

The Recovery Context is consumed by:

- Recovery Planner
- Snapshot Restoration
- Validation Framework
- Administrative Tools

Every consumer receives the same context.

---

## Design Principles

The Recovery Context shall:

- Be immutable
- Be deterministic
- Be serializable
- Be auditable
- Be shared

---

## Definition of Done

This chapter is complete when every recovery operation executes from one immutable Recovery Context.
