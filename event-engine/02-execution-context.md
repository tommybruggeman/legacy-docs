# Chapter 2 — Execution Context

## Purpose

Every season rollover executes within a single immutable Execution Context.

The Execution Context represents the authoritative metadata for one rollover execution and provides every event with a shared view of the execution.

It is the source of truth for the lifetime of the rollover.

---

## Responsibilities

The Execution Context is responsible for storing:

- Execution identity
- League identity
- Source season
- Target season
- Execution mode
- Requesting user
- Execution options
- Engine version
- Catalog version
- Plan version
- Start timestamp

---

## Example

```python
ExecutionContext(
    execution_id="rollover_2026_to_2027",
    league_id="league_001",
    source_season=2026,
    target_season=2027,
    execution_mode="commit",
    requested_by="user_123",
    started_at="2026-12-31T20:00:00Z",
    engine_version="1.0.0",
    catalog_version="1.0.0",
    plan_version="1.0.0",
)
```

---

## Execution Modes

### Dry Run

Evaluates the rollover without permanently modifying league data.

Characteristics:

- Executes planning
- Executes validation
- Simulates event execution
- Produces reports
- Produces warnings
- Produces expected changes

No permanent writes occur.

---

### Commit

Executes the approved rollover against production data.

Characteristics:

- Creates snapshots
- Executes handlers
- Persists mutations
- Records audit history
- Runs validation
- Produces final reports

---

## Immutability

After execution begins, the following fields must never change:

- execution_id
- league_id
- source_season
- target_season
- execution_mode
- requested_by
- engine_version
- catalog_version
- started_at

Runtime state belongs in execution results, not the execution context.

---

## Lifecycle

```text
Create
      │
      ▼
Validate
      │
      ▼
Freeze
      │
      ▼
Read During Execution
      │
      ▼
Archive
```

---

## Access Rules

Every event handler receives the same Execution Context.

Handlers may:

- Read values
- Use values
- Log values

Handlers may not:

- Modify values
- Replace values
- Recreate values

---

## Design Principles

The Execution Context shall:

- Be immutable
- Be deterministic
- Be globally available
- Exist exactly once per execution
- Be fully auditable
- Be serializable

---

## Definition of Done

This chapter is complete when every rollover can be represented by one immutable Execution Context that every system can safely consume.
