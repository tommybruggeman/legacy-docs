# Chapter 2 — Architectural Principles

## Purpose

Architectural Principles define the non-negotiable rules that guide Legacy platform design.

They provide a consistent framework for evaluating:

- New features
- Schema changes
- Service boundaries
- AI behavior
- Operational workflows
- Technical tradeoffs

When implementation choices conflict, these principles determine the preferred direction.

---

## Principle 1 — The Application Owns Truth

Legacy business truth should be determined by application logic and canonical data.

Examples include:

- Whether a trade is legal
- Whether a contract is active
- Whether a team is over the salary cap
- Whether a player is a free agent
- Whether a draft pick is owned
- Whether a rollover event completed

User interfaces and language models may display or explain truth, but they do not define it.

```text
Canonical Data

↓

Deterministic Services

↓

Validated Conclusion

↓

UI or AI Explanation
```

---

## Principle 2 — Business Rules Must Be Explicit

League rules should never be hidden inside:

- UI conditionals
- SQL fragments
- Prompts
- Background jobs
- One-off scripts

Rules should be represented through documented services, policies, or validation rules.

Every business rule should have:

- Clear ownership
- Defined inputs
- Defined outputs
- Versioning
- Tests

---

## Principle 3 — State Transitions Must Be Deterministic

Given the same:

- Initial state
- Inputs
- Rule version
- Event sequence

Legacy should always produce the same result.

No core state transition should depend on:

- Execution timing
- Randomness
- Model interpretation
- Undocumented database behavior
- UI state

---

## Principle 4 — AI Explains but Does Not Decide Truth

AI should enter after Legacy has determined:

- What question was asked
- What facts are required
- What evidence exists
- What calculations are true
- What conclusion is allowed
- What uncertainty remains

```text
User Question
      │
      ▼
Deterministic Analysis
      │
      ▼
Approved Answer Plan
      │
      ▼
AI Explanation
```

The AI layer may improve language, context, and usability.

It may not override validated facts.

---

## Principle 5 — Every Important Operation Must Be Auditable

Important operations include:

- Transactions
- Contract changes
- Salary adjustments
- Draft-pick transfers
- Season rollover
- Recovery operations
- Administrative overrides
- AI recommendations

Each operation should record:

- Actor
- Action
- Timestamp
- Previous state
- New state
- Related records
- Reason or source

Audit history should be immutable.

---

## Principle 6 — Failures Must Be Recoverable

Any operation capable of changing significant league state should have:

- Precondition validation
- Atomic execution
- Checkpoints
- Failure classification
- Recovery planning
- Post-recovery validation

A failure should never leave the platform in an ambiguous state.

---

## Principle 7 — Data Ownership Must Be Clear

Every canonical record should have one owning subsystem.

Examples:

| Data | Owning Subsystem |
|------|------------------|
| League Rules | Rulebook |
| Team Roster | Team and Roster Domain |
| Contracts | Contract Domain |
| Draft Picks | Draft Domain |
| Event History | Event Engine |
| Validation Results | Validation Framework |
| Snapshots | Snapshot System |
| Recovery Plans | Recovery Engine |

Multiple systems may read the same data.

Only one should define its lifecycle.

---

## Principle 8 — Shared Context Should Be Immutable

Shared execution objects should not be modified by consumers.

Examples include:

- Execution Context
- Validation Context
- Recovery Context
- Evaluation Context
- AI Evidence Packet

When state changes, the system should create a new version or result object rather than modifying prior evidence.

---

## Principle 9 — Interfaces Must Be Explicit

Subsystems should communicate through documented contracts.

They should not rely on:

- Private implementation details
- Shared internal classes
- Undocumented columns
- Direct mutation of another subsystem's records

Explicit contracts preserve testability and future evolution.

---

## Principle 10 — Historical State Must Be Preserved

Legacy should distinguish between:

- Current state
- Historical state
- Derived state
- Projected state

Historical records should not be overwritten simply because current truth has changed.

Examples:

```text
Player Contract in 2026

Player Contract in 2027

Current Contract

Projected Contract
```

These are different concepts and should remain distinguishable.

---

## Principle 11 — Validation Is a Platform Capability

Validation should not be limited to form inputs.

It should protect:

- Database writes
- Event execution
- Transactions
- Season rollover
- Recovery
- AI conclusions
- Administrative actions

Validation belongs throughout the architecture.

---

## Principle 12 — Administrative Power Must Be Constrained

Administrators may need elevated capabilities, but those capabilities must still respect:

- Authentication
- Authorization
- Audit history
- Validation
- Recovery safeguards
- Confirmation requirements

Administrative access must never become an untracked bypass.

---

## Principle 13 — Systems Should Be Independently Testable

Each subsystem should support testing without requiring the entire platform.

This includes:

- Unit tests
- Contract tests
- Integration tests
- Golden datasets
- Failure simulations

Subsystem boundaries should make testing easier rather than harder.

---

## Principle 14 — External Systems Are Dependencies, Not Owners

External platforms may provide:

- Authentication
- NFL data
- Sleeper league metadata
- Notifications
- AI generation
- File storage

Legacy should validate and normalize external data before relying on it.

External identifiers may be stored, but internal canonical IDs should govern platform relationships.

---

## Principle 15 — Prefer Explicit Complexity Over Hidden Complexity

Complex fantasy league rules cannot always be simplified.

The architecture should represent necessary complexity clearly rather than hiding it inside shortcuts.

Good explicit complexity includes:

- State machines
- Event catalogs
- Validation rules
- Recovery plans
- Evaluation objects
- Versioned contracts

Hidden complexity creates operational risk.

---

## Decision Framework

When evaluating an implementation choice, ask:

1. Which subsystem owns this behavior?
2. What canonical data does it use?
3. Is the result deterministic?
4. Can the operation be audited?
5. Can failure be recovered?
6. Is the interface explicit?
7. Can the behavior be tested independently?
8. Does AI explain or define the result?
9. Is historical state preserved?
10. Does this create hidden coupling?

A design that cannot answer these questions should not be considered complete.

---

## Definition of Done

This chapter is complete when Legacy has a documented set of architectural principles capable of guiding implementation decisions across data, services, AI, operations, administration, and future platform expansion.
