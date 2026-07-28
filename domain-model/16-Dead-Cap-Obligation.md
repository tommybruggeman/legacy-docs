---
title: Dead Cap Obligation
document: Domain Model
entity: Dead Cap Obligation
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 15-Player-Release.md
---

# Dead Cap Obligation

## Purpose

A Dead Cap Obligation represents the remaining financial liability incurred by a Franchise after terminating an active Contract before its natural expiration.

Dead Cap exists independently of the released Player and persists until fully satisfied.

---

# Canonical Identity

Every Dead Cap Obligation shall possess one immutable canonical identifier.

---

# Owned State

- Originating Contract
- Franchise
- Remaining Obligation
- Annual Allocation
- Status
- Creation Timestamp
- Completion Timestamp

---

# Relationships

Belongs to one Franchise.

Originates from one Player Release.

References one historical Contract.

---

# Lifecycle

Created

↓

Active

↓

Reduced

↓

Satisfied

↓

Historical

---

# Commands

- Create Dead Cap
- Apply Annual Reduction
- Satisfy Obligation

---

# Emitted Events

- DeadCapCreated
- DeadCapReduced
- DeadCapSatisfied

---

# Invariants

- Dead Cap cannot exist without a released Contract.
- Remaining obligation shall never become negative.
- Historical obligations remain immutable.

---

# AI Interpretation

Dead Cap is a financial liability, not an active roster asset.

AI shall include active Dead Cap in all cap calculations.
