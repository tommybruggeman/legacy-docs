---
title: Transaction
document: Domain Model
entity: Transaction
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 13-Trade.md
  - 15-Player-Release.md
  - 18-Waiver-Claim.md
---

# Transaction

## Purpose

A Transaction is the canonical record of every state-changing operation within a League.

Transactions provide the permanent audit trail from which league history is reconstructed.

---

# Canonical Identity

Every Transaction shall possess one immutable canonical identifier.

---

# Owned State

- Transaction Type
- Initiating Actor
- Timestamp
- Affected Entities
- Result
- Metadata

---

# Relationships

May originate from:

- Trade
- Waiver Claim
- Player Release
- Draft Selection
- Commissioner Action
- Contract Update

Produces one or more Audit Events.

---

# Lifecycle

Created

↓

Executed

↓

Historical

---

# Commands

Transactions are immutable after execution.

---

# Emitted Events

- TransactionRecorded

---

# Invariants

- Every state change produces exactly one Transaction.
- Transactions are immutable.
- Historical ordering is preserved.

---

# AI Interpretation

Transactions are the primary source of historical truth for reasoning, auditing, and league reconstruction.
