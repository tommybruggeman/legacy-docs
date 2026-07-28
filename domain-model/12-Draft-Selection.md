---
title: Draft Selection
document: Domain Model
entity: Draft Selection
---

# Draft Selection

## Purpose

A Draft Selection records the execution of one Draft Pick.

A Draft Selection permanently associates a Draft Pick with the Player selected.

---

# Owned State

- Draft Pick
- Selected Player
- Selecting Franchise
- Timestamp
- Selection Number

---

# Relationships

Belongs to one Rookie Draft.

Consumes one Draft Pick.

Creates one Contract.

---

# Lifecycle

Pending

↓

Executed

↓

Historical

---

# Commands

- Execute Selection

---

# Emitted Events

- DraftSelectionCompleted

---

# Invariants

- One Player per Selection.
- One Draft Pick per Selection.
- Executed selections are immutable.

---

# AI Interpretation

Selections define historical acquisition, not current ownership.
