
---
title: Franchise Management
document: Rulebook
chapter: 4
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 02-League-Identity.md
  - 03-League-Lifecycle.md
  - 05-Contracts.md
  - 06-Salary-Cap.md
---

# Chapter 4 — Franchise Management

## Purpose

A franchise is the permanent competitive entity controlled by an owner within a Legacy League.

While owners, players, contracts, and competitive fortunes may change over time, the franchise itself persists for the lifetime of the league.

Franchises serve as the primary container for roster assets, financial obligations, historical achievements, and competitive identity.

---

# Business Rules

## Rule 4.1 — Permanent Identity

Every franchise shall possess a permanent identity within its league.

A franchise is created when the league is initialized and remains active until the league is archived or dissolved.

Franchises are never recreated during annual rollover.

---

## Rule 4.2 — Ownership

Each franchise is assigned one primary owner.

A commissioner may transfer ownership between users without affecting the franchise's historical identity.

Ownership changes do not reset:

- Championship history
- Transaction history
- Draft history
- Contract history
- Financial history
- Franchise records

---

## Rule 4.3 — Franchise Assets

Every asset belongs to exactly one franchise unless explicitly designated as a league asset.

Examples include:

- Active players
- Rookie rights
- Future draft selections
- Salary cap space
- Dead cap obligations
- Contract rights
- FAAB balance (if enabled)

Assets may only change franchises through league-approved transactions.

---

## Rule 4.4 — Franchise Responsibilities

Each franchise is responsible for maintaining a legal roster under current league rules.

Responsibilities include:

- Remaining under the salary cap
- Maintaining legal roster sizes
- Complying with contract rules
- Managing draft selections
- Meeting lineup requirements
- Satisfying future financial obligations

The platform shall continuously evaluate franchise compliance.

---

# User Experience

Owners should think of themselves as general managers of a professional franchise rather than managers of a seasonal fantasy roster.

The platform should reinforce long-term planning by preserving every meaningful decision throughout the life of the league.

When ownership changes, the incoming owner inherits the complete competitive and financial state of the franchise.

---

# System Requirements

The platform shall:

- Maintain one canonical franchise record per team.
- Associate every roster asset with a franchise.
- Preserve historical ownership changes.
- Preserve all completed transactions.
- Allow commissioners to transfer ownership without recreating data.
- Maintain franchise identity across all seasons.

---

# Validation Rules

The platform shall reject:

- Assets assigned to multiple franchises.
- Transactions involving unknown franchises.
- Duplicate franchise identifiers within the same league.
- Ownership transfers that would orphan franchise records.

---

# Edge Cases

## Expansion

Future platform versions may support league expansion by creating additional franchises without modifying existing franchise identities.

## Replacement Owners

If an owner leaves the league, the franchise remains active and may be reassigned to a new owner without affecting league history.

## Temporary Vacancies

A franchise may exist without an assigned owner for a limited period while awaiting replacement.

League operations should continue unless commissioner intervention is required.

---

# Future Considerations

Potential enhancements include:

- Multiple co-owners
- Franchise branding customization
- Franchise valuations
- Hall of Fame tracking
- Lifetime franchise statistics
- Achievement systems

These additions should extend the franchise model without altering its core identity.

---

# Design Notes

### Why franchises instead of owners?

Owners come and go.

Franchises create continuity.

This mirrors professional sports, where organizations persist even as management, coaches, and players change.

By anchoring history to the franchise instead of the owner, Legacy preserves the narrative of the league across decades of play.

---

# Related Documents

- Chapter 2 — League Identity
- Chapter 3 — League Lifecycle
- Chapter 5 — Contracts
- Chapter 6 — Salary Cap
