---
title: Event Catalog
document: Event Catalog
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - ../rulebook/README.md
  - ../domain-model/README.md
  - ../ai-architecture/README.md
---

# Event Catalog

## Purpose

The Event Catalog defines the canonical domain events emitted by the Legacy platform.

A domain event records that a meaningful business fact has occurred.

Examples include:

- A League was created.
- A Contract was signed.
- A Draft Pick changed ownership.
- A Trade was executed.
- A Player was released.
- A Season advanced.
- A Recommendation was generated.

Events describe completed facts.

They do not describe intentions, requests, or future possibilities.

---

# Guiding Principle

> Events state what happened, not what should happen.

Commands request change.

Rules determine whether change is allowed.

Domain services execute valid change.

Events record the result.

```text
Command
   │
   ▼
Validation
   │
   ▼
State Transition
   │
   ▼
Domain Event
   │
   ▼
Consumers and Projections
```

---

# Authority

This catalog is the canonical source for:

- Event names
- Event meanings
- Event ownership
- Event payload requirements
- Event versioning
- Event ordering
- Event consumers
- Event replay expectations
- Event immutability requirements

Application code, database triggers, background workers, integrations, analytics pipelines, and AI systems shall use the event definitions documented here.

If an implementation conflicts with this catalog, the catalog represents the intended architecture until the documentation or implementation is formally revised.

---

# Relationship to the Rulebook

The Rulebook defines what behavior is permitted.

The Event Catalog records what behavior occurred.

For example:

```text
Rulebook

A Franchise may release an eligible Player Contract.
```

```text
Event Catalog

PlayerReleased
ContractTerminated
DeadCapObligationCreated
```

Events shall never redefine business rules.

They record the result of rules being applied.

---

# Relationship to the Domain Model

The Domain Model defines the entities that exist.

The Event Catalog defines meaningful changes involving those entities.

Examples:

```text
Domain Entity              Events

League                     LeagueCreated
                           LeagueArchived

League Season              LeagueSeasonStarted
                           LeagueSeasonCompleted

Contract                   ContractCreated
                           ContractExtended
                           ContractExpired

Trade                      TradeProposed
                           TradeAccepted
                           TradeExecuted

Draft Pick                 DraftPickTransferred
                           DraftPickConsumed
```

Every event shall reference canonical Domain Model identities.

Events shall not introduce alternate entity identities or duplicate entity definitions.

---

# Relationship to AI Architecture

The AI Architecture consumes events as historical evidence and may emit advisory events of its own.

Examples include:

- AIRecommendationGenerated
- PlayerEvaluationCompleted
- TeamEvaluationCompleted
- TransactionEvaluationCompleted

AI-generated events are informational.

They shall not directly modify league state.

A recommendation may lead a User to submit a Command, but the recommendation itself is not a transaction.

```text
AI Recommendation
       │
       ▼
User Decision
       │
       ▼
Command
       │
       ▼
Validated State Change
       │
       ▼
Domain Event
```

---

# What Is a Domain Event?

A Domain Event is an immutable record that a meaningful business occurrence has completed.

A valid event shall be:

- Written in the past tense
- Factually true
- Immutable
- Timestamped
- Attributable
- Versioned
- Associated with a canonical entity
- Sufficiently structured for downstream consumers

Examples:

```text
TradeExecuted
ContractExtended
WaiverClaimAwarded
LeagueSeasonAdvanced
```

Invalid examples:

```text
ExecuteTrade
ExtendContract
ProcessWaivers
AdvanceSeason
```

Those are Commands because they request behavior.

---

# Event Naming Convention

Event names shall use:

```text
PascalCase
PastTense
BusinessMeaning
```

Preferred format:

```text
<Entity><ActionInPastTense>
```

Examples:

```text
LeagueCreated
FranchiseArchived
ContractExtended
DraftPickTransferred
TradeExecuted
PlayerReleased
```

For events representing a process rather than one entity, the name should still describe the completed business fact:

```text
WaiverProcessingCompleted
SeasonRolloverCompleted
RosterValidationFailed
```

Avoid:

- Technical implementation terms
- Database table names
- Generic names such as `Updated`
- Present-tense actions
- Ambiguous abbreviations
- Vendor-specific terminology

Poor:

```text
RowInserted
ContractUpdated
SleeperSyncDone
TxProcessed
```

Preferred:

```text
ContractCreated
ContractSalaryChanged
ExternalRosterSynchronized
TransactionRecorded
```

---

# Event Categories

The Event Catalog organizes events by domain area.

## League Events

Events involving League identity, configuration, lifecycle, or governance.

Examples:

- LeagueCreated
- LeagueConfigured
- LeagueArchived
- LeagueConfigurationActivated

---

## Season and Phase Events

Events involving seasonal progression and operational phases.

Examples:

- LeagueSeasonScheduled
- LeagueSeasonStarted
- LeaguePhaseAdvanced
- LeagueSeasonCompleted

---

## Franchise and Membership Events

Events involving competitive entities, Users, invitations, roles, and access.

Examples:

- FranchiseCreated
- LeagueMemberInvited
- LeagueMembershipActivated
- FranchiseOwnershipAssigned

---

## Player and Roster Events

Events involving player registration, roster designation, and availability.

Examples:

- PlayerRegistered
- RosterAssignmentCreated
- RosterDesignationChanged
- RosterAssignmentRemoved

---

## Contract and Financial Events

Events involving contracts, salary cap, dead cap, options, and financial obligations.

Examples:

- ContractCreated
- ContractExtended
- ContractExpired
- DeadCapObligationCreated
- TeamOptionExercised

---

## Draft Events

Events involving Draft Picks, Rookie Drafts, and Draft Selections.

Examples:

- DraftPickCreated
- DraftPickTransferred
- RookieDraftStarted
- PlayerDrafted
- DraftPickConsumed

---

## Trade Events

Events involving trade proposals, approvals, rejection, execution, and asset movement.

Examples:

- TradeProposed
- TradeAccepted
- TradeRejected
- TradeExecuted
- TradeAssetTransferred

---

## Waiver and Free Agency Events

Events involving claims, waiver processing, and player acquisition.

Examples:

- WaiverClaimSubmitted
- WaiverClaimCancelled
- WaiverClaimAwarded
- WaiverClaimRejected

---

## Release Events

Events involving contract termination, roster removal, and dead cap creation.

Examples:

- PlayerReleaseRequested
- PlayerReleased
- ContractTerminated
- DeadCapObligationCreated

---

## Transaction and Audit Events

Events involving permanent transaction records and audit history.

Examples:

- TransactionRecorded
- AuditEventCreated
- CommissionerActionRecorded

---

## AI Advisory Events

Events involving generated evaluations and recommendations.

Examples:

- PlayerEvaluationCompleted
- TeamEvaluationCompleted
- LeagueEvaluationCompleted
- TransactionEvaluationCompleted
- AIRecommendationGenerated

These events never authorize or execute league actions.

---

## Integration Events

Events exchanged with external systems.

Examples:

- ExternalLeagueImported
- ExternalRosterSynchronized
- ExternalPlayerMetadataUpdated
- IntegrationSynchronizationFailed

Integration events should describe business-relevant outcomes rather than low-level API operations.

---

# Event Versus Command

Commands and Events have different meanings.

## Command

A Command requests that something happen.

Examples:

```text
CreateLeague
SubmitTrade
AcceptTrade
ReleasePlayer
AdvanceSeason
```

Commands may:

- Succeed
- Fail validation
- Be rejected
- Produce multiple events
- Produce no events

---

## Event

An Event confirms that something happened.

Examples:

```text
LeagueCreated
TradeSubmitted
TradeAccepted
PlayerReleased
LeagueSeasonAdvanced
```

Events cannot be rejected after publication because they describe completed facts.

---

# Event Versus Entity State

Current state answers:

> What is true now?

Events answer:

> What happened over time?

Example:

```text
Current Contract State

Player: Example Player
Franchise: Franchise A
Salary: $18
Years Remaining: 2
Status: Active
```

Possible event history:

```text
ContractCreated
ContractSalaryChanged
ContractExtended
```

Current state may be reconstructed from events when event completeness and ordering are guaranteed.

However, Legacy may also maintain current-state projections for efficient application reads.

---

# Event Structure

Every event shall use a standard envelope.

```json
{
  "event_id": "uuid",
  "event_type": "TradeExecuted",
  "event_version": 1,
  "occurred_at": "2026-07-28T18:30:00Z",
  "recorded_at": "2026-07-28T18:30:01Z",
  "league_id": "uuid",
  "season_id": "uuid",
  "aggregate_type": "Trade",
  "aggregate_id": "uuid",
  "actor": {
    "actor_type": "User",
    "actor_id": "uuid"
  },
  "correlation_id": "uuid",
  "causation_id": "uuid",
  "transaction_id": "uuid",
  "payload": {},
  "metadata": {}
}
```

The exact storage representation will be defined by the Database Specification.

The semantic requirements are defined here.

---

# Required Event Envelope Fields

## event_id

A globally unique immutable identifier for the event.

An event identifier shall never be reused.

---

## event_type

The canonical event name defined by this catalog.

---

## event_version

The schema version of the event payload.

Versions begin at:

```text
1
```

---

## occurred_at

The timestamp at which the business event occurred.

This represents domain time.

---

## recorded_at

The timestamp at which Legacy persisted the event.

This represents system recording time.

`recorded_at` may be later than `occurred_at`.

---

## aggregate_type

The canonical Domain Model entity that owns the event.

Examples:

- League
- Contract
- Trade
- DraftPick
- WaiverClaim

---

## aggregate_id

The canonical identifier of the owning aggregate.

---

## actor

The authenticated or system actor responsible for initiating the action.

Actor types may include:

- User
- Commissioner
- System
- Integration
- AI

AI actors may generate advisory events only.

---

## correlation_id

An identifier grouping all operations and events belonging to one business workflow.

Example:

A Player Release may produce:

- PlayerReleased
- ContractTerminated
- RosterAssignmentRemoved
- DeadCapObligationCreated
- TransactionRecorded

All may share one `correlation_id`.

---

## causation_id

The identifier of the Command or prior Event that directly caused this event.

This supports traceability across workflows.

---

## transaction_id

The canonical Transaction associated with the state change when applicable.

Not every informational event must have a Transaction.

Every league-state-changing event should.

---

## payload

The event-specific business data.

Payload fields are defined in each event chapter.

---

## metadata

Non-domain operational information.

Examples:

- Request identifier
- Source application
- Integration provider
- Trace identifier
- Schema producer
- Import batch

Metadata shall not contain critical business facts required to interpret the event.

Critical facts belong in the payload.

---

# Event Ownership

Every event shall have one owning aggregate.

The owning aggregate is the entity responsible for enforcing the state transition that produced the event.

Examples:

```text
Event                         Owning Aggregate

ContractCreated               Contract
TradeExecuted                 Trade
DraftPickTransferred          DraftPick
WaiverClaimAwarded            WaiverClaim
LeagueSeasonAdvanced          LeagueSeason
```

An event may reference multiple entities but has only one owner.

---

# Event Payload Principles

Payloads shall contain enough information for consumers to understand the completed fact without relying on unstable application state.

Payloads should include:

- Canonical identifiers
- Relevant before and after values
- Effective date or season
- Material financial values
- Ownership changes
- Status transitions
- Rule or configuration versions when required

Payloads should not include:

- Entire database rows without purpose
- Secrets
- Authentication tokens
- Unbounded conversation content
- Vendor-specific response bodies
- Derived display text
- Mutable presentation fields unless materially relevant

---

# Before and After Values

Events involving a state transition should include material before and after values.

Example:

```json
{
  "contract_id": "uuid",
  "previous_salary": 14,
  "new_salary": 18,
  "previous_years_remaining": 1,
  "new_years_remaining": 3
}
```

This improves:

- Auditing
- Historical reconstruction
- Debugging
- AI evidence resolution
- Projection rebuilding

Not every event requires complete snapshots.

Only business-relevant changes should be captured.

---

# Immutability

Published events are immutable.

Events shall never be:

- Edited
- Overwritten
- Reinterpreted in place
- Physically reused for a different occurrence

If an event was recorded incorrectly, Legacy shall emit a correcting event.

Example:

```text
ContractSalaryRecordedIncorrectly
ContractSalaryCorrected
```

The original event remains part of history.

---

# Event Ordering

Events emitted by one aggregate shall have deterministic ordering.

Where required, events should include an aggregate sequence number.

Example:

```json
{
  "aggregate_id": "uuid",
  "aggregate_sequence": 14
}
```

Consumers shall not assume that events from different aggregates arrive in globally perfect order.

Cross-aggregate workflows should use:

- correlation identifiers
- transaction identifiers
- timestamps
- explicit workflow status

---

# Atomic Workflows

One valid Command may produce multiple Domain Events.

Example:

```text
ReleasePlayer
      │
      ▼
PlayerReleased
ContractTerminated
RosterAssignmentRemoved
DeadCapObligationCreated
TransactionRecorded
```

These events represent separate completed facts within one atomic business operation.

If the underlying state transition fails, none of the associated state-change events should be committed.

---

# Event Publication

An event is considered published only after the associated state transition has been durably persisted.

Legacy should avoid workflows where:

```text
Event Published
      │
      ▼
State Persistence Fails
```

The implementation should use a reliable publication mechanism such as an outbox pattern or equivalent transactional approach.

The exact implementation belongs in the Database Specification and ADRs.

---

# Event Consumers

Events may be consumed by:

- Read-model projectors
- Audit systems
- Notification services
- Analytics pipelines
- Background jobs
- External integrations
- Cache invalidation systems
- AI evidence builders
- Historical reconstruction tools

Consumers shall not alter the meaning of an event.

---

# Idempotency

Event consumers shall be idempotent.

Processing the same `event_id` more than once must not create duplicate business effects.

Consumers should record processed event identifiers when duplicate delivery is possible.

---

# Replay

Events may be replayed to:

- Rebuild projections
- Reconstruct history
- Repair derived data
- Recalculate analytics
- Rebuild AI evaluation context
- Validate migration results

Replay shall not unintentionally repeat external side effects.

Examples of side effects requiring protection:

- Sending duplicate emails
- Sending duplicate notifications
- Charging payments
- Reposting integration actions

Replay-safe consumers shall distinguish projection rebuilding from live side-effect execution.

---

# Event Versioning

Event schemas evolve through explicit versions.

Example:

```text
TradeExecuted v1
TradeExecuted v2
```

A version change is required when:

- A field changes meaning
- A required field is added
- A field type changes
- Payload structure becomes incompatible
- Consumer interpretation would change

A version change may not be required when:

- An optional backward-compatible field is added
- Documentation is clarified without semantic change
- Metadata changes without affecting domain meaning

Published historical events retain their original versions.

---

# Backward Compatibility

Consumers should support all event versions still present in the event store or provide deterministic upcasting.

Upcasting transforms an older event representation into the current internal representation without changing the original stored event.

Example:

```text
Stored Event v1
      │
      ▼
Upcaster
      │
      ▼
Current Internal Representation
```

---

# Event Corrections

Incorrect facts are corrected through new events.

Example:

```text
DraftPickTransferred
      │
      ▼
Administrative Error Discovered
      │
      ▼
DraftPickTransferReversed
```

Correction events shall reference:

- The original event
- The reason for correction
- The correcting actor
- The corrected business state

Corrections shall preserve the complete historical chain.

---

# Event Time

Legacy distinguishes between:

## Occurrence Time

When the business event happened.

## Recording Time

When Legacy stored the event.

## Effective Time

When the event's business effect begins, if different.

Example:

A future League Configuration may be approved today but become effective next season.

```json
{
  "occurred_at": "2026-07-28T18:30:00Z",
  "effective_at": "2027-01-01T00:00:00Z"
}
```

Event-specific documents shall state when `effective_at` is required.

---

# Historical Truth

Events are the canonical chronological record of meaningful platform activity.

Current-state tables may be corrected or rebuilt.

Published events remain historical facts about what the system recorded as occurring.

When historical interpretation requires the rules in effect at that time, events should reference:

- League Configuration version
- Rulebook version when necessary
- Season
- League Phase
- Effective timestamp

---

# Privacy and Security

Events shall not contain unnecessary sensitive data.

Avoid storing:

- Passwords
- Authentication tokens
- API keys
- Session credentials
- Full payment information
- Unnecessary personal data

User-identifying information should generally be referenced by canonical identifier rather than duplicated throughout event payloads.

Event access shall follow authorization and data-retention requirements.

---

# Audit Requirements

Every state-changing event shall be attributable to:

- An actor
- A Command or cause
- An owning aggregate
- A transaction or workflow
- A timestamp
- A payload version

Commissioner and administrative actions require especially clear attribution.

No privileged state change should occur without an auditable event trail.

---

# AI Interpretation

The AI Evidence Resolver may use events to reconstruct:

- Ownership history
- Contract history
- Trade history
- Draft history
- Roster movement
- Seasonal progression
- User decisions
- Prior recommendations

When current state and event history appear inconsistent, the AI shall not silently choose one.

It should:

1. Apply documented source precedence.
2. Identify the inconsistency.
3. Avoid unsupported conclusions.
4. Report insufficient or conflicting evidence when necessary.

AI systems shall treat advisory events differently from authoritative state-change events.

For example:

```text
AIRecommendationGenerated
```

is evidence that a recommendation was produced.

It is not evidence that the recommended action occurred.

---

# Event Documentation Standard

Every event chapter shall include:

1. Purpose
2. Event Name
3. Category
4. Owning Aggregate
5. Trigger
6. Preconditions
7. Payload
8. Referenced Entities
9. Emitting Actor
10. Correlation and Causation
11. Consumers
12. Ordering Requirements
13. Idempotency Requirements
14. Replay Behavior
15. Versioning
16. Invariants
17. AI Interpretation
18. Example

---

# Canonical Event Chapter Template

```markdown
---
title: Event Name
document: Event Catalog
event: EventName
version: 1
status: Draft
author: Legacy Product Architecture
last_updated: YYYY-MM-DD
depends_on:
  - ../domain-model/Relevant-Entity.md
---

# EventName

## Purpose

Describe the completed business fact represented by this event.

## Event Name

`EventName`

## Category

Identify the event category.

## Owning Aggregate

Identify the aggregate that owns the event.

## Trigger

Describe the successful Command or state transition that produces the event.

## Preconditions

List the conditions that must already be true before the event may occur.

## Payload

Define required and optional event-specific fields.

## Referenced Entities

List all canonical Domain Model entities referenced by the event.

## Emitting Actor

Define permitted actor types.

## Correlation and Causation

Describe expected workflow identifiers and related events.

## Consumers

List expected consumer categories.

## Ordering Requirements

Define aggregate sequence or workflow-order expectations.

## Idempotency Requirements

Describe how consumers prevent duplicate effects.

## Replay Behavior

Define whether and how the event may be replayed.

## Versioning

Document the current schema version and compatibility expectations.

## Invariants

List facts that must always remain true.

## AI Interpretation

Explain how the AI Architecture may interpret this event.

## Example

Provide a representative event envelope and payload.
```

---

# Proposed Catalog Structure

The Event Catalog should be grouped by domain area while preserving numbered reading order.

```text
event-catalog/

README.md

01-League-Events.md
02-Season-and-Phase-Events.md
03-Franchise-and-Membership-Events.md
04-Player-and-Roster-Events.md
05-Contract-and-Financial-Events.md
06-Draft-Events.md
07-Trade-Events.md
08-Waiver-and-Free-Agency-Events.md
09-Release-Events.md
10-Transaction-and-Audit-Events.md
11-AI-Advisory-Events.md
12-Integration-Events.md
```

Each chapter may define multiple closely related events.

The catalog does not require one file per individual event unless an event becomes complex enough to justify its own specification.

---

# Initial Canonical Event Inventory

The following inventory establishes the starting scope of the Event Catalog.

## League

- LeagueCreated
- LeagueConfigured
- LeagueArchived
- LeagueConfigurationCreated
- LeagueConfigurationUpdated
- LeagueConfigurationActivated
- LeagueConfigurationSuperseded

## Season and Phase

- LeagueSeasonScheduled
- LeagueSeasonStarted
- LeaguePhaseActivated
- LeaguePhaseCompleted
- LeaguePhaseAdvanced
- LeagueSeasonCompleted
- LeagueSeasonAdvanced

## Franchise and Membership

- FranchiseCreated
- FranchiseConfigured
- FranchiseArchived
- LeagueMemberInvited
- LeagueInvitationAccepted
- LeagueMembershipActivated
- LeagueMembershipRoleChanged
- LeagueMembershipDeactivated
- FranchiseOwnershipAssigned
- FranchiseOwnershipTransferred

## Player and Roster

- PlayerRegistered
- PlayerMetadataUpdated
- PlayerRetired
- RosterAssignmentCreated
- RosterDesignationChanged
- RosterAssignmentRemoved

## Contract and Financial

- ContractCreated
- ContractSalaryChanged
- ContractExtended
- ContractExpired
- ContractTerminated
- TeamOptionCreated
- TeamOptionExercised
- TeamOptionDeclined
- TeamOptionExpired
- DeadCapObligationCreated
- DeadCapObligationReduced
- DeadCapObligationSatisfied

## Draft

- DraftPickCreated
- DraftPickTransferred
- DraftPickConsumed
- RookieDraftScheduled
- RookieDraftStarted
- RookieDraftPaused
- RookieDraftResumed
- PlayerDrafted
- RookieDraftCompleted

## Trade

- TradeProposed
- TradeAssetAdded
- TradeAssetRemoved
- TradeAccepted
- TradeRejected
- TradeCancelled
- TradeApproved
- TradeExecuted
- TradeAssetTransferred
- TradeFailed

## Waiver and Free Agency

- WaiverClaimSubmitted
- WaiverClaimCancelled
- WaiverProcessingStarted
- WaiverClaimAwarded
- WaiverClaimRejected
- WaiverProcessingCompleted
- FreeAgentAcquired

## Release

- PlayerReleaseRequested
- PlayerReleased
- PlayerReleaseRejected

## Transaction and Audit

- TransactionRecorded
- AuditEventCreated
- CommissionerActionRecorded
- HistoricalCorrectionRecorded

## AI Advisory

- PlayerEvaluationCompleted
- TeamEvaluationCompleted
- LeagueEvaluationCompleted
- TransactionEvaluationCompleted
- AIRecommendationGenerated
- AIRecommendationPresented
- AIRecommendationAccepted
- AIRecommendationDismissed
- AIResponseValidationFailed

## Integration

- ExternalLeagueImported
- ExternalLeagueSynchronized
- ExternalRosterSynchronized
- ExternalPlayerMetadataUpdated
- IntegrationSynchronizationFailed

This inventory may evolve as event chapters expose missing business facts.

New events shall not be added solely to represent low-level technical activity.

---

# Events That Must Not Exist

Avoid generic events that conceal business meaning.

Do not use:

```text
EntityUpdated
DatabaseChanged
RecordSaved
ProcessCompleted
SyncFinished
ActionPerformed
```

Prefer specific facts:

```text
ContractExtended
DraftPickTransferred
LeagueSeasonCompleted
ExternalRosterSynchronized
```

Avoid emitting events for every field mutation.

Events should represent business-significant facts, not raw persistence operations.

---

# Event Granularity

Events should be granular enough to communicate one clear business fact.

Too broad:

```text
LeagueUpdated
```

Better:

```text
LeagueNameChanged
LeagueConfigurationActivated
LeagueArchived
```

Too narrow:

```text
ContractSalaryDigitChanged
```

Better:

```text
ContractSalaryChanged
```

The correct granularity is the smallest meaningful business fact that consumers may need independently.

---

# Event Lifecycle

Events do not possess mutable lifecycles.

An event has only two meaningful conditions:

```text
Not Published
      │
      ▼
Published
```

Once published, the event is permanent.

Any subsequent change is represented by another event.

---

# Validation Requirements

Before publication, every event shall be validated for:

- Canonical event type
- Supported event version
- Required envelope fields
- Required payload fields
- Valid canonical identifiers
- Owning aggregate
- Actor attribution
- Timestamp validity
- Correlation and causation requirements
- Applicable transaction association
- Payload data types

Invalid events shall not be published.

---

# Observability

Event processing should expose enough telemetry to diagnose:

- Publication failures
- Consumer failures
- Replay operations
- Duplicate delivery
- Out-of-order delivery
- Schema incompatibility
- Projection lag
- Dead-letter processing

Operational telemetry is not itself a Domain Event unless it represents a meaningful business occurrence.

---

# Testing Requirements

Every event definition should be supported by tests covering:

- Successful emission
- Failed Command produces no success event
- Required payload fields
- Actor attribution
- Aggregate ownership
- Event version
- Idempotent consumption
- Replay behavior
- Ordering behavior
- Historical correction behavior

Golden event fixtures may be maintained for compatibility testing.

---

# Change Management

Changes to this catalog require review when they affect:

- Event names
- Event meanings
- Payload semantics
- Aggregate ownership
- Required fields
- Versioning
- Ordering
- Replay guarantees
- Consumer expectations

Material architectural changes should also produce an Architecture Decision Record.

---

# Completion Criteria

The Event Catalog is complete when:

- Every meaningful state transition has a canonical event.
- Every event has one documented owner.
- Every event has a defined payload.
- Commands and events are clearly separated.
- State-changing events are attributable and auditable.
- Event versions are explicit.
- Consumers can process events idempotently.
- Historical events can be replayed safely.
- AI systems can distinguish authoritative events from advisory events.
- Database and API specifications can implement the catalog without inventing new event semantics.

---

# Final Principle

Legacy does not treat history as a collection of mutable rows.

It treats history as a sequence of completed business facts.

The Domain Model defines what exists.

The Rulebook defines what is allowed.

Commands request change.

Events preserve what happened.
