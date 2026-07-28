---
title: Draft Picks
document: Rulebook
chapter: 18
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 14-Trades.md
  - 17-Rookie-Draft.md
---

# Chapter 18 — Draft Picks

## Purpose

Draft Picks represent future selection rights within a Rookie Draft.

Draft Picks are transferable league assets with independent ownership.

A Draft Pick is an asset.

It is not a player.

---

# System Ownership

This chapter governs:

- Draft Pick ownership
- Draft Pick transfers
- Draft Pick consumption
- Draft Pick history

---

# Business Rules

## Rule 18.1 — Ownership

Every Draft Pick has exactly one owning franchise.

Ownership may change through league-approved transactions.

---

## Rule 18.2 — Transfers

Draft Picks may be transferred through trades.

Ownership transfers immediately upon successful trade execution.

---

## Rule 18.3 — Consumption

Selecting a player consumes the associated Draft Pick.

Consumed Draft Picks remain part of league history.

---

## Rule 18.4 — Historical Preservation

Historical ownership of every Draft Pick shall remain permanently recorded.

---

# State Transition

```text
Created
     │
Owned
     │
Traded
     │
Consumed
     │
Historical Record
```

---

# Validation Rules

Reject:

- Duplicate ownership.
- Consuming an already-consumed Draft Pick.
- Trading nonexistent Draft Picks.

---

# Invariants

- Every Draft Pick has one owner.
- Every Draft Pick may be consumed once.
- Historical ownership remains immutable.

---

# Canonical Principles

Draft Picks are assets.

Draft Picks transfer ownership.

Draft Picks are permanently historical.

---

# Related Documents

- Chapter 14 — Trades
- Chapter 17 — Rookie Draft
