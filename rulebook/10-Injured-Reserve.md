---
title: Injured Reserve
document: Rulebook
chapter: 10
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 07-Roster-Management.md
  - 06-Salary-Cap.md
  - 09-Taxi-Squad.md
  - 15-Annual-Rollover.md
---

# Chapter 10 — Injured Reserve

## Purpose

The Injured Reserve (IR) designation provides a mechanism for franchises to retain players who satisfy league-defined injury eligibility requirements while applying league-specific roster and financial rules.

Injured Reserve is a roster designation only.

Assigning a player to Injured Reserve does not modify player ownership, contract ownership, salary, or contract duration.

---

# System Ownership

This chapter governs:

- Injured Reserve eligibility
- Injured Reserve occupancy
- Injured Reserve movement
- Injured Reserve validation
- Injured Reserve roster behavior

This chapter does not govern:

- Contract creation
- Contract expiration
- Salary cap calculations
- Player ownership
- League injury reporting

---

# Business Rules

## Rule 10.1 — Eligibility

Only players satisfying the league's configured Injured Reserve eligibility requirements may occupy an Injured Reserve designation.

Eligibility is determined by league configuration.

Examples include:

- NFL Injured Reserve designation
- PUP designation
- Out designation
- Commissioner-approved injury status

The platform shall validate eligibility before every assignment.

---

## Rule 10.2 — Occupancy

Each Injured Reserve position may contain one player.

A player may occupy only one roster designation at any time.

Moving a player to Injured Reserve automatically removes the player from the previous roster designation.

---

## Rule 10.3 — Activation

Players may be activated from Injured Reserve once they satisfy league activation requirements.

Activation moves the player to the Active Roster.

Activation does not modify:

- Contract
- Salary
- Remaining Years
- Acquisition Method

Only the roster designation changes.

---

## Rule 10.4 — Loss of Eligibility

If a player occupying Injured Reserve no longer satisfies league eligibility requirements, the platform shall identify the roster as requiring correction.

Commissioners or franchise owners must restore the roster to a legal state before completing restricted roster transactions.

Automatic movement shall never occur unless explicitly enabled by league configuration.

---

## Rule 10.5 — Salary Treatment

Injured Reserve salary treatment is determined by league configuration.

Possible implementations include:

- Full salary counts
- Partial salary counts
- Salary exemption

The Salary Cap engine remains the canonical authority for all financial calculations.

---

## Rule 10.6 — Contract Progression

Injured Reserve designation shall not pause or alter contract progression.

Contracts continue to:

- Decrease Remaining Years during Annual Rollover.
- Expire according to contract rules.
- Participate in salary calculations according to league configuration.

---

# State Transitions

The Injured Reserve designation supports the following transitions.

```text
Eligible
    │
    ▼
Active Roster
    │
    ▼
Injured Reserve
    │
    ▼
Active Roster
```

League configuration may further restrict activation or assignment timing.

---

# Validation Rules

The platform shall reject:

- Ineligible Injured Reserve assignments.
- Assignments exceeding configured IR capacity.
- Duplicate IR occupancy.
- Players assigned to multiple roster designations.
- Transactions violating configured Injured Reserve rules.

Validation occurs before roster changes are committed.

---

# Invariants

The following conditions must always remain true:

- Every Injured Reserve player belongs to exactly one franchise.
- Every Injured Reserve player has exactly one active contract.
- Every Injured Reserve player occupies exactly one roster designation.
- Every Injured Reserve assignment satisfies league eligibility rules.
- Injured Reserve designation never changes player ownership.

---

# Edge Cases

## League Rule Changes

If league eligibility rules change during the offseason, the platform shall revalidate every Injured Reserve player during Annual Rollover.

---

## Commissioner Overrides

Commissioners may override eligibility restrictions when resolving league administration issues.

Every override shall be recorded in the league audit log.

---

# Future Considerations

Future versions may support:

- Multiple Injured Reserve categories
- Temporary reserve lists
- Automatic eligibility synchronization
- Conditional activation windows
- Injury recovery tracking

These enhancements shall extend the Injured Reserve model without altering its deterministic behavior.

---

# Canonical Principles

Injured Reserve is a roster designation.

Injured Reserve does not modify contracts.

Injured Reserve does not modify ownership.

Injured Reserve modifies roster state while preserving contract state.

---

# Related Documents

- Chapter 6 — Salary Cap
- Chapter 7 — Roster Management
- Chapter 9 — Taxi Squad
- Chapter 15 — Annual Rollover
