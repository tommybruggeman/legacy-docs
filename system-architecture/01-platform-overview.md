# Chapter 1 — Platform Overview

## Purpose

The Platform Overview defines Legacy as one complete fantasy league operating system.

Legacy is not a collection of disconnected league-management features.

It is a coordinated platform responsible for:

- Maintaining league state
- Enforcing league rules
- Executing transactions
- Managing contracts and salary
- Advancing seasons
- Evaluating players and teams
- Supporting league owners
- Preserving historical accuracy
- Recovering safely from failures

The architecture must support all of these responsibilities without allowing one subsystem to become the source of truth for another.

---

## Platform Mission

Legacy exists to provide a reliable, intelligent, and auditable operating system for complex fantasy leagues.

The platform should support leagues with:

- Custom rules
- Salary caps
- Multi-year contracts
- Dead cap
- Draft-pick ownership
- Free agency
- Trades
- Historical records
- Seasonal rollover
- AI-assisted decision support

The platform must remain adaptable enough to support different league structures without sacrificing deterministic behavior.

---

## Platform Model

```text
League Rules
      │
      ▼
Domain State
      │
      ▼
Business Operations
      │
      ▼
Validation
      │
      ▼
Persistent League State
      │
      ▼
Analytics and AI
```

Every layer depends on validated information from the layers below it.

---

## Core Platform Capabilities

## League Management

Legacy maintains the identity and configuration of each league.

This includes:

- League metadata
- League membership
- Team assignment
- Owner permissions
- League rules
- Season configuration

---

## Team Management

Legacy maintains each franchise as a persistent league entity.

This includes:

- Team identity
- Owners and co-owners
- Rosters
- Contracts
- Salary commitments
- Draft assets
- Historical performance

---

## Player Management

Legacy maintains canonical player records independent of any one league.

League-specific state is stored separately.

```text
Canonical Player

↓

League Player State

↓

Team Roster State

↓

Contract State
```

This separation allows one player to exist across many leagues while preserving league-specific ownership and contract information.

---

## Contract and Salary Management

Legacy supports deterministic contract and salary behavior.

This includes:

- Contract creation
- Contract reduction
- Contract expiration
- Salary allocation
- Dead cap
- Cap adjustments
- Free-agent transitions

Contract rules should be enforced by services and validation rather than user-interface assumptions.

---

## Draft Management

Legacy tracks draft assets as persistent league property.

This includes:

- Draft years
- Draft rounds
- Original ownership
- Current ownership
- Pick trades
- Draft completion
- Future-pick advancement

Draft assets should remain independently traceable throughout their lifecycle.

---

## Transaction Management

Legacy supports structured transactions.

Examples include:

- Trades
- Free-agent signings
- Waiver claims
- Contract extensions
- Player releases
- Draft selections
- Administrative adjustments

Every transaction should produce explicit state changes and permanent audit records.

---

## Season Management

Legacy manages the complete season lifecycle.

```text
Preseason

↓

Regular Season

↓

Playoffs

↓

Offseason

↓

Draft and Free Agency

↓

Season Rollover

↓

Next Season
```

Season transitions should be deterministic and recoverable.

---

## Evaluation Systems

Legacy converts league facts into decision-ready evaluations.

Core evaluation objects include:

```text
PlayerEvaluation

↓

TeamEvaluation

↓

LeagueEvaluation

↓

TransactionEvaluation
```

Evaluations should use deterministic inputs and explainable methods.

---

## AI Assistance

The GM Assistant helps owners understand their league and make better decisions.

The AI layer should:

- Interpret natural-language questions
- Determine required facts
- Retrieve relevant evidence
- Use deterministic calculations
- Produce structured conclusions
- Explain those conclusions conversationally

The AI layer should never invent league facts or override deterministic system conclusions.

---

## Operational Systems

Legacy includes operational systems that ensure reliability.

These include:

- Validation Framework
- Snapshot System
- Recovery Engine
- Admin Tools
- Audit History
- Diagnostics

Operational reliability is part of the product architecture rather than an afterthought.

---

## Platform Users

Legacy supports multiple user types.

### League Owner

Manages one or more teams.

### Commissioner

Administers league settings and league-specific operations.

### Platform Administrator

Monitors system health and supports recovery.

### Developer

Builds and maintains platform services.

### AI Consumer

Interacts with Legacy through the GM Assistant.

Each user type should receive only the capabilities required for their role.

---

## Platform Boundaries

Legacy owns:

- League data
- League rules
- League transactions
- League calculations
- League evaluations
- Season transitions
- Historical records

External systems may provide:

- Authentication
- NFL player metadata
- Injury information
- Platform integrations
- Notifications
- AI-generated prose

External systems should never become the canonical source for Legacy-owned state.

---

## Architectural Goals

The platform should be:

- Deterministic
- Auditable
- Recoverable
- Extensible
- Secure
- Explainable
- League-aware
- Historically accurate

---

## Definition of Done

This chapter is complete when Legacy is clearly defined as one integrated fantasy league operating system with explicit responsibilities across league management, transactions, season operations, evaluation, AI assistance, and platform reliability.
