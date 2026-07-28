---
title: Intent Resolution
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 02-Question.md
---

# Intent Resolution

## Purpose

Intent Resolution determines which reasoning pipeline should execute.

While the Question stage determines *what* the user is asking, Intent Resolution determines *how* Legacy should answer it.

Intent selection controls every downstream component of the assistant.

---

# Objectives

Intent Resolution shall:

- Classify the primary intent.
- Identify any secondary intents.
- Determine required reasoning modules.
- Identify required calculations.
- Identify required evidence.
- Route the request into the correct deterministic workflow.

---

# Inputs

Intent Resolution receives:

- Canonical Question
- Referenced Entities
- Conversation Context
- User Context

---

# Outputs

Intent Resolution produces:

- Primary Intent
- Secondary Intents
- Required Evidence Types
- Required Calculation Modules
- Required Rule Modules
- Required Evaluation Modules

---

# Intent Categories

## Trade Evaluation

Examples:

- Should I make this trade?
- Who wins this trade?
- Is this fair?

Requires:

- Trade Evaluation
- Player Evaluation
- Team Evaluation
- Rule Evaluation

---

## Player Evaluation

Examples:

- Is Garrett Wilson worth buying?
- Rank these players.
- Who has more dynasty value?

Requires:

- Player Evaluation
- League Context
- Market Context

---

## Team Evaluation

Examples:

- Evaluate my roster.
- What position should I improve?
- How competitive am I?

Requires:

- Team Evaluation
- League Evaluation
- Competitive Window

---

## Contract Analysis

Examples:

- Can I extend this player?
- Who should I cut?
- How much cap space do I have?

Requires:

- Contracts
- Salary Cap
- League Rules

---

## Draft Strategy

Examples:

- Who should I draft?
- Should I trade this pick?
- Best rookie available?

Requires:

- Draft Picks
- Player Evaluations
- League Trends

---

## League Analysis

Examples:

- Who is rebuilding?
- Who are contenders?
- Which position is scarce?

Requires:

- League Evaluation
- Team Evaluations
- Transactions

---

## Rules Clarification

Examples:

- Can I carry 12 players?
- How does taxi eligibility work?

Requires:

- League Configuration
- Rule Evaluation

---

# Multiple Intents

Some questions require multiple reasoning paths.

Example:

"Should I trade Garrett Wilson for two firsts?"

Requires:

- Trade Evaluation
- Player Evaluation
- League Evaluation
- Future Draft Analysis

Intent Resolution shall build a complete execution plan before evidence collection begins.

---

# Intent Priority

When multiple intents exist:

1. Rules
2. Transactions
3. Team
4. Player
5. League

Higher-priority intents determine pipeline ordering.

---

# Failure Conditions

Intent Resolution fails when:

- The question is ambiguous.
- Required entities are unresolved.
- Multiple incompatible interpretations exist.

When failure occurs, clarification shall be requested before reasoning begins.

---

# Design Principles

Intent Resolution is deterministic.

Equivalent questions shall always produce identical intent classifications.

Intent Resolution never performs football analysis.

It only selects the reasoning path.

---

# Guiding Principle

Intent determines which parts of Legacy think.

Correct routing is essential for correct recommendations.
