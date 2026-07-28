---
title: Rookie Draft
document: Rulebook
chapter: 17
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 08-Rookie-Contracts.md
  - 18-Draft-Picks.md
---

# Chapter 17 — Rookie Draft

## Purpose

The Rookie Draft is the exclusive mechanism for introducing newly eligible players into a Legacy League.

The Rookie Draft determines initial franchise ownership of rookies and initiates the creation of rookie contracts.

The Rookie Draft is an event.

It is not responsible for managing contracts after player selection.

---

# System Ownership

This chapter governs:

- Draft execution
- Player selection
- Pick consumption
- Initial ownership assignment

This chapter does not govern:

- Draft pick ownership
- Rookie contract rules
- Salary calculations

---

# Business Rules

## Rule 17.1 — Draft Eligibility

Only players designated as draft eligible may be selected.

Each player may be selected exactly once.

---

## Rule 17.2 — Draft Order

Selections occur according to the league's official draft order.

No franchise may draft outside its assigned selection unless ownership of the pick has changed.

---

## Rule 17.3 — Selection

A completed selection shall:

- Assign initial player ownership.
- Consume the draft selection.
- Generate a rookie contract.
- Record draft history.

---

## Rule 17.4 — Completion

The Rookie Draft concludes after every scheduled draft selection has either been exercised or forfeited according to league rules.

---

# State Transition

```text
Draft Eligible
      │
Selected
      │
Ownership Assigned
      │
Rookie Contract Created
```

---

# Validation Rules

The platform shall reject:

- Duplicate player selections.
- Invalid draft selections.
- Drafting an already-owned player.
- Selecting outside the active draft order.

---

# Invariants

- Every drafted player is selected once.
- Every completed selection consumes exactly one draft pick.
- Every drafted player receives exactly one rookie contract.

---

# Canonical Principles

The Rookie Draft creates ownership.

The Rookie Draft consumes draft picks.

The Rookie Draft creates rookie contracts.

---

# Related Documents

- Chapter 8 — Rookie Contracts
- Chapter 18 — Draft Picks
