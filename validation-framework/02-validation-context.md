# Chapter 2 — Validation Context

## Purpose

Every validation run executes within a single immutable Validation Context.

The Validation Context provides every validation rule with a consistent view of the execution being evaluated.

---

## Responsibilities

The Validation Context stores:

- Validation ID
- Execution ID
- League ID
- Source Season
- Target Season
- Validation Stage
- Validation Set
- Event ID (when applicable)
- Validation Version
- Timestamp

---

## Example

```python
ValidationContext(
    validation_id="validation_001",
    execution_id="rollover_2026_2027",
    league_id="league_123",
    source_season=2026,
    target_season=2027,
    stage="post_event",
    validation_set="post_contract_expiration",
    event_id="season.contracts.expire",
)
```

---

## Lifecycle

```text
Create Context
       │
       ▼
Validate Context
       │
       ▼
Freeze Context
       │
       ▼
Execute Rules
       │
       ▼
Archive Context
```

The Validation Context exists only for the lifetime of one validation run.

---

## Immutability

After validation begins, the following values must never change:

- Validation ID
- Execution ID
- League ID
- Seasons
- Validation Stage
- Validation Set
- Event ID
- Validation Version

Any change requires a new Validation Context.

---

## Context Scope

Every rule receives the same Validation Context.

Rules may read from the context.

Rules may never modify the context.

---

## Context Relationships

```text
Execution Context
        │
        ▼
Validation Context
        │
        ▼
Validation Rules
```

The Validation Context supplements the Execution Context but never replaces it.

---

## Design Principles

The Validation Context shall:

- Be immutable
- Be deterministic
- Be auditable
- Be serializable
- Be shared across every rule in the validation run

---

## Definition of Done

This chapter is complete when every validation run executes within one immutable Validation Context.
