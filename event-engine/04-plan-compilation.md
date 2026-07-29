# Chapter 4 — Plan Compilation

## Purpose

The Execution Plan is the ordered collection of events that will be executed during a season rollover.

The Plan Compiler transforms a validated Execution Context into a deterministic execution plan by selecting the required events from the Event Catalog.

The compiler determines **what** will execute.

It does not determine **how** those events execute.

---

## Responsibilities

The Plan Compiler is responsible for:

- Reading the Execution Context
- Selecting eligible events
- Excluding inapplicable events
- Assigning execution order
- Building the execution plan
- Validating plan completeness
- Producing an immutable plan

---

## Inputs

The Plan Compiler receives:

- Execution Context
- Event Catalog
- League Configuration
- Engine Version

---

## Outputs

The Plan Compiler produces a single immutable Execution Plan.

Example:

```python
ExecutionPlan(
    execution_id="rollover_2026_2027",
    events=[
        PlannedEvent(...),
        PlannedEvent(...),
        PlannedEvent(...),
    ]
)
```

---

## Compilation Flow

```text
Execution Context
        │
        ▼
Read Event Catalog
        │
        ▼
Evaluate Eligibility
        │
        ▼
Include Required Events
        │
        ▼
Exclude Invalid Events
        │
        ▼
Build Execution Plan
        │
        ▼
Freeze Plan
```

---

## Plan Requirements

Every Execution Plan must:

- Contain only registered events
- Contain no duplicate events
- Be deterministic
- Be immutable
- Reference valid handlers
- Reference declared dependencies
- Contain execution metadata

---

## Plan Immutability

After compilation completes:

- Events cannot be added
- Events cannot be removed
- Event order cannot change
- Dependencies cannot change

If the plan must change, a new plan must be compiled.

---

## Validation

The Plan Compiler validates:

- Duplicate events
- Unknown events
- Missing required events
- Missing handlers
- Invalid event metadata
- Invalid execution options

Compilation fails if any validation error exists.

---

## Design Principles

The Plan Compiler shall:

- Produce deterministic plans
- Produce immutable plans
- Produce complete plans
- Never invent events
- Never bypass catalog definitions

---

## Definition of Done

This chapter is complete when every rollover request can be transformed into one immutable Execution Plan containing every event required for execution.
