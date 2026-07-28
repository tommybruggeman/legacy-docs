---
title: Trade Asset
document: Domain Model
entity: Trade Asset
---

# Trade Asset

## Purpose

A Trade Asset represents one transferable object included within a Trade.

---

# Owned State

- Asset Type
- Asset Identifier
- Current Owner
- Destination Owner

---

# Relationships

Belongs to one Trade.

References exactly one transferable asset.

---

# Lifecycle

Attached

↓

Transferred

↓

Historical

---

# Commands

- Attach Asset
- Remove Asset

---

# Emitted Events

- TradeAssetAdded
- TradeAssetTransferred

---

# Invariants

- Every Trade Asset references one transferable entity.
- Ownership changes only through successful Trade execution.

---

# AI Interpretation

Trade Assets are individual valuation units inside a Trade.
