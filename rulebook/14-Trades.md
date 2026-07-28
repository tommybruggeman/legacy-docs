---
title: Trades
document: Rulebook
chapter: 14
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 05-Contracts.md
  - 06-Salary-Cap.md
  - 07-Roster-Management.md
  - 12-Player-Releases.md
---

# Chapter 14 — Trades

## Purpose

A trade is an atomic transaction that transfers ownership of one or more league assets between franchises.

Trades are the exclusive mechanism for voluntarily exchanging assets between franchises.

---

# System Ownership

This chapter governs:

- Trade construction
- Trade validation
- Trade execution
- Asset ownership transfer
- Transaction history

This chapter does not govern:

- Trade evaluation
- Commissioner veto policy
- Salary cap calculations
- Contract rules

---

# Business Rules

## Rule 14.1 — Trade Assets

Trades may include any asset type recognized by the league.

Examples include:

- Players
- Contracts
- Draft selections
- Salary cap adjustments
- Future considerations
- League-defined assets

---

## Rule 14.2 — Atomic Execution

A trade executes as a single transaction.

If any asset transfer fails validation, no ownership changes shall occur.

Partial execution is prohibited.

---

## Rule 14.3 — Ownership Transfer

Upon successful execution:

- Asset ownership transfers immediately.
- Historical ownership is preserved.
- Contracts remain attached to transferred players unless league rules specify otherwise.

---

## Rule 14.4 — Validation

The platform shall validate:

- Asset ownership.
- Roster legality.
- Salary cap compliance.
- Contract legality.
- League restrictions.

Validation occurs immediately before execution.

---

## Rule 14.5 — Audit History

Every executed trade shall permanently record:

- Timestamp
- Participating franchises
- Assets exchanged
- Commissioner actions
- Final transaction state

---

# State Transition

```text
Trade Proposed
       │
Validation
       │
Approved
       │
Execute
       │
Ownership Updated
       │
History Recorded
```

---

# Invariants

- Every asset has exactly one owner.
- Trades preserve league history.
- Trades execute atomically.
- Failed trades produce no ownership changes.

---

# Canonical Principles

Trades transfer ownership.

Trades never rewrite history.

Trades either fully succeed or fully fail.

---

# Related Documents

- Chapter 5 — Player Contracts
- Chapter 6 — Salary Cap
- Chapter 7 — Roster Management
