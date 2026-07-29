# Chapter 3 — Snapshot Lifecycle

## Purpose

The Snapshot Lifecycle defines every stage through which a Snapshot passes, from initial request to long-term archival.

Every Snapshot follows the same deterministic lifecycle.

---

## Lifecycle Overview

```text
Request Snapshot
        │
        ▼
Build Snapshot
        │
        ▼
Verify Snapshot
        │
        ▼
Persist Snapshot
        │
        ▼
Register Snapshot
        │
        ▼
Reference Snapshot
        │
        ▼
Archive Snapshot
```

---

## Stage 1 — Request

A subsystem requests a Snapshot.

Supported callers include:

- Event Engine
- Recovery Engine
- Administrative Tools
- Testing Framework

---

## Stage 2 — Build

The Snapshot Builder gathers all required league state from the database using a consistent execution context.

---

## Stage 3 — Verify

Before persistence, the Snapshot undergoes integrity verification.

Verification includes:

- Referential integrity
- Schema validation
- Serialization
- Required domain checks
- Checksum generation

---

## Stage 4 — Persist

Verified Snapshots are atomically written to storage.

Partial writes are not permitted.

---

## Stage 5 — Register

Metadata is added to the Snapshot Registry for efficient discovery and retrieval.

---

## Stage 6 — Reference

Snapshots may be consumed by:

- Recovery Engine
- Validation Framework
- Replay tools
- Administrative reporting

Snapshots remain read-only.

---

## Stage 7 — Archive

Snapshots eventually transition to archival storage while remaining retrievable.

---

## Definition of Done

This chapter is complete when every Snapshot follows one immutable lifecycle from creation through archival.
