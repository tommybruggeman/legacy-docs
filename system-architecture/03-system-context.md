# Chapter 3 — System Context

## Purpose

The System Context defines Legacy's position relative to its users, external services, and surrounding technical environment.

It establishes:

- Who interacts with Legacy
- Which systems Legacy depends upon
- Which data Legacy owns
- Which responsibilities remain external
- Where trust boundaries exist

The System Context prevents external integrations from becoming confused with internal platform ownership.

---

## Context Diagram

```text
                         League Owners
                               │
                               ▼
                        Legacy Platform
                               │
      ┌──────────────┬─────────┼─────────┬──────────────┐
      ▼              ▼         ▼         ▼              ▼
Authentication   Sleeper   NFL Data   OpenAI      Notifications
 Provider          API     Providers                Services
```

Legacy sits at the center of the fantasy league operating environment.

---

## Primary Actors

## League Owner

A League Owner interacts with:

- Team Portal
- Rosters
- Contracts
- Salary information
- Trades
- Draft assets
- GM Assistant
- League history

League Owners should only access leagues and teams associated with their memberships.

---

## Commissioner

A Commissioner manages league-specific configuration and operations.

Commissioner capabilities may include:

- League settings
- Owner invitations
- Team assignments
- Contract corrections
- Salary adjustments
- Draft configuration
- Season rollover approval
- League-level overrides

Commissioner actions should be authorized and audited.

---

## Platform Administrator

A Platform Administrator manages system-wide operational concerns.

Capabilities may include:

- Execution monitoring
- Recovery approval
- Diagnostic inspection
- Audit review
- Platform configuration
- User support

Platform Administrators should not modify league state without explicit, audited operations.

---

## Developer

Developers interact with:

- Source repositories
- Database migrations
- Test environments
- Deployment systems
- Logs
- Operational tooling

Development access should remain separate from production user access.

---

## External Systems

## Authentication Provider

The Authentication Provider owns:

- User credentials
- Login sessions
- Password recovery
- Identity verification

Legacy owns:

- League memberships
- Team assignments
- Platform roles
- Application permissions

```text
Authentication Identity

↓

Legacy Membership

↓

League and Team Access
```

Authentication proves who the user is.

Legacy determines what the user may access.

---

## Sleeper API

Sleeper may provide:

- League identifiers
- League metadata
- Rosters
- Matchups
- NFL player identifiers
- Draft information

Legacy should treat Sleeper data as imported external evidence.

Legacy-owned contract, salary, rule, and evaluation state should remain canonical inside Legacy.

---

## NFL Data Providers

External NFL data may include:

- Player identity
- Team assignment
- Position
- Injury status
- Statistics
- Depth charts
- Transaction news

Legacy should normalize external data into canonical player and evidence models.

No external provider should directly mutate league-owned state.

---

## OpenAI

OpenAI may provide:

- Natural-language interpretation
- Explanatory prose
- Conversational formatting
- Contextual summaries

OpenAI should not own:

- League facts
- Rule evaluation
- Trade legality
- Salary calculations
- Player rankings
- Recovery decisions

Model output should be constrained by deterministic evidence and validated answer plans.

---

## Notification Services

Notification providers may deliver:

- Email invitations
- Transaction alerts
- Rollover notices
- Recovery alerts
- Administrative notifications

Notification delivery should remain separate from transaction success.

A failed email should not reverse a valid league operation unless the workflow explicitly requires delivery.

---

## Storage and Infrastructure

Infrastructure providers may host:

- Database
- Object storage
- Application runtime
- Background workers
- Monitoring systems

Infrastructure availability affects operations but does not define business behavior.

---

## Data Ownership Boundaries

Legacy owns:

- Leagues
- Teams
- League memberships
- League rules
- Contracts
- Salary state
- Draft assets
- Transactions
- Evaluations
- Event history
- Validation results
- Snapshots
- Recovery plans
- Audit records

External systems own:

- Authentication credentials
- External player feeds
- External league metadata
- Notification transport
- AI model execution

---

## Trust Boundaries

```text
Untrusted User Input
        │
        ▼
Application Validation
        │
        ▼
Authorized Service Layer
        │
        ▼
Canonical Data
```

All external input should be treated as untrusted until:

- Authenticated
- Authorized
- Validated
- Normalized
- Audited where required

---

## Integration Direction

External systems should connect through adapters.

```text
External Provider

↓

Provider Adapter

↓

Canonical Contract

↓

Legacy Service
```

Adapters isolate provider-specific behavior from core platform logic.

---

## Failure Boundaries

External failures should not corrupt internal state.

Examples:

- Sleeper unavailable
- OpenAI unavailable
- Email provider unavailable
- NFL feed delayed
- Storage temporarily unavailable

Each dependency should define:

- Timeout behavior
- Retry behavior
- Fallback behavior
- Error classification
- Operational visibility

---

## System Context Guarantees

The System Context guarantees:

1. Legacy remains the canonical owner of league state.
2. Authentication remains separate from authorization.
3. External data is normalized before use.
4. AI is restricted to interpretation and explanation.
5. External failures do not silently corrupt internal state.
6. Provider-specific behavior is isolated through adapters.
7. User and administrative access follow explicit trust boundaries.

---

## Definition of Done

This chapter is complete when Legacy's users, external dependencies, ownership boundaries, trust boundaries, and integration responsibilities are clearly defined within one system-level context model.
