# Chapter 6 — Dispatch and Execution

## Purpose

The Dispatcher is responsible for executing each planned event in the order determined by the Dependency Resolver.

The Dispatcher coordinates execution but never performs business logic.

---

## Responsibilities

The Dispatcher is responsible for:

- Selecting the next executable event
- Invoking the registered handler
- Tracking execution status
- Recording execution timing
- Capturing structured results
- Reporting execution failures

---

## Execution Flow

```text
Select Next Event
        │
        ▼
Validate Readiness
        │
        ▼
Invoke Handler
        │
        ▼
Receive Result
        │
        ▼
Validate Result
        │
        ▼
Persist Result
        │
        ▼
Continue
```

---

## Handler Interface

Every handler should expose a common execution interface.

Example:

```python
result = handler.execute(
    context=execution_context,
    event=planned_event,
)
```

Each handler returns a structured Event Result.

---

## Execution States

Every event moves through the following lifecycle:

```text
Pending
    │
    ▼
Running
    │
 ┌──┴──────────┐
 ▼             ▼
Succeeded    Failed
 │             │
 ▼             ▼
Complete    Stop Execution
```

Supported states include:

- Pending
- Running
- Succeeded
- Succeeded With Warnings
- Skipped
- Failed
- Blocked
- Rolled Back

---

## Dispatcher Rules

The Dispatcher shall:

- Execute one event at a time
- Respect dependency order
- Respect execution policies
- Never skip validation
- Never ignore failures
- Never execute unknown handlers

---

## Failure Behavior

If an event fails:

```text
Current Event
      │
      ▼
Mark Failed
      │
      ▼
Persist Result
      │
      ▼
Stop Dispatch
      │
      ▼
Begin Recovery Evaluation
```

No additional events execute after a blocking failure.

---

## Design Principles

The Dispatcher shall:

- Be deterministic
- Be observable
- Be auditable
- Respect execution policies
- Produce structured results
- Stop on blocking failures

---

## Definition of Done

This chapter is complete when the Event Engine can safely dispatch every planned event in deterministic order while accurately tracking execution state and preserving complete execution history.
