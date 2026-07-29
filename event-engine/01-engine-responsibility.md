# Chapter 1 — Engine Responsibility

## Purpose

The Event Engine is the orchestration layer of the Season Rollover system.

Its responsibility is to coordinate the execution of rollover events in a deterministic, auditable, and recoverable manner.

The engine never performs business logic itself. Instead, it executes approved event handlers according to the Event Catalog and Execution Plan.

---

## Responsibilities

The Event Engine is responsible for:

- Creating the execution context
- Compiling the execution plan
- Resolving event dependencies
- Determining execution order
- Dispatching event handlers
- Tracking execution progress
- Recording structured event results
- Enforcing execution policies
- Coordinating validation
- Coordinating snapshots
- Producing execution summaries

---

## Non-Responsibilities

The Event Engine is **not** responsible for:

- Player business logic
- Contract calculations
- Salary cap calculations
- Draft logic
- Free agency rules
- League configuration
- Database migrations
- Player valuation
- Trade recommendations
- UI rendering

Those responsibilities belong to dedicated systems.

---

## Engine Philosophy

The Event Engine coordinates work.

Handlers perform work.

Validators verify work.

Snapshots preserve work.

Recovery restores work.

Audit records work.

Every responsibility belongs to exactly one system.

---

## Engine Lifecycle

```text
Rollover Request
        │
        ▼
Execution Context
        │
        ▼
Execution Plan
        │
        ▼
Dependency Resolution
        │
        ▼
Validation
        │
        ▼
Event Dispatch
        │
        ▼
Result Collection
        │
        ▼
Final Validation
        │
        ▼
Execution Complete
```

---

## Engine Invariants

The following statements must always remain true:

- Every executed event exists in the Event Catalog.
- Every event has one registered handler.
- Events execute only after their dependencies succeed.
- Execution order is deterministic.
- Every event produces a structured result.
- Every execution has one immutable execution context.
- Every execution produces an audit trail.
- Blocking failures stop execution.
- Validation cannot be skipped during commit mode.

---

## System Boundaries

```text
                 Season Rollover Engine
                          │
      ┌───────────────────┼───────────────────┐
      ▼                   ▼                   ▼
 Event Catalog      Event Handlers      Validation
      │                   │                   │
      └───────────────┬───┘                   │
                      ▼                       ▼
                 Snapshot System       Recovery Engine
                      │
                      ▼
                 Audit System
```

---

## Design Principles

The Event Engine shall:

- Be deterministic
- Be repeatable
- Be auditable
- Be recoverable
- Be idempotent
- Be observable
- Fail safely
- Never bypass validation
- Never invent business logic

---

## Definition of Done

This chapter is complete when the engine has a clearly defined responsibility and every future component can determine whether a responsibility belongs to the engine or another subsystem.
