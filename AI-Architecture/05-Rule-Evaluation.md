
---
title: Rule Evaluation
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 04-Evidence-Resolver.md
---

# Rule Evaluation

## Purpose

Rule Evaluation determines what actions are permitted under the active League Configuration and Rulebook before strategic analysis begins.

The purpose of Rule Evaluation is not to determine whether an action is advisable.

Its purpose is to determine whether an action is possible.

---

# Guiding Principle

Legacy shall never recommend an action that violates league rules.

Legality is evaluated before desirability.

---

# Inputs

Rule Evaluation receives:

- Evidence Packet
- League Configuration
- Rulebook
- Current League State

---

# Outputs

Rule Evaluation produces:

- Legal Actions
- Illegal Actions
- Conditional Actions
- Rule Violations
- Required Constraints

---

# Rule Categories

## Roster Rules

Examples:

- Roster limits
- Taxi eligibility
- IR eligibility

---

## Contract Rules

Examples:

- Extension eligibility
- Contract length
- Salary requirements

---

## Financial Rules

Examples:

- Salary cap
- Dead cap
- Minimum cap compliance

---

## Draft Rules

Examples:

- Draft order
- Pick ownership
- Pick trading

---

## Transaction Rules

Examples:

- Trade deadlines
- Waivers
- Free agency
- Commissioner approval

---

# Rule Outcomes

Every evaluated action shall receive one classification.

## Legal

The action satisfies every applicable rule.

---

## Illegal

The action violates one or more rules.

Reasoning terminates for that action.

---

## Conditional

The action becomes legal if specific conditions are satisfied.

Example:

A trade becomes legal after creating sufficient cap space.

---

## Unverifiable

Required evidence is unavailable.

No recommendation shall be produced.

---

# Rule Precedence

When multiple rules apply:

1. Rulebook
2. League Configuration
3. League State
4. Transaction Context

Higher-precedence rules override lower-precedence interpretations.

---

# Determinism

Rule Evaluation must always produce identical outputs for identical inputs.

No probabilistic reasoning is permitted.

---

# Design Principles

Rule Evaluation answers:

"Can this happen?"

It never answers:

"Should this happen?"

Strategic reasoning begins only after legality has been established.

---

# Guiding Principle

Rules define the boundaries within which intelligent decision-making may occur.

Legacy respects those boundaries before considering strategy.
