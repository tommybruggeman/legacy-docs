# Chapter 3 — Validation Rule Contract

## Purpose

Every validation rule must conform to a common contract.

The contract guarantees that every rule behaves consistently regardless of the domain it validates.

---

## Responsibilities

Every Validation Rule is responsible for:

- Evaluating one deterministic condition
- Returning one structured result
- Declaring its metadata
- Declaring its severity
- Declaring its validation stage

A rule should perform exactly one logical validation.

---

## Required Properties

Every Validation Rule should define:

- Rule ID
- Name
- Version
- Category
- Stage
- Scope
- Severity
- Description

---

## Example

```python
ValidationRule(
    rule_id="contracts.no_negative_years",
    version="1.0.0",
    category="Contracts",
    stage="post_event",
    scope="League",
    severity="Blocking",
)
```

---

## Rule Lifecycle

```text
Load Rule
      │
      ▼
Receive Validation Context
      │
      ▼
Evaluate Condition
      │
      ▼
Generate Result
      │
      ▼
Return Validation Result
```

---

## Rule Requirements

Every rule must:

- Produce deterministic results
- Evaluate one condition
- Return one structured Validation Result
- Avoid side effects
- Avoid modifying league data

Rules must never perform business mutations.

---

## Rule Scope

Rules may validate:

- Execution
- League
- Team
- Player
- Contract
- Roster
- Draft Pick
- Salary
- Snapshot

Each rule declares exactly one primary scope.

---

## Rule Outcomes

Every rule returns one outcome.

Possible outcomes include:

- Passed
- Warning
- Failed
- Critical Failure

These outcomes contribute to the overall Validation Decision.

---

## Design Principles

Validation Rules shall:

- Be deterministic
- Be reusable
- Be independent
- Be observable
- Be versioned
- Be side-effect free

---

## Definition of Done

This chapter is complete when every validation rule can be executed through a common contract that guarantees consistent behavior across the entire Validation Framework.
