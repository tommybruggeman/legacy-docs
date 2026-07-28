---
title: Audit Event
document: Domain Model
entity: Audit Event
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 19-Transaction.md
---

# Audit Event

## Purpose

An Audit Event records an immutable fact describing a specific change resulting from a Transaction.

While a Transaction represents the overall operation, Audit Events describe the individual state changes produced by that operation.

---

# Canonical Identity

Every Audit Event shall possess one immutable canonical identifier.

---

# Owned State

- Parent Transaction
- Entity Type
- Entity Identifier
- Previous State
- New State
- Timestamp

---

# Relationships

Belongs to one Transaction.

References one domain entity.

---

# Lifecycle

Created

↓

Historical

Audit Events are never modified or deleted.

---

# Commands

Audit Events are system-generated.

No manual commands exist.

---

# Emitted Events

None.

Audit Events are terminal records.

---

# Invariants

- Every Audit Event belongs to exactly one Transaction.
- Audit Events are immutable.
- Every state transition is historically attributable.

---

# AI Interpretation

Audit Events represent the lowest level of historical truth.

When reconstructing league history, AI shall reason from Audit Events rather than inferred state whenever historical accuracy is required.
