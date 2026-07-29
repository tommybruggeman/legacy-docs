# Chapter 3 — Execution Dashboard

## Purpose

The Execution Dashboard provides administrators with a real-time operational view of every Season Rollover execution.

It serves as the primary command center for monitoring rollover progress, identifying failures, and understanding execution health without directly inspecting logs or database records.

The dashboard is an observational interface.

It does not execute business logic.

---

## Responsibilities

The Execution Dashboard is responsible for displaying:

- Active executions
- Completed executions
- Failed executions
- Execution progress
- Current event
- Current validation stage
- Execution duration
- Overall execution health

The dashboard should provide actionable visibility while minimizing unnecessary operational detail.

---

## Dashboard Overview

```text
                Execution Dashboard

┌─────────────────────────────────────────────┐
│ Active Rollovers                 3          │
│ Completed Today                 18          │
│ Failed Today                     1          │
│ Average Duration             4m 12s         │
└─────────────────────────────────────────────┘

Current Execution

League
Current Event
Validation Status
Recovery Status
Progress
Elapsed Time
```

---

## Execution Summary

Each execution should expose:

- Execution ID
- League ID
- League Name
- Source Season
- Target Season
- Current Status
- Current Event
- Progress Percentage
- Start Time
- Estimated Completion

---

## Status Indicators

Recommended execution statuses:

```text
Queued

Planning

Executing

Validating

Checkpointing

Recovering

Completed

Failed

Cancelled
```

Status values should remain mutually exclusive.

---

## Progress Visualization

Execution progress should display:

```text
Events Completed

42 / 48

███████████████████░░░░
87%
```

Progress should be calculated from completed Event Catalog entries rather than elapsed time.

---

## Execution Timeline

Each execution should expose a timeline.

Example:

```text
12:01 Planning Started

12:02 Initial Snapshot

12:03 Contract Reduction

12:04 Validation Passed

12:05 Contract Expiration

12:06 Checkpoint Created

12:07 Free Agency

12:08 Validation Failed
```

The timeline should be immutable.

---

## Failure Indicators

Failures should immediately surface:

- Failed Event
- Failure Category
- Validation Severity
- Recovery Status
- Recovery Recommendation

Operators should identify failure location within seconds.

---

## Navigation

Administrators should be able to navigate directly to:

- Validation Console
- Snapshot Browser
- Recovery Console
- Audit Viewer
- Diagnostics

The dashboard should serve as the operational entry point.

---

## Refresh Strategy

The dashboard should support:

- Automatic refresh
- Manual refresh
- Historical execution viewing

Live execution updates should not require full page reloads.

---

## Design Principles

The Execution Dashboard shall:

- Be operationally focused
- Be read-first
- Be responsive
- Be deterministic
- Prioritize execution health

---

## Definition of Done

This chapter is complete when administrators can understand the state, health, and progress of every rollover execution from a single operational dashboard.
