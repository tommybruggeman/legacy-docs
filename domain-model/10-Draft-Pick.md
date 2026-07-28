---
title: Draft Pick
document: Domain Model
entity: Draft Pick
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 04-Franchise.md
---

# Draft Pick

## Purpose

A Draft Pick represents the right to make a future selection during a Rookie Draft.

Draft Picks are transferable assets independent of Players.

---

# Canonical Identity

Every Draft Pick is uniquely identified by:

- League
- Season
- Round
- Selection

---

# Owned State

A Draft Pick owns:

- Current Owner
- Draft Position
- Draft Year
- Status

---

# Relationships

Belongs to one League.

Owned by one Franchise.

Consumed by one Draft Selection.

---

# Lifecycle

Created

↓

Owned

↓

Traded

↓

Consumed

↓

Historical

---

# Commands

- Transfer Pick
- Consume Pick

---

# Emitted Events

- DraftPickTransferred
- DraftPickConsumed

---

# Consumed Events

- TradeExecuted
- DraftSelectionMade

---

# Validation Rules

Reject:

- Duplicate Draft Picks.
- Multiple owners.
- Consuming an already consumed Pick.

---

# Invariants

- Every Draft Pick has exactly one owner.
- Every Draft Pick is consumed at most once.
- Historical ownership remains immutable.

---

# Historical Requirements

Draft Pick ownership history shall remain permanently preserved.

---

# AI Interpretation

AI shall treat Draft Picks as future assets with independent ownership and valuation.
