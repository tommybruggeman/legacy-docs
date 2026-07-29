# Chapter 4 — Layered Architecture

## Purpose

The Layered Architecture defines how Legacy responsibilities are organized from user interaction through persistence.

Each layer has a specific role.

Higher layers depend on published behavior from lower layers, while lower layers remain independent of presentation concerns.

This structure prevents business logic, data access, orchestration, and AI behavior from becoming mixed together.

---

## Architecture Overview

```text
┌─────────────────────────────────────────────┐
│ Presentation Layer                          │
├─────────────────────────────────────────────┤
│ Application Layer                           │
├─────────────────────────────────────────────┤
│ Orchestration Layer                         │
├─────────────────────────────────────────────┤
│ Domain Service Layer                        │
├─────────────────────────────────────────────┤
│ Evaluation and Intelligence Layer           │
├─────────────────────────────────────────────┤
│ Integration Layer                           │
├─────────────────────────────────────────────┤
│ Persistence Layer                           │
├─────────────────────────────────────────────┤
│ Infrastructure Layer                        │
└─────────────────────────────────────────────┘
```

Each layer owns a different class of responsibility.

---

## Presentation Layer

The Presentation Layer contains user-facing interfaces.

Examples:

- Team Portal
- Commissioner Portal
- GM Assistant
- Admin Tools
- Public pages
- Forms
- Dashboards

The Presentation Layer is responsible for:

- Rendering data
- Collecting user input
- Displaying validation feedback
- Triggering application operations
- Managing interface state

It should not own business rules.

---

## Application Layer

The Application Layer translates user intent into platform use cases.

Examples:

- Propose trade
- Sign free agent
- Extend contract
- Invite owner
- Start rollover
- Ask GM Assistant question

The Application Layer is responsible for:

- Request validation
- Authorization checks
- Use-case coordination
- Response construction
- Transaction boundaries

It delegates domain logic to lower layers.

---

## Orchestration Layer

The Orchestration Layer coordinates multi-step workflows.

Examples:

- Season rollover
- Trade processing
- Recovery execution
- Evaluation pipelines
- AI question answering

The Orchestration Layer determines:

- Execution order
- Dependency flow
- Context propagation
- Failure routing
- Completion requirements

It should coordinate domain services rather than implement their rules.

---

## Domain Service Layer

The Domain Service Layer contains core business behavior.

Examples:

- Contract calculations
- Salary-cap calculations
- Roster eligibility
- Draft-pick transfer
- Trade legality
- Season transitions
- Free-agent creation

This layer owns deterministic business operations.

```text
Input State

↓

Domain Rules

↓

State Transition

↓

Validated Result
```

---

## Rulebook Layer

The Rulebook defines the policies consumed by Domain Services.

Examples:

- Maximum roster size
- Salary-cap threshold
- Contract duration limits
- Draft-pick trading limits
- Dead-cap treatment
- Rollover timing

The Rulebook should remain declarative wherever possible.

---

## Evaluation and Intelligence Layer

This layer converts canonical facts into structured analysis.

Examples:

- PlayerEvaluation
- TeamEvaluation
- LeagueEvaluation
- TransactionEvaluation
- Recommendation objects
- AI Answer Plans

The layer should produce explainable, deterministic outputs before language generation occurs.

---

## AI Explanation Layer

The AI Explanation Layer converts approved system conclusions into natural language.

```text
Evidence Packet

↓

Deterministic Conclusion

↓

Answer Plan

↓

OpenAI Explanation

↓

Validated Response
```

The AI layer must not bypass the Domain Service or Evaluation layers.

---

## Integration Layer

The Integration Layer connects internal subsystems and external providers.

It owns:

- Interface contracts
- Provider adapters
- Context propagation
- Version compatibility
- Cross-system orchestration contracts

Examples:

- Sleeper adapter
- OpenAI adapter
- Notification adapter
- Authentication adapter
- NFL data adapter

---

## Persistence Layer

The Persistence Layer stores canonical platform data.

It is responsible for:

- Database repositories
- Query access
- Transaction support
- Data integrity constraints
- Snapshot persistence
- Audit persistence

The Persistence Layer should not define business meaning.

Database constraints may enforce invariants, but application services should explain and manage them.

---

## Infrastructure Layer

The Infrastructure Layer provides technical runtime capabilities.

Examples:

- Database hosting
- Object storage
- Background workers
- Application hosting
- Monitoring
- Secret management
- Deployment pipelines

Infrastructure supports platform execution without owning league behavior.

---

## Dependency Direction

Recommended dependency direction:

```text
Presentation

↓

Application

↓

Orchestration

↓

Domain Services

↓

Persistence Interfaces
```

Infrastructure implementations satisfy lower-level interfaces.

Core business logic should not depend directly on UI or provider-specific code.

---

## Invalid Dependency Examples

```text
Database Query
    │
    └── Defines Trade Legality ❌
```

Trade legality belongs to the Domain Service Layer.

```text
OpenAI Prompt
    │
    └── Calculates Salary Cap ❌
```

Salary calculations belong to deterministic services.

```text
Streamlit Page
    │
    └── Performs Season Rollover Logic ❌
```

The page should invoke an Application or Orchestration service.

---

## Cross-Layer Request Example

```text
Owner Submits Trade

↓

Presentation Layer

↓

Trade Application Service

↓

Authorization

↓

Trade Evaluation

↓

Rulebook and Domain Services

↓

Validation

↓

Transaction Persistence

↓

Audit Record

↓

Response Rendering
```

Each layer contributes one clearly defined responsibility.

---

## Layer Isolation

Each layer should expose narrow public interfaces.

Benefits include:

- Easier testing
- Reduced coupling
- Safer migrations
- Clearer ownership
- Easier provider replacement
- More reliable AI behavior

---

## Shared Objects

Cross-layer communication should use structured objects.

Examples:

- Commands
- Queries
- Context objects
- Domain results
- Validation results
- Evaluation objects
- Error objects

Raw dictionaries and undocumented records should not become long-term contracts.

---

## Layered Architecture Guarantees

The Layered Architecture guarantees:

1. User interfaces do not own business rules.
2. Orchestration does not replace domain logic.
3. AI does not define canonical conclusions.
4. Persistence does not define business meaning.
5. External providers remain isolated through adapters.
6. Each layer can be tested independently.
7. Dependencies flow toward stable domain abstractions.
8. Platform behavior remains explainable across the full request lifecycle.

---

## Definition of Done

This chapter is complete when Legacy responsibilities are clearly separated across presentation, application, orchestration, domain, evaluation, integration, persistence, and infrastructure layers with explicit dependency direction and ownership boundaries.
