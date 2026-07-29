# Chapter 4 — Validation Console

## Purpose

The Validation Console provides administrators with complete visibility into every validation performed during a Season Rollover execution.

It allows operators to understand why validation passed or failed without reviewing application logs.

The Validation Console is a diagnostic interface.

It does not execute validation.

---

## Responsibilities

The Validation Console is responsible for displaying:

- Validation history
- Validation Sets
- Validation Rules
- Validation Results
- Validation Decisions
- Rule versions
- Failure evidence

---

## Console Overview

```text
Validation Execution

Validation ID

Execution ID

Validation Stage

Validation Decision

Rules Executed

Passed

Warnings

Failures
```

---

## Rule Explorer

Administrators should be able to inspect:

- Rule ID
- Rule Name
- Rule Version
- Severity
- Status
- Duration
- Failure Message

Every rule should expose structured evidence.

---

## Validation Timeline

Example:

```text
Request Validation

↓

Pre-Rollover Validation

↓

Contract Reduction Validation

↓

Contract Expiration Validation

↓

Final State Validation
```

Operators should immediately identify where validation failed.

---

## Result Inspection

Each Validation Result should include:

- Status
- Severity
- Rule Message
- Violations Found
- Records Examined
- Related Event

Results should remain immutable.

---

## Filtering

The Validation Console should support filtering by:

- Execution
- League
- Validation Stage
- Severity
- Rule Category
- Rule Status

Filtering should not alter stored results.

---

## Failure Investigation

When validation fails, operators should immediately see:

- Failed Rule
- Severity
- Related Event
- Suggested Recovery Path

The Validation Console should reduce time-to-diagnosis.

---

## Design Principles

The Validation Console shall:

- Be read-only
- Be deterministic
- Be searchable
- Be auditable
- Present structured evidence

---

## Definition of Done

This chapter is complete when administrators can inspect every validation decision and every validation rule executed during a rollover.
