---
title: Player Releases
document: Rulebook
chapter: 12
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 05-Contracts.md
  - 06-Salary-Cap.md
  - 07-Roster-Management.md
  - 11-Dead-Cap.md
---

# Chapter 12 — Player Releases

## Purpose

This chapter defines the deterministic process for removing a player from a franchise.

A player release terminates the player's active roster relationship with the franchise while preserving all required financial and historical obligations.

A release is a state transition.

It is not the deletion of a player or contract.

---

# System Ownership

This chapter governs:

- Release eligibility
- Release execution
- Roster removal
- Contract termination
- Dead Cap generation events
- Historical recording

This chapter does not govern:

- Dead Cap calculations
- Salary Cap calculations
- Contract creation
- Player acquisition

---

# Business Rules

## Rule 12.1 — Release Eligibility

Any player currently under contract may be released unless prohibited by league configuration.

League configuration may restrict releases during specific league periods.

---

## Rule 12.2 — Release Event

Executing a release shall:

- Remove the player from the franchise roster.
- Terminate the active contract.
- Generate any required Dead Cap obligation.
- Record the release in league history.
- Recalculate franchise roster validity.
- Recalculate franchise salary cap.

The release event is atomic.

Either every required operation succeeds or none are committed.

---

## Rule 12.3 — Player Ownership

Following a successful release:

- The franchise no longer owns the player's contract.
- The player becomes eligible for acquisition according to league rules.
- Historical ownership remains preserved.

---

## Rule 12.4 — Dead Cap

If league rules require Dead Cap, the platform shall generate a Dead Cap obligation immediately following contract termination.

Dead Cap creation is governed by Chapter 11.

---

## Rule 12.5 — Historical Preservation

Every release shall permanently preserve:

- Franchise
- Player
- Contract
- Season
- Timestamp
- Dead Cap obligation
- Commissioner override (if applicable)

---

# State Transition

```text
Under Contract
      │
      ▼
Release Requested
      │
Validation
      │
      ▼
Contract Terminated
      │
      ▼
Roster Updated
      │
      ▼
Dead Cap Generated
      │
      ▼
Player Becomes Available
```

---

# Validation Rules

The platform shall reject:

- Releases for players not under contract.
- Duplicate release requests.
- Releases violating league restrictions.
- Releases producing invalid financial records.

---

# Invariants

The following conditions must always remain true:

- A released player no longer occupies a franchise roster designation.
- Historical contracts remain preserved.
- Historical release events remain preserved.
- Dead Cap obligations remain linked to the originating contract.

---

# Edge Cases

## Commissioner Releases

Commissioners may execute administrative releases.

Administrative releases shall be permanently identified within the audit log.

---

## Rule Changes

League rule changes affect future release events only unless historical recalculation is explicitly requested.

---

# Canonical Principles

A release removes ownership.

A release does not erase history.

A release may create financial obligations.

Every release is permanently auditable.

---

# Related Documents

- Chapter 5 — Player Contracts
- Chapter 6 — Salary Cap
- Chapter 11 — Dead Cap
