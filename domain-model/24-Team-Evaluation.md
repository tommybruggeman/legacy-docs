---
title: Team Evaluation
document: Domain Model
entity: Team Evaluation
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 04-Franchise.md
---

# Team Evaluation

## Purpose

A Team Evaluation represents the overall strategic assessment of a Franchise.

It measures roster quality, competitive outlook, financial flexibility, and long-term sustainability.

---

# Owned State

- Franchise
- Overall Rating
- Competitive Window
- Positional Strengths
- Positional Weaknesses
- Salary Cap Health
- Draft Capital
- Timestamp

---

# Relationships

Belongs to one Franchise.

Aggregates many Player Evaluations.

---

# Invariants

- Evaluations are snapshots.
- Historical evaluations remain attributable.
- Team Evaluations do not modify league state.

---

# AI Interpretation

Team Evaluations summarize the competitive position of a Franchise at a specific moment in time.
