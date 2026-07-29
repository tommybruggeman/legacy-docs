# Chapter 8 — Failure and Stop Policy

## Purpose

The Event Engine must fail safely.

Whenever the engine cannot guarantee the correctness of continued execution, it must immediately stop processing additional events.

Protecting league integrity always takes priority over completing a rollover.

---

## Failure Philosophy

Failures should never be hidden.

Failures should never be ignored.

Failures should never be converted into warnings.

Every failure must be explicitly recorded and evaluated.

---

## Blocking Failures

Examples of blocking failures include:

- Missing event handler
- Failed dependency
- Validation failure
- Snapshot failure
- Database write failure
- Execution lock loss
- Unhandled exception
- Corrupted execution context
- Invalid Event Result

Any blocking failure immediately halts execution.

---

## Stop Flow

```text
Blocking Failure
        │
        ▼
Record Failure
        │
        ▼
Persist Event Result
        │
        ▼
Stop Dispatcher
        │
        ▼
Block Remaining Events
        │
        ▼
Evaluate Recovery
```

---

## Event Classification

Failures should be categorized.

Suggested categories:

- Validation
- Dependency
- Handler
- Database
- Snapshot
- Authorization
- Configuration
- System

Categorization improves recovery reporting and troubleshooting.

---

## Remaining Events

After a blocking failure:

Completed events remain completed.

The failed event is marked Failed.

All downstream events become Blocked.

Blocked events must never execute until recovery has been completed.

---

## Failure Recording

Every failure record should include:

- Execution ID
- Event ID
- Failure category
- Failure message
- Timestamp
- Stack trace (if available)
- Handler version
- Engine version

This information becomes part of the execution audit.

---

## Engine Guarantees

The engine guarantees that:

- No additional events execute after a blocking failure.
- Failures are never hidden.
- Partial execution is fully recorded.
- Recovery always begins from a known state.

---

## Design Principles

The Failure Policy shall:

- Fail closed
- Preserve data integrity
- Produce complete diagnostics
- Prevent silent corruption
- Support deterministic recovery

---

## Definition of Done

This chapter is complete when every blocking failure consistently stops execution, preserves execution history, and prepares the rollover for recovery.
