---
title: Contract
document: Domain Model
entity: Contract
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 04-Franchise.md
  - 07-Player.md
---

# Contract

## Purpose

A Contract represents the financial agreement binding a Player to a Franchise within a League.

Contracts define ownership, salary obligations, and duration.

Contracts are the canonical representation of player ownership.

---

# Canonical Identity

Every Contract shall possess one immutable canonical identifier.

---

# Owned State

A Contract owns:

- Player
- Franchise
- Salary
- Remaining Years
- Status
- Acquisition Method

---

# Relationships

Belongs to one Franchise.

References one Player.

May generate one or more Dead Cap Obligations.

---

# Lifecycle

Created

↓

Active

↓

Extended

↓

Expired

or

Released

↓

Historical

---

# Commands

- Create Contract
- Extend Contract
- Release Contract
- Expire Contract

---

# Emitted Events

- ContractCreated
- ContractExtended
- ContractReleased
- ContractExpired

---

# Consumed Events

- RookieDraftSelection
- FreeAgentSigning
- TradeExecuted

---

# Validation Rules

Reject:

- Negative salary.
- Invalid contract duration.
- Multiple active Contracts for the same Player within one League.

---

# Invariants

- One active Contract per Player per League.
- Every Contract references one Player.
- Every Contract references one Franchise.

---

# Historical Requirements

Expired and released Contracts remain permanently historical.

---

# AI Interpretation

AI shall determine player ownership through active Contracts.

Roster assignments shall never determine ownership.
