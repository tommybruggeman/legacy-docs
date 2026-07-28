---
title: Free Agency
document: Rulebook
chapter: 16
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 05-Contracts.md
  - 06-Salary-Cap.md
  - 12-Player-Releases.md
---

# Chapter 16 — Free Agency

## Purpose

Free Agency governs the acquisition of players not currently owned by any franchise.

A free agent has no active franchise ownership but remains eligible for acquisition according to league rules.

---

# System Ownership

This chapter governs:

- Free agent availability
- Player acquisition
- Contract creation
- Ownership assignment

---

# Business Rules

## Rule 16.1 — Eligibility

A player enters Free Agency when no franchise owns an active contract for that player.

---

## Rule 16.2 — Acquisition

Acquiring a free agent shall:

- Create a new contract.
- Assign ownership.
- Assign a roster designation.
- Validate salary cap compliance.
- Record the acquisition.

---

## Rule 16.3 — Contract Creation

The league configuration determines:

- Initial salary
- Contract duration
- Acquisition cost
- Signing restrictions

---

## Rule 16.4 — Validation

The platform shall validate:

- Player availability
- Roster legality
- Salary cap compliance
- League acquisition restrictions

---

# State Transition

```text
Unowned Player
      │
Available
      │
Signed
      │
Contract Created
      │
Roster Assigned
```

---

# Invariants

- A player may have at most one active owner.
- Every signed player receives a contract.
- Free agents have no franchise ownership.

---

# Canonical Principles

Free Agency creates new ownership.

Free Agency creates new contracts.

Ownership begins only after successful validation.

---

# Related Documents

- Chapter 5 — Player Contracts
- Chapter 6 — Salary Cap
- Chapter 12 — Player Releases
