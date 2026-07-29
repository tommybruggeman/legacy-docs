# Chapter 10 — Engine Observability

## Purpose

The Event Engine must be fully observable.

Every execution should produce enough information for an administrator or developer to understand exactly what occurred without reproducing the rollover.

Observability exists to improve debugging, auditing, performance analysis, and operational confidence.

---

## Responsibilities

The Event Engine is responsible for recording:

- Execution lifecycle
- Event lifecycle
- Execution timing
- Event timing
- Validation results
- Warnings
- Errors
- Performance metrics
- Audit metadata

---

## Observability Levels

The engine should expose information at four levels.

### Execution Level

Overall rollover information.

Examples:

- Execution ID
- League ID
- Source season
- Target season
- Execution mode
- Overall duration
- Final status

---

### Event Level

Information for each executed event.

Examples:

- Event ID
- Handler
- Status
- Duration
- Records examined
- Records modified

---

### Validation Level

Validation outcomes.

Examples:

- Preconditions
- Postconditions
- Final validation
- Failed rules
- Warning rules

---

### System Level

Operational information.

Examples:

- Engine version
- Catalog version
- Handler version
- Snapshot version
- Recovery status

---

## Execution Timeline

```text
Execution Started
        │
        ▼
Planning
        │
        ▼
Validation
        │
        ▼
Dispatch
        │
        ▼
Execution
        │
        ▼
Final Validation
        │
        ▼
Execution Complete
```

Every stage should include timestamps and duration metrics.

---

## Performance Metrics

Suggested metrics include:

- Total execution duration
- Average event duration
- Slowest event
- Fastest event
- Records examined
- Records modified
- Validation duration
- Snapshot duration

---

## Logging Principles

Logs should answer:

- What happened?
- When did it happen?
- Why did it happen?
- How long did it take?
- What changed?
- What failed?

Logs should never replace structured execution data.

---

## Design Principles

Observability shall:

- Be comprehensive
- Be structured
- Be deterministic
- Be queryable
- Support troubleshooting
- Support auditing

---

## Definition of Done

This chapter is complete when every rollover execution can be reconstructed from its recorded execution history without reproducing the original execution.
