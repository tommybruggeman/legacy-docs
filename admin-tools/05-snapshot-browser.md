# Chapter 5 — Snapshot Browser

## Purpose

The Snapshot Browser provides administrators with secure access to every Snapshot created during rollover execution.

It enables inspection, comparison, and verification of Snapshots without exposing direct database manipulation.

The Snapshot Browser is an inspection tool.

It never modifies Snapshot contents.

---

## Responsibilities

The Snapshot Browser is responsible for:

- Listing Snapshots
- Displaying Snapshot metadata
- Viewing Snapshot contents
- Comparing Snapshots
- Verifying integrity
- Supporting recovery investigations

---

## Browser Overview

```text
Snapshots

Snapshot ID

Execution ID

Type

Created

Schema Version

Integrity Status

Recovery Eligible
```

---

## Snapshot Types

The browser should clearly identify:

- Initial Snapshot
- Checkpoint Snapshot
- Final Snapshot
- Manual Snapshot

Each type should display its intended recovery role.

---

## Snapshot Details

Selecting a Snapshot should display:

- Metadata
- Captured domains
- Record counts
- Schema version
- Integrity verification
- Related execution

Administrators should not need raw storage access.

---

## Snapshot Comparison

Administrators should be able to compare:

```text
Snapshot A

↓

Field Differences

↓

Snapshot B
```

Examples include:

- Contract differences
- Roster changes
- Salary changes
- Draft pick changes

Comparison should remain read-only.

---

## Integrity Display

The browser should expose:

- Integrity Status
- Checksum
- Verification Timestamp
- Validation Version

Recovery-eligible Snapshots should be clearly identified.

---

## Search & Filtering

Snapshots should be searchable by:

- Snapshot ID
- League
- Execution
- Event
- Snapshot Type
- Date

---

## Design Principles

The Snapshot Browser shall:

- Be read-only
- Preserve immutability
- Support investigation
- Be performant
- Be version-aware

---

## Definition of Done

This chapter is complete when administrators can inspect, compare, and verify every Snapshot without modifying historical league state.
