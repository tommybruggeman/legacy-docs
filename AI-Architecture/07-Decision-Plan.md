---
title: Decision Plan
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 06-Calculation.md
---

# Decision Plan

## Purpose

The Decision Plan is the deterministic reasoning engine of Legacy.

It transforms verified evidence, validated rules, and deterministic calculations into the best supported recommendation.

The Decision Plan is the authoritative answer.

Everything after this stage exists only to communicate the decision.

---

# Guiding Principle

Legacy reasons before it speaks.

The Decision Plan represents that reasoning.

---

# Inputs

The Decision Plan receives:

- Intent
- Evidence Packet
- Rule Evaluation
- Calculation Results
- User Objective
- Conversation Context

---

# Outputs

The Decision Plan produces:

- Primary Recommendation
- Alternative Recommendations
- Supporting Evidence
- Supporting Calculations
- Tradeoffs
- Confidence
- Assumptions
- Uncertainty

---

# Responsibilities

The Decision Plan shall:

- Compare available options.
- Rank alternatives.
- Resolve tradeoffs.
- Respect league rules.
- Respect user objectives.
- Maximize long-term value.
- Produce a structured recommendation.

---

# Recommendation Strategy

Every recommendation should optimize for the user's stated objective.

Examples include:

Win Now

↓

Championship probability

Rebuild

↓

Long-term asset appreciation

Cap Flexibility

↓

Financial efficiency

Draft Strategy

↓

Future value optimization

The optimal recommendation depends upon context.

---

# Tradeoffs

Every recommendation should acknowledge competing priorities.

Examples:

Acquire an elite player

↓

Lose future flexibility

Trade a veteran

↓

Increase rebuilding timeline

Spend cap space

↓

Reduce future options

---

# Alternatives

The Decision Plan should rarely produce only one path.

Instead it should rank multiple viable options.

Example:

Option 1

Best overall recommendation

Option 2

More aggressive

Option 3

Lower risk

---

# Confidence

Confidence represents the strength of available evidence.

Confidence should decrease when:

- evidence is incomplete
- projections dominate
- uncertainty increases

Confidence should never represent certainty.

---

# Assumptions

Every assumption should be explicitly identified.

Examples:

Player remains healthy.

League settings remain unchanged.

Contract rules remain constant.

---

# Uncertainty

The Decision Plan shall distinguish:

Known Facts

↓

Calculated Facts

↓

Reasonable Projections

↓

Speculation

The recommendation should communicate these distinctions clearly.

---

# Design Principles

The Decision Plan answers:

"What is the best course of action?"

It does not answer:

"How should this be explained?"

---

# Guiding Principle

If the Decision Plan changes, the recommendation changes.

If only the wording changes, the Decision Plan remains identical.
