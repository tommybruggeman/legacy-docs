---
title: Transaction Evaluation
document: Domain Model
entity: Transaction Evaluation
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 19-Transaction.md
---

# Transaction Evaluation

## Purpose

A Transaction Evaluation represents the strategic assessment of a proposed or completed league transaction.

Transaction Evaluations measure legality, fairness, strategic impact, financial implications, and long-term consequences.

---

# Owned State

- Transaction
- Legality Assessment
- Fairness Assessment
- Strategic Impact
- Financial Impact
- Recommendation
- Confidence
- Timestamp

---

# Relationships

References one Transaction.

May aggregate Player, Team, and League Evaluations.

May produce one AI Recommendation.

---

# Invariants

- Evaluations never execute transactions.
- Every evaluation references exactly one Transaction.
- Historical evaluations remain immutable.

---

# AI Interpretation

Transaction Evaluations are the primary decision-support object used by the GM Assistant when analyzing trades, waiver claims, releases, contract changes, and other league transactions.
