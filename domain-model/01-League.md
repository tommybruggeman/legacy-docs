---
title: League
document: Domain Model
entity: League
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - README.md
---

# League

## Purpose

A League is the highest-level domain entity within Legacy.

A League defines the permanent competitive environment in which franchises, players, contracts, transactions, and seasons exist.

A League is intended to persist indefinitely.

League identity survives ownership changes, seasonal transitions, configuration updates, and software upgrades.

---

# Canonical Identity

Every League shall possess one immutable canonical identifier.

Display names may change.

Canonical identity shall not.

---

# Owned State

The League owns:

- League identity
- League configuration
- Franchise membership
- Historical records
- Seasonal progression
- Governance rules

The League does not own players, contracts, or franchises directly.

---

# Relationships

A League contains many:

- Seasons
- Franchises
- Members
- Drafts
- Transactions
- Audit Events

A League references one active League Season.

---

# Lifecycle

Created

↓

Configured

↓

Active

↓

Archived (optional)

League deletion is prohibited once meaningful history exists.

---

# Commands

- Create League
- Update League Settings
- Archive League
- Advance Season

---

# Emitted Events

- LeagueCreated
- LeagueConfigured
- LeagueArchived
- SeasonAdvanced

---

# Consumed Events

- FranchiseJoined
- SeasonCompleted

---

# Validation Rules

The platform shall reject:

- Duplicate canonical identifiers
- Invalid league configurations
- Multiple active seasons

---

# Invariants

- Every League has one canonical identifier.
- Every League has exactly one active season.
- Every League owns its complete history.
- League history is immutable.

---

# Historical Requirements

League history shall never be deleted.

Administrative corrections create new historical records.

---

# AI Interpretation

AI shall treat the League as the root scope for all reasoning.

All recommendations, evaluations, and rule interpretation occur within the context of exactly one League.
