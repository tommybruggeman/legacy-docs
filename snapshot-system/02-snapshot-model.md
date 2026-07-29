# Chapter 2 — Snapshot Model

## Purpose

The Snapshot Model defines the canonical structure of every Snapshot created by the Snapshot System.

Every Snapshot must conform to this model regardless of snapshot type.

---

## Responsibilities

Each Snapshot records:

- Snapshot ID
- Execution ID
- League ID
- Snapshot Type
- Source Season
- Target Season
- Event ID
- Validation ID
- Schema Version
- Snapshot Version
- Created Timestamp

It also contains the captured league state.

---

## Example

```python
Snapshot(
    snapshot_id="snapshot_001",
    execution_id="rollover_2026_2027",
    league_id="league_123",
    snapshot_type="CHECKPOINT",
    source_season=2026,
    target_season=2027,
    event_id="season.contracts.expire",
    validation_id="validation_014",
    schema_version="1.0.0",
)
```

---

## Snapshot Domains

Each Snapshot should capture:

- League
- Teams
- Rosters
- Contracts
- Salary data
- Draft picks
- League rules
- Cap adjustments
- Event progress
- Validation references

The model should contain every domain required to restore league state.

---

## Metadata

Metadata describes the Snapshot itself.

Examples include:

- Creation time
- Creator
- Version
- Execution
- Snapshot type
- Integrity checksum

Metadata must be queryable without loading the full Snapshot payload.

---

## Payload

The payload contains the immutable league state.

The payload should remain independent of storage implementation.

---

## Design Principles

The Snapshot Model shall:

- Be immutable
- Be versioned
- Be serializable
- Be self-describing
- Be deterministic

---

## Definition of Done

This chapter is complete when every Snapshot conforms to one canonical data model that supports persistence, retrieval, validation, and recovery.
