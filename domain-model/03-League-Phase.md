---
title: League Phase
document: Domain Model
entity: League Phase
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 02-League-Season.md
---

# League Phase

## Purpose

A League Phase defines the operational state of an active League Season.

League Phases determine which commands are legal at any point in time.

A League Season contains exactly one active League Phase.

---

# Canonical Identity

A League Phase is uniquely identified by:

- League Identifier
- Season
- Phase Type

---

# Owned State

A League Phase owns:

- Phase type
- Activation timestamp
- Completion timestamp
- Command permissions

---

# Relationships

Belongs to one League Season.

Controls command availability across the platform.

---

# Lifecycle

Scheduled

↓

Active

↓

Completed

---

# Commands

- Activate Phase
- Complete Phase

---

# Emitted Events

- LeaguePhaseActivated
- LeaguePhaseCompleted

---

# Consumed Events

- SeasonStarted
- SeasonCompleted

---

# Validation Rules

Reject:

- Multiple active phases.
- Invalid phase transitions.
- Commands prohibited by the current phase.

---

# Invariants

- Exactly one active phase.
- Phase transitions are deterministic.
- Commands obey active phase restrictions.

---

# Historical Requirements

Every phase transition shall be permanently recorded.

---

# AI Interpretation

AI shall evaluate transaction legality using the active League Phase before evaluating any other business rules.

Phase restrictions take precedence over transaction-specific logic.
