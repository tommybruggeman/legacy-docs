# Chapter 7 — Snapshot Retrieval

## Purpose

Snapshot Retrieval provides deterministic access to previously persisted Snapshots.

The Retrieval subsystem allows Recovery, Validation, Administration, and Replay services to locate and load the correct Snapshot without ambiguity.

---

## Responsibilities

Snapshot Retrieval is responsible for:

- Finding Snapshots
- Loading Snapshots
- Verifying integrity
- Validating compatibility
- Returning immutable Snapshot objects

Retrieval never modifies stored Snapshots.

---

## Retrieval Flow

```text
Snapshot Request
        │
        ▼
Locate Metadata
        │
        ▼
Load Snapshot
        │
        ▼
Verify Integrity
        │
        ▼
Validate Compatibility
        │
        ▼
Return Snapshot
```

---

## Lookup Methods

Snapshots may be retrieved by:

- Snapshot ID
- Execution ID
- League ID
- Event ID
- Snapshot Type
- Timestamp

The Snapshot Registry provides efficient lookup.

---

## Compatibility Checks

Before returning a Snapshot, Retrieval should verify:

- Supported schema version
- Supported Snapshot version
- Successful checksum validation
- Required metadata present

Incompatible Snapshots should not be restored.

---

## Returned Object

The Retrieval service should return an immutable Snapshot object.

Consumers must treat returned Snapshots as read-only.

---

## Failure Handling

Possible retrieval failures include:

- Snapshot not found
- Corrupted Snapshot
- Unsupported version
- Integrity verification failure
- Missing metadata

Failures should produce structured errors.

---

## Design Principles

Snapshot Retrieval shall:

- Be deterministic
- Be read-only
- Be version-aware
- Be integrity-first
- Be storage-independent

---

## Definition of Done

This chapter is complete when authorized systems can reliably locate, verify, and retrieve immutable Snapshots for recovery, replay, validation, and auditing.
