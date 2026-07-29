# System Architecture

## Overview

The System Architecture documentation defines how every major Legacy subsystem fits together as one complete fantasy league platform.

It provides the highest-level architectural view of the repository.

While individual folders describe specific domains such as league rules, event execution, validation, snapshots, recovery, and AI reasoning, the System Architecture explains how those domains collaborate to produce reliable platform behavior.

```text
Legacy Platform
      │
      ▼
System Architecture
      │
 ┌────┼───────────────┬───────────────┬───────────────┐
 ▼    ▼               ▼               ▼               ▼
Domain Rulebook   Event Systems   State Safety   AI Systems
Model                              Systems
```

The System Architecture is the primary entry point for engineers, operators, and future contributors.

---

# Purpose

The System Architecture exists to:

- Define the overall Legacy platform structure
- Establish subsystem ownership
- Explain runtime interactions
- Document architectural principles
- Define system boundaries
- Clarify data ownership
- Support implementation planning
- Guide future platform evolution

---

# Architecture Scope

This documentation covers:

- Platform structure
- Domain boundaries
- Runtime orchestration
- Data ownership
- Integration contracts
- Security boundaries
- Operational concerns
- Deployment considerations
- Architecture decisions

It does not replace detailed subsystem documentation.

Instead, it connects those subsystems into one coherent platform model.

---

# Major Subsystems

The Legacy platform is organized into the following major architectural areas.

## Domain Model

Defines the canonical business objects used throughout the platform.

Examples:

- League
- Team
- Owner
- Player
- Contract
- Draft Pick
- Transaction
- Season
- Evaluation

---

## Rulebook

Defines the authoritative league rules and business constraints governing platform behavior.

Examples:

- Salary cap rules
- Roster limits
- Contract rules
- Draft rules
- Transaction rules
- Season transition rules

---

## Season Lifecycle

Defines the states and transitions of a fantasy league season.

Examples:

- Preseason
- Regular season
- Playoffs
- Offseason
- Draft
- Free agency
- Rollover

---

## Rollover Pipeline

Defines the ordered process used to transition a league from one season to the next.

Examples:

- Pre-rollover checks
- Contract reduction
- Contract expiration
- Free-agent creation
- Draft advancement
- Final validation

---

## Event Catalog

Defines the complete set of supported system events.

Each Event Catalog entry describes:

- Event identity
- Dependencies
- Inputs
- Outputs
- Validation requirements
- Snapshot behavior
- Recovery behavior

---

## Event Engine

Executes events in deterministic dependency order.

The Event Engine owns:

- Event sequencing
- Handler execution
- Dependency evaluation
- Idempotency
- Execution history

---

## Validation Framework

Determines whether league state is valid before, during, and after system operations.

The Validation Framework owns:

- Validation rules
- Validation sets
- Validation execution
- Severity classification
- Validation decisions

---

## Snapshot System

Captures immutable representations of league state.

The Snapshot System supports:

- Checkpoints
- Recovery points
- Historical inspection
- State comparison
- Integrity verification

---

## Recovery Engine

Restores valid league state when execution fails.

The Recovery Engine owns:

- Failure analysis
- Recovery planning
- Snapshot restoration
- Resume execution
- Recovery validation

---

## Admin Tools

Provides secure operational visibility and controlled administrative governance.

Admin Tools includes:

- Execution dashboards
- Validation consoles
- Snapshot inspection
- Recovery controls
- Audit history
- Diagnostics

---

## Integration

Defines how subsystems communicate.

The Integration layer owns:

- Interface contracts
- Context propagation
- Lifecycle orchestration
- Version compatibility
- Cross-system testing

---

## AI Architecture

Defines how Legacy interprets owner questions, retrieves evidence, performs deterministic analysis, and generates natural-language responses.

The AI system should explain platform conclusions rather than invent them.

```text
User Question
      │
      ▼
Query Understanding
      │
      ▼
Evidence Resolution
      │
      ▼
Deterministic Analysis
      │
      ▼
Answer Plan
      │
      ▼
OpenAI Explanation
```

---

# Platform Architecture

```text
                         User Interfaces
                               │
                               ▼
                        Application Layer
                               │
                ┌──────────────┼──────────────┐
                ▼              ▼              ▼
           Admin Tools     League Features   GM Assistant
                │              │              │
                └──────────────┼──────────────┘
                               ▼
                     Orchestration Layer
                               │
          ┌────────────────────┼────────────────────┐
          ▼                    ▼                    ▼
     Event Engine      Validation Framework   Recovery Engine
          │                    │                    │
          └────────────────────┼────────────────────┘
                               ▼
                        Snapshot System
                               │
                               ▼
                      Domain Service Layer
                               │
                  ┌────────────┼────────────┐
                  ▼            ▼            ▼
              Rulebook    Domain Model   Evaluations
                               │
                               ▼
                        Persistence Layer
```

---

# Architectural Principles

The Legacy platform follows these core principles:

1. Business rules belong to the application.
2. State transitions must be deterministic.
3. AI explains conclusions but does not define truth.
4. Every important operation must be auditable.
5. Every failure must have a safe recovery path.
6. Subsystems communicate through explicit contracts.
7. Shared context should be immutable.
8. Data ownership must be clearly defined.
9. Administrative control must never bypass integrity safeguards.
10. Every subsystem must remain independently testable.

---

# Recommended Reading Order

```text
1. system-architecture
2. domain-model
3. rulebook
4. season-lifecycle
5. rollover-pipeline
6. event-catalog
7. event-engine
8. validation-framework
9. snapshot-system
10. recovery-engine
11. admin-tools
12. integration
13. AI-Architecture
```

This order moves from platform-level concepts into implementation-specific subsystems.

---

# Proposed Chapter Structure

```text
system-architecture/
├── README.md
├── 01-platform-overview.md
├── 02-architectural-principles.md
├── 03-system-context.md
├── 04-layered-architecture.md
├── 05-subsystem-map.md
├── 06-runtime-data-flow.md
├── 07-data-ownership.md
├── 08-security-boundaries.md
├── 09-observability-and-operations.md
├── 10-deployment-architecture.md
├── 11-architecture-decisions.md
└── 12-system-architecture-summary.md
```

---

# Documentation Guarantees

The System Architecture documentation guarantees:

1. Every major subsystem has a defined responsibility.
2. Every subsystem has clear ownership boundaries.
3. Runtime interactions are explicitly documented.
4. Data ownership is clearly assigned.
5. Cross-system communication uses documented contracts.
6. Architectural decisions remain traceable.
7. Future features can be evaluated against established principles.
8. Implementation teams share one canonical platform model.

---

# Definition of Done

The System Architecture is complete when:

- Every major Legacy subsystem is represented.
- Ownership boundaries are unambiguous.
- Runtime data flow is documented.
- Integration paths are explicit.
- Security and operational boundaries are defined.
- Deployment responsibilities are documented.
- Architecture decisions are recorded.
- The repository provides one coherent implementation blueprint.

At that point, the Legacy documentation repository becomes a complete architectural specification for building, operating, and evolving the platform.
