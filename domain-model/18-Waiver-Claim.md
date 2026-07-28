---
title: Waiver Claim
document: Domain Model
entity: Waiver Claim
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 07-Player.md
---

# Waiver Claim

## Purpose

A Waiver Claim represents a Franchise's request to acquire an unowned Player during the league's waiver process.

---

# Canonical Identity

Every Waiver Claim shall possess one immutable canonical identifier.

---

# Owned State

- Claiming Franchise
- Requested Player
- Priority
- Submission Timestamp
- Resolution Status

---

# Relationships

Belongs to one League.

References one Player.

May create one Contract.

---

# Lifecycle

Submitted

↓

Pending

↓

Awarded

or

Rejected

↓

Historical

---

# Commands

- Submit Claim
- Cancel Claim
- Resolve Claims

---

# Emitted Events

- WaiverClaimSubmitted
- WaiverClaimAwarded
- WaiverClaimRejected

---

# Invariants

- A claim references one Player.
- Awarded claims create ownership.
- Resolution order follows league rules.

---

# AI Interpretation

AI shall evaluate waiver opportunities using league priority, roster needs, and projected player value.
