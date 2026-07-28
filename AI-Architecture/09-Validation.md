---
title: Validation
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 08-LLM-Explanation.md
---

# Validation

## Purpose

Validation is the final deterministic checkpoint before a response is delivered to the user.

Its purpose is to ensure that the generated explanation accurately represents the Decision Plan and does not introduce unsupported conclusions.

Validation protects the integrity of the Legacy GM Assistant by ensuring every response remains factually grounded, rule compliant, and internally consistent.

---

# Guiding Principle

No response should reach the user unless it faithfully represents the deterministic reasoning performed by Legacy.

The explanation is validated—not trusted.

---

# Inputs

Validation receives:

- Decision Plan
- LLM Explanation
- Evidence Packet
- Rule Evaluation
- Calculation Results
- Conversation Context

---

# Outputs

Validation produces one of three outcomes:

## Valid

The response is approved for delivery.

---

## Recoverable Error

The response may be regenerated without repeating deterministic reasoning.

Examples:

- Missing explanation
- Poor wording
- Formatting problems

---

## Validation Failure

The response is rejected.

The assistant shall regenerate the explanation or request clarification.

---

# Validation Categories

## Evidence Validation

Confirm every factual statement can be traced to evidence.

Examples:

- Player ownership
- Contract terms
- League configuration
- Historical transactions
- Injury information

Every factual claim must have supporting evidence.

---

## Rule Validation

Confirm every recommendation satisfies:

- League Rulebook
- League Configuration
- Current League State

Illegal recommendations shall never pass validation.

---

## Calculation Validation

Confirm every numerical statement matches deterministic calculations.

Examples:

- Salary Cap
- Dead Cap
- Trade Value
- Draft Capital
- Team Rankings

The LLM shall never modify calculated values.

---

## Recommendation Validation

Confirm the explanation communicates the same recommendation contained within the Decision Plan.

Examples:

The Decision Plan recommends keeping a player.

↓

The explanation shall never suggest trading the player.

---

## Consistency Validation

Verify that:

- conclusions match evidence
- recommendations match calculations
- confidence matches uncertainty
- assumptions remain unchanged

---

# Hallucination Prevention

Validation shall reject responses containing:

- invented statistics
- fabricated contracts
- nonexistent league rules
- unsupported rankings
- imaginary transactions
- fictional evidence

When evidence is unavailable, the assistant shall acknowledge uncertainty rather than fabricate information.

---

# Confidence Validation

Confidence must be proportional to available evidence.

Validation shall reduce confidence when:

- evidence is incomplete
- projections dominate
- external information is uncertain
- assumptions materially affect the recommendation

Confidence is not certainty.

---

# Response Integrity

Every response should satisfy the following checklist:

✓ The question was understood.

✓ The correct reasoning pipeline executed.

✓ Evidence supports every factual claim.

✓ Rules were enforced.

✓ Calculations are correct.

✓ Recommendations match the Decision Plan.

✓ The explanation introduces no new conclusions.

✓ Confidence accurately reflects uncertainty.

---

# Failure Recovery

If validation fails:

1. Reject the explanation.
2. Preserve the Decision Plan.
3. Regenerate the explanation.
4. Revalidate.
5. Return the first valid response.

Deterministic reasoning shall never be repeated unless underlying evidence has changed.

---

# Design Principles

Validation is independent.

It shall never:

- reinterpret evidence
- rerank players
- recalculate values
- modify recommendations

Validation confirms correctness.

It does not create correctness.

---

# Guiding Principle

Users should never receive a response that Legacy itself cannot defend.

Validation is the final safeguard protecting the credibility of the GM Assistant.
