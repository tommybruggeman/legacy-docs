---
title: Evidence Resolver
document: AI Architecture
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 03-Intent.md
---

# Evidence Resolver

## Purpose

The Evidence Resolver gathers every fact required to answer the user's question before any reasoning occurs.

It is the bridge between Legacy's data and its deterministic reasoning engine.

The Evidence Resolver never makes recommendations.

It only establishes the factual foundation from which recommendations can be made.

---

# Guiding Principle

No recommendation may be generated from incomplete evidence when required evidence is available.

If required evidence cannot be obtained, the assistant shall acknowledge the limitation rather than fabricate missing information.

---

# Inputs

The Evidence Resolver receives:

- Intent
- Canonical Question
- Referenced Entities
- Current League
- Current Season
- Conversation Context

---

# Outputs

The Evidence Resolver produces a structured Evidence Packet.

An Evidence Packet may contain:

- League Configuration
- Franchise Information
- Contracts
- Roster Assignments
- Salary Cap
- Dead Cap
- Draft Picks
- Transactions
- League Trends
- Player Metadata
- External Football Intelligence

---

# Evidence Sources

## Internal Sources

- Domain Model entities
- League database
- Historical transactions
- Contracts
- Rulebook
- Previous evaluations

---

## External Sources

- Player news
- Injury reports
- Depth charts
- Coaching changes
- NFL transactions
- Schedule information

External evidence supplements internal truth but never overrides league-specific data.

---

# Evidence Completeness

Evidence shall be classified as:

Complete

All required information is available.

Partial

Some required evidence is unavailable.

Insufficient

The request cannot be answered reliably.

---

# Evidence Freshness

Every evidence item should include:

- Source
- Timestamp
- Confidence
- Version

Outdated evidence should be deprioritized when fresher evidence exists.

---

# Evidence Conflicts

When evidence conflicts:

Priority order:

1. League Database
2. Rulebook
3. League Configuration
4. Internal Historical Records
5. Trusted External Sources

Conflicts shall never be silently ignored.

---

# Missing Evidence

If evidence cannot be retrieved:

Legacy shall:

- identify missing information
- explain why it matters
- continue only when safe

Otherwise reasoning shall terminate.

---

# Evidence Packet

The Evidence Packet becomes the canonical input to every downstream reasoning stage.

No downstream stage should independently retrieve information already contained within the packet.

---

# Performance Principles

Evidence should be gathered once.

Reasoning modules consume shared evidence rather than performing duplicate retrieval.

This minimizes latency while ensuring consistent conclusions across all reasoning modules.

---

# Design Principles

The Evidence Resolver owns facts.

It does not:

- rank players
- evaluate trades
- calculate values
- determine legality
- produce recommendations

Its sole responsibility is establishing truth.

---

# Guiding Principle

The quality of every recommendation is limited by the quality of the evidence supporting it.

Legacy therefore prioritizes complete, accurate, and reproducible evidence before any football reasoning begins.
