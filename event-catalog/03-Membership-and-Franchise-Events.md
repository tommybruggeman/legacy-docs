---
title: Membership and Franchise Events
document: Event Catalog
version: 1.0.0-draft
status: Draft
author: Legacy Product Architecture
last_updated: 2026-07-28
depends_on:
  - README.md
  - 01-League-Lifecycle-Events.md
  - ../domain-model/04-Franchise.md
  - ../domain-model/05-User.md
  - ../domain-model/06-League-Membership.md
---

# Membership and Franchise Events

## Purpose

This chapter defines the canonical events governing:

- Franchise creation
- Franchise identity
- Franchise lifecycle
- League invitations
- User membership
- Membership roles
- Franchise ownership
- Co-owner access
- Ownership transfer
- Membership deactivation

These events establish who may access a League and which Franchise each authorized User represents.

Membership and Franchise identity must remain separate.

A User may belong to multiple Leagues.

A Franchise belongs to one League.

A League Membership defines a User's authorized relationship to a League.

Franchise ownership defines the User's operational relationship to a competitive Franchise.

---

# Scope

This chapter defines:

- `FranchiseCreated`
- `FranchiseNameChanged`
- `FranchiseConfigured`
- `FranchiseArchived`
- `FranchiseRestored`
- `LeagueMemberInvited`
- `LeagueInvitationResent`
- `LeagueInvitationRevoked`
- `LeagueInvitationExpired`
- `LeagueInvitationAccepted`
- `LeagueMembershipActivated`
- `LeagueMembershipRoleChanged`
- `LeagueMembershipDeactivated`
- `FranchiseOwnershipAssigned`
- `FranchiseCoOwnerAssigned`
- `FranchiseCoOwnerRemoved`
- `FranchiseOwnershipTransferred`

---

# Identity Model

The canonical relationship is:

```text
User
  │
  ▼
League Membership
  │
  ├── League role
  └── Optional Franchise relationship
          │
          ▼
       Franchise
```

Legacy shall not infer Franchise ownership solely from:

- Email text
- Display name
- Sleeper username
- Owner name
- Team name
- Session state
- An external platform identifier

Ownership requires an authoritative membership or ownership event and current-state relationship.

---

# General Invariants

1. Every Franchise belongs to exactly one League.
2. Every League Membership joins one User to one League.
3. A User may have only one active membership row per League unless multiple-role membership is explicitly modeled.
4. Franchise ownership requires an active League Membership.
5. A Franchise owner must belong to the same League as the Franchise.
6. Accepting an invitation does not create a second User identity when the email belongs to an existing User.
7. Invitation tokens shall never appear in published event payloads.
8. Deactivating membership does not delete historical actions.
9. Ownership transfer preserves Franchise identity.
10. Franchise archival does not delete Contracts, Draft Picks, Transactions, or historical ownership.
11. AI actors cannot invite members, activate memberships, or assign ownership.
12. External provider owner names are aliases, not canonical User identities.

---

# FranchiseCreated

## Purpose

`FranchiseCreated` records the creation of a competitive Franchise within a League.

## Event Name

`FranchiseCreated`

## Category

Franchise Lifecycle

## Owning Aggregate

Franchise

## Trigger

The event is emitted when a valid `CreateFranchise` Command succeeds.

It may occur during League creation or later expansion.

## Preconditions

- The League exists.
- The actor has Franchise administration authority.
- League size rules permit another Franchise.
- A unique canonical `franchise_id` has been assigned.
- The Franchise name satisfies validation.
- External team mappings do not conflict.

## Required Payload

```json
{
  "franchise_id": "uuid",
  "league_id": "uuid",
  "franchise_name": "Tommy's Team",
  "status": "active",
  "created_by_user_id": "uuid"
}
```

## Optional Payload

```json
{
  "external_provider": "sleeper",
  "external_roster_id": "7",
  "external_owner_identifier": "Dburruel",
  "display_order": 1
}
```

## Referenced Entities

- Franchise
- League
- User

## Permitted Actors

- Commissioner
- Authorized League Administrator
- Authorized League Creation Workflow
- Authorized Integration Import

## Related Events

Creation may correlate with:

- `FranchiseOwnershipAssigned`
- `LeagueMemberInvited`
- `TransactionRecorded`
- `AuditEventCreated`

## Consumers

- League standings
- Roster services
- Contract services
- Draft services
- Trade services
- Authorization context
- AI evidence builders

## Ordering Requirements

The League must exist first.

Ownership assignment, when known, follows or occurs atomically with creation.

## Idempotency Requirements

A provider import shall not create duplicate Franchises when the external mapping already resolves to a canonical Franchise.

## Replay Behavior

Replay reconstructs the Franchise projection.

Replay shall not send invitations or create external teams.

## Versioning

Current version:

```text
1
```

## Invariants

- The Franchise belongs to one League.
- Franchise identity is not defined by owner name.
- Franchise identity survives ownership changes.
- External roster identifiers supplement canonical identity.
- Creation does not prove a User owns the Franchise.

## AI Interpretation

The AI may treat the Franchise as a competitive team in the League.

It shall not identify its current owner without ownership or membership evidence.

## Example

```json
{
  "event_type": "FranchiseCreated",
  "event_version": 1,
  "aggregate_type": "Franchise",
  "aggregate_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
  "payload": {
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "franchise_name": "Tommy's Team",
    "status": "active",
    "created_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9",
    "external_provider": "sleeper",
    "external_roster_id": "7"
  }
}
```

---

# FranchiseNameChanged

## Purpose

`FranchiseNameChanged` records a change to the Franchise's display name while preserving its identity.

## Event Name

`FranchiseNameChanged`

## Category

Franchise Lifecycle

## Owning Aggregate

Franchise

## Trigger

A valid `ChangeFranchiseName` Command succeeds.

## Preconditions

- The Franchise exists.
- The Franchise is not archived unless archived edits are explicitly permitted.
- The actor has authority.
- The new name is valid.
- The name differs from the current name.

## Required Payload

```json
{
  "franchise_id": "uuid",
  "league_id": "uuid",
  "previous_name": "Tommy's Team",
  "new_name": "The Cap Kings",
  "changed_by_user_id": "uuid"
}
```

## Permitted Actors

- Franchise Owner
- Authorized Co-Owner
- Commissioner
- Authorized League Administrator

## Consumers

- Standings
- Navigation
- Notifications
- Audit systems
- AI context builders

## Ordering Requirements

The event follows `FranchiseCreated`.

## Idempotency Requirements

No event is emitted when the name does not materially change.

## Replay Behavior

Replay updates the projected current name.

## Versioning

Current version:

```text
1
```

## Invariants

- Franchise identity remains unchanged.
- Previous and new names differ.
- Historical references may preserve the name effective at that time.
- A name change does not alter ownership.

## AI Interpretation

The AI should resolve previous and new names to the same Franchise.

## Example

```json
{
  "event_type": "FranchiseNameChanged",
  "event_version": 1,
  "aggregate_type": "Franchise",
  "aggregate_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
  "payload": {
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "previous_name": "Tommy's Team",
    "new_name": "The Cap Kings",
    "changed_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9"
  }
}
```

---

# FranchiseConfigured

## Purpose

`FranchiseConfigured` records a material change to Franchise-level settings that do not belong to roster, contract, or ownership events.

Examples may include:

- Logo
- Abbreviation
- Display preferences
- External provider mapping
- Franchise-specific metadata

## Event Name

`FranchiseConfigured`

## Category

Franchise Lifecycle

## Owning Aggregate

Franchise

## Trigger

A valid `ConfigureFranchise` Command succeeds.

## Preconditions

- The Franchise exists.
- The actor has authority.
- Changed settings are supported.
- External mappings satisfy uniqueness rules.
- The configuration change does not rewrite competitive history.

## Required Payload

```json
{
  "franchise_id": "uuid",
  "league_id": "uuid",
  "changed_fields": [],
  "previous_values": {},
  "new_values": {},
  "configured_by_user_id": "uuid"
}
```

## Consumers

- Franchise projections
- External integrations
- User interface
- Audit systems
- AI alias resolution

## Idempotency Requirements

No event is emitted for a no-op configuration request.

## Replay Behavior

Replay applies the documented configuration changes.

## Versioning

Current version:

```text
1
```

## Invariants

- Critical ownership changes use ownership events instead.
- Roster changes use roster events.
- Contract changes use contract events.
- Canonical Franchise identity is unchanged.

## AI Interpretation

The AI may use external mapping or abbreviation changes for entity resolution.

It shall not infer ownership from display metadata.

## Example

```json
{
  "event_type": "FranchiseConfigured",
  "event_version": 1,
  "aggregate_type": "Franchise",
  "aggregate_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
  "payload": {
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "changed_fields": [
      "abbreviation"
    ],
    "previous_values": {
      "abbreviation": "TOM"
    },
    "new_values": {
      "abbreviation": "CAP"
    },
    "configured_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9"
  }
}
```

---

# FranchiseArchived

## Purpose

`FranchiseArchived` records that a Franchise was removed from normal active competition while preserving its history.

## Event Name

`FranchiseArchived`

## Category

Franchise Lifecycle

## Owning Aggregate

Franchise

## Trigger

A valid `ArchiveFranchise` Command succeeds.

## Preconditions

- The Franchise exists.
- The Franchise is active.
- The actor has authority.
- Blocking roster, contract, trade, or Draft Pick obligations are resolved according to League rules.
- The archive reason is recorded.

## Required Payload

```json
{
  "franchise_id": "uuid",
  "league_id": "uuid",
  "previous_status": "active",
  "new_status": "archived",
  "archive_reason": "League contraction",
  "archived_by_user_id": "uuid"
}
```

## Consumers

- League structure
- Authorization
- Trade services
- Draft services
- Contract services
- Audit systems
- AI evidence builders

## Ordering Requirements

The event follows `FranchiseCreated`.

Required ownership or membership deactivation events may precede or follow according to the atomic workflow.

## Idempotency Requirements

An archived Franchise shall not be archived twice.

## Replay Behavior

Replay marks the Franchise archived.

It shall not delete roster, contract, or transaction history.

## Versioning

Current version:

```text
1
```

## Invariants

- Archival preserves Franchise identity.
- Historical standings remain valid.
- Archival does not silently transfer assets.
- Asset disposition requires separate events.
- Ownership history remains intact.

## AI Interpretation

The AI may analyze the Franchise historically but shall not treat it as an active trade or waiver participant.

## Example

```json
{
  "event_type": "FranchiseArchived",
  "event_version": 1,
  "aggregate_type": "Franchise",
  "aggregate_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
  "payload": {
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "previous_status": "active",
    "new_status": "archived",
    "archive_reason": "League contraction",
    "archived_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9"
  }
}
```

---

# FranchiseRestored

## Purpose

`FranchiseRestored` records the return of an archived Franchise to active or setup status.

## Event Name

`FranchiseRestored`

## Category

Franchise Lifecycle

## Owning Aggregate

Franchise

## Trigger

A valid `RestoreFranchise` Command succeeds.

## Preconditions

- The Franchise exists.
- The Franchise is archived.
- League size permits restoration.
- The actor has authority.
- Asset and ownership validation passes.
- The target status is valid.

## Required Payload

```json
{
  "franchise_id": "uuid",
  "league_id": "uuid",
  "previous_status": "archived",
  "new_status": "active",
  "restoration_reason": "League expanded back to ten teams",
  "restored_by_user_id": "uuid"
}
```

## Consumers

- League structure
- Authorization
- Roster services
- Contract services
- Draft services
- AI evidence builders

## Idempotency Requirements

A non-archived Franchise cannot be restored again.

## Replay Behavior

Replay restores projected status but does not recreate prior ownership or assets without corresponding events.

## Versioning

Current version:

```text
1
```

## Invariants

- Franchise identity remains unchanged.
- Restoration does not silently return previously transferred assets.
- Ownership must be validated independently.
- Prior history remains intact.

## AI Interpretation

The AI must evaluate the restored Franchise's current roster, Contracts, Draft Picks, ownership, and competitive status rather than assuming it resumed its prior state.

## Example

```json
{
  "event_type": "FranchiseRestored",
  "event_version": 1,
  "aggregate_type": "Franchise",
  "aggregate_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
  "payload": {
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "previous_status": "archived",
    "new_status": "active",
    "restoration_reason": "League expanded back to ten teams",
    "restored_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9"
  }
}
```

---

# LeagueMemberInvited

## Purpose

`LeagueMemberInvited` records that an invitation was issued to an email address for membership in a League and, when applicable, association with a specific Franchise.

## Event Name

`LeagueMemberInvited`

## Category

Membership Invitation

## Owning Aggregate

LeagueMembership

When membership has not yet been created, the invitation workflow may use a dedicated invitation aggregate identifier.

## Trigger

A valid `InviteLeagueMember` Command succeeds.

## Preconditions

- The League exists.
- The actor has invitation authority.
- The email address is valid.
- The requested role is valid.
- The Franchise, when specified, belongs to the League.
- The invitation does not create a prohibited duplicate active membership.
- A secure invitation token is generated and stored outside the event payload.
- An expiration time is established.

## Required Payload

```json
{
  "invitation_id": "uuid",
  "league_id": "uuid",
  "invited_email_normalized": "owner@example.com",
  "requested_role": "owner",
  "invited_by_user_id": "uuid",
  "expires_at": "2026-08-04T18:30:00Z"
}
```

## Optional Payload

```json
{
  "franchise_id": "uuid",
  "delivery_method": "email",
  "existing_user_id": "uuid"
}
```

## Security Requirements

The event shall not contain:

- Invitation token
- Token hash
- Authentication link containing the token
- Session credentials

## Referenced Entities

- League
- Invitation
- Franchise
- Existing User when resolved
- Inviting User

## Permitted Actors

- Commissioner
- Authorized League Administrator
- Authorized Franchise Owner for permitted co-owner invitations

## Consumers

- Invitation email service
- Invitation status projection
- Audit systems
- Commissioner dashboard
- Notification services

## Ordering Requirements

The event precedes acceptance, expiration, revocation, or resend events.

## Idempotency Requirements

Repeated submission with the same idempotency key shall not issue multiple invitations.

Product policy may permit a new invitation after revocation or expiration.

## Replay Behavior

Replay restores invitation history.

Replay shall never resend the email.

## Versioning

Current version:

```text
1
```

## Invariants

- Invitation email is normalized.
- The invitation belongs to one League.
- The requested Franchise belongs to the same League.
- Invitation issuance does not activate membership.
- Invitation issuance does not prove email delivery.
- No secret token appears in the event.

## AI Interpretation

The AI may state that an invitation was issued.

It shall not state that the invited person joined until acceptance and membership activation events exist.

## Example

```json
{
  "event_type": "LeagueMemberInvited",
  "event_version": 1,
  "aggregate_type": "LeagueMembership",
  "aggregate_id": "8c835ec1-24a2-4e78-9281-d54ca995f165",
  "payload": {
    "invitation_id": "8c835ec1-24a2-4e78-9281-d54ca995f165",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "invited_email_normalized": "owner@example.com",
    "requested_role": "owner",
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "invited_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9",
    "expires_at": "2026-08-04T18:30:00Z",
    "delivery_method": "email"
  }
}
```

---

# LeagueInvitationResent

## Purpose

`LeagueInvitationResent` records that a valid pending invitation was sent again.

## Event Name

`LeagueInvitationResent`

## Category

Membership Invitation

## Owning Aggregate

LeagueMembership or Invitation

## Trigger

A valid `ResendLeagueInvitation` Command succeeds.

## Preconditions

- The invitation exists.
- The invitation has not been accepted or revoked.
- Resend limits permit another delivery.
- The actor has authority.
- A valid invitation token remains available or a replacement token is securely generated.

## Required Payload

```json
{
  "invitation_id": "uuid",
  "league_id": "uuid",
  "resent_by_user_id": "uuid",
  "resent_at": "2026-07-30T18:30:00Z",
  "resend_count": 1
}
```

## Optional Payload

```json
{
  "previous_expires_at": "2026-08-04T18:30:00Z",
  "new_expires_at": "2026-08-06T18:30:00Z"
}
```

## Security Requirements

No invitation secret may appear in the payload.

## Consumers

- Invitation status projections
- Audit systems
- Delivery analytics

## Idempotency Requirements

Duplicate retries of one resend request must not send multiple emails.

## Replay Behavior

Replay updates resend history without sending an email.

## Invariants

- The original invitation identity remains unchanged unless product policy explicitly creates a replacement invitation.
- Resend does not activate membership.
- Resend does not prove delivery.

## AI Interpretation

The AI may report that the invitation was sent again and remains pending.

## Example

```json
{
  "event_type": "LeagueInvitationResent",
  "event_version": 1,
  "aggregate_type": "LeagueMembership",
  "aggregate_id": "8c835ec1-24a2-4e78-9281-d54ca995f165",
  "payload": {
    "invitation_id": "8c835ec1-24a2-4e78-9281-d54ca995f165",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "resent_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9",
    "resent_at": "2026-07-30T18:30:00Z",
    "resend_count": 1
  }
}
```

---

# LeagueInvitationRevoked

## Purpose

`LeagueInvitationRevoked` records that a pending invitation was invalidated before acceptance.

## Event Name

`LeagueInvitationRevoked`

## Category

Membership Invitation

## Owning Aggregate

LeagueMembership or Invitation

## Trigger

A valid `RevokeLeagueInvitation` Command succeeds.

## Preconditions

- The invitation exists.
- The invitation is pending.
- The actor has authority.
- The invitation has not already been accepted, revoked, or expired.

## Required Payload

```json
{
  "invitation_id": "uuid",
  "league_id": "uuid",
  "revoked_by_user_id": "uuid",
  "revoked_at": "2026-07-31T18:30:00Z",
  "revocation_reason": "Wrong email address"
}
```

## Consumers

- Invitation validation
- Commissioner dashboard
- Audit systems
- Notification services

## Idempotency Requirements

An already revoked invitation cannot be revoked again.

## Replay Behavior

Replay marks the invitation revoked.

## Invariants

- A revoked invitation cannot be accepted.
- Revocation does not deactivate an already active membership.
- Secrets remain excluded.

## AI Interpretation

The AI may treat the invitation as no longer valid.

## Example

```json
{
  "event_type": "LeagueInvitationRevoked",
  "event_version": 1,
  "aggregate_type": "LeagueMembership",
  "aggregate_id": "8c835ec1-24a2-4e78-9281-d54ca995f165",
  "payload": {
    "invitation_id": "8c835ec1-24a2-4e78-9281-d54ca995f165",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "revoked_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9",
    "revoked_at": "2026-07-31T18:30:00Z",
    "revocation_reason": "Wrong email address"
  }
}
```

---

# LeagueInvitationExpired

## Purpose

`LeagueInvitationExpired` records that a pending invitation reached its expiration time without acceptance.

## Event Name

`LeagueInvitationExpired`

## Category

Membership Invitation

## Owning Aggregate

LeagueMembership or Invitation

## Trigger

The event is emitted by an authorized expiration process when the invitation is no longer valid.

## Preconditions

- The invitation exists.
- It remains pending.
- Current time is at or after `expires_at`.
- It has not been accepted or revoked.

## Required Payload

```json
{
  "invitation_id": "uuid",
  "league_id": "uuid",
  "expired_at": "2026-08-04T18:30:00Z"
}
```

## Permitted Actors

- System

## Consumers

- Invitation validation
- Commissioner dashboard
- Notification services
- Audit systems

## Idempotency Requirements

The invitation expires only once.

## Replay Behavior

Replay restores expired status without sending notifications.

## Invariants

- An expired invitation cannot be accepted.
- Expiration does not create or deactivate membership.
- A new invitation requires a new authorized workflow.

## AI Interpretation

The AI may state that the invitation expired before joining occurred.

## Example

```json
{
  "event_type": "LeagueInvitationExpired",
  "event_version": 1,
  "aggregate_type": "LeagueMembership",
  "aggregate_id": "8c835ec1-24a2-4e78-9281-d54ca995f165",
  "actor": {
    "actor_type": "System",
    "actor_id": "invitation-expiration-service"
  },
  "payload": {
    "invitation_id": "8c835ec1-24a2-4e78-9281-d54ca995f165",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "expired_at": "2026-08-04T18:30:00Z"
  }
}
```

---

# LeagueInvitationAccepted

## Purpose

`LeagueInvitationAccepted` records that an authenticated User successfully accepted a valid League invitation.

Acceptance resolves the invitation to a canonical User.

Membership activation may occur in the same workflow but remains a separate business fact.

## Event Name

`LeagueInvitationAccepted`

## Category

Membership Invitation

## Owning Aggregate

LeagueMembership

## Trigger

A valid `AcceptLeagueInvitation` Command succeeds.

## Preconditions

- The invitation exists.
- The invitation is pending and unexpired.
- The invitation has not been revoked.
- The accepting User is authenticated.
- The accepting identity is permitted to claim the invited email or invitation.
- League and Franchise references remain valid.
- Duplicate membership rules are satisfied.

## Required Payload

```json
{
  "invitation_id": "uuid",
  "league_id": "uuid",
  "accepted_by_user_id": "uuid",
  "accepted_at": "2026-08-01T18:30:00Z",
  "requested_role": "owner"
}
```

## Optional Payload

```json
{
  "franchise_id": "uuid",
  "membership_id": "uuid",
  "email_match_status": "verified"
}
```

## Permitted Actors

- User

## Related Events

Acceptance may correlate with:

- `LeagueMembershipActivated`
- `FranchiseOwnershipAssigned`
- `FranchiseCoOwnerAssigned`
- `AuditEventCreated`

## Consumers

- Membership services
- Authorization
- Onboarding
- Notifications
- Audit systems
- AI user context

## Ordering Requirements

This event follows `LeagueMemberInvited`.

Membership activation follows or occurs atomically.

## Idempotency Requirements

The same invitation cannot be accepted more than once.

Repeated callback or page submission shall resolve to the existing accepted state.

## Replay Behavior

Replay restores accepted invitation status.

Replay shall not create duplicate memberships or send welcome messages.

## Versioning

Current version:

```text
1
```

## Invariants

- Acceptance identifies one canonical User.
- The invitation becomes unusable after acceptance.
- Acceptance does not silently create ownership without an ownership event.
- Existing Users retain their canonical User identity.
- No duplicate League membership is created.

## AI Interpretation

The AI may state that the User accepted the invitation.

It should confirm membership activation and Franchise ownership separately before treating the User as an active owner.

## Example

```json
{
  "event_type": "LeagueInvitationAccepted",
  "event_version": 1,
  "aggregate_type": "LeagueMembership",
  "aggregate_id": "3e39869f-c146-4841-8338-8ec70e850c2c",
  "payload": {
    "invitation_id": "8c835ec1-24a2-4e78-9281-d54ca995f165",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "accepted_by_user_id": "342af078-f96d-4311-aa8c-f47444173257",
    "accepted_at": "2026-08-01T18:30:00Z",
    "requested_role": "owner",
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "membership_id": "3e39869f-c146-4841-8338-8ec70e850c2c",
    "email_match_status": "verified"
  }
}
```

---

# LeagueMembershipActivated

## Purpose

`LeagueMembershipActivated` records that a User obtained an active authorized relationship with a League.

## Event Name

`LeagueMembershipActivated`

## Category

League Membership

## Owning Aggregate

LeagueMembership

## Trigger

A valid `ActivateLeagueMembership` Command succeeds, often following invitation acceptance.

## Preconditions

- The League exists.
- The User exists.
- No prohibited duplicate active membership exists.
- The requested role is valid.
- Required invitation or commissioner authorization exists.
- Any linked Franchise belongs to the League.

## Required Payload

```json
{
  "membership_id": "uuid",
  "league_id": "uuid",
  "user_id": "uuid",
  "role": "owner",
  "status": "active",
  "activated_at": "2026-08-01T18:30:01Z"
}
```

## Optional Payload

```json
{
  "invitation_id": "uuid",
  "franchise_id": "uuid",
  "activated_by_user_id": "uuid"
}
```

## Permitted Actors

- User through invitation acceptance
- Commissioner
- Authorized League Administrator
- Authorized System Workflow

## Consumers

- Authorization
- League navigation
- Franchise context
- GM Assistant identity resolution
- Audit systems
- Notifications

## Ordering Requirements

When invitation-based, `LeagueInvitationAccepted` precedes or shares the atomic workflow.

## Idempotency Requirements

An existing active membership shall not be duplicated.

## Replay Behavior

Replay restores active membership projections.

## Versioning

Current version:

```text
1
```

## Invariants

- The membership joins one User to one League.
- The role is supported.
- The optional Franchise belongs to the same League.
- Membership activation does not itself prove primary ownership unless ownership is encoded by policy and supported by a separate ownership event.
- Authorization derives from canonical identity, not display names.

## AI Interpretation

This event is authoritative evidence that the User may access the League under the documented role.

For owner-specific recommendations, the AI should also resolve the Franchise relationship.

## Example

```json
{
  "event_type": "LeagueMembershipActivated",
  "event_version": 1,
  "aggregate_type": "LeagueMembership",
  "aggregate_id": "3e39869f-c146-4841-8338-8ec70e850c2c",
  "payload": {
    "membership_id": "3e39869f-c146-4841-8338-8ec70e850c2c",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "user_id": "342af078-f96d-4311-aa8c-f47444173257",
    "role": "owner",
    "status": "active",
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "activated_at": "2026-08-01T18:30:01Z"
  }
}
```

---

# LeagueMembershipRoleChanged

## Purpose

`LeagueMembershipRoleChanged` records a change to a User's League-level authorization role.

## Event Name

`LeagueMembershipRoleChanged`

## Category

League Membership

## Owning Aggregate

LeagueMembership

## Trigger

A valid `ChangeLeagueMembershipRole` Command succeeds.

## Preconditions

- The membership exists and is active.
- The actor has authority.
- The new role is supported.
- Governance invariants remain satisfied.
- The change does not leave the League without required administration.

## Required Payload

```json
{
  "membership_id": "uuid",
  "league_id": "uuid",
  "user_id": "uuid",
  "previous_role": "owner",
  "new_role": "commissioner",
  "changed_by_user_id": "uuid",
  "changed_at": "2026-08-02T18:30:00Z"
}
```

## Consumers

- Authorization
- Commissioner tools
- Audit systems
- Notifications
- AI user context

## Idempotency Requirements

Changing to the existing role produces no event.

## Replay Behavior

Replay updates the membership role.

## Versioning

Current version:

```text
1
```

## Invariants

- Previous and new roles differ.
- Role change does not automatically transfer Franchise ownership.
- Required League governance remains valid.
- Historical actions retain the role context recorded at their event time.

## AI Interpretation

The AI may use the current role for authorization-sensitive behavior but must not infer ownership solely from commissioner status.

## Example

```json
{
  "event_type": "LeagueMembershipRoleChanged",
  "event_version": 1,
  "aggregate_type": "LeagueMembership",
  "aggregate_id": "3e39869f-c146-4841-8338-8ec70e850c2c",
  "payload": {
    "membership_id": "3e39869f-c146-4841-8338-8ec70e850c2c",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "user_id": "342af078-f96d-4311-aa8c-f47444173257",
    "previous_role": "owner",
    "new_role": "commissioner",
    "changed_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9",
    "changed_at": "2026-08-02T18:30:00Z"
  }
}
```

---

# LeagueMembershipDeactivated

## Purpose

`LeagueMembershipDeactivated` records that a User's active access to a League ended.

## Event Name

`LeagueMembershipDeactivated`

## Category

League Membership

## Owning Aggregate

LeagueMembership

## Trigger

A valid `DeactivateLeagueMembership` Command succeeds.

## Preconditions

- The membership exists and is active.
- The actor has authority or the User is permitted to leave.
- Required ownership succession is resolved.
- Governance requirements remain satisfied.
- The deactivation reason is recorded.

## Required Payload

```json
{
  "membership_id": "uuid",
  "league_id": "uuid",
  "user_id": "uuid",
  "previous_status": "active",
  "new_status": "inactive",
  "deactivation_reason": "Owner left the league",
  "deactivated_at": "2026-08-15T18:30:00Z"
}
```

## Optional Payload

```json
{
  "deactivated_by_user_id": "uuid",
  "former_franchise_id": "uuid"
}
```

## Consumers

- Authorization
- League navigation
- Franchise ownership validation
- Notifications
- Audit systems
- AI user context

## Ordering Requirements

Primary ownership must be transferred or removed according to League rules before membership deactivation completes.

## Idempotency Requirements

An inactive membership cannot be deactivated again.

## Replay Behavior

Replay removes active access in projections.

Replay shall not resend departure notifications.

## Versioning

Current version:

```text
1
```

## Invariants

- Historical membership remains recorded.
- Historical actions retain actor attribution.
- Deactivation does not delete the User.
- Deactivation does not silently transfer Franchise ownership.
- Active ownership cannot remain attached to an inactive membership unless explicitly supported.

## AI Interpretation

The AI shall not use an inactive membership as current authorization or current Franchise identity.

Historical actions remain attributable to the User.

## Example

```json
{
  "event_type": "LeagueMembershipDeactivated",
  "event_version": 1,
  "aggregate_type": "LeagueMembership",
  "aggregate_id": "3e39869f-c146-4841-8338-8ec70e850c2c",
  "payload": {
    "membership_id": "3e39869f-c146-4841-8338-8ec70e850c2c",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "user_id": "342af078-f96d-4311-aa8c-f47444173257",
    "previous_status": "active",
    "new_status": "inactive",
    "deactivation_reason": "Owner left the league",
    "deactivated_at": "2026-08-15T18:30:00Z",
    "former_franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177"
  }
}
```

---

# FranchiseOwnershipAssigned

## Purpose

`FranchiseOwnershipAssigned` records that an active League Member became the primary owner of a Franchise.

## Event Name

`FranchiseOwnershipAssigned`

## Category

Franchise Ownership

## Owning Aggregate

Franchise

## Trigger

A valid `AssignFranchiseOwnership` Command succeeds.

## Preconditions

- The Franchise exists and is active.
- The User exists.
- The User has active membership in the same League.
- The membership role permits ownership.
- The Franchise does not have a conflicting primary owner.
- The actor has assignment authority.

## Required Payload

```json
{
  "franchise_id": "uuid",
  "league_id": "uuid",
  "membership_id": "uuid",
  "user_id": "uuid",
  "ownership_role": "primary_owner",
  "assigned_at": "2026-08-01T18:30:02Z",
  "assigned_by_user_id": "uuid"
}
```

## Consumers

- Authorization
- Team portal
- GM Assistant identity resolution
- Trade authorization
- Draft authorization
- Notification services
- Audit systems

## Ordering Requirements

`LeagueMembershipActivated` must precede or occur atomically with ownership assignment.

## Idempotency Requirements

Assigning the same active owner to the same Franchise produces no duplicate event.

## Replay Behavior

Replay restores the ownership projection.

## Versioning

Current version:

```text
1
```

## Invariants

- The User, membership, Franchise, and League relationships are consistent.
- Primary ownership is unique unless shared-primary ownership is explicitly supported.
- Ownership is not inferred from `owner_email`.
- Ownership is not inferred from an external username.
- Ownership assignment does not change Franchise identity.

## AI Interpretation

This is authoritative evidence linking a User to the Franchise for owner-specific analysis.

The AI should use canonical identifiers rather than owner display names.

## Example

```json
{
  "event_type": "FranchiseOwnershipAssigned",
  "event_version": 1,
  "aggregate_type": "Franchise",
  "aggregate_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
  "payload": {
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "membership_id": "3e39869f-c146-4841-8338-8ec70e850c2c",
    "user_id": "342af078-f96d-4311-aa8c-f47444173257",
    "ownership_role": "primary_owner",
    "assigned_at": "2026-08-01T18:30:02Z",
    "assigned_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9"
  }
}
```

---

# FranchiseCoOwnerAssigned

## Purpose

`FranchiseCoOwnerAssigned` records that an active League Member received authorized co-owner access to a Franchise.

## Event Name

`FranchiseCoOwnerAssigned`

## Category

Franchise Ownership

## Owning Aggregate

Franchise

## Trigger

A valid `AssignFranchiseCoOwner` Command succeeds.

## Preconditions

- The Franchise exists.
- A valid primary owner exists when required.
- The co-owner User has active League membership.
- The role permits co-ownership.
- Co-owner limits are not exceeded.
- The actor has authority.

## Required Payload

```json
{
  "franchise_id": "uuid",
  "league_id": "uuid",
  "membership_id": "uuid",
  "user_id": "uuid",
  "ownership_role": "co_owner",
  "assigned_at": "2026-08-03T18:30:00Z",
  "assigned_by_user_id": "uuid"
}
```

## Consumers

- Authorization
- Team portal
- Trade and Draft permissions
- Notifications
- Audit systems
- AI user context

## Idempotency Requirements

The same co-owner relationship cannot be created twice.

## Replay Behavior

Replay restores co-owner authorization.

## Versioning

Current version:

```text
1
```

## Invariants

- Co-owner membership belongs to the same League.
- Co-ownership does not replace primary ownership.
- Co-owner capabilities follow League Configuration.
- Display email fields do not establish co-ownership.

## AI Interpretation

The AI may provide Franchise context to an authorized co-owner according to the user's permissions.

It should distinguish primary owner and co-owner roles when responsibility matters.

## Example

```json
{
  "event_type": "FranchiseCoOwnerAssigned",
  "event_version": 1,
  "aggregate_type": "Franchise",
  "aggregate_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
  "payload": {
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "membership_id": "af4c2d06-c4b9-4409-b17a-94b7492d6b28",
    "user_id": "fa710ac2-bb25-4dc2-a74d-c68b420bfd8e",
    "ownership_role": "co_owner",
    "assigned_at": "2026-08-03T18:30:00Z",
    "assigned_by_user_id": "342af078-f96d-4311-aa8c-f47444173257"
  }
}
```

---

# FranchiseCoOwnerRemoved

## Purpose

`FranchiseCoOwnerRemoved` records that a User's co-owner relationship with a Franchise ended.

## Event Name

`FranchiseCoOwnerRemoved`

## Category

Franchise Ownership

## Owning Aggregate

Franchise

## Trigger

A valid `RemoveFranchiseCoOwner` Command succeeds.

## Preconditions

- The co-owner relationship exists.
- The actor has authority.
- Removal does not violate required governance rules.

## Required Payload

```json
{
  "franchise_id": "uuid",
  "league_id": "uuid",
  "membership_id": "uuid",
  "user_id": "uuid",
  "removed_at": "2026-08-20T18:30:00Z",
  "removed_by_user_id": "uuid",
  "removal_reason": "Co-owner access no longer required"
}
```

## Consumers

- Authorization
- Team portal
- Audit systems
- Notifications
- AI user context

## Idempotency Requirements

A removed relationship cannot be removed again.

## Replay Behavior

Replay removes co-owner access from current projections.

## Versioning

Current version:

```text
1
```

## Invariants

- Historical co-owner actions remain attributable.
- Removal does not deactivate League membership unless separately requested.
- Removal does not affect primary ownership.
- Removal does not alter Franchise assets.

## AI Interpretation

The AI shall not treat the User as a current co-owner after removal.

## Example

```json
{
  "event_type": "FranchiseCoOwnerRemoved",
  "event_version": 1,
  "aggregate_type": "Franchise",
  "aggregate_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
  "payload": {
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "membership_id": "af4c2d06-c4b9-4409-b17a-94b7492d6b28",
    "user_id": "fa710ac2-bb25-4dc2-a74d-c68b420bfd8e",
    "removed_at": "2026-08-20T18:30:00Z",
    "removed_by_user_id": "342af078-f96d-4311-aa8c-f47444173257",
    "removal_reason": "Co-owner access no longer required"
  }
}
```

---

# FranchiseOwnershipTransferred

## Purpose

`FranchiseOwnershipTransferred` records that primary ownership of a Franchise moved from one active League Member to another.

The Franchise retains its identity, roster, Contracts, Draft Picks, transaction history, and competitive history.

## Event Name

`FranchiseOwnershipTransferred`

## Category

Franchise Ownership

## Owning Aggregate

Franchise

## Trigger

A valid `TransferFranchiseOwnership` Command succeeds.

## Preconditions

- The Franchise exists and is active.
- A current primary owner exists.
- The incoming owner exists.
- The incoming owner has valid League membership.
- The incoming membership permits ownership.
- The actor has transfer authority.
- Required consent or commissioner approval is satisfied.
- The outgoing and incoming Users differ.

## Required Payload

```json
{
  "franchise_id": "uuid",
  "league_id": "uuid",
  "previous_owner_user_id": "uuid",
  "previous_owner_membership_id": "uuid",
  "new_owner_user_id": "uuid",
  "new_owner_membership_id": "uuid",
  "transferred_at": "2026-08-25T18:30:00Z",
  "transferred_by_user_id": "uuid",
  "transfer_reason": "Franchise sold to a replacement owner"
}
```

## Optional Payload

```json
{
  "outgoing_membership_action": "deactivate",
  "incoming_role": "owner"
}
```

## Related Events

The workflow may also emit:

- `LeagueMembershipActivated`
- `LeagueMembershipDeactivated`
- `LeagueMembershipRoleChanged`
- `FranchiseCoOwnerRemoved`
- `TransactionRecorded`
- `AuditEventCreated`

## Consumers

- Authorization
- Team portal
- Trade and Draft permissions
- Notifications
- Audit systems
- AI identity resolution
- Historical ownership reporting

## Ordering Requirements

The incoming membership must be active before or atomically with transfer.

The old owner relationship ends as the new relationship begins.

The system should not leave the Franchise with ambiguous primary ownership.

## Idempotency Requirements

The same transfer cannot execute twice.

## Replay Behavior

Replay reconstructs ownership history and current ownership projection.

Replay shall not resend transfer notifications.

## Versioning

Current version:

```text
1
```

## Invariants

- The Franchise identity remains unchanged.
- Franchise assets remain attached to the Franchise unless separately transferred.
- Previous and new owners differ.
- Both memberships belong to the same League.
- Historical transactions remain attributed to the actor who performed them.
- The transfer does not rewrite past ownership.
- Owner display names are not canonical transfer identifiers.

## AI Interpretation

The AI may use this event to determine ownership at a specific historical time.

For current owner analysis, it should select the latest valid ownership state after considering later transfers, archival, or membership deactivation.

## Example

```json
{
  "event_type": "FranchiseOwnershipTransferred",
  "event_version": 1,
  "aggregate_type": "Franchise",
  "aggregate_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
  "payload": {
    "franchise_id": "8ae83448-ad66-45f4-a612-d5c19893f177",
    "league_id": "15b77cca-c259-4543-877f-523d57946a20",
    "previous_owner_user_id": "342af078-f96d-4311-aa8c-f47444173257",
    "previous_owner_membership_id": "3e39869f-c146-4841-8338-8ec70e850c2c",
    "new_owner_user_id": "fa710ac2-bb25-4dc2-a74d-c68b420bfd8e",
    "new_owner_membership_id": "af4c2d06-c4b9-4409-b17a-94b7492d6b28",
    "transferred_at": "2026-08-25T18:30:00Z",
    "transferred_by_user_id": "ea36a990-c791-48a9-afb2-4c61ebad38f9",
    "transfer_reason": "Franchise sold to a replacement owner"
  }
}
```

---

# Canonical Invitation Workflow

```text
InviteLeagueMember
        │
        ▼
LeagueMemberInvited
        │
        ├── LeagueInvitationResent
        ├── LeagueInvitationRevoked
        ├── LeagueInvitationExpired
        │
        └── AcceptLeagueInvitation
                 │
                 ▼
        LeagueInvitationAccepted
                 │
                 ▼
        LeagueMembershipActivated
                 │
                 ├── FranchiseOwnershipAssigned
                 └── FranchiseCoOwnerAssigned
```

---

# Canonical Ownership Transfer Workflow

```text
TransferFranchiseOwnership
          │
          ▼
Validate Incoming Membership
          │
          ├── Activate Membership if Required
          │
          ▼
FranchiseOwnershipTransferred
          │
          ├── Update Authorization Projection
          ├── Deactivate Outgoing Membership if Requested
          ├── Send Notifications
          └── Record Audit History
```

---

# Identity Resolution Requirements

When resolving the current User's Franchise, Legacy should use:

1. Authenticated `user_id`
2. Active `league_id`
3. Active League Membership
4. Canonical `franchise_id`
5. Current ownership or co-ownership relationship

Legacy shall not use this fallback as authoritative identity:

```text
owner_name
owner_email
team_name
external_username
```

Those values may support alias resolution but must resolve to canonical identities before access or reasoning occurs.

---

# Validation Checklist

Before publishing Membership or Franchise events, Legacy shall verify:

- The League exists.
- Canonical User identity is resolved.
- Canonical Franchise identity is resolved when required.
- Membership belongs to the correct League.
- Ownership roles are valid.
- The actor is authorized.
- Invitations contain no secret tokens.
- Duplicate memberships are prevented.
- Primary ownership remains unambiguous.
- Historical ownership is preserved.
- External aliases do not replace canonical IDs.
- Authorization projections can be rebuilt from the event history.

---

# Final Principle

A Franchise is not an owner name.

A League Member is not an email address.

A User is not an external username.

Legacy preserves each identity separately so that access, ownership, history, and AI reasoning remain correct even when:

- Owners change
- Emails change
- Franchise names change
- External platforms change
- Invitations are resent
- Memberships are deactivated
- Franchises are transferred

Identity must remain canonical before intelligence can remain trustworthy.
