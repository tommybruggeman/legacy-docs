# Chapter 3 — Event Registry

## Purpose

The Event Registry connects Event Catalog definitions to executable event handlers.

It is the authoritative lookup table used by the Event Engine during execution.

The registry is the only approved mechanism for resolving an event into executable code.

---

## Responsibilities

The Event Registry is responsible for:

- Registering handlers
- Resolving handlers
- Verifying registrations
- Preventing duplicate registrations
- Supporting handler versioning

---

## Example

```python
EVENT_REGISTRY = {
    "season.players.age": AgePlayersHandler,
    "season.contracts.reduce_years": ReduceContractYearsHandler,
    "season.contracts.expire": ExpireContractsHandler,
    "season.rosters.reset_ir": ResetIRHandler,
    "season.rosters.reset_taxi": ResetTaxiHandler,
}
```

---

## Registration Rules

Every event must:

- Exist in the Event Catalog
- Have one registered handler
- Have one handler version
- Declare dependencies
- Return a structured result

Events without handlers cannot execute.

---

## Resolution Process

```text
Event ID
      │
      ▼
Registry Lookup
      │
      ▼
Handler Found?
      │
 ┌────┴────┐
 │         │
Yes        No
 │         │
 ▼         ▼
Dispatch   Validation Failure
```

---

## Validation

Before execution begins, the registry validates:

- Missing handlers
- Duplicate handlers
- Invalid event identifiers
- Unsupported versions
- Registration conflicts

Any validation failure prevents execution.

---

## Versioning

Each registration should identify the implementation version.

Example:

```text
Event ID:
season.contracts.expire

Handler:
ExpireContractsHandler

Version:
1.0.0
```

Historical executions should always be able to identify which handler version executed an event.

---

## Design Principles

The Event Registry shall:

- Be deterministic
- Be immutable during execution
- Prevent duplicate registrations
- Prevent missing handlers
- Support future handler versioning
- Support validation before execution

---

## Definition of Done

This chapter is complete when every Event Catalog entry can be resolved into exactly one executable handler through a validated, deterministic registry.
