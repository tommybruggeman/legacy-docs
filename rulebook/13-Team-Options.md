---
title: Team Options
document: Rulebook
chapter: 13
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 05-Contracts.md
  - 08-Rookie-Contracts.md
  - 15-Annual-Rollover.md
---

# Chapter 13 — Team Options

## Purpose

Team Options provide a franchise with the right, but not the obligation, to extend a qualifying player's contract according to league-defined rules.

A Team Option is a contractual decision point.

It is not an automatic contract extension.

---

# System Ownership

This chapter governs:

- Team Option eligibility
- Team Option availability
- Team Option execution
- Contract modification resulting from exercised options

This chapter does not govern:

- Initial contract creation
- Contract extensions outside the option system
- Player acquisition

---

# Business Rules

## Rule 13.1 — Eligibility

Only contracts satisfying league-defined eligibility requirements may receive a Team Option.

Eligibility criteria are determined by league configuration.

Examples include:

- Rookie contracts only
- Specific contract lengths
- Draft round restrictions
- Years remaining on contract

---

## Rule 13.2 — Availability

A Team Option becomes available only during league-defined option windows.

Outside the option window, contracts continue under their existing terms.

---

## Rule 13.3 — Exercise

Exercising a Team Option modifies the existing contract.

The platform shall apply league-defined modifications, including:

- Remaining years
- Salary adjustment
- Contract expiration
- Option status

The original contract history shall remain preserved.

---

## Rule 13.4 — Declining

Declining a Team Option leaves the contract unchanged.

The contract continues toward expiration under its current terms.

---

## Rule 13.5 — One-Time Execution

Each Team Option may be exercised at most once.

Once exercised or declined, the option is permanently resolved.

---

# State Transition

```text
Eligible Contract
        │
 Option Window Opens
        │
        ▼
Decision Pending
   │          │
Exercise    Decline
   │          │
   ▼          ▼
Contract   Contract
Modified   Unchanged
```

---

# Validation Rules

The platform shall reject:

- Team Options outside the permitted window.
- Duplicate executions.
- Ineligible contracts.
- Contract modifications violating league rules.

---

# Invariants

The following conditions must always remain true:

- Every Team Option references one contract.
- Every Team Option is resolved exactly once.
- Exercising an option preserves contract history.
- Declining an option preserves contract history.

---

# Edge Cases

## Commissioner Override

Commissioners may manually resolve Team Options when correcting league administration issues.

Every override shall be permanently recorded.

---

## Rule Changes

Future Team Option rule changes affect only unresolved options unless explicitly configured otherwise.

---

# Future Considerations

Future versions may support:

- Multiple option years
- Mutual options
- Player options
- Vesting options
- Conditional salary escalators

These features shall extend the Team Option model while preserving deterministic behavior.

---

# Canonical Principles

A Team Option grants a decision.

A Team Option does not guarantee an extension.

Every Team Option resolves exactly once.

---

# Related Documents

- Chapter 5 — Player Contracts
- Chapter 8 — Rookie Contracts
- Chapter 15 — Annual Rollover
