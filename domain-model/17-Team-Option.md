---
title: Team Option
document: Domain Model
entity: Team Option
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 08-Contract.md
---

# Team Option

## Purpose

A Team Option represents a contractual right allowing a Franchise to extend or modify an existing Contract under predefined league rules.

---

# Canonical Identity

A Team Option is uniquely identified by its originating Contract.

---

# Owned State

- Parent Contract
- Option Type
- Eligibility
- Exercise Window
- Status

---

# Relationships

Belongs to one Contract.

May create a new Contract state upon execution.

---

# Lifecycle

Created

↓

Eligible

↓

Exercised

or

Expired

↓

Historical

---

# Commands

- Exercise Option
- Decline Option

---

# Emitted Events

- TeamOptionCreated
- TeamOptionExercised
- TeamOptionExpired

---

# Invariants

- An option may only be exercised once.
- Expired options cannot be reactivated.
- Options never exist independently of Contracts.

---

# AI Interpretation

AI shall evaluate both the immediate and future cap implications of exercising a Team Option.
