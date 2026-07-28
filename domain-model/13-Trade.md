---
title: Trade
document: Domain Model
entity: Trade
---

# Trade

## Purpose

A Trade is an atomic transaction that exchanges assets between franchises.

---

# Owned State

- Participating Franchises
- Status
- Approval State
- Execution Timestamp

---

# Relationships

Contains many Trade Assets.

Produces one Transaction.

---

# Lifecycle

Proposed

↓

Pending

↓

Approved

↓

Executed

or

Rejected

↓

Historical

---

# Commands

- Propose Trade
- Accept Trade
- Reject Trade
- Execute Trade

---

# Emitted Events

- TradeProposed
- TradeAccepted
- TradeExecuted
- TradeRejected

---

# Invariants

- Trades execute atomically.
- Trades preserve ownership history.

---

# AI Interpretation

Trades are ownership transfer events.

Trade value is evaluated separately.
