---
title: User
document: Domain Model
entity: User
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 04-Franchise.md
---

# User

## Purpose

A User represents an authenticated person interacting with the Legacy platform.

Users manage Franchises through League Membership.

Users do not own league assets directly.

---

# Canonical Identity

Every User shall possess one immutable canonical identifier.

Email addresses, display names, and authentication providers may change.

Canonical identity shall not.

---

# Owned State

A User owns:

- Authentication identity
- Profile information
- User preferences
- Notification settings

Users do not own Franchises, Contracts, or Players.

---

# Relationships

A User may belong to many Leagues.

A User may manage many Franchises.

A User may possess multiple League Memberships.

---

# Lifecycle

Registered

↓

Verified

↓

Active

↓

Disabled

↓

Archived

---

# Commands

- Register User
- Update Profile
- Disable Account
- Reactivate Account

---

# Emitted Events

- UserRegistered
- UserUpdated
- UserDisabled

---

# Consumed Events

- InvitationAccepted

---

# Validation Rules

Reject:

- Duplicate canonical identities.
- Invalid authentication providers.

---

# Invariants

- Every User has one canonical identity.
- Authentication is independent of League Membership.
- Users never directly own league assets.

---

# Historical Requirements

User participation history shall remain attributable even after account deactivation.

---

# AI Interpretation

AI shall identify Users as authenticated actors rather than competitive entities.
