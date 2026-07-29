# Chapter 4 — Checkpoint Strategy

## Purpose

Checkpoint Snapshots provide intermediate recovery points throughout rollover execution.

Rather than always restoring from the Initial Snapshot, the Recovery Engine can resume from the most recent verified Checkpoint Snapshot.

---

## Philosophy

Checkpoints should exist after meaningful state transitions.

They should reduce recovery cost without creating unnecessary storage overhead.

---

## Example Timeline

```text
Initial Snapshot
        │
        ▼
Contract Reduction
        │
Checkpoint
        │
        ▼
Contract Expiration
        │
Checkpoint
        │
        ▼
Free Agency Generation
        │
Checkpoint
        │
        ▼
Draft Advancement
        │
Checkpoint
        │
        ▼
Final Snapshot
```

---

## Candidate Checkpoints

Recommended checkpoint locations include:

- Contract Reduction
- Contract Expiration
- Free Agency Creation
- Roster Reset
- Draft Advancement

---

## Requirements

Every Checkpoint Snapshot must:

- Follow successful validation
- Record execution progress
- Be immutable
- Be verified
- Be fully recoverable

---

## Recovery Flow

```text
Failure
    │
    ▼
Locate Latest Checkpoint
    │
    ▼
Restore Snapshot
    │
    ▼
Resume Remaining Events
```

---

## Design Principles

Checkpoint placement should:

- Minimize replay
- Limit storage growth
- Preserve determinism
- Simplify recovery

---

## Definition of Done

This chapter is complete when checkpoint placement provides efficient, deterministic recovery points throughout rollover execution.
