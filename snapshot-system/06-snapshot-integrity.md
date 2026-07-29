# Chapter 6 — Snapshot Integrity

## Purpose

Snapshot Integrity ensures that every Snapshot accurately represents the league state it claims to capture.

A Snapshot is only useful for recovery if its contents are complete, internally consistent, and uncorrupted.

Integrity verification protects the Recovery Engine from restoring invalid or incomplete league state.

---

## Responsibilities

Snapshot Integrity is responsible for:

- Verifying Snapshot completeness
- Detecting corruption
- Validating schema compatibility
- Confirming referential integrity
- Generating integrity metadata
- Preventing invalid Snapshots from being persisted

Integrity validation occurs before a Snapshot becomes eligible for recovery.

---

## Integrity Pipeline

```text
Snapshot Built
        │
        ▼
Schema Validation
        │
        ▼
Domain Validation
        │
        ▼
Referential Validation
        │
        ▼
Checksum Generation
        │
        ▼
Integrity Passed
        │
        ▼
Persist Snapshot
```

Any failed stage prevents persistence.

---

## Integrity Checks

Every Snapshot should successfully pass:

### Schema Validation

Verify:

- Required fields exist
- Correct data types
- Supported schema version
- Required metadata present

---

### Domain Validation

Verify required domains exist.

Examples:

- League
- Teams
- Rosters
- Contracts
- Salary
- Draft Picks
- Rules

---

### Referential Integrity

Verify all relationships remain valid.

Examples:

- Contracts reference valid players.
- Players reference valid teams.
- Draft picks reference valid owners.
- League references remain valid.

---

### Record Validation

Examples include:

- Expected record counts
- No duplicate identifiers
- No orphan records
- No invalid references

---

### Checksum Validation

Generate a checksum representing the Snapshot contents.

Future retrieval should verify the checksum before restoration.

---

## Failure Handling

```text
Integrity Failure
        │
        ▼
Reject Snapshot
        │
        ▼
Log Failure
        │
        ▼
Notify Event Engine
```

An invalid Snapshot must never be persisted.

---

## Integrity Metadata

Each Snapshot should record:

- Integrity Status
- Verification Timestamp
- Checksum
- Validation Version
- Schema Version

---

## Design Principles

Snapshot Integrity shall:

- Be deterministic
- Be comprehensive
- Be repeatable
- Be independent of storage
- Prevent invalid recovery points

---

## Definition of Done

This chapter is complete when every persisted Snapshot has successfully passed deterministic integrity verification and is guaranteed to represent a complete, internally consistent recovery point.
