---
title: Question
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 01-Request-Lifecycle.md
---

# Question

## Purpose

The Question stage transforms free-form user language into a canonical representation of the information being requested.

It answers one question only:

> What is the user asking?

No football reasoning occurs during this stage.

---

# Responsibilities

The Question stage shall:

- Identify the primary request.
- Resolve referenced entities.
- Normalize terminology.
- Remove conversational ambiguity.
- Preserve user intent.
- Produce a structured Question object.

---

# Inputs

The Question stage receives:

- User Message
- Conversation History
- Current League Context
- Current User Context

---

# Outputs

The Question stage produces:

- Canonical Question
- Referenced Entities
- Required Context
- Confidence
- Missing Information

---

# Canonical Question Types

Examples include:

## Trade

Should I make this trade?

Evaluate this trade.

Who wins this trade?

---

## Player

Who is more valuable?

Rank these players.

Should I buy this player?

---

## Team

Evaluate my roster.

What position should I improve?

How competitive am I?

---

## Contracts

Can I extend this contract?

How much cap space do I have?

Who should I release?

---

## Draft

Who should I draft?

Should I trade this pick?

How valuable is this pick?

---

## League

How strong is my league?

Who are the contenders?

What trends exist?

---

# Entity Resolution

Questions often reference entities indirectly.

Examples:

"My quarterback"

↓

Current starting QB(s)

"My first"

↓

Current first-round draft pick

"His running back"

↓

Opponent roster lookup

The Question stage resolves references before reasoning begins.

---

# Ambiguity Resolution

If multiple interpretations exist, the Question stage shall select the highest-confidence interpretation.

If confidence is insufficient, clarification shall be requested before proceeding.

Example:

"Should I trade him?"

Without an identifiable player, evaluation cannot continue.

---

# Out of Scope

The Question stage shall not:

- rank players
- evaluate trades
- calculate cap space
- infer legality
- make recommendations

Those responsibilities belong to later stages.

---

# Success Criteria

The Question stage completes successfully when:

- The user's request is fully understood.
- Referenced entities are resolved.
- Required context is identified.
- A structured Question object is produced.

Only then may Intent Resolution begin.

---

# Design Principles

Questions should be interpreted consistently.

Equivalent wording should produce identical Question objects.

Examples:

"Should I trade Garrett Wilson?"

"Is moving Garrett Wilson a good idea?"

"I want advice on Garrett Wilson."

All represent the same canonical question despite differing language.

---

# Guiding Principle

Understanding the wrong question guarantees the wrong answer.

Every subsequent stage depends upon the accuracy of the Question stage.

Legacy therefore prioritizes precise question understanding before any football reasoning begins.
