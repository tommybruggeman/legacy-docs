# Legacy Documentation

> **The architectural specification for Legacy — a deterministic fantasy football operating system.**

---

# Overview

Legacy is designed to be more than a fantasy football application.

It is a complete operating system for dynasty leagues with support for:

- Salary Caps
- Multi-Year Contracts
- Dead Cap
- Draft Pick Ownership
- League Rule Enforcement
- Season Rollover
- Historical League State
- AI-Assisted General Management
- Deterministic Decision Making

This repository serves as the canonical architectural specification for the Legacy platform.

It documents how every subsystem works, how they interact, and the engineering principles that guide future development.

---

# Guiding Philosophy

Legacy is built around one central principle:

> **Business truth belongs to deterministic application logic. AI exists to explain that truth—not create it.**

Every architectural decision in this repository reinforces that philosophy.

---

# Architecture Overview

```text
                          Legacy Platform

                                 │
                                 ▼
                        System Architecture
                                 │
      ┌───────────────┬──────────────┬──────────────┐
      ▼               ▼              ▼              ▼
 Domain Model     Rulebook      Runtime Systems   AI Systems
      │               │              │              │
      ▼               ▼              ▼              ▼
 Players       League Rules    Event Engine     GM Assistant
 Contracts     Salary Rules    Validation       Query Engine
 Draft Picks   Trade Rules     Snapshots        Evaluations
 Teams          Rollover        Recovery         OpenAI
```

---

# Repository Structure

| Folder | Purpose |
|---------|---------|
| `system-architecture` | Platform-wide architecture and subsystem relationships |
| `domain-model` | Canonical business entities and relationships |
| `rulebook` | League rules and business policies |
| `event-catalog` | Definitions of every supported platform event |
| `event-engine` | Deterministic event execution engine |
| `validation-framework` | Validation rules, execution, and decisions |
| `snapshot-system` | Immutable state checkpoints |
| `recovery-engine` | Failure recovery and rollback planning |
| `admin-tools` | Operational dashboards and administrative workflows |
| `integration` | Contracts and subsystem communication |
| `AI-Architecture` | GM Assistant reasoning and conversational AI |

---

# Platform Layers

```text
Presentation

↓

Application

↓

Orchestration

↓

Domain Services

↓

Validation

↓

Persistence

↓

Infrastructure
```

Each layer owns a specific responsibility and communicates through explicit interfaces.

---

# Core Design Principles

Legacy follows several non-negotiable architectural principles.

## Deterministic Business Logic

Business decisions must produce identical results for identical inputs.

---

## Explicit Ownership

Every subsystem owns one domain.

Ownership never overlaps.

---

## Immutable History

Historical state should never be silently overwritten.

---

## Event-Driven Execution

Complex workflows execute through ordered events with explicit dependencies.

---

## Validation Everywhere

Validation protects every important state transition.

---

## Safe Recovery

Every important operation can recover from failure through immutable Snapshots.

---

## Explainable AI

OpenAI generates explanations.

Legacy determines conclusions.

---

# Documentation Reading Order

New contributors should read the documentation in the following order:

1. System Architecture
2. Domain Model
3. Rulebook
4. Event Catalog
5. Event Engine
6. Validation Framework
7. Snapshot System
8. Recovery Engine
9. Admin Tools
10. Integration
11. AI Architecture

This sequence moves from platform concepts toward implementation details.

---

# Architectural Goals

Legacy aims to be:

- Deterministic
- Explainable
- Recoverable
- Auditable
- Extensible
- Secure
- AI-Assisted
- League-Aware

These goals guide every architectural decision within the platform.

---

# Documentation Status

| Subsystem | Status |
|-----------|--------|
| System Architecture | ✅ Complete |
| Domain Model | ✅ Complete |
| Rulebook | ✅ Complete |
| Event Catalog | ✅ Complete |
| Event Engine | ✅ Complete |
| Validation Framework | ✅ Complete |
| Snapshot System | ✅ Complete |
| Recovery Engine | ✅ Complete |
| Admin Tools | ✅ Complete |
| Integration | ✅ Complete |
| AI Architecture | ✅ Complete |

---

# Intended Audience

This repository is intended for:

- Platform Engineers
- Backend Developers
- Frontend Developers
- Commissioners
- Technical Contributors
- Future Maintainers
- AI Engineers

It is not end-user documentation.

---

# Future Evolution

Legacy has been intentionally designed so new capabilities can be introduced without restructuring the existing platform.

Examples include:

- Simulation Engine
- Dynasty Rankings
- Trade Market Engine
- Startup Draft Generator
- Salary Optimization
- League Analytics
- Mobile Applications
- Public API
- Additional AI Assistants

The architecture should evolve through documented subsystem ownership and explicit integration contracts.

---

# Repository Vision

Legacy is intended to become the definitive operating system for advanced fantasy football leagues.

This documentation establishes the architectural foundation required to build, maintain, and evolve the platform for years to come while preserving deterministic behavior, operational reliability, and explainable AI-assisted decision making.
