# Chapter 1 — Engine Responsibility

## Purpose

The Event Engine is responsible for orchestrating the execution of a season rollover...

---

## Responsibilities

| Responsibility | Owned by Engine |
|----------------|-----------------|
| Compile execution plan | ✅ |
| Execute handlers | ✅ |
| Age players | ❌ |
| Reduce contracts | ❌ |

---

## Execution Lifecycle

```text
Rollover Request
    ↓
Execution Context
    ↓
Execution Plan
    ↓
Validation
    ↓
Dispatch
    ↓
Results
```

---

## Engine Boundaries

### Owns

- Execution planning
- Event ordering
- Dependency resolution
- Result tracking

### Does Not Own

- Business logic
- Player calculations
- League rules
- Salary cap rules
