# Chapter 8 — Resume Execution

## Purpose

Resume Execution restarts the Event Engine after successful recovery.

Rather than restarting the entire rollover, execution continues from the first incomplete event identified by the Recovery Plan.

---

## Responsibilities

Resume Execution is responsible for:

- Determining the resume point
- Rebuilding execution context
- Reconstructing dependency state
- Restarting the Event Engine
- Preserving execution history

Resume Execution never repeats successfully completed events.

---

## Resume Workflow

```text
Recovery Validation Passed
          │
          ▼
Load Recovery Plan
          │
          ▼
Identify Resume Event
          │
          ▼
Rebuild Execution Context
          │
          ▼
Restart Event Engine
```

Execution resumes from the first incomplete event.

---

## Resume Point

The Recovery Plan identifies:

- Last successful event
- Last verified checkpoint
- First incomplete event

Example:

```text
Completed

✓ Contracts Reduced
✓ Contracts Expired
✓ Free Agency Created

Resume

→ Draft Advancement
```

The Event Engine begins with the Draft Advancement event.

---

## Context Reconstruction

Before resuming, the Recovery Engine rebuilds:

- Execution Context
- Dependency graph
- Event status
- Validation state
- Progress tracking

The rebuilt context should match the restored Snapshot.

---

## Resume Guarantees

Resume Execution guarantees:

- Completed events remain complete.
- Successful Snapshots remain unchanged.
- Event ordering remains deterministic.
- Validation continues normally.
- Audit history is preserved.

---

## Failure During Resume

If another failure occurs:

```text
Resume Execution
       │
       ▼
Failure
       │
       ▼
Recovery Engine
```

Nested failures begin a new Recovery operation with a new Recovery Context.

---

## Design Principles

Resume Execution shall:

- Preserve completed work
- Minimize replay
- Be deterministic
- Maintain execution ordering
- Integrate seamlessly with the Event Engine

---

## Definition of Done

This chapter is complete when recovered rollovers can resume from the first incomplete event while preserving deterministic execution history and league integrity.
