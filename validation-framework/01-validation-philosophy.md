# Chapter 1 — Validation Philosophy

## Purpose

The Validation Framework exists to protect league integrity.

Its responsibility is to determine whether league state is valid before, during, and after a season rollover.

Validation never modifies league state.

Validation only evaluates league state and produces deterministic decisions.

---

## Validation Philosophy

Validation is independent.

It does not belong to:

- Event Handlers
- Event Engine
- Recovery Engine
- Admin Tools

Those systems consume validation decisions.

Only the Validation Framework determines whether league state is valid.

---

## Validation Principles

The Validation Framework shall:

- Be deterministic
- Be repeatable
- Be auditable
- Be versioned
- Be observable
- Be independent
- Be immutable
- Be fail-safe

---

## Validation Responsibilities

Validation is responsible for determining:

- Is the rollover request valid?
- Is the league eligible?
- Can an event execute?
- Did an event produce a legal result?
- Can execution continue?
- Is the final league state valid?

Validation is not responsible for repairing invalid data.

---

## Separation of Responsibilities

```text
Event Engine
      │
Coordinates Execution
      │
      ▼
Validation Framework
      │
Determines Correctness
      │
      ▼
Validation Decision
```

The Event Engine executes.

The Validation Framework evaluates.

---

## Fail-Safe Design

Whenever correctness cannot be proven, validation fails.

Validation should never assume correctness.

Validation should always require evidence.

---

## Determinism

Given:

- The same league state
- The same validation rules
- The same validation version
- The same execution context

Validation must always produce the same result.

---

## Core Guarantees

The Validation Framework guarantees:

- Every rule is evaluated consistently.
- Every result is structured.
- Every decision is auditable.
- Every blocking failure is explicit.
- Every validation run is reproducible.

---

## Definition of Done

This chapter is complete when the philosophy and responsibility of the Validation Framework are clearly separated from every other subsystem.
