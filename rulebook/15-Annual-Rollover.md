---
title: Annual Rollover
document: Rulebook
chapter: 15
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 05-Contracts.md
  - 06-Salary-Cap.md
  - 09-Taxi-Squad.md
  - 10-Injured-Reserve.md
  - 11-Dead-Cap.md
---

# Chapter 15 — Annual Rollover

## Purpose

Annual Rollover advances a Legacy League from one league year to the next.

It is the canonical event that progresses long-term league state.

No other system may independently advance league seasons.

---

# System Ownership

This chapter governs:

- League year advancement
- Contract progression
- Dead Cap progression
- Roster reset events
- Seasonal validation

---

# Business Rules

## Rule 15.1 — Season Advancement

Annual Rollover increments the active league season exactly once.

---

## Rule 15.2 — Contract Progression

Every active contract decreases Remaining Years by one.

Contracts reaching zero remaining years transition to expiration.

---

## Rule 15.3 — Dead Cap Progression

Remaining Dead Cap obligations decrease according to league rules.

Completed obligations expire.

Historical records remain preserved.

---

## Rule 15.4 — Roster Reset

League configuration may automatically reset:

- Taxi Squad assignments
- Injured Reserve assignments
- Temporary roster designations

Automatic resets shall be deterministic.

---

## Rule 15.5 — Validation

Following rollover the platform shall validate:

- Roster legality
- Salary cap compliance
- Contract state
- Team Options
- Future draft ownership

---

# State Transition

```text
Season N
     │
Annual Rollover
     │
Season N+1
```

---

# Invariants

- One rollover advances one season.
- Contracts advance exactly once.
- League history is preserved.
- Financial history is preserved.

---

# Canonical Principles

Annual Rollover is the only event that advances league time.

---

# Related Documents

- Chapter 5 — Contracts
- Chapter 6 — Salary Cap
- Chapter 11 — Dead Cap
