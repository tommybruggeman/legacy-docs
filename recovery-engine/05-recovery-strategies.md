# Chapter 5 — Recovery Strategies

## Purpose

Recovery Strategies define the approved methods by which the Recovery Engine restores league integrity after a failure.

Each strategy represents a different balance between preserving completed work and minimizing operational risk.

---

## Strategy Selection

```text
Failure
    │
    ▼
Failure Classification
    │
    ▼
Recovery Planning
    │
    ▼
Select Strategy
```

The selected strategy becomes part of the immutable Recovery Plan.

---

## Resume

Resume execution without restoring a Snapshot.

Use when:

- No state mutation occurred.
- Failure happened before commit.

Advantages:

- Fastest recovery
- No replay

---

## Partial Rollback

Restore the latest verified Checkpoint Snapshot.

Replay only the remaining events.

Advantages:

- Preserves completed work
- Reduces execution time

This should be the preferred strategy whenever possible.

---

## Full Rollback

Restore the Initial Snapshot.

Replay the entire rollover.

Advantages:

- Simplest recovery path
- Maximum confidence

Disadvantages:

- Longest execution time

---

## Abort

Terminate execution.

Use when:

- Recovery is unsafe.
- Required Snapshots are unavailable.
- Administrative review is required.

League state remains unchanged after restoration to the chosen recovery point.

---

## Strategy Comparison

| Strategy | Restore Snapshot | Replay Required | Typical Usage |
|-----------|------------------|-----------------|---------------|
| Resume | No | No | Pre-commit failures |
| Partial Rollback | Latest Checkpoint | Partial | Most execution failures |
| Full Rollback | Initial Snapshot | Entire rollover | Severe corruption |
| Abort | Optional | None | Unsafe recovery |

---

## Strategy Requirements

Every Recovery Strategy must:

- Produce deterministic outcomes
- Preserve audit history
- Validate restored state
- Integrate with the Event Engine
- Prevent corruption

---

## Design Principles

Recovery Strategies shall:

- Favor minimal replay
- Prioritize safety
- Remain deterministic
- Be fully observable
- Support future extension

---

## Definition of Done

This chapter is complete when the Recovery Engine can consistently select and execute the appropriate recovery strategy for every supported failure scenario while preserving league integrity.
