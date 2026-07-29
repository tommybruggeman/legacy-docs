# Chapter 7 — Event Results

## Purpose

Every event executed by the Event Engine must produce a structured Event Result.

The Event Result is the official record of what occurred during the execution of an event and serves as the foundation for auditing, validation, recovery, and execution reporting.

An event is not considered complete until its Event Result has been recorded.

---

## Responsibilities

The Event Result is responsible for recording:

- Event identity
- Execution identity
- Execution status
- Start time
- End time
- Duration
- Records examined
- Records modified
- Warnings
- Errors
- Outputs
- Metadata

---

## Example

```python
EventResult(
    execution_id="rollover_2026_2027",
    event_id="season.contracts.expire",
    status="SUCCEEDED",
    started_at="2026-12-31T18:00:01Z",
    completed_at="2026-12-31T18:00:03Z",
    duration_ms=1843,
    records_examined=224,
    records_modified=18,
    warnings=[],
    errors=[],
    outputs={
        "expired_contracts":18
    }
)
```

---

## Event Statuses

Every Event Result must end in exactly one terminal state.

```text
Pending
    │
    ▼
Running
    │
 ┌──┴─────────────────────────┐
 ▼                            ▼
Succeeded                 Failed
 │                            │
 ▼                            ▼
Complete              Recovery Required
```

Supported statuses:

- Pending
- Running
- Succeeded
- Succeeded With Warnings
- Skipped
- Failed
- Blocked
- Rolled Back

---

## Success Criteria

An event succeeds when:

- The handler completes successfully.
- Validation succeeds.
- The expected outputs are produced.
- No blocking errors occur.

An event may legitimately modify zero records.

Zero modifications do not automatically indicate failure.

---

## Failure Criteria

An event fails when:

- An exception occurs.
- Validation fails.
- Dependencies are missing.
- Required data is unavailable.
- Execution policies are violated.

Failures must always include sufficient diagnostic information for recovery.

---

## Event Metrics

Each Event Result should capture operational metrics.

Examples include:

- Records examined
- Records modified
- Execution duration
- Warnings generated
- Errors generated

These metrics provide observability without requiring inspection of application logs.

---

## Persistence

Every Event Result must be persisted before the next event begins.

This guarantees that execution history survives interruptions and supports execution resume.

---

## Design Principles

Event Results shall:

- Be immutable
- Be auditable
- Be deterministic
- Be serializable
- Be machine-readable
- Be human-readable

---

## Definition of Done

This chapter is complete when every executed event produces one immutable Event Result that completely describes the outcome of that execution.
