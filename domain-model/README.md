---
title: Legacy Domain Model
document: Domain Model
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - ../rulebook/
---

# Legacy Domain Model

## Purpose

The Domain Model defines the canonical entities, relationships, states, and invariants that exist within the Legacy platform.

The Rulebook defines how Legacy must behave.

The Domain Model defines the objects through which that behavior is represented.

This documentation is written for AI agents and engineers responsible for interpreting, implementing, validating, or modifying Legacy.

An implementation is incorrect when its data model permits a state prohibited by the Rulebook or fails to represent a state required by the Rulebook.

---

# Authority

The Domain Model is subordinate to the Rulebook.

When a Domain Model document conflicts with the Rulebook:

1. The Rulebook remains authoritative.
2. The conflicting Domain Model document must be corrected.
3. Implementation must not rely on the conflicting definition.

The Domain Model is authoritative over database schemas, application models, API objects, and AI evidence structures unless an approved Architecture Decision Record explicitly establishes a different representation.

---

# Modeling Requirements

Every domain entity document shall define:

- Purpose
- Canonical identity
- Owned state
- Relationships
- Lifecycle
- Commands
- Emitted events
- Consumed events
- Validation rules
- Invariants
- Historical requirements
- AI interpretation rules

---

# Canonical Identity

Every persistent domain entity shall have one stable canonical identifier.

Display names, external provider identifiers, owner names, team names, and imported labels shall not replace canonical identity.

Aliases may assist resolution but shall not become authoritative identifiers.

---

# State Ownership

Every field or state transition shall have exactly one canonical owner.

Derived values may be calculated by other systems, but derived values shall not become independent sources of truth.

When two systems appear to own the same state, the model is incomplete and must be corrected before implementation.

---

# Relationship Rules

Relationships shall be explicit.

The platform shall not infer permanent relationships from display text, naming conventions, or temporary external identifiers.

Every relationship shall define:

- Source entity
- Target entity
- Cardinality
- Ownership
- Required or optional status
- Historical behavior

---

# Lifecycle Rules

Every entity with mutable state shall define an explicit lifecycle.

Lifecycle documentation shall identify:

- Initial state
- Permitted transitions
- Transition preconditions
- Transition side effects
- Terminal states
- Historical preservation requirements

Implicit lifecycle transitions are prohibited.

---

# Command Rules

Commands request changes to domain state.

Every command shall define:

- Required inputs
- Acting authority
- Preconditions
- Validation sequence
- Atomic state changes
- Emitted events
- Failure behavior

A failed command shall produce no partial state mutation.

---

# Event Rules

Events record completed domain state transitions.

Every event shall be:

- Immutable
- Attributable
- Timestamped
- Associated with canonical entity identifiers
- Sufficient to explain the resulting state

Events describe what occurred.

Commands describe what was requested.

The two shall not be treated as interchangeable.

---

# Invariant Rules

Invariants define conditions that must always remain true.

Every implementation shall enforce entity invariants before committing state changes.

An entity document may inherit platform-wide invariants from the Rulebook but shall restate any invariant required to understand that entity independently.

---

# Historical Preservation

Legacy is a historical franchise platform.

Entities and relationships that participate in meaningful league history shall not be permanently deleted merely because they are no longer active.

Inactive, expired, released, transferred, or completed records shall transition into historical states.

Corrections shall create attributable history rather than erase prior events.

---

# External Data

External platforms may provide identifiers, player information, injury status, matchup data, or other imported evidence.

External data is not automatically canonical.

Each entity document shall identify:

- Which external fields may be stored
- Which external provider is the source
- Whether the field is authoritative, advisory, or cached
- How conflicts are resolved
- What occurs when external data is unavailable

---

# AI Interpretation

AI agents shall reason from canonical entities and validated relationships.

AI shall not infer ownership, identity, contract status, roster status, or transaction legality from display text when canonical identifiers or deterministic evaluations are available.

When entity state is incomplete, contradictory, or unavailable, AI shall report the limitation rather than fabricate the missing state.

---

# Initial Entity Catalog

The initial Legacy Domain Model shall include:

1. League
2. League Season
3. League Phase
4. Franchise
5. User
6. League Membership
7. Player
8. Contract
9. Roster Assignment
10. Draft Pick
11. Rookie Draft
12. Draft Selection
13. Trade
14. Trade Asset
15. Player Release
16. Dead Cap Obligation
17. Team Option
18. Waiver Claim
19. Transaction
20. Audit Event
21. League Configuration
22. AI Recommendation
23. Evidence Packet
24. Decision Plan
25. Player Evaluation
26. Team Evaluation
27. League Evaluation
28. Transaction Evaluation

This catalog may expand only when a new concept has distinct identity, state, lifecycle, or ownership.

Implementation convenience alone is not sufficient justification for creating a domain entity.

---

# Document Structure

Each entity shall be defined in an independent Markdown file.

```text
domain-model/
├── README.md
├── 01-League.md
├── 02-League-Season.md
├── 03-League-Phase.md
├── 04-Franchise.md
├── 05-User.md
├── 06-League-Membership.md
├── 07-Player.md
├── 08-Contract.md
├── 09-Roster-Assignment.md
├── 10-Draft-Pick.md
├── 11-Rookie-Draft.md
├── 12-Draft-Selection.md
├── 13-Trade.md
├── 14-Trade-Asset.md
├── 15-Player-Release.md
├── 16-Dead-Cap-Obligation.md
├── 17-Team-Option.md
├── 18-Waiver-Claim.md
├── 19-Transaction.md
├── 20-Audit-Event.md
├── 21-League-Configuration.md
├── 22-AI-Recommendation.md
├── 23-Evidence-Packet.md
├── 24-Decision-Plan.md
├── 25-Player-Evaluation.md
├── 26-Team-Evaluation.md
├── 27-League-Evaluation.md
└── 28-Transaction-Evaluation.md
