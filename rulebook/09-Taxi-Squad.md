---
title: Taxi Squad
document: Rulebook
chapter: 9
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 07-Roster-Management.md
  - 08-Rookie-Contracts.md
  - 10-Injured-Reserve.md
  - 15-Annual-Rollover.md
---

# Chapter 9 — Taxi Squad

## Purpose

The Taxi Squad provides a dedicated roster designation for eligible developmental players.

The Taxi Squad allows franchises to retain long-term assets while applying league-specific roster and financial rules that differ from the Active Roster.

The Taxi Squad is a roster designation, not a contract type. Assigning a player to the Taxi Squad does not create, modify, extend, or replace the player's contract.

---

# System Ownership

This chapter governs:

- Taxi Squad eligibility
- Taxi Squad occupancy
- Taxi Squad movement
- Taxi Squad validation
- Taxi Squad salary cap treatment

This chapter does not define:

- Contract creation
- Contract expiration
- Player ownership
- Salary cap calculations
- Annual rollover

Those systems are governed by their respective chapters.

---

# Business Rules

## Rule 9.1 — Eligibility

Only players satisfying the league's configured Taxi Squad eligibility requirements may occupy a Taxi Squad designation.

Eligibility requirements are league configuration and may include:

- Rookie status
- Years of NFL experience
- Seasons under contract
- Draft status
- Commissioner-defined rules

Legacy shall evaluate eligibility before every Taxi Squad assignment.

---

## Rule 9.2 — Occupancy

Each Taxi Squad position may contain one player.

A player may occupy only one roster designation at any time.

Assigning a player to the Taxi Squad automatically removes that player from any previous roster designation.

---

## Rule 9.3 — Promotion

A Taxi Squad player may be promoted to the Active Roster.

Promotion immediately subjects the player to all Active Roster rules.

Promotion does not modify:

- Contract
- Salary
- Remaining Years
- Acquisition Method

Only the roster designation changes.

---

## Rule 9.4 — Demotion

Whether an Active Roster player may return to the Taxi Squad is determined by league configuration.

If reverse movement is prohibited, the platform shall permanently reject future Taxi Squad assignments for that player during the current eligibility period.

---

## Rule 9.5 — Salary Treatment

Taxi Squad players may receive alternative salary cap treatment.

Examples include:

- Full salary counts
- Reduced salary counts
- No salary counts

Salary treatment is determined by league configuration.

The Salary Cap engine remains the canonical authority for financial calculations.

---

## Rule 9.6 — Contract Progression

Taxi Squad designation shall not pause or alter contract progression.

During Annual Rollover:

- Remaining Years decrease.
- Contract expiration is evaluated.
- Salary progression follows league rules.

Roster designation shall never modify contract progression.

---

# State Transitions

The Taxi Squad supports the following transitions.

```text
Not Eligible
      │
      ▼
 Eligible
      │
      ▼
 Active Roster
      │
      ├──────────────┐
      ▼              │
 Taxi Squad          │
      │              │
      ▼              │
 Active Roster ◄─────┘
```

Additional transitions may be restricted by league configuration.

---

# Validation Rules

The platform shall reject:

- Ineligible Taxi Squad assignments.
- Taxi Squad assignments exceeding configured capacity.
- Duplicate Taxi Squad occupancy.
- Players occupying multiple roster designations.
- Transactions violating configured Taxi Squad movement rules.

Validation occurs before roster changes are committed.

---

# Invariants

The following conditions must always remain true:

- Every Taxi Squad player belongs to exactly one franchise.
- Every Taxi Squad player has exactly one active contract.
- Every Taxi Squad player occupies exactly one roster designation.
- Every Taxi Squad assignment satisfies current league eligibility rules.
- Taxi Squad designation never changes player ownership.

---

# Edge Cases

## League Rule Changes

If Taxi Squad eligibility changes between seasons, the platform shall validate all existing Taxi Squad players during Annual Rollover.

Commissioners may be required to resolve newly invalid assignments.

---

## Manual Commissioner Actions

Commissioners may override Taxi Squad validation when correcting league administration issues.

Every override shall be recorded in the league audit log.

---

# Future Considerations

Future versions of Legacy may support:

- Multiple Taxi Squad tiers
- Protected developmental players
- Automatic promotions
- Taxi Squad poaching
- Conditional eligibility windows

These features shall extend the Taxi Squad model without changing its core responsibilities.

---

# Canonical Principles

A Taxi Squad is a roster designation.

A Taxi Squad is not a contract.

A Taxi Squad is not player ownership.

A Taxi Squad modifies roster state while preserving contract state.

---

# Related Documents

- Chapter 7 — Roster Management
- Chapter 8 — Rookie Contracts
- Chapter 10 — Injured Reserve
- Chapter 15 — Annual Rollover
