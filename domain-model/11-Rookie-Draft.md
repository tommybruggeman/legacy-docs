---
title: Rookie Draft
document: Domain Model
entity: Rookie Draft
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 10-Draft-Pick.md
---

# Rookie Draft

## Purpose

A Rookie Draft is a scheduled league event responsible for assigning draft-eligible players to franchises.

The Rookie Draft consumes Draft Picks and creates Rookie Contracts.

---

# Canonical Identity

A Rookie Draft is uniquely identified by:

- League
- Season

---

# Owned State

- Draft status
- Current selection
- Selection history
- Timer state
- Configuration

---

# Relationships

Contains many Draft Selections.

Consumes many Draft Picks.

Creates many Contracts.

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

- Start Draft
- Pause Draft
- Resume Draft
- Complete Draft

---

# Emitted Events

- RookieDraftStarted
- RookieSelected
- RookieDraftCompleted

---

# Invariants

- One Rookie Draft per League Season.
- Every Draft Pick is consumed at most once.
- Completed drafts are immutable.

---

# AI Interpretation

The Rookie Draft is the canonical origin of rookie ownership.
