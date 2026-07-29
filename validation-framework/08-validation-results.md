# Chapter 8 — Validation Results

## Purpose

Every validation rule execution produces a Validation Result.

Validation Results provide structured evidence describing the outcome of a rule evaluation and become part of the permanent execution history.

---

## Responsibilities

A Validation Result records:

- Validation ID
- Rule ID
- Rule Version
- Status
- Severity
- Message
- Records Examined
- Violations Found
- Execution Duration
- Timestamp

---

## Example

```python
ValidationResult(
    validation_id="validation_001",
    rule_id="contracts.no_negative_years",
    status="FAILED",
    severity="BLOCKING",
    records_examined=248,
    violations_found=3,
    message="Three contracts contain negative years.",
)
```

---

## Result Lifecycle

```text
Rule Execution
        │
        ▼
Evaluate Condition
        │
        ▼
Generate Result
        │
        ▼
Persist Result
        │
        ▼
Aggregate Decision
```

---

## Status Values

Supported statuses include:

- Passed
- Warning
- Failed
- Critical Failure

Each Validation Result ends in exactly one status.

---

## Persistence

Validation Results must be persisted before the next rule executes.

Persisted results support:

- Auditing
- Recovery
- Reporting
- Resume
- Historical analysis

---

## Immutability

Validation Results are immutable.

If validation must be repeated:

- Create a new Validation Context.
- Execute the rules again.
- Produce a new Validation Result.

Historical Validation Results are never modified.

---

## Aggregation

Multiple Validation Results combine into one Validation Decision.

Example:

```text
24 Rules Executed

22 Passed
1 Warning
1 Blocking Failure

Final Decision

FAILED
```

---

## Design Principles

Validation Results shall:

- Be deterministic
- Be immutable
- Be serializable
- Be auditable
- Be machine-readable

---

## Definition of Done

This chapter is complete when every validation rule execution produces one structured Validation Result that permanently records its outcome.
