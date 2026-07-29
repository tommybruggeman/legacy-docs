# Chapter 9 — Idempotency and Resume

## Purpose

The Event Engine must safely handle repeated executions.

Events should never produce duplicate mutations simply because an execution was retried or resumed after interruption.

Idempotency protects league integrity during unexpected failures.

---

## Idempotency

Idempotency means that executing an event multiple times produces the same final state as executing it once.

The Event Engine enforces idempotency through execution policies.

---

## Execution Policies

Supported policies include:

### Once

The event may execute only once.

```text
Execute
    │
    ▼
Complete

Second Attempt
    │
    ▼
Rejected
```

---

### Safe Repeat

The event may execute repeatedly without changing the final outcome.

Examples include:

- Validation
- Report generation
- Snapshot verification

---

### Resume Only

The event may resume an interrupted execution but may not restart from the beginning.

---

### Replace Previous

The event replaces an earlier generated artifact.

Examples include regenerated reports or temporary calculations.

---

## Resume Flow

```text
Interrupted Execution
          │
          ▼
Load Execution State
          │
          ▼
Inspect Event Results
          │
          ▼
Determine Resume Point
          │
          ▼
Continue Execution
```

The engine resumes from the first incomplete event.

Previously completed events are not re-executed unless their execution policy explicitly permits it.

---

## Resume Validation

Before resuming, the engine validates:

- Execution Context
- Execution Lock
- Event Results
- Snapshot Availability
- League State
- Engine Version

Resume is rejected if validation fails.

---

## Interrupted Execution Example

```text
Age Players               ✓
Reduce Contracts          ✓
Expire Contracts          ✗
Free Agency               Blocked
Reset Taxi                Blocked
```

Execution resumes from:

```text
Expire Contracts
```

Not from:

```text
Age Players
```

---

## Engine Guarantees

The Event Engine guarantees that:

- Completed events are preserved.
- Duplicate mutations are prevented.
- Resume begins from the correct location.
- Execution history remains intact.

---

## Design Principles

Idempotency shall:

- Prevent duplicate writes
- Support safe retries
- Support safe recovery
- Preserve deterministic execution
- Protect league integrity

---

## Definition of Done

This chapter is complete when interrupted rollovers can safely resume without repeating completed work or introducing duplicate state changes.
