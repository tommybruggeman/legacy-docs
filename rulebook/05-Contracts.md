
---
title: Player Contracts
document: Rulebook
chapter: 5
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 04-Franchise-Management.md
  - 06-Salary-Cap.md
  - 07-Rookie-Contracts.md
  - 08-Team-Options.md
---

# Chapter 5 — Player Contracts

## Purpose

Contracts transform players from simple roster assets into long-term financial commitments.

Within Legacy, every player under franchise control exists under a defined contractual agreement that governs salary, duration, roster eligibility, and future financial obligations.

Contracts introduce strategic decision-making by requiring owners to balance present competitiveness against future flexibility.

---

# Business Rules

## Rule 5.1 — Contract Requirement

Every player on a franchise roster must exist in exactly one of the following states:

- Under Contract
- Unsigned Free Agent
- Expired Contract (awaiting processing)
- Waiver Claim (pending)

No rostered player may exist without a valid contractual status.

---

## Rule 5.2 — Contract Ownership

Each contract belongs to one franchise.

Contracts may transfer only through league-approved transactions such as:

- Trades
- Initial Rookie Draft
- Veteran Auction
- Commissioner Assignment

When a player is traded, the entire contract transfers with the player unless a league rule explicitly states otherwise.

---

## Rule 5.3 — Contract Components

Every contract contains the following required fields:

- Player
- Franchise
- Annual Salary
- Remaining Contract Years
- Contract Type
- Acquisition Method
- Effective Season
- Expiration Season
- Contract Status

These fields form the canonical contract record.

---

## Rule 5.4 — Remaining Years

Contract duration is measured as **remaining seasons**, not calendar years.

At each annual rollover:

- Remaining Years decrease by one.
- Contracts reaching zero years expire.
- Expired contracts are processed according to league rules.

Contract length never changes during an active season unless modified by an approved transaction.

---

## Rule 5.5 — Salary Commitment

The full salary of every active contract counts against the franchise salary cap unless modified by another rule (such as Injured Reserve or Taxi Squad).

Salary obligations remain attached to the contract until the contract expires or is terminated.

---

## Rule 5.6 — Contract Status

Every contract shall have one current status.

Examples include:

- Active
- Taxi Squad
- Injured Reserve
- Suspended
- Expired
- Released
- Dead Cap
- Extension Pending

A contract may only have one primary status at a time.

---

## Rule 5.7 — Historical Preservation

Expired, released, and completed contracts remain part of permanent league history.

Historical contracts shall never be permanently deleted.

---

# User Experience

Owners should immediately understand:

- How much a player costs.
- How many years remain.
- When the contract expires.
- The future financial impact.
- Any upcoming contract decisions.

Contract information should be consistently visible throughout the platform.

---

# System Requirements

The platform shall:

- Maintain one canonical contract per rostered player.
- Preserve every historical version of a contract.
- Prevent duplicate active contracts.
- Associate contracts with franchise history.
- Support future contract extensions.
- Support future contract restructuring.

---

# Validation Rules

The platform shall reject:

- Contracts without salaries.
- Contracts without remaining years.
- Negative contract lengths.
- Multiple active contracts for the same player within the same league.
- Contracts assigned to nonexistent franchises.

---

# Edge Cases

## Mid-Season Acquisition

Players acquired during the season retain the existing contract unless league rules specify otherwise.

---

## Contract Correction

Commissioners may manually correct administrative errors.

Every correction shall be recorded in the historical audit log.

---

## League Rule Changes

Changes to contract rules affect future contracts unless explicitly stated otherwise.

Historical contracts should preserve the rules under which they were created whenever practical.

---

# Future Considerations

Future versions may introduce:

- Contract restructuring
- Guaranteed salary
- Performance incentives
- Player options
- Franchise tags
- Buyouts
- Multi-year extensions
- Contract renegotiation windows

These features should build upon the contract model rather than replace it.

---

# Design Notes

### Why Contracts?

Professional franchises are built around financial commitments rather than player ownership alone.

Contracts introduce opportunity cost.

Every long-term commitment reduces future flexibility.

This creates meaningful strategic choices and distinguishes Legacy from traditional fantasy football platforms.

---

### Canonical Principle

Players are temporary.

Contracts are deterministic.

Franchises are permanent.

Every roster decision should respect that relationship.

---

# Related Documents

- Chapter 4 — Franchise Management
- Chapter 6 — Salary Cap
- Chapter 7 — Rookie Contracts
- Chapter 8 — Team Options
