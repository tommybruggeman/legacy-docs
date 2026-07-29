# Validation Framework

## Overview

The Validation Framework is the independent correctness layer of the Season Rollover system.

Its responsibility is to determine whether league state is valid before, during, and after a rollover execution.

The Event Engine coordinates execution.

Event Handlers perform mutations.

The Validation Framework determines whether those mutations produced a legal and internally consistent league state.

```text
Rollover Request
        │
        ▼
Pre-Rollover Validation
        │
        ▼
Event Engine
        │
        ▼
Event Handler
        │
        ▼
Post-Event Validation
        │
        ▼
Next Event
        │
        ▼
Final Rollover Validation
```

Validation is not optional.

A rollover cannot begin, continue, or complete unless the required validation stages succeed.

---

## Purpose

The Validation Framework exists to protect league integrity.

It detects invalid state before that state can spread through the rollover pipeline.

It answers questions such as:

- Is the league eligible for rollover?
- Does every team exist?
- Are contract records valid?
- Are roster assignments legal?
- Are salary values internally consistent?
- Are draft picks valid and unique?
- Did an event produce the expected result?
- Can the next event safely execute?
- Is the final target-season state complete?
- Should execution stop?

The framework must answer these questions deterministically.

---

## Core Responsibility

The Validation Framework owns:

- Validation rule registration
- Validation rule execution
- Validation stage coordination
- Validation scope selection
- Validation result collection
- Validation severity classification
- Blocking-decision production
- Validation reporting
- Validation versioning
- Validation audit history

The Validation Framework does not own:

- Event execution
- Business-rule mutation
- Snapshot creation
- Rollback execution
- Administrative authorization
- UI rendering
- Hidden database repair
- Automatic correction of invalid data

Validation identifies invalid state.

It does not silently repair it.

---

## Validation Stages

Validation occurs at multiple points throughout a rollover.

### Request Validation

Determines whether the rollover request is structurally valid.

Examples:

- League ID exists
- Source season is valid
- Target season is valid
- Source and target seasons differ correctly
- Execution mode is supported
- Requesting user is authorized

---

### Pre-Rollover Validation

Determines whether the league is eligible to begin rollover.

Examples:

- No active rollover already exists
- League season matches the requested source season
- Required teams exist
- Required league settings exist
- Contract data is readable
- Draft data is available
- No blocking integrity errors exist

---

### Pre-Event Validation

Determines whether one event may safely execute.

Examples:

- Dependencies succeeded
- Required inputs exist
- Expected source state exists
- Event has not already executed illegally
- Required lock is active

---

### Post-Event Validation

Determines whether an event produced a legal result.

Examples:

- Contract years were reduced correctly
- Expired contracts were removed from active rosters
- Free-agent records were created
- IR designations were reset
- Taxi assignments were reset
- Draft picks advanced correctly

---

### Final Validation

Determines whether the rollover may be marked complete.

Examples:

- League current season equals target season
- Every required event succeeded
- No player exists on multiple active rosters
- No active contract has zero years remaining
- Every expired player is represented correctly
- Draft picks reference valid teams and seasons
- Salary totals reconcile
- League settings remain intact

---

## Validation Pipeline

```text
Validation Request
        │
        ▼
Resolve Validation Stage
        │
        ▼
Load Validation Rules
        │
        ▼
Build Validation Context
        │
        ▼
Execute Rules
        │
        ▼
Collect Results
        │
        ▼
Classify Severity
        │
        ▼
Produce Validation Decision
```

Every validation run must produce a structured result.

---

## Validation Rule

A Validation Rule represents one deterministic correctness check.

Example:

```python
ValidationRule(
    rule_id="contracts.no_active_zero_year_contracts",
    stage="post_event",
    scope="league",
    severity="blocking",
    version="1.0.0",
)
```

Each rule should define:

- Rule ID
- Rule version
- Validation stage
- Validation scope
- Severity
- Required inputs
- Evaluation logic
- Expected output
- Failure message
- Remediation guidance

---

## Validation Context

Every validation run executes within a Validation Context.

Example:

```python
ValidationContext(
    validation_id="validation_abc123",
    execution_id="rollover_2026_2027",
    league_id="league_001",
    source_season=2026,
    target_season=2027,
    stage="post_event",
    event_id="season.contracts.expire",
    requested_at="2026-12-31T18:00:00Z",
)
```

The Validation Context should remain immutable during the validation run.

---

## Validation Scope

Rules may execute against different scopes.

Supported scopes may include:

```text
Execution
League
Team
Player
Contract
Roster
Draft Pick
Salary
Event
Snapshot
```

A rule must declare its scope explicitly.

The framework should never infer scope from implementation details.

---

## Validation Severity

Every Validation Result must include a severity.

Recommended severities:

### Information

Provides useful execution information.

Does not affect execution.

### Warning

Identifies a non-blocking concern.

Execution may continue, but the warning must be recorded.

### Blocking

Identifies invalid state that prevents execution from continuing.

The Event Engine must stop.

### Critical

Identifies severe integrity risk or possible corruption.

Execution must stop and recovery evaluation must begin.

---

## Validation Decision

After all required rules execute, the framework produces one Validation Decision.

Recommended decisions:

```text
Passed
Passed With Warnings
Failed
Critical Failure
Unable To Validate
```

Example:

```python
ValidationDecision(
    validation_id="validation_abc123",
    status="FAILED",
    rules_executed=24,
    rules_passed=22,
    warnings=1,
    blocking_failures=1,
    may_continue=False,
)
```

The Event Engine should consume the Validation Decision rather than interpreting individual rules independently.

---

## Validation Result

Every rule execution must produce one structured Validation Result.

Example:

```python
ValidationResult(
    validation_id="validation_abc123",
    rule_id="contracts.no_active_zero_year_contracts",
    status="FAILED",
    severity="BLOCKING",
    records_examined=84,
    violations_found=3,
    message="Three active contracts have zero remaining years.",
    affected_records=[
        "contract_101",
        "contract_204",
        "contract_318",
    ],
)
```

Validation Results must be:

- Structured
- Persisted
- Auditable
- Serializable
- Human-readable
- Machine-readable

---

## Rule Registry

The Validation Rule Registry connects rule identifiers to executable validators.

Example:

```python
VALIDATION_RULE_REGISTRY = {
    "league.exists": LeagueExistsValidator,
    "teams.required_teams_exist": RequiredTeamsValidator,
    "contracts.no_negative_years": NoNegativeContractYearsValidator,
    "contracts.no_active_zero_year_contracts": NoActiveZeroYearContractsValidator,
    "rosters.no_duplicate_active_players": NoDuplicateActivePlayersValidator,
    "draft_picks.unique_ownership": UniqueDraftPickOwnershipValidator,
}
```

Every required rule must:

- Exist in the validation catalog
- Have one registered validator
- Have one version
- Return a structured result

Unknown or unregistered rules cannot execute.

---

## Rule Categories

Validation rules should be organized by domain.

Suggested categories:

```text
Execution Validation
League Validation
Team Validation
Roster Validation
Player Validation
Contract Validation
Salary Validation
Draft Validation
Free Agency Validation
Snapshot Validation
Final State Validation
```

This organization allows validation runs to target only the domains relevant to a specific stage or event.

---

## Validation Sets

A Validation Set is a named collection of rules executed together.

Examples:

```text
pre_rollover_core
pre_contract_reduction
post_contract_reduction
post_contract_expiration
post_free_agency_creation
post_roster_reset
post_draft_advance
final_rollover
```

Example:

```python
ValidationSet(
    validation_set_id="post_contract_expiration",
    rules=[
        "contracts.no_active_zero_year_contracts",
        "contracts.expired_contracts_inactive",
        "rosters.expired_players_removed",
        "free_agency.expired_players_available",
    ],
)
```

Validation Sets must be versioned and deterministic.

---

## Event Integration

Each Event Catalog entry should declare its validation requirements.

Example:

```text
Event:
season.contracts.expire

Pre-Event Validation Set:
pre_contract_expiration

Post-Event Validation Set:
post_contract_expiration
```

The Event Engine should never guess which validations apply to an event.

The association must be explicit.

---

## Failure Behavior

When validation produces a blocking or critical result:

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
        │
        ▼
Begin Recovery Evaluation
```

Validation failures must never be silently converted into warnings.

---

## Data Integrity Principles

The Validation Framework should enforce invariants such as:

- Every league has valid teams.
- Every active roster assignment references a valid player and team.
- No player has multiple active assignments in the same league.
- No active contract has zero or negative remaining years.
- Contract salary values are valid.
- Dead-cap records reconcile with contract actions.
- Draft picks reference valid seasons, rounds, and teams.
- Required league settings remain unchanged unless explicitly modified.
- Source-season data does not remain active incorrectly.
- Target-season records are complete.
- Every event result corresponds to an approved execution.

These invariants form the legal definition of league state.

---

## Determinism

The same validation inputs, rule versions, and league state must produce the same results.

Validation rules must not depend on:

- Randomness
- Non-versioned external data
- Hidden runtime assumptions
- UI state
- Undeclared environment values
- Natural-language interpretation

Validation must remain reproducible.

---

## Validation Immutability

Once a Validation Result is persisted, it should never be edited.

Corrections require:

- A new validation run
- A new validation ID
- A new result set

Historical validation evidence must remain intact.

---

## Observability

Every validation run should record:

- Validation ID
- Execution ID
- League ID
- Stage
- Event ID, when applicable
- Validation Set
- Rule versions
- Start time
- Completion time
- Duration
- Rules executed
- Passed rules
- Warnings
- Blocking failures
- Critical failures
- Final decision

An administrator should be able to determine exactly why execution was allowed or stopped.

---

## Versioning

Every validation run should record:

```text
Validation Framework Version
Validation Set Version
Rule Versions
Schema Version
```

Historical rollovers must remain auditable against the validation rules that existed at execution time.

---

## Security

Validation data may contain sensitive league-state information.

The framework should enforce:

- Authenticated access
- League-scoped authorization
- Immutable audit records
- Controlled administrative overrides
- Restricted access to affected-record details

Validation overrides should be exceptional and permanently recorded.

---

## Override Policy

Commit-mode validation should not normally be bypassed.

Where administrative overrides are supported, they must require:

- Elevated authorization
- Explicit reason
- User identity
- Timestamp
- Rule ID
- Execution ID
- Permanent audit record

Critical integrity rules should not support override unless an explicit recovery workflow permits it.

---

## Relationship to Other Systems

```text
Event Catalog
      │
      ▼
Event Engine
      │
      ▼
Validation Framework
      │
 ┌────┴───────────┐
 ▼                ▼
Continue       Stop Execution
                  │
                  ▼
           Recovery Engine
```

The Validation Framework also integrates with:

```text
Snapshot System
Audit System
Admin Tools
Execution Reporting
```

---

## Proposed Chapter Structure

```text
validation-framework/
├── README.md
├── 01-validation-philosophy.md
├── 02-validation-context.md
├── 03-validation-rule-contract.md
├── 04-validation-rule-registry.md
├── 05-validation-sets.md
├── 06-validation-pipeline.md
├── 07-validation-severity.md
├── 08-validation-results.md
├── 09-pre-rollover-validation.md
├── 10-event-validation.md
├── 11-final-state-validation.md
├── 12-validation-overrides.md
├── 13-validation-observability.md
├── 14-validation-testing.md
└── 15-validation-summary.md
```

---

## Framework Guarantees

The Validation Framework should guarantee that:

1. Every required validation rule is registered.
2. Every rule produces a structured result.
3. Every validation run has one immutable context.
4. Every blocking failure stops execution.
5. Every decision is supported by persisted rule results.
6. Every event uses explicitly declared validation sets.
7. Every final rollover passes final-state validation.
8. Every validation result is auditable.
9. Every rule version is recorded.
10. No invalid league state is silently accepted.

---

## Definition of Done

The Validation Framework is complete when it can:

- Validate rollover requests
- Validate league eligibility
- Validate individual events
- Validate post-event state
- Validate final target-season state
- Execute versioned validation sets
- Produce structured rule results
- Produce one validation decision
- Stop execution on blocking failures
- Persist complete validation history
- Support deterministic testing
- Integrate with the Event Engine
- Provide evidence for recovery and administration

At that point, the Season Rollover system will have an independent mechanism for proving that every execution begins, continues, and ends in a legal league state.
