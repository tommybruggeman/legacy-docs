---
title: League Calendar
document: Rulebook
chapter: 23
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
---

# Chapter 23 — League Calendar

## Purpose

The League Calendar defines the chronological structure of a Legacy League.

Every league operation occurs within a defined league phase.

League phases determine which commands are valid and which operations are prohibited.

Time is a first-class concept within Legacy.

---

# System Ownership

This chapter governs:

- League phases
- Phase transitions
- Command availability
- Seasonal progression

---

# Business Rules

## Rule 23.1 — League Phases

A league shall exist in exactly one phase at any given time.

Example phases may include:

- Preseason
- Rookie Draft
- Free Agency
- Regular Season
- Playoffs
- Offseason

The available phases are determined by league configuration.

---

## Rule 23.2 — Phase Restrictions

Commands may be restricted by the active league phase.

Examples include:

- Rookie Draft commands are valid only during the Rookie Draft phase.
- Certain contract actions may be restricted during the Playoffs.
- Annual Rollover may occur only during the Offseason.

---

## Rule 23.3 — Phase Transition

Transitioning between phases shall trigger any required deterministic system events.

Examples include:

- Opening Free Agency
- Closing Trade Windows
- Beginning Annual Rollover

---

# Invariants

- Exactly one active league phase.
- Phase transitions are deterministic.
- Commands honor phase restrictions.

---

# Canonical Principles

Time governs legality.

League phases determine available actions.

No command may ignore the League Calendar.
