# Chapter 7 — Validation Severity

## Purpose

Every Validation Result must communicate not only whether a rule passed or failed, but also the severity of the outcome.

Validation Severity determines how the Event Engine should respond to a validation result.

Not every validation failure requires execution to stop.

---

## Responsibilities

Validation Severity is responsible for:

- Classifying validation outcomes
- Determining execution impact
- Supporting recovery decisions
- Standardizing validation reporting
- Preventing inconsistent failure handling

---

## Severity Levels

The Validation Framework supports four severity levels.

### Information

Informational results provide additional execution context.

Characteristics:

- No execution impact
- Recorded in audit history
- Visible in execution reports

Examples:

- League contains no expiring contracts.
- No taxi squad players exist.

---

### Warning

Warnings indicate unexpected conditions that do not prevent safe execution.

Characteristics:

- Execution continues
- Warning recorded
- Administrator notified

Examples:

- Team has no draft picks.
- Player metadata is incomplete.

---

### Blocking

Blocking failures prevent safe execution.

Characteristics:

- Execution immediately stops
- Event marked failed
- Recovery evaluation begins

Examples:

- Invalid roster assignment.
- Missing required contract.
- Duplicate active player.

---

### Critical

Critical failures indicate possible corruption or severe integrity risk.

Characteristics:

- Immediate execution stop
- Recovery required
- Administrative investigation required

Examples:

- Snapshot unavailable.
- Database inconsistency detected.
- Multiple active league states.

---

## Severity Flow

```text
Validation Result
        │
        ▼
Determine Severity
        │
 ┌──────┼───────────────┐
 ▼      ▼        ▼      ▼
Info Warning Blocking Critical
 │      │        │      │
 ▼      ▼        ▼      ▼
Log    Continue Stop   Recovery
```

---

## Severity Rules

Severity is determined by the validation rule.

The Event Engine consumes the severity but never changes it.

Validation Rules must never dynamically change severity during execution.

---

## Engine Behavior

The Event Engine should respond as follows:

| Severity | Continue Execution |
|-----------|-------------------|
| Information | Yes |
| Warning | Yes |
| Blocking | No |
| Critical | No |

---

## Design Principles

Validation Severity shall:

- Be deterministic
- Be explicit
- Be immutable
- Be versioned
- Be auditable

---

## Definition of Done

This chapter is complete when every Validation Result consistently communicates the operational impact of the validation outcome.
