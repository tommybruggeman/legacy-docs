---
title: Commissioner Authority
document: Rulebook
chapter: 21
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
---

# Chapter 21 — Commissioner Authority

## Purpose

Commissioners administer a Legacy League.

The Commissioner possesses administrative authority but does not possess unrestricted authority.

All Commissioner actions shall be deterministic, auditable, and attributable.

---

# System Ownership

This chapter governs:

- Administrative permissions
- Override authority
- League configuration
- Manual corrections

---

# Business Rules

## Rule 21.1 — Administrative Authority

Commissioners may perform administrative actions explicitly permitted by the platform.

Administrative authority shall never bypass system auditing.

---

## Rule 21.2 — Overrides

Commissioners may resolve exceptional league situations through administrative overrides.

Overrides shall preserve league integrity.

---

## Rule 21.3 — Audit

Every Commissioner action shall permanently record:

- User
- Timestamp
- Action
- Previous State
- New State
- Reason (optional)

---

## Rule 21.4 — Restricted Authority

Commissioners shall not directly modify immutable historical records except through approved administrative correction procedures.

---

# Validation Rules

Reject:

- Unauthorized administrative actions.
- Invalid state transitions.
- Historical deletions.

---

# Invariants

- Every administrative action is attributable.
- Every override is auditable.
- Historical integrity is preserved.

---

# Canonical Principles

Commissioners administer the league.

Commissioners do not rewrite history.

Every override leaves evidence.

---

# Related Documents

- Chapter 24 — Audit Log
