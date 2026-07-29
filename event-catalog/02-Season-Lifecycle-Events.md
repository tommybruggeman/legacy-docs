---
title: Season Lifecycle Events
document: Event Catalog
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - README.md
  - 01-League-Lifecycle-Events.md
  - ../domain-model/02-League-Season.md
  - ../domain-model/03-League-Phase.md
  - ../domain-model/21-League-Configuration.md
---

# Season Lifecycle Events

## Purpose

This chapter defines the canonical events that describe the lifecycle of a League Season and its operational phases.

Season lifecycle events establish:

- When a season is scheduled
- When a season becomes active
- Which phase governs League activity
- When phases begin and end
- When a season completes
- When annual rollover occurs
- How seasonal state advances without rewriting history

These events are critical because many League rules depend on the current season and phase.

Examples include:

- Contract-year reduction
- Free-agent expiration
- Rookie Draft availability
- Trade windows
- Waiver processing
- Roster deadlines
- Taxi and IR reset behavior
- Draft Pick advancement
- Dead-cap progression

---

# Scope

This chapter defines:

- `LeagueSeasonScheduled`
- `LeagueSeasonStarted`
- `LeagueSeasonCompleted`
- `LeagueSeasonAdvanced`
- `LeaguePhaseCreated`
- `LeaguePhaseActivated`
- `LeaguePhaseCompleted`
- `LeaguePhaseAdvanced`
- `SeasonRolloverStarted`
- `SeasonRolloverCompleted`
- `SeasonRolloverFailed`

---

# General Invariants

1. Every League Season belongs to exactly one League.
2. A League may not have two active seasons covering the same effective time.
3. A League Phase belongs to exactly one League Season.
4. Only one mutually exclusive operational phase may be active when the phase model requires exclusivity.
5. Completed seasons and phases are immutable historical records.
6. Season advancement shall not rewrite prior season values.
7. Annual rollover must be atomic or recoverable.
8. Failed rollover shall not be represented as completed.
9. Contract, Draft Pick, roster, and financial changes caused by rollover must be represented by their own events.
10. Season lifecycle events coordinate workflows but do not replace entity-specific state-change events.

---

# LeagueSeasonScheduled

## Purpose

`LeagueSeasonScheduled` records the creation of a future or upcoming League Season.

Scheduling establishes the intended season identity and calendar without making the season active.

## Event Name

`LeagueSeasonScheduled`

## Category

Season Lifecycle

## Owning Aggregate

LeagueSeason

## Trigger

The event is emitted when a valid `ScheduleLeagueSeason` Command succeeds.

## Preconditions

- The League exists.
- The League is not permanently closed.
- The actor has season administration authority.
- The season year or sequence is valid.
- No duplicate season identity exists for the League.
- The proposed date range does not create an invalid overlap.
- An applicable League Configuration exists or is scheduled.

## Required Payload

```json
{
  "season_id": "uuid",
  "league_id": "uuid",
  "season_year": 2027,
  "season_sequence": 2,
  "status": "scheduled",
  "scheduled_start_at": "2027-01-01T00:00:00Z",
  "scheduled_end_at": "2027-12-31T23:59:59Z",
  "configuration_id": "uuid",
  "scheduled_by_user_id": "uuid"
}
```

## Optional Payload

```json
{
  "prior_season_id": "uuid",
  "external_season_identifier": "2027",
  "initial_phase_id": "uuid"
}
```

## Referenced Entities

- League Season
- League
- League Configuration
- Prior League Season
- League Phase
- User

## Permitted Actors

- Commissioner
- Authorized League Administrator
- Authorized System Scheduler

## Consumers

- Season projections
- Draft planning
- Contract services
- Scheduler
- Notification services
- AI evidence builders
- Audit systems

## Ordering Requirements

The event must occur after `LeagueCreated`.

When a prior season exists, `prior_season_id` should identify it.

## Idempotency Requirements

Scheduling the same season with the same idempotency key shall not create duplicate seasons.

## Replay Behavior

Replay reconstructs the scheduled season record.

Replay shall not start the season or execute rollover.

## Versioning

Current version:

```text
1
```

## Invariants

- `season_id` is unique.
- `season_year` is valid for the League.
- The configuration belongs to the same League.
- Scheduling does not activate the season.
- Scheduling does not reduce contract years.
- Scheduling does not advance Draft Picks.

## AI Interpretation

The AI may use this event to understand future season planning.

It shall not treat a scheduled season as the current season until `LeagueSeasonStarted` occurs.

## Example

```json
{
  "event_type": "LeagueSeasonScheduled",
  "event_version": 1,
  "aggregate_type": "LeagueSeason",
  "aggregate_id": "8019c956-c18d-4561-9c63-c14a15c79013",
  "payload": {
    "season_id": "8019c956-c18d-4561-9c63-c14a15c79013",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "season_year": 2027,
    "season_sequence": 2,
    "status": "scheduled",
    "scheduled_start_at": "2027-01-01T00:00:00Z",
    "scheduled_end_at": "2027-12-31T23:59:59Z",
    "configuration_id": "19879056-2e68-46fa-a23d-47676bc926e3",
    "scheduled_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9"
  }
}
```

---

# LeagueSeasonStarted

## Purpose

`LeagueSeasonStarted` records that a scheduled League Season became the active season.

This event establishes the season context used by downstream rules and transactions.

## Event Name

`LeagueSeasonStarted`

## Category

Season Lifecycle

## Owning Aggregate

LeagueSeason

## Trigger

The event is emitted when a valid `StartLeagueSeason` Command succeeds.

It may occur after a completed annual rollover.

## Preconditions

- The League exists and is operational.
- The season exists.
- The season is scheduled or ready.
- No conflicting season is active.
- The governing League Configuration is active.
- Required prior-season completion or rollover conditions are satisfied.
- The initial phase is valid.
- The actor or system workflow is authorized.

## Required Payload

```json
{
  "season_id": "uuid",
  "league_id": "uuid",
  "season_year": 2027,
  "previous_status": "scheduled",
  "new_status": "active",
  "configuration_id": "uuid",
  "initial_phase_id": "uuid",
  "started_at": "2027-01-01T00:00:00Z"
}
```

## Optional Payload

```json
{
  "prior_season_id": "uuid",
  "rollover_correlation_id": "uuid",
  "started_by_user_id": "uuid"
}
```

## Referenced Entities

- League Season
- League
- League Configuration
- League Phase
- Prior League Season
- User

## Permitted Actors

- Commissioner
- Authorized League Administrator
- Authorized Season Automation

## Related Events

A season start may correlate with:

- `SeasonRolloverCompleted`
- `LeaguePhaseActivated`
- `TransactionRecorded`
- `AuditEventCreated`

## Consumers

- Contract calculations
- Roster validation
- Draft services
- Trade services
- Waiver processing
- Financial projections
- AI context builders
- Application dashboards

## Ordering Requirements

When rollover is required, `SeasonRolloverCompleted` must occur before or atomically with `LeagueSeasonStarted`.

The initial phase must be activated consistently with the season start.

## Idempotency Requirements

An active season shall not be started twice.

## Replay Behavior

Replay marks the season active and restores its start context.

Replay shall not reapply contract reductions or roster resets.

Those effects must be reconstructed from their own events.

## Versioning

Current version:

```text
1
```

## Invariants

- Only one season is active for the League.
- The configuration belongs to the League.
- The initial phase belongs to the season.
- Starting the season does not itself imply that every annual rollover mutation occurred.
- Rollover completeness must be established independently.

## AI Interpretation

This event is strong evidence for current-season determination.

The AI should still evaluate later season completion or advancement events before treating this season as current.

## Example

```json
{
  "event_type": "LeagueSeasonStarted",
  "event_version": 1,
  "aggregate_type": "LeagueSeason",
  "aggregate_id": "8019c956-c18d-4561-9c63-c14a15c79013",
  "payload": {
    "season_id": "8019c956-c18d-4561-9c63-c14a15c79013",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "season_year": 2027,
    "previous_status": "scheduled",
    "new_status": "active",
    "configuration_id": "19879056-2e68-46fa-a23d-47676bc926e3",
    "initial_phase_id": "d77a844a-1988-4884-a7a0-32ac615b5440",
    "started_at": "2027-01-01T00:00:00Z"
  }
}
```

---

# LeagueSeasonCompleted

## Purpose

`LeagueSeasonCompleted` records that all required activities for a League Season finished and the season became historical.

## Event Name

`LeagueSeasonCompleted`

## Category

Season Lifecycle

## Owning Aggregate

LeagueSeason

## Trigger

The event is emitted when a valid `CompleteLeagueSeason` Command succeeds.

## Preconditions

- The season exists.
- The season is active.
- Required competition outcomes are finalized.
- Required phases are complete.
- Pending transactions that block completion are resolved.
- Required season-end validations pass.
- The actor or system is authorized.

## Required Payload

```json
{
  "season_id": "uuid",
  "league_id": "uuid",
  "season_year": 2026,
  "previous_status": "active",
  "new_status": "completed",
  "completed_at": "2026-12-31T23:59:59Z"
}
```

## Optional Payload

```json
{
  "champion_franchise_id": "uuid",
  "final_phase_id": "uuid",
  "completed_by_user_id": "uuid",
  "next_season_id": "uuid"
}
```

## Referenced Entities

- League Season
- League
- Franchise
- League Phase
- Next League Season
- User

## Permitted Actors

- Commissioner
- Authorized League Administrator
- Authorized Season Automation

## Consumers

- Historical standings
- Draft order services
- Contract rollover preparation
- Analytics
- Notifications
- AI historical analysis
- Audit systems

## Ordering Requirements

All required `LeaguePhaseCompleted` events must precede season completion.

## Idempotency Requirements

A completed season shall not emit another completion event.

## Replay Behavior

Replay marks the season completed.

Replay shall not re-award championships, recreate Draft Picks, or trigger rollover automatically.

## Versioning

Current version:

```text
1
```

## Invariants

- Completed seasons are immutable.
- Completion does not delete active contracts.
- Completion does not itself reduce contract years.
- Completion does not automatically begin the next season.
- Championship identity, when present, references a Franchise in the same League.

## AI Interpretation

The AI may treat the season's outcomes as final.

It should not assume that annual rollover has completed merely because the season ended.

## Example

```json
{
  "event_type": "LeagueSeasonCompleted",
  "event_version": 1,
  "aggregate_type": "LeagueSeason",
  "aggregate_id": "f3e7de6d-a95e-4d41-9659-c40fe47d3cf4",
  "payload": {
    "season_id": "f3e7de6d-a95e-4d41-9659-c40fe47d3cf4",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "season_year": 2026,
    "previous_status": "active",
    "new_status": "completed",
    "completed_at": "2026-12-31T23:59:59Z",
    "champion_franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177"
  }
}
```

---

# LeagueSeasonAdvanced

## Purpose

`LeagueSeasonAdvanced` records the League's canonical transition from one season context to the next.

This is a coordination event.

It does not replace the detailed events produced by annual rollover.

## Event Name

`LeagueSeasonAdvanced`

## Category

Season Lifecycle

## Owning Aggregate

LeagueSeason

## Trigger

The event is emitted after all required season-transition steps complete successfully.

## Preconditions

- The prior season is complete.
- The next season exists.
- Annual rollover has completed when required.
- The next season is valid.
- No conflicting season remains active.
- The transition has passed validation.

## Required Payload

```json
{
  "league_id": "uuid",
  "previous_season_id": "uuid",
  "previous_season_year": 2026,
  "new_season_id": "uuid",
  "new_season_year": 2027,
  "advanced_at": "2027-01-01T00:00:00Z"
}
```

## Optional Payload

```json
{
  "rollover_correlation_id": "uuid",
  "advanced_by_user_id": "uuid"
}
```

## Referenced Entities

- League
- Previous League Season
- New League Season
- User

## Permitted Actors

- Authorized Season Automation
- Commissioner
- Authorized League Administrator

## Consumers

- Current-season projections
- Application routing
- Contract services
- Draft services
- Reporting
- AI evidence builders

## Ordering Requirements

This event must occur after:

- `LeagueSeasonCompleted` for the prior season
- `SeasonRolloverCompleted`, when required

It may occur before or atomically with `LeagueSeasonStarted` for the new season depending on the implementation contract.

## Idempotency Requirements

The same prior-to-next season transition shall not be recorded twice.

## Replay Behavior

Replay updates the League's projected season pointer.

Replay shall not repeat rollover mutations.

## Versioning

Current version:

```text
1
```

## Invariants

- Previous and new seasons differ.
- Both seasons belong to the same League.
- The new season follows the prior season under League sequencing rules.
- Advancement is not evidence of every individual rollover change unless those events also exist.

## AI Interpretation

The AI may use this event to resolve the League's season transition.

Detailed contract, roster, Draft Pick, and financial effects must be confirmed through entity-specific events or current state.

## Example

```json
{
  "event_type": "LeagueSeasonAdvanced",
  "event_version": 1,
  "aggregate_type": "LeagueSeason",
  "aggregate_id": "8019c956-c18d-4561-9c63-c14a15c79013",
  "payload": {
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "previous_season_id": "f3e7de6d-a95e-4d41-9659-c40fe47d3cf4",
    "previous_season_year": 2026,
    "new_season_id": "8019c956-c18d-4561-9c63-c14a15c79013",
    "new_season_year": 2027,
    "advanced_at": "2027-01-01T00:00:00Z"
  }
}
```

---

# LeaguePhaseCreated

## Purpose

`LeaguePhaseCreated` records the creation of a phase within a League Season.

Examples include:

- Offseason
- Rookie Draft
- Preseason
- Regular Season
- Trade Deadline
- Playoffs
- Season Close
- Contract Processing
- Free Agency

## Event Name

`LeaguePhaseCreated`

## Category

Season Phase

## Owning Aggregate

LeaguePhase

## Trigger

The event is emitted when a valid `CreateLeaguePhase` Command succeeds.

## Preconditions

- The League Season exists.
- The phase type is supported.
- The phase sequence is valid.
- The phase schedule does not create an invalid overlap.
- The actor or system is authorized.

## Required Payload

```json
{
  "phase_id": "uuid",
  "season_id": "uuid",
  "league_id": "uuid",
  "phase_type": "offseason",
  "phase_sequence": 1,
  "status": "scheduled",
  "scheduled_start_at": "2027-01-01T00:00:00Z",
  "scheduled_end_at": "2027-04-30T23:59:59Z"
}
```

## Referenced Entities

- League Phase
- League Season
- League

## Permitted Actors

- Commissioner
- Authorized League Administrator
- Authorized Season Scheduler

## Consumers

- Rule evaluation
- Transaction windows
- Application navigation
- Scheduling
- AI context builders

## Ordering Requirements

The event must occur after `LeagueSeasonScheduled` or within the same atomic scheduling workflow.

## Idempotency Requirements

The same phase identity shall not be created twice.

## Replay Behavior

Replay reconstructs the phase schedule.

## Versioning

Current version:

```text
1
```

## Invariants

- The phase belongs to exactly one season.
- The phase sequence is valid.
- Creating a phase does not activate it.
- The phase type determines applicable rules only after activation.

## AI Interpretation

The AI may use scheduled phases for future planning but must use the active phase for current legality.

## Example

```json
{
  "event_type": "LeaguePhaseCreated",
  "event_version": 1,
  "aggregate_type": "LeaguePhase",
  "aggregate_id": "d77a844a-1988-4884-a7a0-32ac615b5440",
  "payload": {
    "phase_id": "d77a844a-1988-4884-a7a0-32ac615b5440",
    "season_id": "8019c956-c18d-4561-9c63-c14a15c79013",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "phase_type": "offseason",
    "phase_sequence": 1,
    "status": "scheduled",
    "scheduled_start_at": "2027-01-01T00:00:00Z",
    "scheduled_end_at": "2027-04-30T23:59:59Z"
  }
}
```

---

# LeaguePhaseActivated

## Purpose

`LeaguePhaseActivated` records that a League Phase became the current operational phase.

The active phase determines which time-dependent rules apply.

## Event Name

`LeaguePhaseActivated`

## Category

Season Phase

## Owning Aggregate

LeaguePhase

## Trigger

The event is emitted when a valid `ActivateLeaguePhase` Command succeeds.

## Preconditions

- The phase exists.
- The season is active or is starting atomically.
- The phase belongs to the active season.
- Any required preceding phase is complete.
- No incompatible phase is active.
- The actor or system is authorized.

## Required Payload

```json
{
  "phase_id": "uuid",
  "season_id": "uuid",
  "league_id": "uuid",
  "phase_type": "offseason",
  "previous_status": "scheduled",
  "new_status": "active",
  "activated_at": "2027-01-01T00:00:00Z"
}
```

## Optional Payload

```json
{
  "previous_phase_id": "uuid",
  "activated_by_user_id": "uuid"
}
```

## Consumers

- Rule evaluation
- Trade services
- Waiver services
- Draft services
- Contract services
- Roster validation
- AI evidence builders
- Notifications

## Ordering Requirements

The preceding phase should be completed before the new phase activates unless overlapping phases are explicitly permitted.

## Idempotency Requirements

An active phase shall not be activated twice.

## Replay Behavior

Replay restores the active-phase projection.

Replay shall not reopen external transaction windows as a side effect.

## Versioning

Current version:

```text
1
```

## Invariants

- The phase belongs to the active season.
- The active phase's configuration is valid.
- Activation does not complete the previous phase unless a corresponding completion event exists.
- Phase activation cannot retroactively legalize prior actions.

## AI Interpretation

This event is authoritative evidence for phase-dependent rules at and after the effective time, subject to later completion or advancement.

## Example

```json
{
  "event_type": "LeaguePhaseActivated",
  "event_version": 1,
  "aggregate_type": "LeaguePhase",
  "aggregate_id": "d77a844a-1988-4884-a7a0-32ac615b5440",
  "payload": {
    "phase_id": "d77a844a-1988-4884-a7a0-32ac615b5440",
    "season_id": "8019c956-c18d-4561-9c63-c14a15c79013",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "phase_type": "offseason",
    "previous_status": "scheduled",
    "new_status": "active",
    "activated_at": "2027-01-01T00:00:00Z"
  }
}
```

---

# LeaguePhaseCompleted

## Purpose

`LeaguePhaseCompleted` records that the activities governed by a League Phase finished.

## Event Name

`LeaguePhaseCompleted`

## Category

Season Phase

## Owning Aggregate

LeaguePhase

## Trigger

The event is emitted when a valid `CompleteLeaguePhase` Command succeeds.

## Preconditions

- The phase exists.
- The phase is active.
- Required phase-specific workflows are complete.
- Blocking transactions are resolved.
- The actor or system is authorized.

## Required Payload

```json
{
  "phase_id": "uuid",
  "season_id": "uuid",
  "league_id": "uuid",
  "phase_type": "rookie_draft",
  "previous_status": "active",
  "new_status": "completed",
  "completed_at": "2027-05-10T23:00:00Z"
}
```

## Optional Payload

```json
{
  "next_phase_id": "uuid",
  "completed_by_user_id": "uuid"
}
```

## Consumers

- Rule evaluation
- Phase scheduling
- Draft services
- Transaction windows
- AI evidence builders
- Audit systems

## Ordering Requirements

All required phase workflows must complete first.

For example, a Rookie Draft phase ordinarily requires `RookieDraftCompleted`.

## Idempotency Requirements

A completed phase shall not emit another completion event.

## Replay Behavior

Replay marks the phase completed.

## Versioning

Current version:

```text
1
```

## Invariants

- Completed phases are historical.
- Completion does not activate the next phase.
- Phase-specific state changes require their own events.
- The phase belongs to the specified season and League.

## AI Interpretation

The AI may treat phase-governed deadlines and activities as closed after this event unless later correction events state otherwise.

## Example

```json
{
  "event_type": "LeaguePhaseCompleted",
  "event_version": 1,
  "aggregate_type": "LeaguePhase",
  "aggregate_id": "442ecac6-5ab7-4f46-b116-a7e003e0ec4c",
  "payload": {
    "phase_id": "442ecac6-5ab7-4f46-b116-a7e003e0ec4c",
    "season_id": "8019c956-c18d-4561-9c63-c14a15c79013",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "phase_type": "rookie_draft",
    "previous_status": "active",
    "new_status": "completed",
    "completed_at": "2027-05-10T23:00:00Z"
  }
}
```

---

# LeaguePhaseAdvanced

## Purpose

`LeaguePhaseAdvanced` records the coordinated transition from one League Phase to another.

## Event Name

`LeaguePhaseAdvanced`

## Category

Season Phase

## Owning Aggregate

LeaguePhase

## Trigger

The event is emitted after the prior phase completes and the next phase becomes authoritative.

## Preconditions

- Both phases exist.
- Both belong to the same season.
- The prior phase is complete.
- The new phase is valid.
- The transition follows configured phase order.

## Required Payload

```json
{
  "league_id": "uuid",
  "season_id": "uuid",
  "previous_phase_id": "uuid",
  "previous_phase_type": "rookie_draft",
  "new_phase_id": "uuid",
  "new_phase_type": "preseason",
  "advanced_at": "2027-05-11T00:00:00Z"
}
```

## Consumers

- Current-phase projections
- Rule evaluation
- Scheduling
- Application navigation
- AI context builders

## Ordering Requirements

This event ordinarily follows:

- `LeaguePhaseCompleted`
- `LeaguePhaseActivated`

for the applicable phases.

## Idempotency Requirements

The same phase transition shall not be recorded twice.

## Replay Behavior

Replay updates the projected current phase.

## Versioning

Current version:

```text
1
```

## Invariants

- Previous and new phases differ.
- Both phases belong to the same season.
- The phase sequence is valid.
- The transition does not substitute for completion and activation events.

## AI Interpretation

The AI may use this event for convenient current-phase resolution but should retain the underlying completion and activation events for full auditability.

## Example

```json
{
  "event_type": "LeaguePhaseAdvanced",
  "event_version": 1,
  "aggregate_type": "LeaguePhase",
  "aggregate_id": "83a90629-e2f1-4396-912b-2af23c656607",
  "payload": {
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "season_id": "8019c956-c18d-4561-9c63-c14a15c79013",
    "previous_phase_id": "442ecac6-5ab7-4f46-b116-a7e003e0ec4c",
    "previous_phase_type": "rookie_draft",
    "new_phase_id": "83a90629-e2f1-4396-912b-2af23c656607",
    "new_phase_type": "preseason",
    "advanced_at": "2027-05-11T00:00:00Z"
  }
}
```

---

# SeasonRolloverStarted

## Purpose

`SeasonRolloverStarted` records the beginning of the controlled annual rollover workflow.

This event establishes a correlation boundary for all seasonal mutations.

## Event Name

`SeasonRolloverStarted`

## Category

Season Transition

## Owning Aggregate

LeagueSeason

## Trigger

The event is emitted when a valid `StartSeasonRollover` Command succeeds.

## Preconditions

- The prior season is complete.
- The next season is scheduled.
- No rollover is already active for the same transition.
- Required backups or validation checks have completed.
- The actor or system is authorized.

## Required Payload

```json
{
  "league_id": "uuid",
  "prior_season_id": "uuid",
  "prior_season_year": 2026,
  "next_season_id": "uuid",
  "next_season_year": 2027,
  "rollover_id": "uuid",
  "started_at": "2027-01-01T00:00:00Z"
}
```

## Optional Payload

```json
{
  "rollover_steps": [
    "reduce_contract_years",
    "expire_contracts",
    "advance_dead_cap",
    "reset_ir",
    "reset_taxi",
    "advance_draft_assets"
  ],
  "started_by_user_id": "uuid"
}
```

## Consumers

- Contract rollover services
- Roster services
- Draft Pick services
- Financial services
- Audit systems
- Observability systems

## Ordering Requirements

The event must precede all state-change events caused by rollover.

All related events should share the rollover `correlation_id`.

## Idempotency Requirements

Only one active rollover may exist for a given season transition.

## Replay Behavior

Replay records that the workflow began.

It shall not reexecute rollover actions.

## Versioning

Current version:

```text
1
```

## Invariants

- Prior and next seasons differ.
- Both seasons belong to the same League.
- Starting rollover does not prove completion.
- No downstream effect should be inferred without its own event.

## AI Interpretation

The AI should treat an incomplete rollover as a potential data-integrity warning.

Recommendations dependent on post-rollover contracts or rosters may be unverifiable until completion.

## Example

```json
{
  "event_type": "SeasonRolloverStarted",
  "event_version": 1,
  "aggregate_type": "LeagueSeason",
  "aggregate_id": "8019c956-c18d-4561-9c63-c14a15c79013",
  "payload": {
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "prior_season_id": "f3e7de6d-a95e-4d41-9659-c40fe47d3cf4",
    "prior_season_year": 2026,
    "next_season_id": "8019c956-c18d-4561-9c63-c14a15c79013",
    "next_season_year": 2027,
    "rollover_id": "72ce12e5-7694-4876-b27d-e15a2ef0c752",
    "started_at": "2027-01-01T00:00:00Z"
  }
}
```

---

# SeasonRolloverCompleted

## Purpose

`SeasonRolloverCompleted` records that every required annual rollover step completed and passed validation.

## Event Name

`SeasonRolloverCompleted`

## Category

Season Transition

## Owning Aggregate

LeagueSeason

## Trigger

The event is emitted only after the complete rollover workflow succeeds.

## Preconditions

- `SeasonRolloverStarted` exists.
- Required contract-year reductions completed.
- Expiring Contracts were processed.
- Required Free Agent records were created.
- Dead-cap obligations advanced.
- IR and Taxi designations reset as configured.
- Draft assets advanced as configured.
- League Configuration remained valid.
- Post-rollover validation passed.
- No blocking rollover error remains.

## Required Payload

```json
{
  "league_id": "uuid",
  "prior_season_id": "uuid",
  "next_season_id": "uuid",
  "rollover_id": "uuid",
  "completed_at": "2027-01-01T00:02:14Z",
  "validation_status": "passed"
}
```

## Optional Payload

```json
{
  "processed_contract_count": 143,
  "expired_contract_count": 18,
  "dead_cap_obligation_count": 12,
  "reset_ir_assignment_count": 7,
  "reset_taxi_assignment_count": 21,
  "advanced_draft_asset_count": 30
}
```

Counts are summary information.

They do not replace entity-specific events.

## Consumers

- Season advancement
- Current-season projections
- Contract services
- Roster services
- Draft services
- AI evidence builders
- Audit systems

## Ordering Requirements

The event must follow all required rollover state-change events and validation.

## Idempotency Requirements

The same rollover shall complete only once.

## Replay Behavior

Replay marks the rollover complete.

Replay shall not repeat mutations.

## Versioning

Current version:

```text
1
```

## Invariants

- Completion requires successful validation.
- Summary counts must match the completed workflow's recorded results.
- No required rollover step may remain pending.
- Completion does not erase prior-season state.
- Entity-level changes remain independently auditable.

## AI Interpretation

The AI may treat next-season contract and roster state as operational after this event, subject to later corrections.

## Example

```json
{
  "event_type": "SeasonRolloverCompleted",
  "event_version": 1,
  "aggregate_type": "LeagueSeason",
  "aggregate_id": "8019c956-c18d-4561-9c63-c14a15c79013",
  "payload": {
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "prior_season_id": "f3e7de6d-a95e-4d41-9659-c40fe47d3cf4",
    "next_season_id": "8019c956-c18d-4561-9c63-c14a15c79013",
    "rollover_id": "72ce12e5-7694-4876-b27d-e15a2ef0c752",
    "completed_at": "2027-01-01T00:02:14Z",
    "validation_status": "passed",
    "processed_contract_count": 143,
    "expired_contract_count": 18,
    "dead_cap_obligation_count": 12,
    "reset_ir_assignment_count": 7,
    "reset_taxi_assignment_count": 21,
    "advanced_draft_asset_count": 30
  }
}
```

---

# SeasonRolloverFailed

## Purpose

`SeasonRolloverFailed` records that the annual rollover workflow did not complete successfully.

This is an operationally significant business event because the League cannot safely advance while seasonal state is inconsistent.

## Event Name

`SeasonRolloverFailed`

## Category

Season Transition

## Owning Aggregate

LeagueSeason

## Trigger

The event is emitted when a rollover step fails and the workflow cannot complete.

## Preconditions

- `SeasonRolloverStarted` exists.
- A required step failed or post-rollover validation failed.
- The failure is material enough to block completion.

## Required Payload

```json
{
  "league_id": "uuid",
  "prior_season_id": "uuid",
  "next_season_id": "uuid",
  "rollover_id": "uuid",
  "failed_at": "2027-01-01T00:01:32Z",
  "failed_step": "expire_contracts",
  "failure_code": "contract_state_conflict",
  "failure_summary": "Three contracts had inconsistent years remaining."
}
```

## Optional Payload

```json
{
  "affected_entity_ids": [
    "uuid",
    "uuid",
    "uuid"
  ],
  "rollback_status": "completed",
  "retry_allowed": true
}
```

## Consumers

- Commissioner alerts
- Operational dashboards
- Season services
- Audit systems
- AI evidence builders
- Recovery workflows

## Ordering Requirements

The event follows `SeasonRolloverStarted`.

It prevents `SeasonRolloverCompleted` until the failure is corrected and the workflow is successfully rerun or resumed.

## Idempotency Requirements

Duplicate reporting of the same failure occurrence should be deduplicated.

A later retry failure may produce a new event with a new `event_id`.

## Replay Behavior

Replay reconstructs the failed workflow status.

Replay shall not reissue alerts unless explicitly enabled.

## Versioning

Current version:

```text
1
```

## Invariants

- A failed rollover is not complete.
- The League must not silently advance while required state remains inconsistent.
- Failure details must be sufficient for diagnosis without exposing secrets.
- Corrective action must produce new events rather than editing the failure event.

## AI Interpretation

The AI should treat League state dependent on the rollover as incomplete or unreliable.

It should avoid firm recommendations involving affected contracts, rosters, Draft Picks, or cap values until the workflow completes.

## Example

```json
{
  "event_type": "SeasonRolloverFailed",
  "event_version": 1,
  "aggregate_type": "LeagueSeason",
  "aggregate_id": "8019c956-c18d-4561-9c63-c14a15c79013",
  "payload": {
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "prior_season_id": "f3e7de6d-a95e-4d41-9659-c40fe47d3cf4",
    "next_season_id": "8019c956-c18d-4561-9c63-c14a15c79013",
    "rollover_id": "72ce12e5-7694-4876-b27d-e15a2ef0c752",
    "failed_at": "2027-01-01T00:01:32Z",
    "failed_step": "expire_contracts",
    "failure_code": "contract_state_conflict",
    "failure_summary": "Three contracts had inconsistent years remaining.",
    "rollback_status": "completed",
    "retry_allowed": true
  }
}
```

---

# Canonical Season Transition Workflow

```text
LeagueSeasonCompleted
        │
        ▼
SeasonRolloverStarted
        │
        ├── ContractYearReduced
        ├── ContractExpired
        ├── RosterDesignationReset
        ├── DeadCapObligationAdvanced
        ├── DraftAssetAdvanced
        └── Other Entity-Specific Events
        │
        ▼
Post-Rollover Validation
        │
        ├── Failure
        │      ▼
        │   SeasonRolloverFailed
        │
        └── Success
               ▼
        SeasonRolloverCompleted
               │
               ▼
        LeagueSeasonAdvanced
               │
               ▼
        LeagueSeasonStarted
               │
               ▼
        LeaguePhaseActivated
```

---

# Rollover Boundary

The rollover coordinator owns workflow completion.

It does not own every affected entity.

For example:

```text
Contract Aggregate
    emits ContractYearReduced

Roster Assignment Aggregate
    emits RosterDesignationReset

Dead Cap Obligation Aggregate
    emits DeadCapObligationAdvanced

Draft Pick Aggregate
    emits DraftPickAdvanced
```

All events share the rollover correlation identifier.

This provides atomic business traceability without assigning all state changes to the League Season aggregate.

---

# Validation Checklist

Before publishing season lifecycle events, Legacy shall verify:

- League and season identities are canonical.
- Season order is valid.
- Configuration references are valid.
- Phase ownership is correct.
- Effective times do not conflict.
- The actor or system is authorized.
- Required rollover events exist.
- Completion events are not emitted before validation.
- Replay will not repeat side effects.
- Current-season and current-phase projections can be reconstructed.

---

# Final Principle

A season transition is not a date change.

It is a controlled transformation of League state.

Legacy shall preserve every material step so that the platform can explain:

- Which season governed an action
- Which phase made an action legal
- Which annual changes occurred
- Whether rollover completed
- Why current state differs from prior-season state
