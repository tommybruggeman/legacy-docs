---
title: League Evaluation
document: Domain Model
entity: League Evaluation
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 01-League.md
---

# League Evaluation

## Purpose

A League Evaluation represents strategic observations about the competitive landscape of an entire League.

---

# Owned State

- League
- Competitive Balance
- Positional Scarcity
- Market Trends
- Draft Environment
- Trade Activity
- Timestamp

---

# Relationships

Belongs to one League.

Aggregates Team Evaluations.

---

# Invariants

- League Evaluations are informational.
- League Evaluations never modify league state.
- Historical evaluations remain reproducible.

---

# AI Interpretation

League Evaluations provide contextual intelligence used when evaluating players, teams, and transactions.
