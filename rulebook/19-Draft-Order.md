---
title: Draft Order
document: Rulebook
chapter: 19
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 17-Rookie-Draft.md
  - 18-Draft-Picks.md
---

# Chapter 19 — Draft Order

## Purpose

Draft Order determines the sequence in which franchises exercise Draft Picks during a Rookie Draft.

The Draft Order is generated prior to the draft and remains immutable throughout draft execution unless modified by commissioner action.

---

# System Ownership

This chapter governs:

- Draft sequence generation
- Pick ordering
- Draft sequencing validation

This chapter does not govern:

- Draft Pick ownership
- Player selection
- Rookie contracts

---

# Business Rules

## Rule 19.1 — Generation

The league shall establish one official Draft Order before each Rookie Draft.

The generation method is determined by league configuration.

Examples include:

- Previous season standings
- Playoff finish
- Lottery
- Commissioner-defined ordering

---

## Rule 19.2 — Immutability

Once the draft begins, the Draft Order shall remain fixed.

Only commissioner intervention may modify the sequence.

---

## Rule 19.3 — Traded Picks

Trading a Draft Pick transfers the selection right but does not alter the Draft Order.

The position remains unchanged.

Only ownership changes.

---

# State Transition

```text
League Season Ends
        │
Draft Order Generated
        │
Draft Begins
        │
Selections Executed
        │
Draft Complete
```

---

# Validation Rules

Reject:

- Duplicate draft positions.
- Missing draft positions.
- Executing selections outside the active order.

---

# Invariants

- Every draft position exists exactly once.
- Every scheduled selection occurs in sequence.
- Draft Order determines timing, not ownership.

---

# Canonical Principles

Draft Order defines sequence.

Draft Picks define ownership.

The Rookie Draft consumes both.

---

# Related Documents

- Chapter 17 — Rookie Draft
- Chapter 18 — Draft Picks
