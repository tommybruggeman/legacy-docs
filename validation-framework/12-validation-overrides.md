# Chapter 12 — Validation Overrides

## Purpose

Validation Overrides provide a controlled mechanism for authorized administrators to bypass specific validation failures when recovery procedures explicitly permit it.

Overrides are exceptional administrative actions.

They are not part of normal rollover execution.

---

## Responsibilities

Validation Overrides are responsible for:

- Authorizing override requests
- Recording override decisions
- Preserving audit history
- Limiting override scope
- Protecting league integrity

---

## Override Principles

Overrides should be:

- Explicit
- Authorized
- Auditable
- Versioned
- Rare

Validation should never be bypassed silently.

---

## Eligible Overrides

Examples may include:

- Non-critical warning acknowledgement
- Temporary metadata inconsistencies
- Administrative recovery procedures

The specific override policy should be defined by league administration.

---

## Non-Eligible Overrides

Critical integrity failures should never support override.

Examples include:

- Duplicate active players
- Corrupted contract data
- Missing execution context
- Failed snapshot creation
- Invalid execution history

These failures require recovery rather than override.

---

## Override Workflow

```text
Validation Failure
        │
        ▼
Administrative Review
        │
 ┌──────┴──────┐
 ▼             ▼
Override     Reject
 │             │
 ▼             ▼
Record      Recovery
Override
```

---

## Override Record

Every override should permanently record:

- Override ID
- Validation ID
- Execution ID
- Rule ID
- User
- Timestamp
- Reason
- Authorization Level

Override records are immutable.

---

## Security

Overrides require:

- Authentication
- Administrative authorization
- League access
- Permanent audit recording

Unauthorized overrides must be rejected.

---

## Design Principles

Validation Overrides shall:

- Protect league integrity
- Require explicit authorization
- Preserve audit history
- Never hide validation failures
- Remain exceptional

---

## Definition of Done

This chapter is complete when authorized administrators can perform controlled, fully audited validation overrides without compromising the integrity or traceability of the season rollover process.
