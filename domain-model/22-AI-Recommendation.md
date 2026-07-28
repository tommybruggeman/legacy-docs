---
title: AI Recommendation
document: Domain Model
entity: AI Recommendation
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 21-League-Configuration.md
---

# AI Recommendation

## Purpose

An AI Recommendation is the final advisory output produced by the Legacy GM Assistant.

Recommendations summarize deterministic analysis into actionable guidance for a User.

Recommendations are advisory and never execute platform actions directly.

---

# Canonical Identity

Every AI Recommendation shall possess one immutable canonical identifier.

---

# Owned State

- Recommendation Type
- Recommendation Summary
- Supporting Evidence Reference
- Confidence
- Timestamp
- Target Franchise
- Recommendation Status

---

# Relationships

Belongs to one League.

May reference one or more Evaluations.

May reference one or more Transactions or Players.

---

# Lifecycle

Generated

↓

Presented

↓

Accepted (optional)

or

Dismissed

↓

Historical

---

# Invariants

- Recommendations never modify league state.
- Every recommendation references supporting evidence.
- Recommendations are historically reproducible.

---

# AI Interpretation

Recommendations are the highest-level advisory object exposed to users.
