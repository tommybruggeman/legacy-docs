# Chapter 1 — Snapshot Philosophy

## Purpose

The Snapshot System preserves immutable, point-in-time representations of league state throughout the Season Rollover process.

Its purpose is not simply to create backups, but to capture execution-aware recovery points that enable deterministic rollback, auditing, debugging, and replay.

Every Snapshot represents exactly one verified moment in a rollover execution.

---

## Philosophy

A Snapshot is a historical fact.

Once created, it never changes.

If league state changes, a new Snapshot is created.

Snapshots are never updated, patched, or overwritten.

Historical accuracy is more important than storage efficiency.

---

## Core Principles

The Snapshot System is built upon six principles.

### Immutability

Snapshots cannot be modified after persistence.

### Determinism

The same league state always produces the same Snapshot.

### Recoverability

Every Snapshot must be capable of restoring league state.

### Auditability

Every Snapshot permanently records execution history.

### Version Awareness

Every Snapshot records the schema and execution versions that created it.

### Independence

Snapshots are independent of the Event Engine, Validation Framework, and Recovery Engine.

---

## Snapshot vs Backup

Snapshots are not infrastructure backups.

| Backup | Snapshot |
|---------|----------|
| Disaster recovery | Execution recovery |
| Entire database | Single league execution |
| Infrastructure-owned | Application-owned |
| Long-term retention | Execution-aware lifecycle |
| Environment-wide | Rollover-specific |

Both systems are valuable but solve different problems.

---

## Responsibilities

Snapshots exist to:

- Preserve league state
- Enable rollback
- Support replay
- Provide audit evidence
- Simplify debugging
- Reduce recovery cost

Snapshots never execute business logic.

---

## Snapshot Guarantees

Every Snapshot guarantees:

- Complete execution context
- Immutable state
- Verified integrity
- Deterministic reconstruction
- Historical traceability

---

## Definition of Done

This chapter is complete when Snapshots are clearly defined as immutable, execution-aware records of league state that serve as the foundation for recovery, auditing, and replay.
