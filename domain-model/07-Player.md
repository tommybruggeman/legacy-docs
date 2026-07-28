---
title: Player
document: Domain Model
entity: Player
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 01-League.md
---

# Player

## Purpose

A Player represents a real-world athlete that may participate within one or more Legacy Leagues.

Players are global entities.

Players exist independently of Leagues, Franchises, Contracts, and ownership.

A Player may simultaneously exist within many independent Legacy Leagues.

---

# Canonical Identity

Every Player shall possess one immutable canonical identifier.

External identifiers (Sleeper, ESPN, NFL, etc.) are aliases and shall never replace the canonical identifier.

---

# Owned State

A Player owns:

- Identity
- Name
- Position
- Team
- External identifiers
- Metadata

A Player does not own:

- Contracts
- Franchises
- Roster assignments
- Draft status

---

# Relationships

A Player may participate in many Leagues.

A Player may have many historical Contracts.

A Player may have one active Contract per League.

---

# Lifecycle

Created

↓

Active

↓

Retired

↓

Historical

---

# Commands

- Create Player
- Update Metadata
- Retire Player
- Merge Duplicate Player

---

# Emitted Events

- PlayerCreated
- PlayerUpdated
- PlayerRetired

---

# Consumed Events

- ExternalPlayerSync

---

# Validation Rules

Reject:

- Duplicate canonical identities.
- Invalid player metadata.

---

# Invariants

- Every Player has one canonical identity.
- A Player exists independently of ownership.
- Retirement does not remove historical records.

---

# Historical Requirements

Player identity shall persist permanently regardless of retirement or league participation.

---

# AI Interpretation

AI shall treat the Player as the real-world individual.

Ownership, contracts, and roster assignments are independent entities.
