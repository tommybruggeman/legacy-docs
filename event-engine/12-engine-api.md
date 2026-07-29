# Chapter 12 — Engine API

## Purpose

The Event Engine exposes a public interface for preparing, executing, resuming, cancelling, and inspecting season rollover executions.

The API defines how external systems interact with the engine.

---

## Responsibilities

The Engine API is responsible for:

- Preparing executions
- Running executions
- Resuming executions
- Cancelling executions
- Retrieving execution status
- Retrieving execution history

---

## Prepare Execution

Creates an Execution Context and compiles an Execution Plan.

Example:

```python
execution = engine.prepare(
    league_id="league_001",
    source_season=2026,
    target_season=2027,
    mode="dry_run",
)
```

No events execute during preparation.

---

## Run Execution

Executes the compiled Execution Plan.

Example:

```python
result = engine.run(execution.id)
```

The engine validates the plan before dispatch begins.

---

## Resume Execution

Continues a previously interrupted execution.

Example:

```python
result = engine.resume(execution.id)
```

Resume begins at the first incomplete event.

---

## Cancel Execution

Stops an execution before completion.

Example:

```python
engine.cancel(execution.id)
```

Cancellation records the execution state but performs no automatic recovery.

---

## Execution Status

Example:

```python
status = engine.status(execution.id)
```

Possible statuses include:

- Requested
- Planning
- Ready
- Running
- Paused
- Failed
- Rolling Back
- Completed
- Cancelled

---

## Execution History

Historical executions should remain queryable.

Example:

```python
history = engine.history(league_id)
```

History supports:

- Auditing
- Reporting
- Debugging
- Operational analysis

---

## API Principles

The Engine API shall:

- Be deterministic
- Be versioned
- Be observable
- Be secure
- Be extensible
- Return structured responses

---

## Definition of Done

This chapter is complete when external systems can safely prepare, execute, monitor, resume, cancel, and inspect season rollover executions through a stable public interface.
