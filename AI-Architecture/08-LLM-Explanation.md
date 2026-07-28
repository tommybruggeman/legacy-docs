---
title: LLM Explanation
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 07-Decision-Plan.md
---

# LLM Explanation

## Purpose

The LLM Explanation stage converts the structured Decision Plan into natural, conversational language.

This stage exists to improve understanding—not to change the underlying recommendation.

---

# Guiding Principle

The language model communicates.

Legacy decides.

---

# Inputs

The LLM receives:

- Decision Plan
- Supporting Evidence
- Calculations
- User Question
- Conversation Context

---

# Outputs

The LLM produces:

- Natural Language Response
- Supporting Narrative
- Follow-up Suggestions
- Contextual Explanations

---

# Responsibilities

The LLM shall:

- Explain recommendations.
- Summarize reasoning.
- Organize complex information.
- Answer follow-up questions.
- Adapt tone to the conversation.

---

# Prohibited Responsibilities

The LLM shall never independently:

- calculate values
- determine legality
- invent evidence
- rank players
- alter recommendations
- fabricate confidence

Those responsibilities belong to deterministic systems.

---

# Explanation Principles

Every explanation should be:

- Accurate
- Clear
- Concise
- Contextual
- Consistent

---

# Transparency

Whenever uncertainty exists, the explanation should communicate:

- what is known
- what is projected
- what assumptions exist

---

# Supporting Evidence

Explanations should reference:

- league rules
- contracts
- player values
- calculations
- historical context

whenever appropriate.

---

# Conversation

The LLM may:

- answer follow-up questions
- simplify complex concepts
- provide examples
- compare alternatives

provided the underlying Decision Plan remains unchanged.

---

# Failure Conditions

If the Decision Plan cannot support an explanation:

the LLM shall request clarification rather than fabricate reasoning.

---

# Design Principles

The explanation should never become more authoritative than the Decision Plan.

Natural language is a presentation layer.

Truth originates elsewhere.

---

# Guiding Principle

Users should trust Legacy because its reasoning is correct—not because its language is persuasive.
