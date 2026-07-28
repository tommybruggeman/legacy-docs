---
title: Roster Management
document: Rulebook
chapter: 7
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 04-Franchise-Management.md
  - 05-Contracts.md
  - 06-Salary-Cap.md
  - 09-Taxi-Squad.md
  - 10-Injured-Reserve.md
---

# Chapter 7 — Roster Management

## Purpose

Roster Management defines how players are organized within a franchise and establishes the rules that determine whether a franchise maintains a legal roster.

A roster is more than a collection of players. It is a structured organization of assets, each occupying a defined roster designation governed by league rules.

The platform continuously evaluates roster legality to ensure every franchise complies with league requirements.

---

# Business Rules

## Rule 7.1 — Franchise Roster

Every franchise maintains one official roster.

All players under franchise control must occupy a valid roster designation.

A player may not exist outside the roster while remaining under contract.

---

## Rule 7.2 — One Designation Per Player

A player may occupy only one roster designation at any point in time.

Examples include:

- Active Roster
- Taxi Squad
- Injured Reserve

A player cannot simultaneously belong to multiple designations.

---

## Rule 7.3 — Roster Designations

Legacy supports multiple roster designations.

Each designation may define its own:

- Eligibility requirements
- Salary cap treatment
- Contract behavior
- Transaction restrictions
- Promotion and demotion rules

The platform shall evaluate players according to the rules of their current designation.

---

## Rule 7.4 — League Configuration

Commissioners configure roster limits during league setup.

Examples include:

- Active roster size
- Taxi squad size
- Injured reserve capacity

Roster limits apply equally to every franchise unless explicitly modified by league rules.

---

## Rule 7.5 — Continuous Validation

Roster legality shall be evaluated whenever a roster-changing event occurs.

Examples include:

- Trades
- Draft selections
- Free-agent signings
- Player releases
- Taxi promotions
- Injured reserve moves
- Commissioner adjustments

No completed transaction shall leave a franchise in an illegal roster state unless commissioner override is used.

---

# User Experience

Owners should immediately understand:

- Total roster size
- Available roster spots
- Active players
- Taxi players
- Injured Reserve players
- Pending roster violations

Roster status should be visible throughout the platform.

Illegal roster conditions should clearly explain what must be corrected.

---

# System Requirements

The platform shall:

- Maintain one canonical roster per franchise.
- Associate every rostered player with exactly one designation.
- Recalculate roster legality after every roster event.
- Support configurable roster limits.
- Support future roster designations without redesign.

---

# Validation Rules

The platform shall reject:

- Players assigned to multiple roster designations.
- Players without a valid designation.
- Transactions exceeding configured roster limits.
- Duplicate roster entries.

Validation shall occur before transactions are finalized.

---

# Edge Cases

## Commissioner Adjustments

Commissioners may manually modify roster assignments when correcting administrative errors.

Every adjustment shall be recorded in the audit history.

---

## Future Designations

Future versions of Legacy may introduce additional roster designations.

Examples include:

- Practice Squad
- Reserve Lists
- Development Squad

The roster model shall support new designations without altering existing roster structures.

---

# Design Notes

### Why Roster Designations?

Professional franchises manage players differently depending on their role within the organization.

Legacy mirrors this by treating Active Roster, Taxi Squad, and Injured Reserve as distinct operational states rather than simply labels.

This approach enables each designation to enforce its own financial, contractual, and eligibility rules while maintaining a consistent underlying roster model.

---

### Canonical Principle

Every player belongs to one franchise.

Every rostered player occupies one designation.

Every designation has deterministic rules.

---

# Related Documents

- Chapter 4 — Franchise Management
- Chapter 5 — Player Contracts
- Chapter 6 — Salary Cap
- Chapter 9 — Taxi Squad
- Chapter 10 — Injured Reserve
