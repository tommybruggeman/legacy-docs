# Chapter 10 — Event Validation

## Purpose

Event Validation verifies that each event is safe to execute before it begins and that the resulting league state is valid after it completes.

Every event participates in validation.

No event may bypass validation during commit execution.

---

## Responsibilities

Event Validation is responsible for:

- Executing pre-event validation
- Executing post-event validation
- Preventing invalid execution
- Detecting invalid mutations
- Producing validation evidence
- Determining whether execution may continue

---

## Validation Lifecycle

```text
Previous Event
        │
        ▼
Pre-Event Validation
        │
 ┌──────┴──────┐
 ▼             ▼
Pass         Fail
 │             │
 ▼             ▼
Execute      Stop Execution
 │
 ▼
Post-Event Validation
 │
 ┌──────┴──────┐
 ▼             ▼
Pass         Fail
 │             │
 ▼             ▼
Next Event   Stop Execution
```

---

## Pre-Event Validation

Pre-Event Validation verifies that an event is eligible to execute.

Examples include:

- Dependencies succeeded
- Required inputs exist
- Execution context is valid
- Event has not already executed
- Required snapshot exists
- Required execution lock is active

No mutations occur during Pre-Event Validation.

---

## Post-Event Validation

Post-Event Validation verifies that the event produced the expected outcome.

Examples include:

- Contracts reduced correctly
- Expired players removed
- Free agents created
- Draft picks advanced
- Salary totals remain valid
- No duplicate roster assignments

---

## Validation Requirements

Every Event Catalog entry should declare:

- Pre-Event Validation Set
- Post-Event Validation Set

Example:

```text
Event

season.contracts.expire

Pre Validation

pre_contract_expiration

Post Validation

post_contract_expiration
```

The Event Engine should never infer validation requirements.

---

## Event Isolation

Validation evaluates one event at a time.

Each event is responsible only for validating:

- Its prerequisites
- Its own mutations
- Its expected outputs

Global validation occurs separately during Final State Validation.

---

## Failure Behavior

If Event Validation fails:

```text
Validation Failure
        │
        ▼
Persist Validation Results
        │
        ▼
Produce Failed Decision
        │
        ▼
Notify Event Engine
        │
        ▼
Stop Execution
```

The failed event becomes the recovery starting point.

---

## Design Principles

Event Validation shall:

- Be deterministic
- Execute before and after every event
- Produce structured results
- Prevent invalid execution
- Protect downstream events

---

## Definition of Done

This chapter is complete when every event executes with explicit Pre-Event and Post-Event Validation before the Event Engine proceeds to the next event.
