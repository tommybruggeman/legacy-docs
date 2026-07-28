---
title: Calculation
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 05-Rule-Evaluation.md
---

# Calculation

## Purpose

The Calculation stage transforms verified evidence into deterministic measurements.

Calculations produce objective values used during strategic decision making.

Unlike evaluations, calculations never express opinions.

They produce measurable facts.

---

# Guiding Principle

Everything that can be calculated should be calculated before anything is judged.

---

# Inputs

The Calculation stage receives:

- Evidence Packet
- Rule Evaluation
- League Configuration

---

# Outputs

The Calculation stage produces one or more Calculation Results.

Examples include:

- Available Salary Cap
- Dead Cap
- Effective Cap Space
- Contract Cost
- Positional Scarcity
- Draft Capital
- Market Value
- Team Strength
- Competitive Window
- Future Financial Flexibility

---

# Calculation Categories

## Financial

Examples:

- Salary Cap
- Dead Cap
- Remaining Cap
- Future Commitments

---

## Roster

Examples:

- Position Depth
- Starter Quality
- Bench Strength
- Positional Balance

---

## Market

Examples:

- Relative Player Value
- Trade Value
- Pick Value
- Replacement Value

---

## Competitive

Examples:

- Championship Probability
- Rebuild Score
- Team Age
- Future Outlook

---

## League

Examples:

- Positional Scarcity
- Market Demand
- League Trends
- Competitive Balance

---

# Calculation Requirements

Every calculation shall be:

- Deterministic
- Reproducible
- Explainable
- Independent
- Versioned

---

# Calculation Dependencies

Calculations may depend upon:

- Rule Evaluation
- League Configuration
- Contracts
- Transactions
- Historical Data

Calculations shall never depend upon LLM output.

---

# Confidence

Each calculation should expose:

- Value
- Units
- Method
- Timestamp
- Confidence

---

# Failure Handling

If a required calculation cannot complete:

- downstream strategic reasoning shall pause
- the missing calculation shall be identified
- unsupported assumptions shall never be invented

---

# Design Principles

Calculations answer:

"What is true numerically?"

They do not answer:

"What should I do?"

---

# Guiding Principle

Every recommendation should be built upon measurable facts rather than intuition.
