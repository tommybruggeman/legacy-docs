---
title: Request Lifecycle
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - README.md
---

# Request Lifecycle

## Purpose

Every interaction with the Legacy GM Assistant follows a deterministic request lifecycle.

The lifecycle exists to ensure that identical questions, given identical league state and evidence, always produce identical conclusions.

Unlike traditional conversational AI systems, Legacy separates reasoning from communication.

Each stage performs one responsibility before passing structured output to the next stage.

---

# Objectives

The Request Lifecycle exists to guarantee that every response is:

- Deterministic
- Explainable
- Auditable
- Reproducible
- Rule Compliant
- Context Aware

---

# Architectural Principle

Each stage owns exactly one responsibility.

No stage should:

- duplicate another stage
- skip another stage
- infer work owned elsewhere

Every stage consumes structured inputs and produces structured outputs.

---

# Lifecycle Overview

```text
User Request
      │
      ▼
Question
      │
      ▼
Intent Resolution
      │
      ▼
Evidence Resolution
      │
      ▼
Rule Evaluation
      │
      ▼
Calculations
      │
      ▼
Decision Planning
      │
      ▼
LLM Explanation
      │
      ▼
Validation
      │
      ▼
Rendered Response
```

---

# Stage 1 — Question

Purpose:

Determine exactly what the user is asking.

Output:

A canonical representation of the user's request.

Examples:

- Evaluate Trade
- Should I Start Player?
- Compare Players
- Cap Question
- Contract Question

No football reasoning occurs during this stage.

---

# Stage 2 — Intent Resolution

Purpose:

Determine the reasoning pathway required.

Examples:

Trade Evaluation

↓

Player Evaluation

↓

League Analysis

↓

Roster Optimization

↓

Draft Planning

Intent determines which reasoning modules execute.

---

# Stage 3 — Evidence Resolution

Purpose:

Collect every fact required to answer the question.

Evidence may originate from:

- League
- Contracts
- Rosters
- Draft Picks
- Historical Transactions
- League Rules
- Player Profiles
- External Football Information

No recommendation is produced until evidence collection completes.

---

# Stage 4 — Rule Evaluation

Purpose:

Determine what actions are legal.

Examples:

Can this trade occur?

Can this player be released?

Can this contract be extended?

Can this waiver claim execute?

Illegal actions terminate further evaluation.

---

# Stage 5 — Calculations

Purpose:

Perform deterministic numerical analysis.

Examples include:

- Salary Cap
- Dead Cap
- Trade Value
- Positional Scarcity
- Competitive Window
- Future Draft Capital
- Roster Strength

Calculations produce objective measurements.

---

# Stage 6 — Decision Planning

Purpose:

Determine the optimal recommendation.

Decision Planning combines:

- Rules
- Calculations
- Evidence
- League Context
- User Objectives

Output is a structured Decision Plan.

---

# Stage 7 — LLM Explanation

Purpose:

Translate the Decision Plan into conversational language.

The LLM:

- explains
- summarizes
- contextualizes

The LLM does not:

- calculate
- rank
- validate
- determine legality

---

# Stage 8 — Validation

Purpose:

Ensure the generated response faithfully represents the Decision Plan.

Validation confirms:

- factual consistency
- rule compliance
- calculation integrity
- recommendation accuracy

Only validated responses reach users.

---

# Failure Handling

If any stage cannot complete successfully, downstream stages shall not execute.

Examples include:

Missing evidence

↓

No recommendation

Illegal transaction

↓

No evaluation

Calculation failure

↓

No Decision Plan

The assistant shall report why the request could not be completed rather than fabricate an answer.

---

# Extensibility

Additional reasoning modules may be introduced without changing the lifecycle.

Future modules may include:

- Dynasty Simulations
- Season Forecasting
- Injury Risk Modeling
- Draft Optimization
- Market Prediction

Provided they preserve lifecycle contracts.

---

# Architectural Guarantees

Every completed request shall satisfy the following guarantees:

- The question was understood.
- The intent was classified.
- Required evidence was collected.
- League rules were enforced.
- Deterministic calculations were completed.
- A structured Decision Plan was produced.
- The explanation reflects the Decision Plan.
- The response passed validation.

Only after satisfying every guarantee may a response be returned.
