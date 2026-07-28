---
title: Player Release
document: Domain Model
entity: Player Release
---

# Player Release

## Purpose

A Player Release records the termination of an active Contract by a Franchise.

The Player Release is the event that ends ownership and may create Dead Cap obligations.

---

# Owned State

- Released Contract
- Released Player
- Releasing Franchise
- Timestamp
- Dead Cap Generated

---

# Relationships

References one Contract.

May create one Dead Cap Obligation.

Creates one Transaction.

---

# Lifecycle

Requested

↓

Executed

↓

Historical

---

# Commands

- Release Player

---

# Emitted Events

- PlayerReleased

---

# Invariants

- Releases never delete Contracts.
- Releases preserve historical ownership.
- Releases execute atomically.

---

# AI Interpretation

A Player Release ends active ownership while preserving financial and historical continuity.
