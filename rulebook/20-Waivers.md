---
title: Waivers
document: Rulebook
chapter: 20
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 16-Free-Agency.md
  - 07-Roster-Management.md
---

# Chapter 20 — Waivers

## Purpose

The Waiver system regulates the acquisition of players whose availability is temporarily restricted.

Waivers provide an ordered and deterministic process for assigning ownership when multiple franchises attempt to acquire the same player.

A waiver claim is a request.

A successful waiver claim transfers ownership.

---

# System Ownership

This chapter governs:

- Waiver eligibility
- Claim submission
- Claim resolution
- Claim priority
- Ownership assignment

This chapter does not govern:

- Free Agency
- Contract creation
- Salary calculations

---

# Business Rules

## Rule 20.1 — Waiver Eligibility

A player enters Waivers when required by league configuration.

Examples include:

- Recently released players
- Undrafted players
- Offseason acquisitions

---

## Rule 20.2 — Claims

Eligible franchises may submit one or more waiver claims.

Each claim references exactly one player.

---

## Rule 20.3 — Resolution

Claims are processed using the league's configured waiver priority method.

The first valid claim receives ownership.

Remaining claims are rejected.

---

## Rule 20.4 — Successful Claim

A successful claim shall:

- Assign player ownership.
- Create a contract if required.
- Assign a roster designation.
- Record the acquisition.

---

# State Transition

```text
Player Available
       │
Placed On Waivers
       │
Claims Submitted
       │
Claims Processed
       │
Ownership Assigned
```

---

# Validation Rules

Reject:

- Duplicate claims.
- Claims exceeding roster limits.
- Claims violating salary cap rules.
- Claims for unavailable players.

---

# Invariants

- One successful claim per player.
- Waiver processing is deterministic.
- Every processed claim becomes historical.

---

# Canonical Principles

Waivers determine acquisition priority.

Waivers never produce multiple owners.

Waivers resolve ownership exactly once.

---

# Related Documents

- Chapter 16 — Free Agency
