
---
title: Salary Cap
document: Rulebook
chapter: 6
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 05-Contracts.md
  - 09-Dead-Cap.md
  - 15-Annual-Rollover.md
---

# Chapter 6 — Salary Cap

## Purpose

The Salary Cap establishes the financial constraints under which every franchise operates.

Rather than limiting player acquisition directly, the Salary Cap creates meaningful strategic tradeoffs by requiring franchises to balance roster quality, contract value, future flexibility, and financial sustainability.

The Salary Cap is enforced continuously throughout the lifecycle of a league.

---

# Business Rules

## Rule 6.1 — League Salary Cap

Every league shall define a maximum salary cap.

The configured salary cap applies equally to every franchise unless modified by league-specific rules.

The salary cap value is a configurable league setting.

---

## Rule 6.2 — Active Cap Calculation

A franchise's active cap usage is the sum of all salaries that count against the cap under current league rules.

Cap usage may include:

- Active player contracts
- Dead cap obligations
- Retained salary (future feature)
- Other league-defined financial obligations

League-specific rules may reduce or exempt certain salaries, such as Taxi Squad or Injured Reserve players.

---

## Rule 6.3 — Continuous Validation

Salary cap compliance shall be evaluated whenever a financial event occurs, including:

- Trades
- Free agent signings
- Rookie draft selections
- Contract extensions
- Player releases
- Commissioner adjustments
- Annual rollover

No transaction may leave a franchise in an illegal salary cap state unless explicitly permitted by league rules.

---

## Rule 6.4 — Cap Compliance

A franchise is considered compliant when its calculated cap usage is less than or equal to the league salary cap.

The platform shall clearly indicate:

- Current cap usage
- Remaining cap space
- Percentage of cap utilized
- Pending financial obligations

---

## Rule 6.5 — Future Obligations

Future contract commitments remain part of franchise planning but do not count against the current season's active salary cap unless defined by league rules.

The platform should present projected cap obligations for future seasons to assist long-term planning.

---

# User Experience

Owners should always understand their financial position without performing manual calculations.

The platform should clearly communicate:

- Total cap
- Used cap
- Available cap
- Dead cap
- Upcoming expirations
- Projected future cap commitments

Financial information should be available before confirming any transaction.

---

# System Requirements

The platform shall:

- Calculate cap usage automatically.
- Recalculate immediately after every financial event.
- Prevent illegal transactions.
- Preserve historical cap data by season.
- Support configurable league cap values.
- Support future league-specific cap rules.

---

# Validation Rules

The platform shall reject:

- Transactions that violate salary cap rules.
- Negative salary cap values.
- Invalid salary calculations.
- Financial records that cannot be reconciled.

Validation shall occur before any transaction is finalized.

---

# Edge Cases

## Commissioner Overrides

Commissioners may override salary cap restrictions when league governance requires administrative intervention.

Every override shall be recorded in the audit history.

---

## Scheduled Rule Changes

Leagues may schedule salary cap changes to become effective in future seasons.

Historical cap calculations shall continue using the cap value that was active during the relevant season.

---

## Temporary Exceptions

Future platform versions may support temporary cap exceptions defined by league rules.

These exceptions should be explicitly tracked and displayed.

---

# Future Considerations

Potential enhancements include:

- Soft salary caps
- Luxury tax systems
- Salary cap carryover
- Retained salary
- Conditional cap exemptions
- Expansion cap adjustments
- Multi-tier financial rules

The underlying calculation engine should support these extensions without redesign.

---

# Design Notes

### Why a Salary Cap?

The Salary Cap exists to create scarcity.

Without financial constraints, roster construction becomes a question of acquisition alone.

By introducing limited financial resources, Legacy rewards planning, discipline, and efficient asset management.

---

### Canonical Principle

Every dollar committed to one player is a dollar unavailable elsewhere.

The Salary Cap transforms financial flexibility into a competitive asset.

---

# Related Documents

- Chapter 5 — Player Contracts
- Chapter 9 — Dead Cap
- Chapter 15 — Annual Rollover
