---
title: AI Architecture
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
---

# AI Architecture

## Purpose

The Legacy GM Assistant is a deterministic football reasoning engine augmented by a Large Language Model.

Unlike traditional AI assistants, the language model is **not responsible for determining truth**.

Instead, the platform separates reasoning from communication.

Legacy first determines the correct answer using league rules, structured data, deterministic calculations, and domain-specific evaluation engines.

Only after the platform has established the correct answer does a language model transform that answer into natural conversation.

This architecture produces recommendations that are:

- Explainable
- Auditable
- Deterministic
- Consistent
- Reproducible

The objective is not simply to generate convincing answers.

The objective is to generate correct answers.

---

# Guiding Philosophy

Legacy is designed around a simple principle:

> **The system should reason like a General Manager before speaking like one.**

Natural language is the final presentation layer—not the decision engine.

Football intelligence originates from deterministic systems that understand:

- League rules
- Contracts
- Salary cap implications
- Roster construction
- Draft capital
- Historical transactions
- Team objectives
- Competitive windows
- Market value
- League context

The language model exists to explain those conclusions clearly, not invent them.

---

# Architectural Principles

## Deterministic First

Every recommendation begins with facts.

Legacy resolves:

- What question was asked
- What information is required
- What evidence is available
- What league rules apply
- What calculations are required
- What conclusions are supported

before generating natural language.

---

## Evidence Before Opinion

The assistant never begins with a conclusion.

It first gathers evidence.

Evidence may include:

- League configuration
- Player information
- Contracts
- Roster assignments
- Draft picks
- Transactions
- Historical evaluations
- League trends
- External football information

Only verified evidence may influence recommendations.

---

## Rules Before Recommendations

Every possible action is validated before it is evaluated.

Examples include:

- Is this trade legal?
- Can this contract be extended?
- Does sufficient cap space exist?
- Is this waiver claim valid?
- Can this roster move occur today?

Recommendations are never produced for illegal actions.

---

## Calculations Before Judgment

Recommendations rely on deterministic calculations.

Examples include:

- Salary cap
- Dead cap
- Contract duration
- Positional scarcity
- Trade value
- Roster strength
- Competitive window
- Draft capital
- Future flexibility

These calculations produce measurable inputs for decision making.

---

## Explain Last

Only after deterministic reasoning is complete does the language model generate an explanation.

The explanation should never modify:

- facts
- legality
- calculations
- rankings
- conclusions

Its responsibility is communication.

---

# High-Level Request Lifecycle

Every request follows the same pipeline.

```text
User Request
      │
      ▼
Question Identification
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
Deterministic Calculations
      │
      ▼
Decision Plan
      │
      ▼
Natural Language Explanation
      │
      ▼
Validation
      │
      ▼
Final Response
```

Each stage has a single responsibility.

No stage should duplicate the responsibility of another stage.

---

# Pipeline Responsibilities

## Question

Determine exactly what the user is asking.

---

## Intent

Identify the category of reasoning required.

Examples:

- Trade evaluation
- Contract question
- Draft strategy
- Waiver recommendation
- Cap analysis
- Lineup advice
- League rules

---

## Evidence Resolution

Collect every piece of information required to answer the question.

No reasoning occurs until evidence collection is complete.

---

## Rule Evaluation

Determine what actions are legal.

Reject impossible recommendations before evaluation begins.

---

## Calculations

Perform deterministic analysis.

Examples include:

- Salary cap
- Player rankings
- Trade value
- Team needs
- Competitive windows
- Future projections

---

## Decision Plan

Produce the best supported recommendation.

The Decision Plan represents the authoritative answer produced by Legacy.

---

## Natural Language Explanation

Transform the Decision Plan into conversational language.

The language model explains.

It does not decide.

---

## Validation

Ensure:

- evidence supports conclusions
- calculations match outputs
- recommendations are legal
- explanations remain faithful to deterministic reasoning

Only validated answers reach the user.

---

# Core Design Principles

The assistant shall:

- Prefer deterministic truth over persuasive language.
- Reject unsupported conclusions.
- Never fabricate evidence.
- Distinguish known facts from uncertainty.
- Cite reasoning whenever practical.
- Explain assumptions explicitly.
- Maintain consistent recommendations when evidence is unchanged.
- Adapt recommendations when league state changes.

---

# Relationship to the Domain Model

The Domain Model defines what exists.

Examples include:

- Players
- Contracts
- Franchises
- Trades
- Draft Picks

The AI Architecture defines how those entities are interpreted.

The reasoning engine consumes Domain Model entities but does not redefine them.

---

# Relationship to the Rulebook

The Rulebook defines platform behavior.

The AI Architecture applies those rules to answer questions.

If a recommendation conflicts with the Rulebook, the Rulebook always prevails.

---

# Relationship to the Database

The database stores facts.

The AI Architecture transforms facts into decisions.

The assistant never substitutes generated information for stored truth.

---

# Relationship to OpenAI

OpenAI is responsible for:

- conversational fluency
- explanation
- summarization
- contextualization
- follow-up interaction

OpenAI is **not** responsible for:

- determining legality
- calculating salary cap
- evaluating contracts
- ranking players
- validating trades
- determining roster ownership
- enforcing league rules

Those responsibilities belong to Legacy.

---

# Future Evolution

This architecture is intentionally modular.

Each stage of the reasoning pipeline may evolve independently provided that:

- interfaces remain stable
- deterministic behavior is preserved
- downstream stages receive complete inputs
- validation guarantees correctness

New reasoning modules may be introduced without changing the overall request lifecycle.

---

# Guiding Principle

The Legacy GM Assistant exists to emulate the reasoning process of an elite dynasty fantasy football General Manager.

Every recommendation should be:

- factually grounded
- strategically sound
- league-aware
- financially accurate
- explainable
- reproducible
- transparent

The system's credibility depends not on how confidently it speaks, but on how correctly it reasons.
