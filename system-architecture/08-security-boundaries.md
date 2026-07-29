# Chapter 8 — Security Boundaries

## Purpose

Security Boundaries define how Legacy protects identities, leagues, teams, administrative operations, and sensitive platform capabilities.

Security should be enforced at multiple layers.

No single control should be treated as sufficient.

```text
Identity

↓

Authentication

↓

Authorization

↓

Validation

↓

Service Boundary

↓

Data Access Policy

↓

Audit
```

---

## Security Goals

Legacy security should protect:

- User identity
- League isolation
- Team privacy
- Administrative capabilities
- Contract and salary data
- Recovery operations
- Snapshots
- Audit records
- Secrets
- External integrations

---

## Trust Zones

The platform contains multiple trust zones.

```text
Public Client

↓

Authenticated Application

↓

Authorized Service Layer

↓

Protected Data Layer

↓

Restricted Administrative Systems
```

Each transition requires explicit verification.

---

## Authentication Boundary

Authentication determines user identity.

The authentication provider may manage:

- Credentials
- Passwords
- Sessions
- Multi-factor authentication
- Account recovery

Legacy should accept only verified identity tokens from the configured provider.

Authentication alone does not grant league access.

---

## Authorization Boundary

Authorization determines what an authenticated user may do.

Authorization should evaluate:

- User ID
- League membership
- Team assignment
- Role
- Requested operation
- Resource ownership
- Administrative scope

Example:

```text
Authenticated User

↓

League Membership Found?

↓

Assigned Team Matches?

↓

Requested Action Allowed?
```

Every protected request should perform authorization.

---

## League Isolation

Users should only access data for leagues where they hold an active membership.

League-scoped records should include `league_id` wherever ownership requires it.

Queries should be scoped before execution rather than filtered only after retrieval.

---

## Team Isolation

Team-scoped operations should verify:

- Active league membership
- Associated league team
- Required role
- Ownership or delegated access

A visible team in the interface is not sufficient proof of authorization.

---

## Commissioner Boundary

Commissioners may have elevated league permissions.

They should not automatically receive platform-wide administrative access.

Commissioner capabilities should be limited to:

- Assigned leagues
- Approved league operations
- Explicit override policies

Every override should be audited.

---

## Platform Administrator Boundary

Platform administrators may access operational tooling.

High-risk actions should require:

- Strong authentication
- Role verification
- Explicit confirmation
- Reason entry
- Immutable audit record

Examples include:

- Recovery approval
- Manual Snapshot creation
- Forced execution abort
- Membership correction
- Data repair

---

## Service Boundary

Presentation code should not connect directly to unrestricted data stores.

Recommended pattern:

```text
UI Request

↓

Application Service

↓

Authorization

↓

Domain or Query Service

↓

Repository

↓

Database
```

Service boundaries centralize security decisions.

---

## Database Boundary

Database controls should include:

- Row-level security where appropriate
- Foreign-key constraints
- Restricted service roles
- Separate administrative credentials
- Least-privilege access
- Encrypted transport
- Backup protection

Database policies should reinforce application authorization rather than replace it.

---

## Snapshot Security

Snapshots may contain complete league state.

Access should be restricted by:

- League scope
- Administrative role
- Recovery permissions
- Audit requirements

Snapshot contents should never be publicly accessible.

---

## Recovery Security

Recovery operations can replace league state and therefore require elevated protection.

Recovery should enforce:

- Exclusive execution lock
- Approved Recovery Plan
- Authorized actor
- Snapshot integrity verification
- Immutable recovery audit
- Post-restore validation

---

## AI Security Boundary

The AI layer should receive only scoped evidence required for the current question.

It should not receive:

- Unrelated leagues
- Unrelated users
- Authentication secrets
- Service credentials
- Raw administrative records
- Unbounded database access

Evidence assembly should occur before model invocation.

---

## Prompt Injection Protection

External text and user-provided content should be treated as untrusted data.

The AI system should distinguish between:

- Platform instructions
- User question
- Retrieved evidence
- External content

Retrieved content should never be allowed to redefine system policy.

---

## Secret Management

Secrets include:

- Database credentials
- OpenAI API keys
- Authentication secrets
- Notification credentials
- Provider tokens

Secrets should:

- Remain outside source control
- Use environment or managed secret storage
- Be scoped by environment
- Be rotated
- Never appear in logs

---

## Environment Boundaries

Development, testing, staging, and production should remain isolated.

Each environment should have separate:

- Databases
- Secrets
- storage
- Provider credentials
- Logs
- Deployment controls

Production data should not be copied into lower environments without approved sanitization.

---

## Audit Security

Audit records should be:

- Append-only
- Timestamped
- Actor-linked
- Protected from normal edits
- Retained according to policy

Administrators should not be able to erase evidence of their own actions.

---

## External Integration Security

Every provider integration should define:

- Authentication method
- Token storage
- Scope
- Timeout
- Retry limits
- Data exchanged
- Failure behavior
- Revocation process

Provider access should use the minimum required scope.

---

## Input Validation

All external input should be validated for:

- Type
- Format
- Length
- Allowed values
- Ownership references
- Business constraints

Input validation protects system boundaries but does not replace authorization.

---

## Security Event Observability

Security-relevant events should include:

- Failed logins
- Authorization denials
- Administrative actions
- Recovery operations
- Secret failures
- Suspicious access patterns
- RLS policy failures
- Repeated invalid requests

Critical events should generate operational alerts.

---

## Security Guarantees

Security Boundaries guarantee:

1. Authentication and authorization remain separate.
2. League and team access is explicitly scoped.
3. Elevated actions require elevated permissions.
4. Direct unrestricted client data access is avoided.
5. Snapshots and recovery operations receive enhanced protection.
6. AI receives only scoped, approved evidence.
7. Secrets remain outside source control and logs.
8. Environments remain isolated.
9. Administrative actions remain permanently auditable.
10. External integrations operate with least privilege.

---

## Definition of Done

This chapter is complete when Legacy has explicit security boundaries across identity, authorization, leagues, teams, services, persistence, AI, recovery, administration, external providers, and runtime environments.
