# Chapter 4 — Recovery Plan

## Purpose

A Recovery Plan is the deterministic execution plan produced after a failure has been analyzed.

It defines exactly how the Recovery Engine will restore league integrity.

Every recovery operation begins with one Recovery Plan.

---

## Responsibilities

A Recovery Plan specifies:

- Recovery Strategy
- Restore Snapshot
- Replay Events
- Validation Requirements
- Resume Point
- Completion Criteria

---

## Example

```python
RecoveryPlan(
    recovery_id="recovery_001",
    strategy="PARTIAL_ROLLBACK",
    restore_snapshot="snapshot_014",
    replay_events=[
        "season.free_agency.generate",
        "season.draft.advance",
    ],
)
```

---

## Planning Flow

```text
Failure
    │
    ▼
Analyze Execution
    │
    ▼
Locate Recovery Point
    │
    ▼
Choose Strategy
    │
    ▼
Generate Plan
```

---

## Plan Requirements

Every Recovery Plan must define:

- One recovery strategy
- One restore point
- Replay boundary
- Resume location
- Validation requirements

No ambiguity should remain.

---

## Plan Immutability

Once approved:

- Strategy
- Restore Snapshot
- Replay events
- Resume point

must remain immutable.

Any modification requires a new Recovery Plan.

---

## Plan Approval

Depending on policy:

- Automatic
- Administrative
- Hybrid

Recovery approval should always be recorded.

---

## Design Principles

Recovery Plans shall:

- Be deterministic
- Be explicit
- Be versioned
- Be auditable
- Be immutable

---

## Definition of Done

This chapter is complete when every failure produces one explicit, deterministic Recovery Plan describing exactly how execution should safely continue.
