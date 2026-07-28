---
title: League Lifecycle Events
document: Event Catalog
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - README.md
  - ../domain-model/01-League.md
  - ../domain-model/21-League-Configuration.md
---

# League Lifecycle Events

## Purpose

This chapter defines the canonical events produced during the lifecycle of a Legacy League.

League lifecycle events describe meaningful changes to:

- League identity
- League status
- League governance
- League configuration
- League availability
- League archival state

These events establish the top-level historical record for a League.

They do not describe seasonal progression, Franchise ownership, membership, roster activity, contracts, or transactions unless those changes are part of League creation or archival workflows.

---

# Scope

This chapter defines:

- `LeagueCreated`
- `LeagueNameChanged`
- `LeagueActivated`
- `LeagueArchived`
- `LeagueRestored`
- `LeagueConfigurationCreated`
- `LeagueConfigurationUpdated`
- `LeagueConfigurationActivated`
- `LeagueConfigurationSuperseded`

---

# Aggregate Ownership

Events in this chapter are owned by one of two aggregates:

| Event | Owning Aggregate |
|---|---|
| `LeagueCreated` | League |
| `LeagueNameChanged` | League |
| `LeagueActivated` | League |
| `LeagueArchived` | League |
| `LeagueRestored` | League |
| `LeagueConfigurationCreated` | LeagueConfiguration |
| `LeagueConfigurationUpdated` | LeagueConfiguration |
| `LeagueConfigurationActivated` | LeagueConfiguration |
| `LeagueConfigurationSuperseded` | LeagueConfiguration |

---

# General Invariants

All League lifecycle events shall satisfy the following invariants:

1. Every event references one canonical `league_id`.
2. A League must exist before any event other than `LeagueCreated` may occur.
3. A League may have only one active configuration for a given effective period.
4. Archiving a League does not delete its historical events.
5. A configuration change does not silently rewrite historical configuration.
6. Configuration activation must preserve the version that governed prior transactions.
7. League lifecycle events shall identify the actor responsible for the change.
8. AI actors shall not create, activate, archive, restore, or configure a League.
9. Integration actors may import League information but may not override commissioner-controlled configuration without an authorized Command.
10. Every state-changing League event should be associated with a canonical Transaction when the platform records the change as a user-visible transaction.

---

# LeagueCreated

## Purpose

`LeagueCreated` records the successful creation of a new League.

This is the first authoritative event in a League's lifecycle.

It establishes the League's canonical identity and initial ownership context.

## Event Name

`LeagueCreated`

## Category

League Lifecycle

## Owning Aggregate

League

## Trigger

The event is emitted when a valid `CreateLeague` Command completes successfully.

## Preconditions

Before this event may occur:

- The requesting User is authenticated.
- The requesting User is permitted to create a League.
- A unique canonical `league_id` has been assigned.
- The League name satisfies validation requirements.
- Required creation settings are present.
- Any external League identifier satisfies uniqueness rules.
- The initial commissioner or creator can be identified.
- The League has not already been created.

## Required Payload

```json
{
  "league_id": "uuid",
  "league_name": "Legacy Dynasty League",
  "created_by_user_id": "uuid",
  "initial_status": "draft",
  "league_format": "dynasty",
  "external_provider": "sleeper",
  "external_league_id": "1257435354890260480",
  "initial_configuration_id": "uuid"
}
```

## Payload Fields

### `league_id`

Canonical League identifier.

### `league_name`

League name at creation time.

### `created_by_user_id`

User responsible for creating the League.

### `initial_status`

Initial lifecycle status.

Supported values may include:

- `draft`
- `setup`
- `active`

### `league_format`

The League's broad competition format.

Examples:

- `dynasty`
- `keeper`
- `redraft`

Legacy's primary supported format is dynasty.

### `external_provider`

Optional external platform associated with the League.

Examples:

- `sleeper`
- `espn`
- `yahoo`
- `manual`

### `external_league_id`

Optional provider-specific League identifier.

This field supplements but never replaces the canonical `league_id`.

### `initial_configuration_id`

Identifier of the initial League Configuration created as part of the League creation workflow.

## Optional Payload

```json
{
  "description": "Ten-team salary-cap dynasty league",
  "timezone": "America/Boise",
  "currency_unit": "contract_dollars",
  "source": "manual_creation"
}
```

## Referenced Entities

- League
- User
- League Configuration

## Permitted Actors

- User
- Commissioner
- Authorized System Migration
- Authorized Integration Import

## Related Events

A successful League creation workflow may also emit:

- `LeagueConfigurationCreated`
- `LeagueMembershipActivated`
- `FranchiseCreated`
- `FranchiseOwnershipAssigned`
- `TransactionRecorded`
- `AuditEventCreated`

All events from the same creation workflow should share a `correlation_id`.

## Consumers

Expected consumers include:

- League projections
- Application navigation
- Authorization services
- Onboarding workflows
- Notification services
- Analytics
- Audit systems
- AI context builders

## Ordering Requirements

`LeagueCreated` must be the first event owned by the League aggregate.

Recommended aggregate sequence:

```text
aggregate_sequence = 1
```

No other League-owned event may precede it.

## Idempotency Requirements

A repeated `CreateLeague` Command using the same idempotency key shall not create a second League.

Consumers shall deduplicate using `event_id`.

## Replay Behavior

Replay may:

- Reconstruct the League projection
- Restore League creation metadata
- Rebuild creator relationships
- Rebuild initial configuration references

Replay shall not:

- Resend creation emails
- Recreate external provider Leagues
- Duplicate memberships
- Duplicate Franchises

## Versioning

Current event version:

```text
1
```

A new major event version is required if the meaning of League identity, creator ownership, or initial configuration changes incompatibly.

## Invariants

- `league_id` is globally unique.
- `created_by_user_id` identifies a valid actor at the time of creation.
- The event cannot occur more than once for the same League.
- The initial configuration must belong to the same League.
- External identifiers cannot become canonical League identifiers.

## AI Interpretation

The AI may interpret this event as evidence that:

- The League exists.
- The League was created at `occurred_at`.
- The named User initiated creation.
- The initial League format and source were established.

The AI shall not infer that:

- The League is currently active.
- The initial configuration remains active.
- Every invited owner joined.
- Every Franchise was successfully configured.

Those conclusions require later events or current-state evidence.

## Example

```json
{
  "event_id": "76121df0-62bc-48fc-a6dd-a045a1a65df4",
  "event_type": "LeagueCreated",
  "event_version": 1,
  "occurred_at": "2026-07-28T18:30:00Z",
  "recorded_at": "2026-07-28T18:30:00Z",
  "league_id": "15b77cca-c259-4543-877f-523d57946a20",
  "aggregate_type": "League",
  "aggregate_id": "15b77cca-c259-4543-877f-523d57946a20",
  "aggregate_sequence": 1,
  "actor": {
    "actor_type": "User",
    "actor_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9"
  },
  "correlation_id": "0a34dffd-268d-4b43-84d9-29a0415fd506",
  "causation_id": "cmd-create-league-001",
  "transaction_id": "59a95d6b-b44d-4753-a89c-ec9e490f163a",
  "payload": {
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "league_name": "Legacy Dynasty League",
    "created_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9",
    "initial_status": "setup",
    "league_format": "dynasty",
    "external_provider": "sleeper",
    "external_league_id": "1257435354890260480",
    "initial_configuration_id": "19879056-2e68-46fa-a23d-47676bc926e3"
  },
  "metadata": {
    "source_application": "legacy-web"
  }
}
```

---

# LeagueNameChanged

## Purpose

`LeagueNameChanged` records a change to the League's user-facing name.

The event preserves both the previous and new names.

## Event Name

`LeagueNameChanged`

## Category

League Lifecycle

## Owning Aggregate

League

## Trigger

The event is emitted when an authorized `ChangeLeagueName` Command succeeds.

## Preconditions

- The League exists.
- The League is not permanently deleted.
- The actor has League administration permission.
- The new name is valid.
- The new name differs from the current name.

## Required Payload

```json
{
  "league_id": "uuid",
  "previous_name": "Old League Name",
  "new_name": "New League Name"
}
```

## Referenced Entities

- League
- User or Commissioner actor

## Permitted Actors

- Commissioner
- Authorized League Administrator

## Consumers

- League projections
- Navigation displays
- Notifications
- Audit systems
- Search indexes
- AI context builders

## Ordering Requirements

The event must occur after `LeagueCreated`.

Sequential name changes must follow League aggregate order.

## Idempotency Requirements

Submitting the current name as the new name shall produce no event.

Repeated processing of the same event shall not create duplicate audit entries or notifications.

## Replay Behavior

Replay updates the League's projected current name.

Historical displays may use event-time names when appropriate.

## Versioning

Current version:

```text
1
```

## Invariants

- `previous_name` and `new_name` must differ.
- Neither name may be empty.
- The change does not alter League identity.
- Historical events retain the League identity that existed before and after the change.

## AI Interpretation

The AI may use this event to resolve historical League names.

It shall continue to treat both names as references to the same canonical League.

## Example

```json
{
  "event_type": "LeagueNameChanged",
  "event_version": 1,
  "aggregate_type": "League",
  "aggregate_id": "15b77cca-c259-4543-877f-523d57946a20",
  "payload": {
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "previous_name": "Boise Dynasty League",
    "new_name": "Legacy Dynasty League"
  }
}
```

---

# LeagueActivated

## Purpose

`LeagueActivated` records that a League completed setup and became operational.

An active League may conduct applicable competition and transaction workflows according to its active season, phase, and configuration.

## Event Name

`LeagueActivated`

## Category

League Lifecycle

## Owning Aggregate

League

## Trigger

The event is emitted when an authorized `ActivateLeague` Command succeeds.

## Preconditions

- The League exists.
- The League is not already active.
- A valid League Configuration is active.
- Required Franchises have been created.
- Required governance roles are assigned.
- Required setup validation has passed.
- The League is not archived.
- Any required current League Season exists.

## Required Payload

```json
{
  "league_id": "uuid",
  "previous_status": "setup",
  "new_status": "active",
  "active_configuration_id": "uuid",
  "activated_by_user_id": "uuid"
}
```

## Optional Payload

```json
{
  "season_id": "uuid",
  "activation_validation_id": "uuid"
}
```

## Referenced Entities

- League
- League Configuration
- League Season
- User

## Permitted Actors

- Commissioner
- Authorized League Administrator
- Authorized System Migration

## Consumers

- Transaction services
- League dashboards
- Scheduling services
- Notification services
- AI evidence builders
- Audit systems

## Ordering Requirements

The event must occur after:

- `LeagueCreated`
- `LeagueConfigurationActivated`

when an active configuration is required for activation.

## Idempotency Requirements

Activating an already active League shall produce no duplicate event.

## Replay Behavior

Replay may restore active status in a League projection.

Replay shall not reopen external transaction windows or resend activation notifications without explicit side-effect authorization.

## Versioning

Current version:

```text
1
```

## Invariants

- The new status must be `active`.
- The active configuration belongs to the same League.
- League activation does not itself begin a League Season unless explicitly represented by `LeagueSeasonStarted`.
- League activation does not guarantee all League Members have accepted invitations.

## AI Interpretation

The AI may interpret this event as evidence that the League became operational at the event time.

Current active status should still be confirmed through current state or later lifecycle events.

## Example

```json
{
  "event_type": "LeagueActivated",
  "event_version": 1,
  "aggregate_type": "League",
  "aggregate_id": "15b77cca-c259-4543-877f-523d57946a20",
  "payload": {
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "previous_status": "setup",
    "new_status": "active",
    "active_configuration_id": "19879056-2e68-46fa-a23d-47676bc926e3",
    "activated_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9"
  }
}
```

---

# LeagueArchived

## Purpose

`LeagueArchived` records that a League was removed from normal active operation while preserving its complete history.

Archival is reversible unless platform policy explicitly makes the League permanently closed.

## Event Name

`LeagueArchived`

## Category

League Lifecycle

## Owning Aggregate

League

## Trigger

The event is emitted when an authorized `ArchiveLeague` Command succeeds.

## Preconditions

- The League exists.
- The League is not already archived.
- The actor has authority to archive the League.
- Any required active transaction workflows have been resolved.
- Archival validation has completed.
- The archive reason is recorded.

## Required Payload

```json
{
  "league_id": "uuid",
  "previous_status": "active",
  "new_status": "archived",
  "archive_reason": "League discontinued by commissioner",
  "archived_by_user_id": "uuid"
}
```

## Optional Payload

```json
{
  "effective_at": "2026-12-31T23:59:59Z",
  "final_season_id": "uuid",
  "restoration_allowed": true
}
```

## Referenced Entities

- League
- User
- League Season

## Permitted Actors

- Commissioner
- Authorized Platform Administrator
- Authorized System Migration

## Consumers

- Authorization services
- League navigation
- Transaction services
- Notification services
- Data retention workflows
- Audit systems
- AI evidence builders

## Ordering Requirements

The event must occur after `LeagueCreated`.

If archival follows completion of a final season, `LeagueSeasonCompleted` should ordinarily precede it.

## Idempotency Requirements

Archiving an already archived League shall not emit a duplicate event.

## Replay Behavior

Replay marks the League projection as archived.

Replay shall not:

- Revoke current sessions again
- Resend archival notifications
- Delete records
- Trigger external platform changes

## Versioning

Current version:

```text
1
```

## Invariants

- Archival does not delete League history.
- Archival does not delete Franchises, Contracts, Draft Picks, or Transactions.
- New League-state-changing Commands are rejected while archived unless explicitly permitted.
- The archive reason is required.
- Archival does not alter canonical League identity.

## AI Interpretation

The AI may analyze an archived League historically.

It shall not recommend executable current transactions unless the League is restored or the user explicitly requests hypothetical analysis.

## Example

```json
{
  "event_type": "LeagueArchived",
  "event_version": 1,
  "aggregate_type": "League",
  "aggregate_id": "15b77cca-c259-4543-877f-523d57946a20",
  "payload": {
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "previous_status": "active",
    "new_status": "archived",
    "archive_reason": "League discontinued after the 2030 season",
    "archived_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9",
    "restoration_allowed": true
  }
}
```

---

# LeagueRestored

## Purpose

`LeagueRestored` records that an archived League returned to an operational or setup state.

Restoration preserves all prior League identity and history.

## Event Name

`LeagueRestored`

## Category

League Lifecycle

## Owning Aggregate

League

## Trigger

The event is emitted when an authorized `RestoreLeague` Command succeeds.

## Preconditions

- The League exists.
- The League is archived.
- Restoration is allowed.
- The actor has restoration authority.
- Required configuration and season validation has completed.
- The target status is valid.

## Required Payload

```json
{
  "league_id": "uuid",
  "previous_status": "archived",
  "new_status": "setup",
  "restored_by_user_id": "uuid",
  "restoration_reason": "League returning for the 2031 season"
}
```

## Optional Payload

```json
{
  "active_configuration_id": "uuid",
  "season_id": "uuid"
}
```

## Referenced Entities

- League
- League Configuration
- League Season
- User

## Permitted Actors

- Commissioner
- Authorized Platform Administrator

## Consumers

- Authorization services
- League navigation
- Transaction services
- Notification services
- Audit systems
- AI context builders

## Ordering Requirements

The event must occur after `LeagueArchived`.

## Idempotency Requirements

Restoring a League that is no longer archived shall not emit a second event.

## Replay Behavior

Replay restores the projected League status.

Replay shall not automatically reactivate transactions or send notifications.

## Versioning

Current version:

```text
1
```

## Invariants

- The restored League retains its original `league_id`.
- Historical seasons remain unchanged.
- Restoration does not automatically resume the prior League Phase.
- A new or reactivated configuration may be required.
- Restoration does not silently reinstate deactivated memberships.

## AI Interpretation

The AI may interpret restoration as evidence that the League returned to operation.

It must evaluate the newly active season, phase, configuration, membership, and roster state rather than assuming the archived state resumed unchanged.

## Example

```json
{
  "event_type": "LeagueRestored",
  "event_version": 1,
  "aggregate_type": "League",
  "aggregate_id": "15b77cca-c259-4543-877f-523d57946a20",
  "payload": {
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "previous_status": "archived",
    "new_status": "setup",
    "restored_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9",
    "restoration_reason": "League returning for the 2031 season"
  }
}
```

---

# LeagueConfigurationCreated

## Purpose

`LeagueConfigurationCreated` records the creation of a versioned League Configuration.

A created configuration may be a draft, scheduled configuration, or immediately activated configuration depending on the workflow.

## Event Name

`LeagueConfigurationCreated`

## Category

League Configuration

## Owning Aggregate

LeagueConfiguration

## Trigger

The event is emitted when a valid `CreateLeagueConfiguration` Command succeeds.

It may also be emitted as part of `CreateLeague`.

## Preconditions

- The League exists or is being created in the same atomic workflow.
- The actor has configuration authority.
- A unique `league_configuration_id` has been assigned.
- Required settings are present.
- Settings satisfy schema-level validation.
- The configuration version is valid.
- The effective period does not violate configuration overlap rules.

## Required Payload

```json
{
  "league_configuration_id": "uuid",
  "league_id": "uuid",
  "configuration_version": 1,
  "status": "draft",
  "effective_season": 2026,
  "created_by_user_id": "uuid",
  "configuration_snapshot": {}
}
```

## Configuration Snapshot

The snapshot may include:

```json
{
  "league_settings": {},
  "roster_configuration": {},
  "financial_configuration": {},
  "draft_configuration": {},
  "waiver_configuration": {},
  "trade_configuration": {},
  "seasonal_configuration": {},
  "ai_configuration": {}
}
```

The payload should contain either:

1. The complete immutable configuration snapshot, or
2. A durable canonical reference to a versioned snapshot whose contents cannot be silently changed.

## Referenced Entities

- League Configuration
- League
- User
- League Season when season-specific

## Permitted Actors

- Commissioner
- Authorized League Administrator
- Authorized System Migration

## Consumers

- Rule evaluation
- League projections
- Configuration comparison tools
- Audit systems
- Season planning
- AI evidence builders

## Ordering Requirements

The first configuration ordinarily follows or shares a transaction with `LeagueCreated`.

Later configuration versions must increment according to the League's configuration versioning rules.

## Idempotency Requirements

The same Command and idempotency key shall not produce multiple configuration versions.

## Replay Behavior

Replay reconstructs the versioned configuration record.

Replay shall not activate the configuration unless a corresponding `LeagueConfigurationActivated` event exists.

## Versioning

Current event version:

```text
1
```

The event payload's `configuration_version` is distinct from `event_version`.

- `event_version` versions the event schema.
- `configuration_version` versions the League Configuration.

## Invariants

- The configuration belongs to exactly one League.
- The configuration version is immutable once activated.
- Draft configurations may be changed only through documented configuration update workflows.
- Historical transactions retain references to the governing configuration.
- Creation does not necessarily mean activation.

## AI Interpretation

The AI may treat this event as evidence that a configuration version exists.

It shall not assume the version governed League behavior until `LeagueConfigurationActivated` occurs.

## Example

```json
{
  "event_type": "LeagueConfigurationCreated",
  "event_version": 1,
  "aggregate_type": "LeagueConfiguration",
  "aggregate_id": "19879056-2e68-46fa-a23d-47676bc926e3",
  "payload": {
    "league_configuration_id": "19879056-2e68-46fa-a23d-47676bc926e3",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "configuration_version": 1,
    "status": "draft",
    "effective_season": 2026,
    "created_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9",
    "configuration_snapshot": {
      "roster_configuration": {
        "active_limit": 20,
        "ir_limit": 4,
        "taxi_limit": 5
      },
      "financial_configuration": {
        "salary_cap": 250,
        "contract_year_limit": 4
      }
    }
  }
}
```

---

# LeagueConfigurationUpdated

## Purpose

`LeagueConfigurationUpdated` records a permitted change to a configuration that has not yet become immutable.

This event is primarily intended for draft or scheduled configurations.

Active historical configuration shall not be silently rewritten.

## Event Name

`LeagueConfigurationUpdated`

## Category

League Configuration

## Owning Aggregate

LeagueConfiguration

## Trigger

The event is emitted when an authorized `UpdateLeagueConfiguration` Command succeeds.

## Preconditions

- The configuration exists.
- The configuration belongs to the specified League.
- The configuration is in a mutable status.
- The actor has configuration authority.
- The requested changes satisfy validation.
- The update does not rewrite historical governing rules.
- The new settings do not conflict with an already active effective period.

## Required Payload

```json
{
  "league_configuration_id": "uuid",
  "league_id": "uuid",
  "configuration_version": 2,
  "changed_fields": [],
  "previous_values": {},
  "new_values": {},
  "updated_by_user_id": "uuid"
}
```

## Referenced Entities

- League Configuration
- League
- User

## Permitted Actors

- Commissioner
- Authorized League Administrator

## Consumers

- Configuration projections
- Rule validation
- Audit systems
- Configuration review interfaces
- AI evidence builders

## Ordering Requirements

The event must occur after `LeagueConfigurationCreated`.

Updates must preserve aggregate sequence.

## Idempotency Requirements

An update that produces no material state change shall emit no event.

## Replay Behavior

Replay applies the documented changes to the projected configuration version.

Replay shall not modify configurations outside the event's aggregate.

## Versioning

Current version:

```text
1
```

## Invariants

- Previous and new values must differ.
- Changed fields must be explicitly named.
- Activated immutable configurations cannot be updated in place.
- Historical configuration meaning cannot be changed retroactively.
- The update does not activate the configuration.

## AI Interpretation

The AI may use this event to understand how a draft configuration evolved.

For legal and historical analysis, the AI must use the configuration version active at the relevant effective time.

## Example

```json
{
  "event_type": "LeagueConfigurationUpdated",
  "event_version": 1,
  "aggregate_type": "LeagueConfiguration",
  "aggregate_id": "19879056-2e68-46fa-a23d-47676bc926e3",
  "payload": {
    "league_configuration_id": "19879056-2e68-46fa-a23d-47676bc926e3",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "configuration_version": 2,
    "changed_fields": [
      "financial_configuration.salary_cap"
    ],
    "previous_values": {
      "financial_configuration.salary_cap": 250
    },
    "new_values": {
      "financial_configuration.salary_cap": 275
    },
    "updated_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9"
  }
}
```

---

# LeagueConfigurationActivated

## Purpose

`LeagueConfigurationActivated` records that a specific League Configuration became authoritative for a defined effective period.

This event establishes which rules and settings govern League behavior.

## Event Name

`LeagueConfigurationActivated`

## Category

League Configuration

## Owning Aggregate

LeagueConfiguration

## Trigger

The event is emitted when an authorized `ActivateLeagueConfiguration` Command succeeds.

## Preconditions

- The League exists.
- The configuration exists.
- The configuration belongs to the League.
- The configuration has passed validation.
- The actor has activation authority.
- The effective period is valid.
- No incompatible configuration is already active for the same effective period.
- Required League votes or approvals have been satisfied when applicable.

## Required Payload

```json
{
  "league_configuration_id": "uuid",
  "league_id": "uuid",
  "configuration_version": 2,
  "previous_status": "approved",
  "new_status": "active",
  "effective_at": "2027-01-01T00:00:00Z",
  "activated_by_user_id": "uuid"
}
```

## Optional Payload

```json
{
  "effective_season": 2027,
  "superseded_configuration_id": "uuid",
  "approval_reference": "uuid"
}
```

## Referenced Entities

- League Configuration
- League
- League Season
- User
- Prior League Configuration

## Permitted Actors

- Commissioner
- Authorized League Administrator
- Authorized System Workflow following approval

## Related Events

Activation may cause or correlate with:

- `LeagueConfigurationSuperseded`
- `LeagueActivated`
- `LeagueSeasonScheduled`
- `AuditEventCreated`

## Consumers

- Rule evaluation
- Transaction validation
- Salary-cap calculation
- Draft services
- Waiver processing
- Roster validation
- AI evidence resolution
- Audit systems

## Ordering Requirements

The event must occur after:

- `LeagueConfigurationCreated`
- Any applicable `LeagueConfigurationUpdated` events
- Required approval events

When replacing another configuration, activation should be coordinated with `LeagueConfigurationSuperseded`.

## Idempotency Requirements

A configuration already active for the same effective period shall not be activated twice.

## Replay Behavior

Replay marks the configuration as active for its documented effective period.

Replay shall not reexecute League votes, approvals, or notifications.

## Versioning

Current version:

```text
1
```

## Invariants

- Only one configuration may govern a given League at one effective instant unless layered configuration is explicitly supported.
- The activated configuration becomes immutable.
- Activation is effective at `effective_at`, which may differ from `occurred_at`.
- Historical transactions retain the configuration that governed them.
- Activation does not retroactively change completed transactions.

## AI Interpretation

This is an authoritative event for determining which League Configuration governed a question at a specific time.

The AI should prioritize the activated configuration whose effective period contains the relevant transaction or analysis date.

## Example

```json
{
  "event_type": "LeagueConfigurationActivated",
  "event_version": 1,
  "aggregate_type": "LeagueConfiguration",
  "aggregate_id": "19879056-2e68-46fa-a23d-47676bc926e3",
  "payload": {
    "league_configuration_id": "19879056-2e68-46fa-a23d-47676bc926e3",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "configuration_version": 2,
    "previous_status": "approved",
    "new_status": "active",
    "effective_at": "2027-01-01T00:00:00Z",
    "effective_season": 2027,
    "activated_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9"
  }
}
```

---

# LeagueConfigurationSuperseded

## Purpose

`LeagueConfigurationSuperseded` records that an active or scheduled League Configuration was replaced by another version.

The superseded configuration remains part of League history.

## Event Name

`LeagueConfigurationSuperseded`

## Category

League Configuration

## Owning Aggregate

LeagueConfiguration

## Trigger

The event is emitted when a replacement configuration becomes authoritative.

## Preconditions

- The superseded configuration exists.
- It belongs to the specified League.
- A valid replacement configuration exists.
- The replacement has been or is atomically being activated.
- The supersession effective time is known.
- Historical effective periods remain unambiguous.

## Required Payload

```json
{
  "league_configuration_id": "uuid",
  "league_id": "uuid",
  "previous_status": "active",
  "new_status": "superseded",
  "replacement_configuration_id": "uuid",
  "superseded_at": "2027-01-01T00:00:00Z"
}
```

## Referenced Entities

- Superseded League Configuration
- Replacement League Configuration
- League

## Permitted Actors

- Commissioner
- Authorized System Workflow

## Consumers

- Rule evaluation
- Historical reconstruction
- Configuration projections
- Transaction validation
- Audit systems
- AI evidence builders

## Ordering Requirements

This event must correlate with a valid replacement configuration.

The transition should not leave the League without an authoritative configuration during an active effective period.

## Idempotency Requirements

A configuration already superseded by the same replacement shall not emit another supersession event.

## Replay Behavior

Replay closes the effective period of the superseded configuration and preserves the replacement reference.

## Versioning

Current version:

```text
1
```

## Invariants

- The superseded and replacement configurations must be different.
- Both configurations belong to the same League.
- Supersession does not delete or mutate historical configuration contents.
- Completed transactions remain governed by the configuration active when they occurred.
- A superseded configuration cannot become active again without an explicit reactivation policy and event.

## AI Interpretation

The AI should use this event to identify the end of a configuration's authoritative period.

It shall not use the replacement configuration to evaluate transactions that occurred before the replacement became effective.

## Example

```json
{
  "event_type": "LeagueConfigurationSuperseded",
  "event_version": 1,
  "aggregate_type": "LeagueConfiguration",
  "aggregate_id": "11111111-2222-3333-4444-555555555555",
  "payload": {
    "league_configuration_id": "11111111-2222-3333-4444-555555555555",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "previous_status": "active",
    "new_status": "superseded",
    "replacement_configuration_id": "19879056-2e68-46fa-a23d-47676bc926e3",
    "superseded_at": "2027-01-01T00:00:00Z"
  }
}
```

---

# Workflow Examples

## League Creation

```text
CreateLeague
      │
      ▼
LeagueCreated
      │
      ├── LeagueConfigurationCreated
      ├── LeagueMembershipActivated
      ├── FranchiseCreated
      ├── FranchiseOwnershipAssigned
      └── TransactionRecorded
```

## League Activation

```text
ActivateLeagueConfiguration
      │
      ▼
LeagueConfigurationActivated
      │
      ▼
ActivateLeague
      │
      ▼
LeagueActivated
```

## Configuration Replacement

```text
CreateLeagueConfiguration
      │
      ▼
LeagueConfigurationCreated
      │
      ▼
UpdateLeagueConfiguration
      │
      ▼
LeagueConfigurationUpdated
      │
      ▼
ActivateLeagueConfiguration
      │
      ├── LeagueConfigurationSuperseded
      └── LeagueConfigurationActivated
```

## League Archival and Restoration

```text
ArchiveLeague
      │
      ▼
LeagueArchived
      │
      ▼
RestoreLeague
      │
      ▼
LeagueRestored
```

---

# Validation Checklist

Before publishing a League lifecycle event, Legacy shall verify:

- The event name is canonical.
- The League identity is valid.
- The owning aggregate is correct.
- The actor is authorized.
- Required before and after values are present.
- Configuration effective periods do not conflict.
- Historical settings are not rewritten.
- Correlation and causation identifiers are present.
- The event version is supported.
- The workflow is idempotent.

---

# Final Principle

League lifecycle events establish the League's institutional history.

They preserve:

- When the League came into existence
- When it became operational
- Which configuration governed it
- When its identity changed
- When it became inactive
- Whether it later returned

League state may change.

League history must remain intact.
