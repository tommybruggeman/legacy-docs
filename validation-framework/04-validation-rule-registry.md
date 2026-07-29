# Chapter 4 — Validation Rule Registry

## Purpose

The Validation Rule Registry is the authoritative lookup table for all validation rules.

It connects Validation Rule identifiers to executable validator implementations and guarantees that every required rule can be resolved before validation begins.

The registry is the only approved mechanism for locating validation rules.

---

## Responsibilities

The Validation Rule Registry is responsible for:

- Registering validation rules
- Resolving validation rules
- Preventing duplicate registrations
- Validating rule definitions
- Supporting rule versioning
- Providing deterministic rule lookup

---

## Example

```python
VALIDATION_RULE_REGISTRY = {
    "league.exists": LeagueExistsValidator,
    "contracts.no_negative_years": NoNegativeContractYearsValidator,
    "contracts.no_active_zero_years": NoActiveZeroYearContractsValidator,
    "rosters.unique_active_players": UniqueActivePlayersValidator,
    "draft.unique_pick_ownership": UniqueDraftPickOwnershipValidator,
}
```

---

## Registration Requirements

Every registered rule must have:

- Rule ID
- Validator
- Version
- Category
- Validation Stage
- Validation Scope
- Severity

Incomplete registrations are invalid.

---

## Registry Validation

Before validation begins, the registry verifies:

- Duplicate Rule IDs
- Missing validators
- Invalid metadata
- Invalid versions
- Unsupported stages
- Unsupported scopes

Validation cannot begin until the registry passes validation.

---

## Rule Resolution

```text
Rule ID
    │
    ▼
Registry Lookup
    │
 ┌──┴────────┐
 │           │
 ▼           ▼
Found      Not Found
 │           │
 ▼           ▼
Execute    Validation Failure
```

---

## Versioning

Every registered validator should expose a version.

Example:

```text
Rule:
contracts.no_negative_years

Validator:
NoNegativeContractYearsValidator

Version:
1.0.0
```

Historical validation runs should always identify which validator implementation executed.

---

## Design Principles

The Validation Rule Registry shall:

- Be deterministic
- Prevent duplicate registrations
- Support versioning
- Validate registrations before execution
- Provide immutable rule lookup

---

## Definition of Done

This chapter is complete when every validation rule can be resolved through one deterministic registry containing one registered implementation.
