# Chapter 2 — Role-Based Access

## Purpose

Role-Based Access Control (RBAC) ensures that administrative capabilities are granted according to responsibility rather than identity.

Every administrative operation should require the minimum permissions necessary to perform that action.

---

## Responsibilities

RBAC is responsible for:

- Authentication
- Authorization
- Permission evaluation
- Role assignment
- Access auditing

Authorization decisions should occur before every administrative operation.

---

## Example Roles

Suggested administrative roles include:

### Viewer

May:

- View dashboards
- Inspect execution history
- Review validation results

Cannot modify system state.

---

### Operator

May:

- View operational information
- Initiate approved recovery workflows
- Restart eligible executions

Cannot modify administrative policy.

---

### Administrator

May:

- Perform all Operator actions
- Approve Recovery Plans
- Execute administrative overrides
- Manage system configuration

---

### System Owner

May:

- Perform all administrative actions
- Manage roles
- Configure platform-wide policies

---

## Authorization Flow

```text
Administrative Request
          │
          ▼
Authenticate User
          │
          ▼
Evaluate Role
          │
 ┌────────┴────────┐
 ▼                 ▼
Authorized     Unauthorized
 │                 │
 ▼                 ▼
Execute         Reject
```

---

## Permission Principles

Permissions should be:

- Explicit
- Least privilege
- Auditable
- Revocable
- Consistent

Permission inheritance should remain simple and predictable.

---

## Audit Requirements

Every authorization decision should record:

- User
- Role
- Requested action
- Authorization result
- Timestamp

Authorization history is part of the permanent administrative audit trail.

---

## Definition of Done

This chapter is complete when every administrative capability is protected by deterministic, role-based authorization with complete auditability.
