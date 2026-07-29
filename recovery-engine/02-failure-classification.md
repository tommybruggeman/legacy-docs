# Chapter 2 — Failure Classification

## Purpose

Every recovery operation begins with classifying the failure that interrupted execution.

Different failure categories require different recovery strategies.

Failure classification ensures recovery remains predictable and deterministic.

---

## Classification Pipeline

```text
Failure Detected
        │
        ▼
Collect Evidence
        │
        ▼
Determine Category
        │
        ▼
Determine Severity
        │
        ▼
Generate Recovery Plan
```

---

## Failure Categories

### Validation Failure

Occurs when the Validation Framework rejects league state.

Examples:

- Duplicate players
- Invalid contracts
- Salary violations
- Invalid draft ownership

---

### Event Failure

Occurs while executing an Event Handler.

Examples:

- Unhandled exception
- Constraint violation
- Transaction failure

---

### Snapshot Failure

Occurs while creating, storing, or verifying a Snapshot.

Examples:

- Failed checksum
- Serialization failure
- Missing Snapshot

---

### Infrastructure Failure

Occurs because required infrastructure is unavailable.

Examples:

- Database outage
- Storage unavailable
- Network interruption

---

### External Dependency Failure

Occurs when required external systems fail.

Examples:

- Authentication unavailable
- Third-party timeout
- Notification service failure

---

### Administrative Failure

Execution intentionally stopped.

Examples:

- Manual abort
- Emergency maintenance
- Administrative pause

---

## Severity Levels

Failures are additionally classified by severity.

Recommended levels:

- Low
- Moderate
- High
- Critical

Severity influences recovery recommendations.

---

## Classification Requirements

Every classified failure should include:

- Failure ID
- Category
- Severity
- Failed Event
- Execution ID
- Timestamp
- Root Cause
- Recovery Recommendation

---

## Design Principles

Failure Classification shall:

- Be deterministic
- Use structured evidence
- Avoid ambiguity
- Produce reproducible results

---

## Definition of Done

This chapter is complete when every execution failure can be consistently classified into one deterministic category that guides recovery planning.
