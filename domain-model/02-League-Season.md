---
title: League Season
document: Domain Model
entity: League Season
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 01-League.md
---

# League Season

## Purpose

A League Season represents one complete competitive cycle within a League.

Seasons partition league history into chronological periods while preserving permanent league identity.

---

# Canonical Identity

A League Season is uniquely identified by:

- League Identifier
- Season Year

No League may contain duplicate Season Years.

---

# Owned State

A League Season owns:

- Season year
- Season status
- Active phase
- Draft
- Standings
- Matchups
- Championship outcome

---

# Relationships

Belongs to one League.

Contains one or more League Phases.

Contains many Transactions.

Contains one Rookie Draft.

---

# Lifecycle

Scheduled

↓

Active

↓

Completed

↓

Historical

---

# Commands

- Start Season
- Complete Season
- Advance Phase

---

# Emitted Events

- SeasonStarted
- SeasonCompleted
- PhaseChanged

---

# Consumed Events

- AnnualRolloverCompleted

---

# Validation Rules

Reject:

- Multiple active seasons.
- Duplicate season years.
- Invalid chronological ordering.

---

# Invariants

- One active season per league.
- Completed seasons are immutable.
- Historical standings remain permanent.

---

# Historical Requirements

Completed seasons shall never be modified except through attributable administrative correction.

---

# AI Interpretation

AI shall interpret every recommendation within the active League Season unless historical analysis is explicitly requested.
