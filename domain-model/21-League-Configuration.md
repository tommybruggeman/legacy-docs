---
title: League Configuration
document: Domain Model
entity: League Configuration
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 01-League.md
  - 02-League-Season.md
---

# League Configuration

## Purpose

League Configuration defines the complete set of configurable rules, limits, and operational settings that govern a League.

Unlike the Rulebook, which defines the platform's universal business behavior, League Configuration defines the league-specific values used to apply those rules.

The Rulebook answers **what is possible**.

League Configuration answers **how this League is configured**.

---

# Canonical Identity

Every League Configuration shall possess one immutable canonical identifier.

Each League shall have exactly one active configuration for a given League Season.

Historical configurations remain permanently associated with their corresponding seasons.

---

# Owned State

A League Configuration owns all configurable league settings, including but not limited to:

### League Settings

- League Name
- Season Year
- Time Zone
- Commissioner Settings

### Roster Configuration

- Active Roster Size
- Bench Size
- Taxi Squad Capacity
- Injured Reserve Capacity
- Positional Limits

### Financial Configuration

- Salary Cap
- Minimum Salary
- Maximum Contract Length
- Rookie Scale Rules
- Dead Cap Formula
- Team Option Rules

### Draft Configuration

- Rookie Draft Rounds
- Draft Order Method
- Draft Clock
- Pick Trading Rules

### Waiver Configuration

- Waiver System
- Waiver Priority Method
- Processing Schedule
- Free Agent Rules

### Trade Configuration

- Trade Review Period
- Commissioner Approval Requirements
- Deadline
- Offseason Trading Rules

### Seasonal Configuration

- Regular Season Length
- Playoff Structure
- Consolation Rules
- Annual Rollover Settings

### AI Configuration

- Recommendation Permissions
- Evaluation Preferences
- Confidence Thresholds
- Explanation Style
- Future configurable AI options

---

# Relationships

A League Configuration belongs to one League.

A League Configuration governs one or more League Seasons.

Every rule evaluation references the active League Configuration.

Every transaction shall be validated against the active League Configuration.

---

# Lifecycle

Created

↓

Activated

↓

Modified

↓

Superseded

↓

Historical

League Configurations are never deleted.

---

# Commands

- Create Configuration
- Update Configuration
- Activate Configuration
- Archive Configuration

Configuration updates that affect historical behavior shall create a new configuration version rather than modifying historical records.

---

# Emitted Events

- LeagueConfigurationCreated
- LeagueConfigurationUpdated
- LeagueConfigurationActivated
- LeagueConfigurationArchived

---

# Consumed Events

- LeagueCreated
- SeasonAdvanced

---

# Validation Rules

Reject:

- Invalid roster sizes
- Invalid salary cap values
- Invalid draft configuration
- Invalid waiver configuration
- Conflicting rule combinations
- Configuration changes prohibited during protected league phases

---

# Invariants

- Every League has exactly one active configuration.
- Every Transaction validates against the active configuration.
- Historical configurations remain immutable.
- Configuration versions are chronologically ordered.
- Rule interpretation is deterministic given a configuration.

---

# Historical Requirements

Every historical League Season shall remain associated with the configuration that governed it.

Changing today's configuration shall never alter historical transaction legality.

Historical recommendations shall always be reproducible using the historical configuration.

---

# AI Interpretation

League Configuration is the primary contextual input for deterministic reasoning.

Before evaluating any recommendation, the AI shall resolve the active League Configuration and apply all relevant settings during:

- Rule Evaluation
- Salary Cap Analysis
- Trade Validation
- Roster Validation
- Draft Evaluation
- Waiver Evaluation
- Contract Analysis
- Team Evaluation

The AI shall never assume platform defaults when an explicit League Configuration exists.

If configuration information is unavailable or incomplete, the AI shall report insufficient evidence rather than infer league rules.
