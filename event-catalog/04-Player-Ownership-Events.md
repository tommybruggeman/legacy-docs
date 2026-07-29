---
title: Player Ownership Events
document: Event Catalog
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - README.md
  - ../domain-model/07-Player.md
  - ../domain-model/09-Roster-Assignment.md
  - ../domain-model/04-Franchise.md
---

# Player Ownership Events

## Purpose

This chapter defines the canonical events that govern player ownership and roster placement throughout the lifecycle of a fantasy football League.

Unlike the Player entity, which represents a real-world athlete, player ownership is entirely League-specific.

A player may exist independently of every League while simultaneously having different ownership, roster designations, and eligibility across many different Leagues.

This chapter documents the events that change those League-specific relationships.

---

# Guiding Principle

A Player is never owned directly.

A Franchise owns a **Roster Assignment**.

The Roster Assignment references the Player.

Ownership changes by changing Roster Assignments—not Players.

---

# Ownership Model

```text
Player
    │
    │ referenced by
    ▼
Roster Assignment
    │
belongs to
    ▼
Franchise
    │
belongs to
    ▼
League
```

The Player remains globally unique.

Ownership exists only through a Roster Assignment.

---

# Roster Designation Model

Roster Assignments may exist in one of several designations.

```text
Active

Bench

Taxi

Injured Reserve

Suspended

Practice Squad

Free Agent
```

Changing designation changes legality.

It does **not** change ownership.

---

# Scope

This chapter defines:

- PlayerRegistered
- PlayerMetadataUpdated
- PlayerRetired

- RosterAssignmentCreated
- RosterAssignmentTransferred
- RosterDesignationChanged
- RosterAssignmentRemoved

- PlayerActivated
- PlayerPlacedOnIR
- PlayerActivatedFromIR

- PlayerPlacedOnTaxi
- PlayerRemovedFromTaxi

- PlayerReleased

---

# General Invariants

Every ownership event shall satisfy:

1. A Player exists independently of ownership.
2. Every active Roster Assignment references exactly one Player.
3. A Player may have at most one active Roster Assignment within a League.
4. Different Leagues may simultaneously own the same Player.
5. Ownership changes never alter Player identity.
6. Designation changes never alter ownership.
7. Historical ownership is immutable.
8. Released Players become Free Agents only within that League.

---

# PlayerRegistered

Purpose

...

Trigger

...

Payload

...

Business Impact

• Adds Player to League player universe.

• Enables future ownership.

• Does not create ownership.

• Does not create a Contract.

AI Interpretation

...

---

# PlayerMetadataUpdated

...

---

# PlayerRetired

...

Business Impact

• Prevents future active acquisition.

• Existing Contracts remain historical.

• May alter roster legality.

• May alter cap calculations.

• AI projections must account for retirement.

---

# RosterAssignmentCreated

Purpose

Represents a Franchise acquiring ownership rights to a Player.

This is the canonical ownership event.

Business Impact

• Creates ownership.

• Places Player on roster.

• Enables Contracts.

• Enables transactions.

• Invalidates roster calculations.

• Invalidates positional depth.

• Invalidates team evaluations.

---

# RosterAssignmentTransferred

Purpose

Transfers ownership between Franchises.

Business Impact

• Removes ownership from one Franchise.

• Creates ownership for another.

• Invalidates both roster projections.

• Invalidates trade history.

• Invalidates league evaluations.

---

# RosterDesignationChanged

Purpose

Changes roster location without changing ownership.

Examples

Bench → IR

Taxi → Active

Active → Taxi

Business Impact

• Changes roster legality.

• Changes lineup eligibility.

• May affect waiver eligibility.

• Ownership unchanged.

---

# PlayerActivated

...

---

# PlayerPlacedOnIR

Business Impact

• Ownership unchanged.

• Designation changes.

• Roster calculations updated.

• Lineup legality updated.

---

# PlayerActivatedFromIR

...

---

# PlayerPlacedOnTaxi

...

---

# PlayerRemovedFromTaxi

...

---

# PlayerReleased

Purpose

Represents the Franchise voluntarily surrendering ownership.

Business Impact

• Removes ownership.

• Terminates roster assignment.

• Creates Free Agent.

• May terminate Contract.

• May create Dead Cap.

• Invalidates cap calculations.

• Invalidates roster calculations.

• Invalidates team evaluation.

---

# Workflow Examples

## Draft

```text
PlayerRegistered

↓

RosterAssignmentCreated

↓

ContractCreated
```

---

## Trade

```text
TradeExecuted

↓

RosterAssignmentTransferred
```

---

## Release

```text
PlayerReleased

↓

RosterAssignmentRemoved

↓

ContractTerminated

↓

DeadCapObligationCreated
```

---

## IR Workflow

```text
PlayerPlacedOnIR

↓

PlayerActivatedFromIR
```

---

## Taxi Workflow

```text
PlayerPlacedOnTaxi

↓

PlayerRemovedFromTaxi
```

---

# AI Interpretation

Ownership events provide the AI with authoritative evidence regarding:

- Current owner
- Historical owner
- Roster designation
- Roster legality
- Positional depth
- Available Free Agents
- Trade eligibility

The AI shall never infer ownership from:

- Team names
- Display names
- External usernames

Ownership shall always be resolved through active Roster Assignments.

---

# Validation Checklist

Before publishing ownership events verify:

✓ Player exists

✓ Franchise exists

✓ League exists

✓ Ownership rules satisfied

✓ Designation valid

✓ Previous ownership resolved

✓ No duplicate assignments

✓ Event ordering preserved

✓ Historical ownership maintained

---

# Final Principle

Players never move.

Ownership moves.

The Player remains constant throughout every League.

Roster Assignments define ownership.

Ownership defines competition.

Everything else—including Contracts, salary cap, trades, waivers, and AI evaluations—builds upon that foundation.
