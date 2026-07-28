---
title: Player Evaluation
document: Domain Model
entity: Player Evaluation
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 07-Player.md
---

# Player Evaluation

## Purpose

A Player Evaluation represents the platform's assessment of a Player within the context of a specific League.

Evaluations combine league rules, contracts, roster construction, and player outlook into a single strategic assessment.

---

# Owned State

- Player
- Evaluation Score
- Tier
- Strategic Role
- Market Value
- Risk Assessment
- Confidence
- Timestamp

---

# Relationships

References one Player.

Belongs to one League.

May contribute to Team Evaluations and Transaction Evaluations.

---

# Invariants

- Evaluations are league-specific.
- Evaluations are point-in-time snapshots.
- Historical evaluations remain immutable.

---

# AI Interpretation

Player Evaluations represent strategic value rather than objective truth.
