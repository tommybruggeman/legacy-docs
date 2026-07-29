# Event Engine

## Overview

The Event Engine is the deterministic orchestration layer for the Legacy season rollover system.

It receives an approved rollover request, determines which cataloged events must run, resolves their dependencies, executes them in the correct order, records the result of every event, and stops safely when execution can no longer continue.

The Event Engine does not contain the business logic for aging players, reducing contract years, expiring contracts, resetting roster designations, advancing draft picks, or performing other season changes.

That logic belongs to individual event handlers.

The Event Engine is responsible for coordinating those handlers.

```text
Rollover Request
    ↓
Execution Context
    ↓
Plan Compilation
    ↓
Dependency Resolution
    ↓
Pre-Execution Validation
    ↓
Ordered Event Dispatch
    ↓
Event Results
    ↓
Post-Execution Validation
    ↓
Completed Rollover
```

---

## Core Responsibility

The Event Engine owns the lifecycle of a rollover execution.

It must answer the following questions:

1. Which league is being rolled over?
2. Which season is the league leaving?
3. Which season is the league entering?
4. Which events are required?
5. Which events are optional?
6. In what order must the events execute?
7. Which handler implements each event?
8. Can the event safely run?
9. Did the event succeed?
10. Can the rollover continue after the result?
11. Can an interrupted rollover resume safely?
12. Is the final league state valid?

The Event Engine must answer these questions deterministically.

The same valid starting state, event plan, and engine version should produce the same execution outcome.

---

## Engine Boundary

The Event Engine owns:

* execution context creation
* event registration
* event-plan compilation
* dependency resolution
* execution ordering
* event dispatch
* event status transitions
* event result collection
* stop and failure policies
* idempotency enforcement
* resume coordination
* execution logging
* final execution summaries

The Event Engine does not own:

* event-specific business rules
* direct UI rendering
* league configuration editing
* manual database repair
* player valuation
* trade evaluation
* free-agent recommendations
* natural-language explanations
* season-specific assumptions not declared in the execution context
* hidden modifications outside approved event handlers

This separation prevents orchestration logic from becoming mixed with domain logic.

---

## Primary Design Principle

The engine coordinates events.

It does not improvise them.

Every event executed by the engine must:

1. Exist in the Event Catalog.
2. Have a registered handler.
3. Declare its dependencies.
4. Declare its expected inputs.
5. Declare its permitted writes.
6. Return a structured result.
7. Support the required idempotency policy.
8. Participate in validation and audit logging.

An event that does not satisfy these requirements must not execute.

---

## High-Level Execution Flow

A rollover begins with a request.

```text
league_id
source_season
target_season
requested_by
execution_mode
selected_options
```

The engine converts that request into an immutable execution context.

The context is then used to compile an event plan.

```text
ExecutionContext
    ↓
Catalog Selection
    ↓
EventPlan
    ↓
Dependency Graph
    ↓
Ordered Execution Plan
```

Before dispatching the first event, the engine validates that:

* the league exists
* the source season is correct
* the target season is valid
* no conflicting rollover is active
* every planned event exists
* every event has a registered handler
* all dependencies can be resolved
* the execution graph contains no cycles
* required snapshots and locks are available
* the league is eligible for rollover

The engine then executes events one at a time in the approved order.

```text
Event 1
    ↓
Validate
    ↓
Execute
    ↓
Record Result
    ↓
Validate Outcome
    ↓
Event 2
```

Execution continues until:

* every event succeeds
* an event returns a blocking failure
* validation fails
* the execution is manually cancelled
* the engine encounters an unrecoverable system error

---

## Execution Context

Every rollover run must have one authoritative execution context.

The execution context provides the stable facts required by the engine and its handlers.

Example:

```python
ExecutionContext(
    execution_id="rollover_2026_to_2027_abc123",
    league_id="league_123",
    source_season=2026,
    target_season=2027,
    requested_by="user_456",
    execution_mode="commit",
    started_at="2026-12-31T18:00:00Z",
    options={
        "reset_ir": True,
        "reset_taxi": True,
        "advance_draft_picks": True,
    },
)
```

The execution context should be immutable after execution begins.

Handlers may read from the context, but they should not rewrite its identity, seasons, mode, or authorization data.

Runtime information that changes during execution belongs in event results or execution state records rather than in the original context.

---

## Execution Modes

The engine should support at least two execution modes.

### Dry Run

A dry run evaluates the rollover without permanently changing production league state.

It should:

* compile the complete plan
* resolve dependencies
* validate preconditions
* execute handlers through dry-run-safe interfaces
* calculate expected changes
* return warnings and failures
* produce a projected execution summary
* avoid permanent domain-state mutations

```text
execution_mode = "dry_run"
```

### Commit

A commit run performs the approved rollover against persistent league state.

It should:

* require authorization
* require a valid pre-rollover snapshot
* acquire the required execution lock
* persist all approved mutations
* record every event result
* stop immediately on blocking failure
* run final validation before marking the rollover complete

```text
execution_mode = "commit"
```

Dry-run and commit modes must use the same event definitions and ordering rules.

The engine must not maintain an unrelated simulation pipeline that can drift away from production behavior.

---

## Event Registry

The Event Registry connects catalog event identifiers to executable handlers.

Example:

```python
EVENT_REGISTRY = {
    "season.snapshot.create": CreateSnapshotHandler,
    "season.players.age": AgePlayersHandler,
    "season.contracts.reduce_years": ReduceContractYearsHandler,
    "season.contracts.expire": ExpireContractsHandler,
    "season.free_agents.create": CreateFreeAgentsHandler,
    "season.rosters.reset_ir": ResetIRHandler,
    "season.rosters.reset_taxi": ResetTaxiHandler,
    "season.draft_picks.advance": AdvanceDraftPicksHandler,
    "season.finalize": FinalizeSeasonHandler,
}
```

The registry must not silently substitute one handler for another.

If a catalog event has no registered handler, plan validation must fail before execution begins.

The registry should support explicit handler versioning so historical rollover executions can identify which implementation was used.

Example:

```text
event_id: season.contracts.expire
handler_version: 1.0.0
```

---

## Event Plan

The Event Plan is the complete list of events approved for one execution.

Each plan item should contain at least:

```python
PlannedEvent(
    event_id="season.contracts.expire",
    sequence=40,
    handler_version="1.0.0",
    required=True,
    dependencies=[
        "season.contracts.reduce_years",
    ],
    execution_policy="once",
)
```

The plan should be compiled before the first event executes.

Once commit execution begins, the plan should not change unless the engine enters an explicit administrative recovery workflow.

This prevents the rollover from making decisions midway through execution based on undocumented conditions.

---

## Dependency Resolution

Events must execute according to declared dependencies rather than file order, dictionary order, or incidental implementation details.

Example:

```text
ReduceContractYears
    ↓
ExpireContracts
    ↓
CreateFreeAgents
```

The engine must prevent `CreateFreeAgents` from running before `ExpireContracts`.

Dependencies should form a directed acyclic graph.

Before execution, the engine must detect:

* missing dependencies
* circular dependencies
* duplicate events
* incompatible events
* invalid ordering overrides
* required events excluded from the plan

If the graph is invalid, the rollover must not begin.

---

## Dispatch and Execution

The dispatcher invokes the registered handler for each planned event.

A handler should receive a controlled interface rather than unrestricted application access.

Example:

```python
result = dispatcher.execute(
    planned_event=planned_event,
    context=execution_context,
    services=authorized_services,
)
```

The engine should wrap every handler execution with:

1. precondition validation
2. event-start logging
3. idempotency verification
4. handler invocation
5. result validation
6. postcondition validation
7. event-result persistence
8. execution-state update

The handler must return a structured event result.

It must not communicate success solely through logs or the absence of an exception.

---

## Event Results

Every event must produce a structured result.

Example:

```python
EventResult(
    event_id="season.contracts.expire",
    status="succeeded",
    started_at="2026-12-31T18:01:10Z",
    completed_at="2026-12-31T18:01:12Z",
    records_examined=84,
    records_changed=11,
    warnings=[],
    errors=[],
    output={
        "expired_contract_ids": [
            "contract_1",
            "contract_2",
        ],
    },
)
```

Supported statuses should include:

```text
pending
running
succeeded
succeeded_with_warnings
skipped
blocked
failed
rolled_back
```

The result must distinguish between:

* an event that succeeded and changed zero records
* an event that was skipped
* an event that could not run
* an event that partially executed
* an event that failed before mutation
* an event that failed after mutation

Zero changed records must not automatically be treated as a failure.

Some leagues may legitimately have no records affected by a specific event.

---

## Failure Policy

The Event Engine must fail closed.

When the engine cannot prove that continuing is safe, it must stop.

Blocking conditions include:

* unknown event
* missing handler
* failed dependency
* invalid execution context
* failed precondition
* unhandled exception
* invalid event result
* postcondition failure
* database consistency failure
* execution lock loss
* unauthorized mutation
* snapshot failure
* idempotency conflict

A failed event must not be silently converted into a warning.

The engine should preserve:

* the failed event
* the failure category
* the error message
* the handler version
* the input context
* the event start and stop times
* mutations completed before failure
* the last valid snapshot
* the recommended recovery action

---

## Stop Behavior

When a blocking failure occurs, the engine must:

1. Stop dispatching new events.
2. Mark the current event as failed or blocked.
3. Mark remaining dependent events as blocked.
4. Preserve already recorded results.
5. release or retain locks according to recovery policy
6. trigger rollback evaluation
7. produce an execution failure summary
8. require an explicit recovery decision before continuing

The engine must never continue through a rollover merely to produce a completed status.

A partially updated league must not be presented as successfully rolled over.

---

## Idempotency

Each event must declare how repeated execution is handled.

Common policies include:

```text
once
safe_repeat
resume_only
replace_previous
```

### Once

The event may execute only once for the execution and target season.

### Safe Repeat

The event may run multiple times and produce the same final state.

### Resume Only

The event may resume an incomplete prior attempt but may not restart from the beginning without recovery preparation.

### Replace Previous

The event may replace a prior generated result when explicitly permitted.

The engine must enforce these policies centrally.

Handlers should also protect themselves against duplicate writes through database constraints, execution identifiers, or mutation ledgers.

---

## Resume Behavior

An interrupted rollover should not automatically begin again from Event 1.

The engine should inspect the persisted execution state.

```text
Event 1: succeeded
Event 2: succeeded
Event 3: failed
Event 4: pending
```

A resume workflow must determine:

* whether Event 3 can safely resume
* whether Event 3 must be rolled back first
* whether completed events remain valid
* whether the execution context has changed
* whether the league state has been manually modified
* whether a new plan compilation is required
* whether the original snapshot is still available

Resume must be an explicit engine capability rather than an accidental rerun of the same function.

---

## Concurrency Control

Only one mutating season rollover should run for a league at a time.

The engine must acquire a league-scoped execution lock before commit execution.

Example:

```text
lock_key = league:{league_id}:season_rollover
```

The lock should prevent:

* two administrators starting rollover simultaneously
* a retry overlapping with the original execution
* separate workers executing the same plan
* conflicting season-management operations
* event handlers being dispatched twice

Dry runs may use a different concurrency policy, but they must not interfere with active commit executions.

---

## Transaction Boundaries

The entire season rollover may be too large or operationally complex for one database transaction.

The engine should therefore define transaction boundaries explicitly.

At minimum:

* each event should have a known transaction strategy
* event results should persist independently
* snapshot creation should occur before mutating events
* partial event mutations must be detectable
* rollback requirements must be declared
* no event should assume the entire rollover shares one open transaction

Where possible, an individual event should execute atomically.

When an event cannot be atomic, it must maintain enough mutation metadata to support recovery.

---

## Validation Integration

Validation occurs at multiple levels.

### Before the Plan

Validate the rollover request and league eligibility.

### Before Each Event

Validate event preconditions and dependency results.

### After Each Event

Validate event-specific postconditions.

### After the Full Plan

Validate the complete league state.

Example final validations:

```text
No player has more than one active team assignment.

No active contract has zero remaining years.

Every expired player is represented correctly in free agency.

Draft picks point to valid seasons and teams.

IR and taxi designations satisfy target-season rules.

League settings were preserved unless an event explicitly changed them.

The league current season equals the target season.

Every planned required event has a successful terminal result.
```

The engine must not mark an execution complete until final validation succeeds.

---

## Observability

Every execution must be inspectable.

The engine should produce:

* execution ID
* league ID
* source season
* target season
* requestor
* mode
* engine version
* plan version
* event list
* event statuses
* event durations
* records examined
* records changed
* warnings
* errors
* snapshots created
* validation results
* final execution status

Logs should help an administrator answer:

```text
What ran?

Why did it run?

In what order did it run?

What changed?

What failed?

Where did execution stop?

Can it safely resume?

Which snapshot can restore the league?
```

---

## Execution Status

The rollover execution itself should have a lifecycle separate from individual event statuses.

Recommended execution statuses:

```text
requested
planning
plan_failed
ready
running
paused
failed
rollback_required
rolling_back
rolled_back
validating
completed
completed_with_warnings
cancelled
```

A rollover is complete only when:

* every required event has an acceptable terminal status
* final validation succeeds
* the target season is active
* the execution summary is persisted
* the execution lock is released

---

## Security and Authorization

The Event Engine performs high-impact league mutations.

Commit execution must require explicit authorization.

The engine should verify:

* authenticated user identity
* league membership
* commissioner or administrator role
* permission to manage the specified league
* permission to initiate season rollover
* permission to retry, resume, skip, or roll back events

Administrative override operations should be more restricted than ordinary rollover execution.

Every override must be recorded in the audit log.

---

## Example Engine Interface

A future engine interface may resemble:

```python
engine = SeasonRolloverEngine(
    registry=event_registry,
    plan_compiler=plan_compiler,
    dependency_resolver=dependency_resolver,
    dispatcher=event_dispatcher,
    validator=rollover_validator,
    snapshot_service=snapshot_service,
    execution_repository=execution_repository,
)

execution = engine.prepare(
    league_id="league_123",
    source_season=2026,
    target_season=2027,
    requested_by="user_456",
    mode="dry_run",
)

report = engine.run(execution.id)
```

For commit mode:

```python
execution = engine.prepare(
    league_id="league_123",
    source_season=2026,
    target_season=2027,
    requested_by="user_456",
    mode="commit",
)

result = engine.run(execution.id)
```

Preparation and execution should remain separate so the plan can be reviewed before permanent mutations begin.

---

## Engine Guarantees

The Event Engine should guarantee that:

1. No unregistered event executes.
2. No event executes before its dependencies.
3. No event executes outside the approved plan.
4. No handler silently determines rollover order.
5. No failure is hidden as success.
6. No completed execution lacks an audit record.
7. No commit run begins without the required lock.
8. No rollover is marked complete before final validation.
9. No resume occurs without inspecting prior execution state.
10. No repeated event bypasses its idempotency policy.
11. No event result exists without an execution ID.
12. No season transition relies on undocumented ordering.

---

## Relationship to Other Systems

The Event Engine depends on the Event Catalog.

```text
Event Catalog
    ↓
Event Engine
    ↓
Event Handlers
```

The engine will later integrate with:

```text
Validation Framework
Snapshot System
Recovery Engine
Audit System
Admin Tools
```

The complete architecture becomes:

```text
Rollover Request
    ↓
Event Catalog
    ↓
Event Engine
    ↓
Event Handlers
    ↓
Validation
    ↓
Snapshots and Recovery
    ↓
Audit and Administration
```

---

## Definition of Done

The Event Engine is complete when it can:

* receive a valid rollover request
* create an immutable execution context
* compile an event plan from the catalog
* resolve the complete dependency graph
* reject invalid or circular plans
* validate every registered handler
* execute events in deterministic order
* persist structured results
* stop on blocking failure
* enforce event idempotency
* support safe execution resume
* maintain league-scoped concurrency locks
* integrate with snapshots and validation
* produce a complete execution report
* mark the rollover complete only after final validation

At that point, the Event Catalog will no longer be documentation alone.

It will become the authoritative source used by a safe, deterministic, and auditable season rollover engine.
