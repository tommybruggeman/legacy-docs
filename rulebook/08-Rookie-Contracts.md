---
title: Rookie Contracts
document: Rulebook
chapter: 8
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 05-Contracts.md
  - 07-Roster-Management.md
  - 20-Rookie-Draft.md
---

# Chapter 8 — Rookie Contracts

## Purpose

This chapter defines how newly drafted players enter the league's contract system.

A rookie contract is the initial contractual agreement between a franchise and a drafted player. Rookie contracts establish a player's first salary, contract duration, acquisition method, and eligibility for future contract actions.

Rookie contracts are deterministic. Every drafted player follows the same lifecycle unless modified by explicit league configuration.

---

# Business Rules

## Rule 8.1 — Contract Creation

A rookie contract is created immediately after a drafted player is successfully assigned to a franchise.

The Rookie Draft is the canonical source for rookie contract creation.

No rookie contract may exist without an associated draft selection unless explicitly created through commissioner action.

---

## Rule 8.2 — Initial Contract Values

Every rookie contract shall contain:

- Player Identifier
- Franchise Identifier
- Draft Selection
- Draft Season
- Initial Salary
- Initial Contract Length
- Acquisition Method
- Effective Season
- Contract Status

League configuration determines salary and contract length.

The contract structure remains identical for every rookie.

---

## Rule 8.3 — Rookie Wage Scale

Legacy supports configurable rookie wage systems.

Examples include:

- Fixed salary by draft round
- Fixed salary by draft pick
- Fully custom salary schedules
- Commissioner-defined contracts

The wage calculation method is a league configuration rather than a platform rule.

---

## Rule 8.4 — Contract Lifecycle

Once created, rookie contracts behave identically to veteran contracts unless another chapter defines an exception.

Rookie contracts:

- Count against the salary cap.
- Advance during annual rollover.
- May expire.
- May be traded.
- May be released.
- May transition to Dead Cap.
- May become eligible for extension or team option according to league rules.

---

## Rule 8.5 — Ownership

The franchise drafting the player owns the rookie contract immediately upon successful draft completion.

If draft rights are traded before the selection, the receiving franchise becomes the initial contract owner.

---

# User Experience

Following every rookie selection, owners should immediately see:

- Player
- Salary
- Remaining Years
- Contract Status
- Contract Expiration

The platform should require no manual contract entry following a successful draft.

---

# System Requirements

The platform shall:

- Automatically generate rookie contracts.
- Prevent duplicate rookie contracts.
- Associate rookie contracts with draft history.
- Preserve the acquisition method.
- Preserve rookie contract history permanently.

---

# Validation Rules

The platform shall reject:

- Rookie contracts without draft provenance.
- Duplicate rookie contracts.
- Invalid rookie salary values.
- Invalid rookie contract lengths.
- Rookie contracts assigned to nonexistent franchises.

---

# Edge Cases

## Commissioner Corrections

Commissioners may modify rookie contracts to correct administrative errors.

Every correction shall be recorded in the league audit log.

---

## Supplemental Drafts

Future platform versions may support additional draft events.

Each draft event should generate contracts using the same deterministic contract creation process.

---

# Future Considerations

Potential enhancements include:

- Slot-based guarantees
- Rookie signing bonuses
- Holdout mechanics
- Draft compensation
- International player exemptions

These additions should extend the rookie contract model without altering its deterministic lifecycle.

---

# Design Notes

### Why Separate Rookie Contracts?

Although rookie contracts become standard player contracts after creation, their origin differs.

Separating the creation process from the ongoing contract lifecycle allows the platform to support multiple draft formats and league configurations while maintaining a single canonical contract model.

---

### Canonical Principle

The Rookie Draft creates rookie contracts.

Contracts govern players.

The Contract system—not the Draft system—owns every player after creation.

---

# Related Documents

- Chapter 5 — Player Contracts
- Chapter 7 — Roster Management
- Chapter 20 — Rookie Draft
