---
title: Conversation State
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 09-Validation.md
---

# Conversation State

## Purpose

Conversation State manages contextual continuity across multiple interactions within a single conversation.

It allows the assistant to maintain awareness of previously discussed entities, recommendations, and user objectives without altering deterministic reasoning.

Conversation State improves conversational efficiency while preserving reproducibility.

---

# Guiding Principle

Context should improve communication.

Context should never replace evidence.

---

# Responsibilities

Conversation State shall:

- Track referenced entities.
- Maintain active discussion topics.
- Remember prior recommendations.
- Preserve unresolved questions.
- Track user objectives.
- Support follow-up reasoning.

Conversation State does not determine truth.

---

# Inputs

Conversation State receives:

- User Messages
- Previous Questions
- Previous Decision Plans
- Previous Recommendations
- Current League Context

---

# Outputs

Conversation State provides:

- Active Context
- Referenced Players
- Referenced Teams
- Referenced Transactions
- User Objectives
- Conversation History

---

# Conversation Scope

Conversation State exists only for the duration of an active conversation unless explicitly persisted elsewhere.

Historical conversations remain separate.

---

# Context Resolution

Examples:

"Would you still make that trade?"

↓

Resolve "that trade" from prior context.

---

"What about his contract?"

↓

Resolve "his" from previously discussed player.

---

"Compare him to Garrett Wilson."

↓

Resolve both players using active conversation context.

---

# Context Priorities

When resolving references:

1. Current User Message
2. Active Conversation Context
3. Current League Context
4. Historical Conversation Context

The closest valid reference always wins.

---

# User Objectives

Conversation State tracks long-lived objectives during a conversation.

Examples:

- Win Now
- Rebuild
- Cap Relief
- Acquire Quarterbacks
- Improve Wide Receiver Depth

Objectives may evolve as new information is introduced.

---

# Recommendation Continuity

The assistant should remain internally consistent.

If evidence has not changed:

Repeated questions should produce equivalent Decision Plans.

If evidence changes:

The assistant should explain why its recommendation changed.

---

# Context Expiration

Conversation State shall expire when:

- a new conversation begins
- the user explicitly resets context
- the referenced entities become ambiguous

Expired context shall not influence future reasoning.

---

# Deterministic Boundaries

Conversation State may improve reference resolution.

It shall never:

- override evidence
- replace calculations
- bypass rule evaluation
- modify Decision Plans

Every new recommendation still executes the complete reasoning pipeline.

---

# Follow-up Questions

Conversation State enables natural conversations.

Examples:

"Why?"

"What if I keep my first?"

"What if he gets injured?"

"Compare that to my other offer."

Each follow-up inherits relevant context while executing fresh deterministic reasoning.

---

# Design Principles

Conversation State exists to eliminate unnecessary repetition for the user.

It is not a shortcut around deterministic reasoning.

Every recommendation remains independently defensible.

---

# Future Extensions

Conversation State may eventually support:

- multi-session continuity
- saved strategic plans
- persistent GM preferences
- season-long planning
- personalized communication styles

These capabilities should extend communication while preserving deterministic reasoning.

---

# Guiding Principle

Legacy should remember the conversation.

It should never remember incorrect conclusions.

Context exists to make the assistant feel natural.

Truth always comes from evidence.
