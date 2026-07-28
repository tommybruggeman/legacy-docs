---
title: Franchise
document: Domain Model
entity: Franchise
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 01-League.md
---

# Franchise

## Purpose

A Franchise is the permanent competitive entity within a League.

Franchises own contracts, draft picks, roster assignments, financial obligations, and competitive history.

A Franchise persists across seasons regardless of changes in ownership.

---

# Canonical Identity

Every Franchise shall possess one immutable canonical identifier.

Team names, owner names, logos, abbreviations, and branding may change without affecting franchise identity.

---

# Owned State

A Franchise owns:

- Active roster
- Contracts
- Draft Picks
- Dead Cap obligations
- Team Options
- Transaction history
- Competitive history

A Franchise does not own Users.

---

# Relationships

A Franchise belongs to one League.

A Franchise may have many League Members over time.

A Franchise owns many Contracts.

A Franchise owns many Draft Picks.

---

# Lifecycle

Created

↓

Configured

↓

Active

↓

Archived (optional)

Franchise deletion is prohibited once meaningful league history exists.

---

# Commands

- Create Franchise
- Rename Franchise
- Transfer Franchise Management
- Archive Franchise

---

# Emitted Events

- FranchiseCreated
- FranchiseRenamed
- FranchiseTransferred
- FranchiseArchived

---

# Consumed Events

- LeagueCreated
- LeagueMemberAssigned

---

# Validation Rules

Reject:

- Duplicate Franchise identifiers.
- Multiple active Franchises with the same canonical identity.
- Deletion of historical Franchises.

---

# Invariants

- Every Franchise belongs to exactly one League.
- Every Franchise owns its historical assets.
- Franchise identity survives owner changes.
- Historical records remain immutable.

---

# Historical Requirements

Ownership changes shall not alter franchise identity.

Historical championships, transactions, and records remain attached to the Franchise.

---

# AI Interpretation

AI shall evaluate the Franchise as the primary competitive entity.

Owner identity is contextual.

Franchise identity is canonical.
