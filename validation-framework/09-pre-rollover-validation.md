# Chapter 9 — Pre-Rollover Validation

## Purpose

Pre-Rollover Validation determines whether a league is eligible to begin a season rollover.

No rollover execution may begin until Pre-Rollover Validation succeeds.

---

## Responsibilities

Pre-Rollover Validation verifies:

- League eligibility
- Season eligibility
- Required data availability
- Required configuration
- Execution readiness

---

## Validation Flow

```text
Rollover Request
        │
        ▼
Pre-Rollover Validation
        │
 ┌──────┴───────┐
 ▼              ▼
Pass         Fail
 │              │
 ▼              ▼
Execute      Reject Request
```

---

## Required Checks

Recommended validation rules include:

### League Validation

- League exists
- League is active
- League is accessible

---

### Season Validation

- Source season exists
- Target season is valid
- Seasons progress correctly

---

### Team Validation

- Required teams exist
- Team ownership is valid
- Duplicate teams do not exist

---

### Contract Validation

- Contract records exist
- Contract years are valid
- Salary values are valid

---

### Roster Validation

- Active rosters exist
- Player assignments are valid
- No duplicate active players

---

### Draft Validation

- Draft picks exist
- Draft ownership is valid
- Pick metadata is complete

---

### Engine Validation

- Event Registry valid
- Validation Registry valid
- Snapshot system available
- Execution lock obtainable

---

## Blocking Conditions

Execution must not begin if:

- League does not exist.
- Season progression is invalid.
- Required data is missing.
- Duplicate league state exists.
- Snapshot preparation fails.
- Validation Registry is invalid.

---

## Successful Validation

Successful validation indicates:

- League is eligible.
- Execution may begin.
- Required systems are available.
- Event planning may proceed.

Passing Pre-Rollover Validation does not guarantee that later validation stages will succeed.

---

## Design Principles

Pre-Rollover Validation shall:

- Execute before planning
- Prevent invalid rollovers
- Detect structural issues
- Protect league integrity
- Produce deterministic results

---

## Definition of Done

This chapter is complete when every rollover request must successfully pass Pre-Rollover Validation before execution planning or event dispatch begins.
