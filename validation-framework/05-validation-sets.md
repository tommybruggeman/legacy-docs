# Chapter 5 — Validation Sets

## Purpose

Validation Sets organize related validation rules into reusable collections.

Instead of selecting individual rules during execution, the Validation Framework executes named Validation Sets that correspond to a specific stage of the rollover pipeline.

---

## Responsibilities

Validation Sets are responsible for:

- Grouping related rules
- Defining stage-specific validation
- Supporting deterministic execution
- Preventing missing rule execution
- Simplifying validation configuration

---

## Example

```python
ValidationSet(
    validation_set_id="post_contract_expiration",
    rules=[
        "contracts.no_active_zero_years",
        "contracts.expired_players_removed",
        "free_agency.expired_players_created",
        "rosters.valid_active_assignments",
    ]
)
```

---

## Validation Set Flow

```text
Validation Stage
        │
        ▼
Resolve Validation Set
        │
        ▼
Load Validation Rules
        │
        ▼
Execute Rules
        │
        ▼
Collect Results
```

---

## Example Validation Sets

Suggested Validation Sets include:

```text
request_validation

pre_rollover

pre_event

post_contract_reduction

post_contract_expiration

post_free_agency

post_roster_reset

post_draft_processing

final_rollover
```

Each Validation Set targets one stage of execution.

---

## Validation Set Requirements

Every Validation Set must:

- Have one unique identifier
- Contain registered rules
- Be versioned
- Be immutable during execution
- Produce deterministic results

---

## Versioning

Every Validation Set should expose:

- Set ID
- Version
- Included Rule Versions

This allows historical validation runs to be reproduced accurately.

---

## Design Principles

Validation Sets shall:

- Be deterministic
- Be reusable
- Be stage-specific
- Be immutable
- Be independently versioned

---

## Definition of Done

This chapter is complete when every stage of the rollover process executes one or more deterministic Validation Sets instead of manually selecting validation rules.
