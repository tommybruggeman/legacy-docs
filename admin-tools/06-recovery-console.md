# Chapter 6 — Recovery Console

## Purpose

The Recovery Console provides administrators with operational visibility into every Recovery Engine operation.

It centralizes Recovery Plans, restoration progress, validation status, and resume operations.

The Recovery Console supervises recovery rather than executing recovery logic.

---

## Responsibilities

The Recovery Console displays:

- Recovery Plans
- Recovery Contexts
- Recovery Strategies
- Snapshot restoration status
- Recovery validation
- Resume execution progress
- Recovery history

---

## Console Overview

```text
Recovery

Recovery ID

Execution ID

Failure Category

Recovery Strategy

Snapshot

Resume Event

Status
```

---

## Recovery Timeline

Example:

```text
Failure Detected

↓

Recovery Planned

↓

Snapshot Restored

↓

Validation Passed

↓

Execution Resumed
```

The timeline should be immutable.

---

## Recovery Details

Each Recovery Plan should display:

- Failure Category
- Failure Severity
- Selected Strategy
- Snapshot Restored
- Replay Events
- Validation Result

The Recovery Plan should be directly traceable from the console.

---

## Administrative Controls

Authorized administrators may:

- Approve Recovery
- Reject Recovery
- Abort Recovery
- Review Recovery History

Controls should respect Role-Based Access Control.

---

## Recovery History

Historical recovery records should remain available indefinitely.

Each recovery should expose:

- Duration
- Final Outcome
- Validation Result
- Administrator Actions

---

## Design Principles

The Recovery Console shall:

- Be operationally focused
- Be auditable
- Be secure
- Be deterministic
- Prioritize recovery transparency

---

## Definition of Done

This chapter is complete when administrators can fully understand and govern every Recovery Engine operation from a centralized operational interface.
