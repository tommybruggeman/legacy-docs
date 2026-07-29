# Chapter 6 — Snapshot Restoration

## Purpose

Snapshot Restoration is responsible for returning a league to a previously verified state using an immutable Snapshot.

It is the core execution step of the Recovery Engine.

Restoration never attempts to repair corrupted league state.

Instead, it replaces the current execution state with a previously validated recovery point.

---

## Responsibilities

Snapshot Restoration is responsible for:

- Locating the restore Snapshot
- Verifying Snapshot integrity
- Restoring captured league state
- Preserving recovery history
- Preventing partial restoration
- Preparing execution for validation

Snapshot Restoration does not determine *which* Snapshot to restore.

That decision belongs to the Recovery Planner.

---

## Restoration Workflow

```text
Recovery Plan
      │
      ▼
Locate Snapshot
      │
      ▼
Verify Integrity
      │
      ▼
Acquire Restore Lock
      │
      ▼
Restore League State
      │
      ▼
Verify Restoration
      │
      ▼
Release Lock
```

Every stage must succeed before progressing.

---

## Restoration Requirements

Before restoration begins, the Recovery Engine must verify:

- Snapshot exists
- Snapshot integrity passed
- Schema version supported
- League identifier matches
- Execution is authorized
- Restore lock acquired

Failure of any prerequisite immediately aborts restoration.

---

## Atomic Restoration

Snapshot restoration must be atomic.

```text
Restore Started
       │
       ▼
Restore Entire Snapshot
       │
 ┌─────┴─────┐
 ▼           ▼
Success    Failure
 │           │
 ▼           ▼
Commit     Roll Back
```

The system must never expose partially restored league state.

---

## Restored Domains

A full restoration should restore:

- League metadata
- Teams
- Rosters
- Contracts
- Salary records
- Dead cap
- Draft picks
- League rules
- Event progress
- Execution metadata

Every recovery domain should be restored together.

---

## Restore Lock

A restore operation requires exclusive access.

During restoration:

- No Event Engine execution
- No administrative edits
- No concurrent recovery
- No Snapshot creation

The restore lock prevents concurrent modification.

---

## Failure Handling

If restoration fails:

```text
Restore Failure
       │
       ▼
Record Failure
       │
       ▼
Maintain Existing Recovery State
       │
       ▼
Administrative Review
```

The Recovery Engine should never leave the league in a partially restored state.

---

## Design Principles

Snapshot Restoration shall:

- Be atomic
- Be deterministic
- Be observable
- Be fully auditable
- Preserve Snapshot immutability

---

## Definition of Done

This chapter is complete when verified Snapshots can be restored atomically, reproducibly, and safely without exposing partially restored league state.
