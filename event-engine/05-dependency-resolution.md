# Chapter 5 — Dependency Resolution

## Purpose

Dependency Resolution ensures that every event executes only after all required predecessor events have successfully completed.

Execution order is determined by declared dependencies, not by file order or registration order.

---

## Responsibilities

The Dependency Resolver is responsible for:

- Reading declared dependencies
- Building the dependency graph
- Detecting circular dependencies
- Detecting missing dependencies
- Producing a valid execution order
- Preventing invalid execution

---

## Example

```text
Age Players
      │
      ▼
Reduce Contract Years
      │
      ▼
Expire Contracts
      │
      ▼
Move Players To Free Agency
```

The final event cannot execute until every predecessor succeeds.

---

## Dependency Graph

The dependency graph is a Directed Acyclic Graph (DAG).

Example:

```text
                Snapshot
                    │
                    ▼
             Age Players
                    │
                    ▼
      Reduce Contract Years
            ┌───────┴────────┐
            ▼                ▼
   Expire Contracts     Reset Taxi
            │                │
            └───────┬────────┘
                    ▼
             Final Validation
```

---

## Validation

The Dependency Resolver validates:

- Missing dependencies
- Circular dependencies
- Duplicate dependencies
- Invalid dependency references
- Self-referencing events
- Unreachable events

Execution cannot begin if dependency validation fails.

---

## Dependency Rules

Every dependency must:

- Reference an existing event
- Reference a registered handler
- Exist within the execution plan
- Execute before the dependent event

Dependencies may never be inferred.

They must be explicitly declared.

---

## Execution Readiness

An event is considered executable only when:

- Every dependency succeeded
- Validation passed
- Required inputs exist
- Execution policies allow execution

Otherwise the event remains blocked.

---

## Design Principles

The Dependency Resolver shall:

- Be deterministic
- Detect cycles
- Prevent invalid ordering
- Produce repeatable execution graphs
- Never ignore missing dependencies

---

## Definition of Done

This chapter is complete when every event in the execution plan has a validated dependency graph and a deterministic execution order.
