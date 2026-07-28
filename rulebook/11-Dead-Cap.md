---
title: Dead Cap
document: Rulebook
chapter: 11
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 05-Contracts.md
  - 06-Salary-Cap.md
  - 10-Injured-Reserve.md
  - 15-Annual-Rollover.md
---

# Chapter 11 — Dead Cap

## Purpose

Dead Cap represents the remaining financial obligation incurred by a franchise after terminating a player's contract before its natural expiration.

Dead Cap preserves the financial consequences of long-term contractual commitments by ensuring that releasing a player does not immediately eliminate future salary obligations.

Dead Cap is a financial obligation.

Dead Cap is not a player.

Dead Cap is not a contract.

---

# System Ownership

This chapter governs:

- Dead Cap creation
- Dead Cap calculation
- Dead Cap progression
- Dead Cap expiration
- Dead Cap salary cap treatment

This chapter does not govern:

- Contract creation
- Contract extensions
- Player ownership
- Salary Cap calculations
- Player Releases

---

# Business Rules

## Rule 11.1 — Dead Cap Creation

Dead Cap may only be created by a player release or other league-defined contract termination event.

A franchise cannot manually create Dead Cap.

---

## Rule 11.2 — Source Contract

Every Dead Cap record shall reference exactly one historical player contract.

The original contract remains part of league history.

Dead Cap is a derived financial obligation created from that contract.

---

## Rule 11.3 — Dead Cap Amount

The amount of Dead Cap generated is determined by league configuration.

Possible calculation methods include:

- Remaining guaranteed salary
- Remaining contract percentage
- Fixed release penalty
- Commissioner-defined formula

The Dead Cap engine shall execute the configured calculation method deterministically.

---

## Rule 11.4 — Salary Cap Treatment

Dead Cap contributes to franchise salary cap usage according to league configuration.

The Salary Cap engine remains the canonical authority for all cap calculations.

---

## Rule 11.5 — Annual Progression

During Annual Rollover:

- Remaining Dead Cap duration decreases.
- Expired Dead Cap obligations are removed.
- Active obligations continue into the next league year.

Dead Cap progression shall follow the same annual lifecycle as other financial obligations unless overridden by league configuration.

---

## Rule 11.6 — Historical Preservation

Expired Dead Cap records shall remain permanently associated with:

- Original player
- Original contract
- Original franchise
- Original release event

Dead Cap history shall never be deleted.

---

# State Transitions

```text
Active Contract
       │
 Release Event
       │
       ▼
 Dead Cap Created
       │
Annual Rollover
       │
       ▼
Remaining Obligation
       │
       ▼
Obligation Complete
       │
       ▼
Historical Record
```

---

# Validation Rules

The platform shall reject:

- Dead Cap without a source contract.
- Duplicate Dead Cap records for the same release event.
- Negative Dead Cap obligations.
- Dead Cap assigned to nonexistent franchises.

Validation occurs before financial records are committed.

---

# Invariants

The following conditions must always remain true:

- Every Dead Cap record references one historical contract.
- Every Dead Cap record belongs to one franchise.
- Dead Cap never owns player rights.
- Dead Cap always contributes to financial history.
- Dead Cap calculations are deterministic.

---

# Edge Cases

## Rule Changes

Changes to Dead Cap formulas affect only future release events unless commissioners explicitly recalculate historical obligations.

---

## Commissioner Adjustments

Commissioners may manually adjust Dead Cap obligations.

Every adjustment shall be permanently recorded in the league audit history.

---

# Future Considerations

Future versions of Legacy may support:

- Guaranteed salary structures
- Partial guarantees
- Contract buyouts
- Post-June release rules
- Deferred cap obligations
- Salary retention

These enhancements shall extend the Dead Cap model while preserving deterministic financial accounting.

---

# Canonical Principles

Dead Cap is a financial obligation.

Dead Cap originates from a historical contract.

Dead Cap preserves the consequences of releasing players.

Dead Cap exists independently of player ownership.

---

# Related Documents

- Chapter 5 — Player Contracts
- Chapter 6 — Salary Cap
- Chapter 15 — Annual Rollover
