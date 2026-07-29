# Chapter 11 — Engine Security

## Purpose

The Event Engine performs high-impact modifications to league data.

Only authorized users and approved system processes may initiate, resume, cancel, or recover a season rollover.

Security exists to protect league integrity and prevent unauthorized state changes.

---

## Responsibilities

The Event Engine is responsible for enforcing:

- Authentication
- Authorization
- Execution ownership
- Administrative permissions
- Execution locking
- Audit recording

---

## Authentication

Every execution request must originate from an authenticated identity.

Anonymous execution is never permitted.

---

## Authorization

Before execution begins, the engine validates that the requesting user has permission to:

- Start a rollover
- Resume a rollover
- Cancel a rollover
- Perform recovery
- Roll back a rollover

Unauthorized requests are rejected before planning begins.

---

## Administrative Operations

Certain operations require elevated permissions.

Examples include:

- Force execution
- Skip an event
- Retry failed events
- Roll back execution
- Override validation

These operations should be restricted to commissioners or system administrators.

---

## Execution Ownership

Each execution records:

- Requesting user
- Creation timestamp
- Execution mode
- Engine version

This information becomes part of the permanent audit history.

---

## Execution Locks

Before commit execution begins, the engine acquires a league-scoped execution lock.

Example:

```text
league:{league_id}:season_rollover
```

The lock prevents:

- Concurrent rollovers
- Duplicate execution
- Conflicting writes
- Race conditions

---

## Audit Requirements

Every privileged operation must record:

- User
- Timestamp
- Operation
- Target league
- Execution ID
- Reason (when applicable)

Audit records should never be editable.

---

## Security Principles

The Event Engine shall:

- Require authentication
- Require authorization
- Prevent concurrent execution
- Protect audit history
- Reject unauthorized mutations
- Fail securely

---

## Definition of Done

This chapter is complete when every execution is protected by authentication, authorization, execution locking, and permanent audit history.
