
---
title: League Identity
document: Rulebook
chapter: 2
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
related_documents:
  - 00-Manifesto.md
  - 01-Platform-Philosophy.md
  - 03-League-Lifecycle.md
---

# Chapter 2 — League Identity

## Purpose

A Legacy League is the highest-level organizational unit within the platform.

Every franchise, player contract, financial record, draft pick, transaction, historical event, and league setting belongs to exactly one league.

This chapter defines the identity of a Legacy league and establishes the foundational characteristics that distinguish one league from another.

---

# 2.1 Definition

A Legacy League represents a persistent competitive organization in which franchises compete across multiple seasons under a shared set of league rules.

Unlike traditional fantasy leagues that reset annually, a Legacy League is intended to continue indefinitely.

The league—not the season—is the primary organizational unit of the platform.

---

# 2.2 League Objectives

Every Legacy League exists to provide:

- Long-term franchise competition
- Deterministic rule enforcement
- Financial management
- Historical preservation
- Commissioner governance
- Strategic roster construction
- Artificial intelligence assistance

Every feature implemented within a league should reinforce one or more of these objectives.

---

# 2.3 League Identity

Each league possesses a permanent identity consisting of:

- League Name
- League Identifier
- Creation Date
- Commissioner
- League Logo
- Active Status
- Rule Configuration
- Financial Configuration

These values uniquely identify a league throughout its lifetime.

---

# 2.4 Configurable League Settings

The following settings may be configured by the commissioner during league creation or through league administration tools.

## General

- League Name
- League Logo
- Commissioner
- Co-Commissioner

## Financial

- Salary Cap
- Rookie Wage Scale
- IR Salary Percentage
- Taxi Squad Salary Percentage

## Administrative

- League Visibility
- Invitation Settings
- Commissioner Permissions

Future versions of Legacy may introduce additional configurable settings without altering the league identity model.

---

# 2.5 Team Capacity

Legacy supports any even number of franchises.

Examples include:

- 8 Teams
- 10 Teams
- 12 Teams
- 14 Teams
- 16 Teams

League size is established during league creation.

Changing league size after league creation is considered an exceptional administrative action and may require commissioner intervention.

---

# 2.6 Canonical League Ownership

Each league has one authoritative source of governance.

Legacy owns:

- Contracts
- Salary Cap
- Dead Cap
- Draft Picks
- Financial Records
- League Rules
- Historical Records
- Franchise Assets

External fantasy providers may temporarily supply:

- Weekly scoring
- NFL schedules
- Live matchup information

Legacy remains the canonical owner of all long-term league data.

---

# 2.7 League Persistence

A Legacy League is intended to exist indefinitely.

Annual league advancement does not create a new league.

Instead, advancing the season updates the existing league while preserving all historical records.

The league identity remains unchanged across every season.

---

# 2.8 Franchise Continuity

Franchises belong to the league—not to individual owners.

Ownership may change over time.

Franchise names may change.

Logos may change.

Managers may change.

Historical achievements remain attached to the franchise itself.

This preserves the continuity of league history across generations of ownership.

---

# 2.9 Relationship to Seasons

A season represents one competitive chapter within the life of a league.

The season is subordinate to the league.

Historical seasons collectively form the permanent history of the league.

Legacy treats seasons as historical records rather than independent league instances.

---

# 2.10 League Lifecycle

Every league progresses through the following lifecycle:

1. League Creation
2. League Configuration
3. Franchise Assignment
4. Active Competition
5. Annual Advancement
6. Historical Preservation

This lifecycle repeats annually without recreating the league itself.

The detailed operational rules governing annual advancement are defined in Chapter 15 — Annual Rollover.

---

# Design Principles

The League Identity model is designed around persistence.

Everything within Legacy assumes that leagues are intended to continue for many years.

This philosophy influences database design, historical preservation, contract management, financial accounting, and artificial intelligence.

A league should feel like a living organization rather than a collection of independent fantasy seasons.

---

# Chapter Summary

A Legacy League is a permanent competitive organization that serves as the foundation for every other object within the platform.

Franchises, seasons, contracts, financial systems, league history, and artificial intelligence all operate within the context of a single persistent league.

Subsequent chapters build upon this identity by defining how leagues evolve throughout their lifecycle.
