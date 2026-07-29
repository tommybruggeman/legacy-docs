# Legacy

> **The canonical architectural specification for the Legacy Fantasy Football Platform.**

---

## Vision

Legacy is being designed as the definitive operating system for advanced dynasty fantasy football leagues.

Rather than treating a league as a collection of disconnected pages, Legacy models every aspect of league management as a deterministic software platform capable of supporting:

- Multi-year player contracts
- Salary caps
- Dead cap accounting
- Draft asset ownership
- Rule enforcement
- Historical league state
- AI-assisted general management
- Recoverable operations
- Long-term league evolution

The goal is to build a platform that commissioners trust, managers enjoy using, and engineers can confidently extend for years to come.

---

# Guiding Philosophy

Legacy is built around one fundamental principle:

> **Business truth belongs to deterministic application logic. AI exists to explain that truth—not create it.**

Every architectural decision in this repository reinforces that philosophy.

---

# Architectural Principles

The platform is designed around several non-negotiable principles.

- Deterministic business logic
- Explicit subsystem ownership
- Layered architecture
- Event-driven execution
- Validation before persistence
- Immutable historical state
- Recoverability through Snapshots
- Explainable AI
- Defense-in-depth security
- Operational reliability

---

# Repository Structure

```text
Legacy

├── AI-Architecture/
├── admin-tools/
├── domain-model/
├── event-catalog/
├── event-engine/
├── integration/
├── recovery-engine/
├── rulebook/
├── snapshot-system/
├── system-architecture/
├── validation-framework/
└── README.md
```

Each directory represents one canonical subsystem of the platform.

Responsibilities do not overlap.

---

# Documentation Overview

| Documentation | Purpose |
|--------------|---------|
| **System Architecture** | Platform structure, ownership, and runtime design |
| **Domain Model** | Business entities and relationships |
| **Rulebook** | League rules and deterministic business policies |
| **Event Catalog** | Canonical business events |
| **Event Engine** | Event execution pipeline |
| **Validation Framework** | State validation and business rule enforcement |
| **Snapshot System** | Immutable league state preservation |
| **Recovery Engine** | Failure recovery and rollback |
| **Admin Tools** | Operational and commissioner tooling |
| **Integration** | Cross-system communication contracts |
| **AI Architecture** | GM Assistant reasoning and conversational architecture |

---

# Recommended Reading Order

Developers new to the project should read the documentation in the following order.

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

This sequence progresses from platform concepts to implementation-specific systems.

---

# Platform Architecture

At a high level, Legacy follows a layered architecture.

```text
Presentation

↓

Application Services

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

Every layer has explicit ownership and communicates through defined interfaces.

---

# AI Philosophy

Legacy intentionally separates business reasoning from conversational reasoning.

```text
User Question

↓

Evidence Retrieval

↓

Deterministic Analysis

↓

Decision

↓

OpenAI Explanation

↓

Response
```

The platform determines what is true.

AI determines how to explain it.

---

# Documentation Status

| Area | Status |
|------|--------|
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

# Architecture Status

The Legacy architecture is considered **Version 1.0**.

The platform's foundational architecture is complete and serves as the canonical reference for all future implementation.

Future work should primarily involve:

- Service implementation
- Database evolution
- Event execution
- Validation logic
- AI capabilities
- Testing
- Performance
- Operational maturity

rather than introducing additional architectural subsystems.

---

# Architecture Freeze

The documents in this repository represent the canonical architecture of the Legacy platform.

Future architectural changes should:

- Update existing documentation.
- Preserve subsystem ownership.
- Avoid duplicate specifications.
- Maintain deterministic behavior.
- Document significant changes through Architecture Decision Records (ADRs).

The goal is to evolve the platform without fragmenting its architectural foundation.

---

# Intended Audience

This repository is intended for:

- Software Engineers
- Platform Architects
- AI Engineers
- Contributors
- Future Maintainers
- Commissioners interested in platform design

It is not intended to serve as end-user documentation.

---

# Long-Term Vision

Legacy is designed to become the premier operating system for advanced fantasy football leagues.

Every architectural decision documented here supports that objective by emphasizing determinism, recoverability, explainability, extensibility, and long-term maintainability.

As implementation progresses, this repository will remain the single source of truth for how the platform is intended to evolve.
