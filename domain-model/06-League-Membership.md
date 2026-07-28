---
title: League Membership
document: Domain Model
entity: League Membership
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - 01-League.md
  - 04-Franchise.md
  - 05-User.md
---

# League Membership

## Purpose

A League Membership associates a User with a Franchise within a specific League.

League Membership defines authorization.

It does not define ownership.

---

# Canonical Identity

A League Membership is uniquely identified by:

- League Identifier
- Franchise Identifier
- User Identifier

---

# Owned State

A League Membership owns:

- Membership role
- Status
- Join timestamp
- Leave timestamp
- Permissions

---

# Relationships

Belongs to one User.

Belongs to one League.

References one Franchise.

---

# Lifecycle

Invited

↓

Accepted

↓

Active

↓

Inactive

↓

Historical

---

# Commands

- Invite Member
- Accept Invitation
- Change Role
- Remove Member

---

# Emitted Events

- MembershipCreated
- MembershipAccepted
- MembershipRemoved
- MembershipRoleChanged

---

# Consumed Events

- UserRegistered
- FranchiseCreated

---

# Validation Rules

Reject:

- Duplicate active memberships for the same role when prohibited by league configuration.
- Memberships referencing nonexistent Users, Leagues, or Franchises.

---

# Invariants

- Every Membership references exactly one User.
- Every Membership references exactly one Franchise.
- Historical Memberships remain attributable.

---

# Historical Requirements

Membership changes shall preserve historical attribution.

Former managers remain associated with historical actions performed during their period of membership.

---

# AI Interpretation

AI shall determine permissions through League Membership.

Franchise ownership, User identity, and authorization are independent concepts and shall not be treated as interchangeable.
