---
title: Roster Assignment
document: Domain Model
entity: Roster Assignment
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 08-Contract.md
---

# Roster Assignment

## Purpose

A Roster Assignment represents the current roster designation of an active Contract.

Roster Assignment defines where a contracted Player exists within a Franchise.

Ownership is determined by the Contract.

Roster Assignment determines usage.

---

# Canonical Identity

A Roster Assignment is uniquely identified by its active Contract.

---

# Owned State

A Roster Assignment owns:

- Designation
- Assignment Timestamp
- Eligibility State

---

# Relationships

Belongs to one Contract.

References one Franchise.

---

# Lifecycle

Assigned

↓

Modified

↓

Removed

---

# Commands

- Assign Roster
- Promote
- Demote
- Activate

---

# Emitted Events

- RosterAssigned
- RosterUpdated

---

# Consumed Events

- ContractCreated

---

# Validation Rules

Reject:

- Multiple active roster assignments.
- Invalid designation.
- Capacity violations.

---

# Invariants

- Every active Contract has exactly one Roster Assignment.
- Every Player occupies one designation.
- Assignment never changes ownership.

---

# Historical Requirements

Roster movement history shall remain permanently attributable.

---

# AI Interpretation

AI shall determine player availability through Roster Assignments.

Ownership continues to be determined by Contracts.
