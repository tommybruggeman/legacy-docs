# Chapter 4 — Context Propagation

## Purpose

Context Propagation ensures that every subsystem participating in a Season Rollover execution operates from the same shared understanding of execution state.

Rather than each subsystem independently reconstructing information, a common Execution Context is propagated throughout the architecture.

This guarantees consistency, determinism, and traceability.

---

## Responsibilities

Context Propagation is responsible for distributing:

- Execution ID
- League ID
- Source Season
- Target Season
- Current Event
- Current Checkpoint
- Validation State
- Recovery State
- Correlation IDs

Every participating subsystem should receive the same immutable context.

---

## Context Flow

```text
Execution Context

       │

 ┌─────┼───────────────┐
 ▼     ▼               ▼
Event Validation Snapshot
Engine Framework System
              │
              ▼
       Recovery Engine
```

Context flows outward from the orchestration layer.

---

## Immutability

Once execution begins:

The following should never change:

- Execution ID
- League ID
- Source Season
- Target Season

Mutable execution progress should be represented by new context snapshots rather than modifying existing ones.

---

## Context Example

```python
ExecutionContext(
    execution_id="rollover_2026_2027",
    league_id="league_123",
    source_season=2026,
    target_season=2027,
    checkpoint="contracts_complete",
)
```

Every subsystem receives this identical structure.

---

## Context Extensions

Subsystems may derive local state.

Example:

```text
Execution Context

↓

Validation Context

↓

Rule Context
```

Derived context should never mutate the original Execution Context.

---

## Failure Context

When failures occur:

```text
Execution Context

↓

Recovery Context

↓

Recovery Plan
```

Recovery extends execution context rather than replacing it.

---

## Design Principles

Context Propagation shall:

- Be immutable
- Be deterministic
- Avoid duplication
- Preserve traceability
- Support future subsystem expansion

---

## Definition of Done

This chapter is complete when every subsystem operates from a consistent, immutable execution context that remains synchronized throughout the rollover lifecycle.
