# Trisphere Enterprise Agent — Identity and Permissions

## Purpose

Enterprise agents must distinguish people, organizations, machines, services, and integrations.

Identity is foundational to authority.

---

## Principal Types

### Human User
A person authenticated into the system.

### Agent Identity
An AI agent with its own permissions, limits, and audit identity.

### Service Account
A non-human technical principal used by backend services.

### Integration Identity
Represents an external connected system or connector.

### API Client
Represents a registered application or machine client.

---

## Organization Model

```text
Human User
    │
    ▼
Membership
    │
    ├── Role
    │    └── Permissions
    │
    ▼
Organization
```

Roles should attach to organization membership, not globally to the user.

A person may have different permissions in different organizations.

---

## Agent Identity

Agents should never be invisible technical processes.

Each agent should have:

- stable ID;
- organization;
- display name;
- purpose;
- owner;
- allowed capabilities;
- denied capabilities;
- cost policy;
- data access policy;
- audit identity;
- status;
- version.

---

## Delegation

When a human asks an agent to act:

```text
Human Principal
       │
       ▼
Delegation Context
       │
       ▼
Agent Identity
       │
       ▼
Capability Request
```

The audit record should distinguish:

> Agent X acted on behalf of User Y.

It should never simply say:

> User Y performed the action.

---

## Permission Model

Permissions should be capability/resource oriented.

Examples:

```text
email.read
email.send
calendar.read
calendar.write
crm.client.read
crm.client.update
accounting.invoice.read
accounting.invoice.write
documents.generate
filesystem.read
filesystem.write
device.control
admin.permissions.write
```

---

## Scope

Permissions should support scope.

Examples:

```text
crm.client.read:
  organization: ACME
  region: Tampa
```

or:

```text
filesystem.read:
  allowed_roots:
    - C:\Company\Shared
```

---

## Human vs Machine Roles

Human and machine roles should remain distinguishable in the schema and policy engine.

Examples:

- Human: owner, manager, employee, accountant.
- Machine: assistant, background-worker, integration, automation.

Do not make the system depend on naming conventions alone.

---

## Session Assurance

Sessions may carry:

- authentication time;
- MFA status;
- device;
- assurance level;
- organization;
- active membership;
- delegated agent context.

High-risk capabilities may require higher assurance.

---

## Identity Provider Independence

Trisphere Identity Core should define canonical identity and authorization concepts.

An external identity provider may implement authentication.

```text
Identity Provider
      ↓
Trisphere Identity Adapter
      ↓
Canonical Identity
      ↓
Permissions / Policy
```

---

## Machine Credential Rules

Machine credentials should:

- be scoped;
- be revocable;
- expire where practical;
- be stored securely;
- not share human passwords;
- not silently inherit owner permissions;
- be rotated.

---

## Audit Requirements

Every action should be attributable to:

- organization;
- actor identity;
- principal identity where delegated;
- session;
- device;
- capability;
- policy version.

---

## Design Goal

The assistant should always be able to answer:

> Who am I acting as, for which organization, under whose authority, and with which permissions?
