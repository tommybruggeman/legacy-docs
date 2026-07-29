# Chapter 5 — Subsystem Map

## Purpose

The Subsystem Map defines every major Legacy subsystem, its ownership boundaries, and its relationships with surrounding systems.

The map provides one canonical reference for understanding where functionality belongs.

It prevents:

- Duplicate ownership
- Hidden coupling
- Business logic leakage
- Unclear implementation responsibility
- Conflicting sources of truth

---

## Platform Subsystem Map

```text
Legacy Platform
│
├── Identity and Access
├── League Management
├── Team and Membership
├── Player Domain
├── Roster Domain
├── Contract Domain
├── Salary Cap Domain
├── Draft Domain
├── Transaction Domain
├── Season Lifecycle
├── Rollover Pipeline
├── Event Catalog
├── Event Engine
├── Validation Framework
├── Snapshot System
├── Recovery Engine
├── Evaluation Systems
├── AI Architecture
├── Admin Tools
├── Integration
├── Audit and Observability
└── Persistence and Infrastructure
```

Each subsystem should have one primary responsibility.

---

## Identity and Access

The Identity and Access subsystem is responsible for:

- User authentication
- User identity mapping
- Session management
- League membership authorization
- Team access
- Administrative roles

It should distinguish between:

```text
Authentication

Who is the user?

Authorization

What may the user access?
```

The external authentication provider may verify identity, but Legacy owns application-level permissions.

---

## League Management

The League Management subsystem owns:

- League identity
- League metadata
- League configuration
- League status
- Season association
- Commissioner assignment
- External league references

It does not own individual team rosters, contracts, or transactions.

---

## Team and Membership

This subsystem owns:

- Team identity
- Team ownership
- Co-owner relationships
- League memberships
- Team access assignments
- Invitation acceptance
- Franchise continuity

A team should remain a persistent league entity even if ownership changes.

---

## Player Domain

The Player Domain owns canonical player identity.

It includes:

- Internal player ID
- Name
- Position
- NFL team
- External identifiers
- Player status
- Identity aliases

League-specific ownership should not be stored in the canonical player record.

---

## Roster Domain

The Roster Domain owns:

- Player-to-team assignment
- Roster slot
- Active status
- Injured reserve status
- Taxi status
- Practice squad status
- Roster eligibility

Roster state is league-specific and season-aware.

---

## Contract Domain

The Contract Domain owns:

- Contract identity
- Contract term
- Salary
- Contract status
- Contract start season
- Contract end season
- Extension history
- Expiration behavior

Contracts should remain historically traceable.

---

## Salary Cap Domain

The Salary Cap Domain owns:

- Salary commitments
- Team cap totals
- Dead cap
- Cap adjustments
- Penalties
- Credits
- Available cap
- Cap compliance

Cap totals should be derived from canonical contract and adjustment records.

---

## Draft Domain

The Draft Domain owns:

- Draft identity
- Draft year
- Draft rounds
- Draft slots
- Pick ownership
- Pick transfers
- Draft selections
- Future-pick state

Draft assets should remain persistent before, during, and after a draft.

---

## Transaction Domain

The Transaction Domain owns:

- Trade proposals
- Trade assets
- Free-agent signings
- Waiver claims
- Releases
- Contract changes
- Administrative adjustments
- Transaction history

Transactions should be explicit business objects rather than disconnected table updates.

---

## Season Lifecycle

The Season Lifecycle subsystem owns:

- Season states
- State transitions
- Transition eligibility
- Season milestones
- League-season association
- Current-season determination

It defines what stage the league is in.

---

## Rollover Pipeline

The Rollover Pipeline owns the ordered transition from one season to the next.

It coordinates:

- Pre-rollover validation
- Contract reduction
- Expiration processing
- Free-agent generation
- Dead-cap processing
- Draft advancement
- Roster cleanup
- Final validation

It does not own the detailed business rules for each domain mutation.

---

## Event Catalog

The Event Catalog owns the definition of supported events.

Each event definition includes:

- Event ID
- Event name
- Dependencies
- Handler reference
- Validation requirements
- Snapshot policy
- Recovery policy
- Version

The Event Catalog defines what may execute.

---

## Event Engine

The Event Engine owns:

- Event sequencing
- Dependency resolution
- Handler invocation
- Event status
- Idempotency control
- Execution history
- Retry coordination

The Event Engine executes events but does not define their business meaning.

---

## Validation Framework

The Validation Framework owns:

- Validation rules
- Validation sets
- Rule execution
- Validation severity
- Validation evidence
- Validation decisions
- Overrides

Validation rules may inspect data across domains without owning that data.

---

## Snapshot System

The Snapshot System owns:

- Snapshot creation
- Snapshot serialization
- Snapshot storage
- Snapshot integrity
- Snapshot retrieval
- Snapshot comparison
- Recovery eligibility

Snapshots represent state but do not define business rules.

---

## Recovery Engine

The Recovery Engine owns:

- Failure classification
- Recovery Context
- Recovery Plans
- Strategy selection
- Snapshot restoration coordination
- Recovery validation
- Resume planning

The Recovery Engine restores safe execution state without duplicating Event Engine behavior.

---

## Evaluation Systems

Evaluation Systems own structured analysis.

Core objects include:

- PlayerEvaluation
- TeamEvaluation
- LeagueEvaluation
- TransactionEvaluation

These systems convert canonical facts into decision-ready conclusions.

---

## AI Architecture

The AI Architecture owns:

- Query interpretation
- Requirement extraction
- Evidence requests
- Answer planning
- Natural-language explanation
- Response validation
- Conversation state

It does not own league truth, calculations, or business-rule decisions.

---

## Admin Tools

Admin Tools owns operational interfaces for:

- Execution monitoring
- Validation inspection
- Snapshot inspection
- Recovery governance
- Audit review
- Diagnostics
- Administrative actions

Admin Tools invoke approved capabilities rather than implementing them.

---

## Integration

The Integration subsystem owns:

- Public contracts
- Provider adapters
- Context propagation
- Version compatibility
- Cross-system orchestration
- Integration testing

It allows systems to collaborate without depending on private implementation details.

---

## Audit and Observability

This subsystem owns:

- Audit records
- Operational logs
- Metrics
- Traces
- Correlation IDs
- Dashboards
- Alerts
- Diagnostic evidence

Audit records and operational logs should remain conceptually distinct.

---

## Persistence and Infrastructure

Persistence owns:

- Repositories
- Database transactions
- Query access
- Schema constraints
- Data serialization

Infrastructure owns:

- Hosting
- Storage
- Workers
- Secrets
- Deployment
- Monitoring services

Neither subsystem should define league business rules.

---

## Ownership Matrix

| Capability | Owning Subsystem |
|------------|------------------|
| User identity | Identity and Access |
| League configuration | League Management |
| Team assignment | Team and Membership |
| Canonical player identity | Player Domain |
| Roster assignment | Roster Domain |
| Contract lifecycle | Contract Domain |
| Salary calculations | Salary Cap Domain |
| Draft assets | Draft Domain |
| Transactions | Transaction Domain |
| Season state | Season Lifecycle |
| Season transition workflow | Rollover Pipeline |
| Event definitions | Event Catalog |
| Event execution | Event Engine |
| State validation | Validation Framework |
| Historical checkpoints | Snapshot System |
| Failure recovery | Recovery Engine |
| Structured analysis | Evaluation Systems |
| Natural-language explanation | AI Architecture |
| Operational governance | Admin Tools |
| Cross-system contracts | Integration |
| Audit and telemetry | Audit and Observability |

---

## Dependency Map

```text
Identity and Access
        │
        ▼
Application Layer
        │
        ▼
League and Team Domains
        │
        ▼
Roster / Contract / Cap / Draft / Transaction Domains
        │
        ▼
Season and Event Systems
        │
        ▼
Validation / Snapshot / Recovery
        │
        ▼
Evaluation and AI
```

Dependencies should flow toward canonical domain ownership.

---

## Boundary Rules

A subsystem may:

- Read another subsystem's public output
- Invoke another subsystem's public contract
- Subscribe to published events
- Use shared immutable context

A subsystem may not:

- Mutate another subsystem's records directly
- Reimplement another subsystem's rules
- Depend on private classes
- Treat a derived value as canonical state

---

## Definition of Done

This chapter is complete when every major Legacy capability has one clearly defined owning subsystem and all cross-subsystem relationships are represented through explicit, non-overlapping boundaries.
