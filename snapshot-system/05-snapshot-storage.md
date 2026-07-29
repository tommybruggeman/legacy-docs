# Chapter 5 — Snapshot Storage

## Purpose

Snapshot Storage provides durable, immutable persistence for every Snapshot created during rollover execution.

The storage layer is responsible only for storing and retrieving Snapshots—it never interprets business rules.

---

## Responsibilities

Snapshot Storage is responsible for:

- Snapshot persistence
- Snapshot retrieval
- Metadata indexing
- Integrity verification
- Version compatibility
- Retention management

---

## Storage Architecture

```text
Snapshot Builder
        │
        ▼
Snapshot Storage
        │
 ┌──────┼────────┐
 ▼      ▼        ▼
Metadata Payload Checksums
```

---

## Storage Requirements

The storage layer must support:

- Atomic writes
- Atomic reads
- Immutable records
- Schema versioning
- Efficient retrieval

---

## Snapshot Components

Each stored Snapshot contains:

### Metadata

Execution identifiers and descriptive information.

### Payload

Complete captured league state.

### Integrity Data

Checksums, schema versions, and verification results.

---

## Immutability

After persistence:

- Snapshots cannot be edited.
- Payloads cannot be replaced.
- Metadata cannot be modified.

Any correction requires creating a new Snapshot.

---

## Retention

Retention policies should define:

- Active storage duration
- Archive transition
- Long-term retention
- Deletion policy (if permitted)

Historical audit requirements should guide retention decisions.

---

## Design Principles

Snapshot Storage shall:

- Be durable
- Be immutable
- Be scalable
- Be version-aware
- Be storage-implementation independent

---

## Definition of Done

This chapter is complete when the Snapshot Storage layer can reliably persist, retrieve, verify, and retain immutable Snapshots throughout their lifecycle.
