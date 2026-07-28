
---
title: League Lifecycle
document: Rulebook
chapter: 3
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 02-League-Identity.md
  - 15-Annual-Rollover.md
---

# Chapter 3 — League Lifecycle

## Purpose

This chapter defines the lifecycle of a Legacy league from creation through indefinite operation.

Unlike traditional fantasy platforms, a Legacy league is never recreated at the conclusion of a season.

Instead, the league evolves continuously while preserving all historical information.

---

# Business Rules

## Rule 3.1

A league exists until it is explicitly archived or deleted by platform administration.

Advancing seasons never creates a new league.

---

## Rule 3.2

League identity never changes.

The following remain constant:

- League ID
- Creation Date
- Historical Records

The following may change:

- Commissioner
- League Name
- League Logo
- Settings

---

## Rule 3.3

Every season belongs to one league.

A season cannot exist independently.

---

# Lifecycle

A Legacy League progresses through the following states.

## Stage 1

League Creation

Actions

• Commissioner creates league

• Initial settings established

• League ID assigned

Outputs

• League record

• Default rule set

• Empty franchises

---

## Stage 2

League Configuration

Commissioner establishes:

- Salary Cap
- Rookie Wage Scale
- League Logo
- Commissioner
- Co-Commissioner

Outputs

Configured League

---

## Stage 3

Franchise Assignment

Owners join the league.

Franchises become associated with users.

Outputs

League Ready

---

## Stage 4

Active Competition

League operations include:

- Drafts
- Trades
- Waivers
- Contracts
- Salary Cap
- AI Assistance

Historical events are continuously recorded.

---

## Stage 5

Annual Advancement

Platform advances all active leagues simultaneously.

Annual advancement includes:

- Contract reduction
- Contract expiration
- Dead cap advancement
- Taxi reset
- IR reset
- Draft initialization
- Historical snapshot

The annual advancement process is defined in Chapter 15.

---

## Stage 6

Repeat

The lifecycle returns to Active Competition.

No new league is created.

History remains intact.

---

# User Experience

League members should perceive the league as a living organization.

Owners never "start over."

Instead they inherit:

- Contracts
- Assets
- History
- Financial obligations
- Championships
- Draft capital

Every offseason should feel like the continuation of an existing franchise.

---

# System Requirements

Legacy must:

✓ Preserve historical records indefinitely

✓ Associate every season with one league

✓ Prevent duplicate league creation during rollover

✓ Maintain league identifiers

✓ Preserve franchise continuity

---

# Validation Rules

The platform shall reject:

• Seasons without a league

• Franchises without a league

• Duplicate active league identifiers

• Historical records not associated with a league

---

# Edge Cases

Changing commissioners does not create a new league.

Changing league names does not create a new league.

Changing logos does not create a new league.

Changing salary caps does not create a new league.

---

# Future Considerations

Future versions may support:

- League mergers
- League cloning
- Expansion teams
- Contraction
- Multi-division structures

These additions should extend—not replace—the lifecycle defined here.

---

# Related Documents

Chapter 2 — League Identity

Chapter 15 — Annual Rollover

ADR-001 — League Persistence

Data Dictionary — League
