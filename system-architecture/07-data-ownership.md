# Chapter 7 — Data Ownership

## Purpose

Data Ownership defines which subsystem is authoritative for each category of Legacy data.

Clear ownership prevents:

- Duplicate sources of truth
- Conflicting mutations
- Untraceable calculations
- Stale projections
- Accidental overwrites
- Cross-domain coupling

Every canonical record must have one owning subsystem.

---

## Canonical, Derived, Historical, and External Data

Legacy should classify data into four categories.

### Canonical Data

Authoritative internal state.

Examples:

- League rules
- Team assignments
- Contracts
- Draft-pick ownership
- Transactions

### Derived Data

Calculated from canonical state.

Examples:

- Team cap total
- Available cap
- Player value
- Trade score
- Roster strength

### Historical Data

Immutable records of past state or actions.

Examples:

- Completed trades
- Prior contracts
- Season snapshots
- Audit records

### External Data

Imported from outside systems.

Examples:

- Sleeper rosters
- NFL injury feeds
- External player identifiers
- OpenAI output

These categories should never be treated interchangeably.

---

## Ownership Principles

Every data object should have:

- One owning subsystem
- One canonical identifier
- Defined lifecycle
- Defined mutation interface
- Defined historical policy
- Defined validation rules

Multiple systems may consume the data.

Only the owner should control its lifecycle.

---

## Core Ownership Matrix

| Data Object | Owner |
|-------------|-------|
| User authentication identity | Authentication Provider |
| Application user profile | Identity and Access |
| League | League Management |
| League membership | Team and Membership |
| League team | Team and Membership |
| Canonical player | Player Domain |
| Roster assignment | Roster Domain |
| Player contract | Contract Domain |
| Cap adjustment | Salary Cap Domain |
| Draft pick | Draft Domain |
| Transaction | Transaction Domain |
| Season state | Season Lifecycle |
| Event definition | Event Catalog |
| Event execution record | Event Engine |
| Validation result | Validation Framework |
| Snapshot | Snapshot System |
| Recovery Plan | Recovery Engine |
| Evaluation object | Evaluation Systems |
| AI Answer Plan | AI Architecture |
| Audit record | Audit and Observability |

---

## League Data

The League Management subsystem owns:

- League ID
- League name
- External league ID
- League status
- Created-by identity
- Current season reference
- Configuration references

League rules should be referenced rather than embedded unpredictably across league records.

---

## Membership and Team Data

The Team and Membership subsystem owns:

- Membership ID
- League ID
- User ID
- Role
- Team ID
- League Team ID
- Ownership status
- Invitation relationship

The team identity should remain distinct from the user identity.

```text
User

↓

League Membership

↓

League Team
```

This preserves franchise continuity when owners change.

---

## Player Data

The Player Domain owns canonical identity.

Recommended canonical fields include:

- Internal player ID
- Full name
- Position
- NFL team
- Active status
- External IDs
- Alias mappings

League-specific attributes should live elsewhere.

Examples:

- Contract salary
- Roster owner
- Taxi status
- League value

---

## Roster Data

The Roster Domain owns the relationship between:

- League
- Team
- Player
- Season
- Roster status

A roster record should identify when and where the assignment applies.

---

## Contract Data

The Contract Domain owns:

- Contract ID
- League ID
- Team ID
- Player ID
- Salary
- Contract years
- Contract status
- Effective season
- Expiration season

Historical contracts should not be deleted merely because they expire.

Expiration should change lifecycle state and produce related free-agent state where appropriate.

---

## Salary Data

The Salary Cap Domain owns:

- Dead cap
- Credits
- Penalties
- Manual adjustments
- Cap exceptions
- Adjustment season

Team cap totals should normally be derived.

```text
Active Contract Salaries

+

Dead Cap

+

Penalties

-

Credits

=

Team Cap Total
```

Stored totals should be treated as projections or caches unless explicitly designated canonical.

---

## Draft Data

The Draft Domain owns:

- Draft ID
- Pick ID
- Draft year
- Round
- Original team
- Current team
- Selection status
- Selected player
- Transaction history

Original ownership and current ownership must remain separately traceable.

---

## Transaction Data

Transactions should serve as immutable records of approved changes.

A Transaction owns:

- Transaction ID
- Type
- Status
- Actor
- League
- Assets
- Effective timestamp
- Validation result
- Applied mutations

Transaction execution may update owned domain records, but the transaction record remains the historical evidence of why the changes occurred.

---

## Event and Execution Data

The Event Catalog owns event definitions.

The Event Engine owns execution records.

These should remain distinct.

```text
Event Definition

What may execute?

Event Execution

What did execute?
```

---

## Validation Data

The Validation Framework owns:

- Rule results
- Validation runs
- Severity
- Evidence
- Overrides
- Final decision

Validation does not own the records it inspects.

---

## Snapshot Data

The Snapshot System owns immutable captured state.

Snapshot contents are historical representations of records owned by other domains.

Restoring a Snapshot should occur through approved recovery workflows.

---

## Evaluation Data

Evaluation Systems own structured analysis.

Evaluations should identify:

- Input evidence version
- Method version
- Generated time
- Scope
- Confidence
- Output

Evaluations are derived data, not replacements for canonical facts.

---

## AI Data

The AI Architecture may own:

- Conversation state
- Parsed query
- Requirement Graph
- Evidence Packet reference
- Answer Plan
- Rendered response
- Response validation

AI-generated prose should not be stored as canonical league truth.

---

## Historical Preservation

Canonical state may change.

Historical evidence should remain immutable.

Example:

```text
Contract v1

↓

Extension Transaction

↓

Contract v2
```

The system should preserve the prior contract version or sufficient history to reconstruct it.

---

## Soft Delete and Lifecycle State

Important business records should usually transition state rather than disappear.

Examples:

```text
ACTIVE

EXPIRED

CANCELLED

SUPERSEDED

ARCHIVED
```

Hard deletion should be limited to:

- Invalid test data
- Privacy-required deletion
- Explicitly disposable technical records

---

## Cached and Materialized Data

Caches may improve performance but must identify:

- Source records
- Calculation version
- Generated time
- Expiration policy
- Rebuild method

A cache must never silently become the only source of truth.

---

## Data Mutation Rules

A subsystem may mutate:

- Records it owns
- Records explicitly delegated through a public command
- Transactionally related records through approved orchestration

It may not directly mutate another subsystem's private tables.

---

## Data Ownership Guarantees

Data Ownership guarantees:

1. Every canonical object has one owner.
2. Derived values remain distinguishable from canonical state.
3. Historical records are preserved.
4. External data is normalized before adoption.
5. Caches are reproducible.
6. Mutation paths are explicit.
7. Team identity remains separate from user identity.
8. AI output never becomes canonical league truth without deterministic validation.

---

## Definition of Done

This chapter is complete when every major Legacy data object has a clearly documented owner, lifecycle, mutation path, historical policy, and distinction between canonical, derived, historical, and external state.
