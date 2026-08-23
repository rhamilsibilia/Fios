# Trisphere Enterprise Agent — Capability Runtime

## Purpose

The Capability Runtime is the controlled boundary between agent reasoning and real-world actions.

The model does not receive raw unrestricted access to systems. It receives approved capabilities.

---

## Capability Contract

Each capability should define:

```yaml
id:
version:
description:
provider:
input_schema:
output_schema:
permissions:
risk_level:
approval_policy:
audit_required:
cost_model:
timeout:
idempotency:
verification:
health_check:
fallback:
```

---

## Example

```yaml
id: gmail.send
version: 1
description: Send an email through an authorized Gmail account.
permissions:
  - email.send
risk_level: L3
approval_policy: organization_defined
audit_required: true
timeout: 30s
verification: provider_message_id
```

---

## Capability Lifecycle

```text
DISCOVER
   ↓
VALIDATE INPUT
   ↓
AUTHORIZE
   ↓
POLICY CHECK
   ↓
APPROVAL (if required)
   ↓
EXECUTE
   ↓
VERIFY
   ↓
RECORD EVIDENCE
   ↓
AUDIT
```

---

## Capability Registry

The registry should make it possible to answer:

- what capabilities exist;
- which are available;
- which provider implements them;
- what permissions they require;
- what risk level they carry;
- whether they are healthy;
- what they cost.

---

## Capability Families

### Communication
- email.read
- email.search
- email.draft
- email.send
- sms.send
- phone.call

### Calendar
- calendar.read
- calendar.create
- calendar.modify
- calendar.cancel

### CRM
- crm.customer.read
- crm.customer.update
- crm.note.create
- crm.opportunity.update

### Documents
- document.search
- document.generate
- document.export
- document.sign.request

### Accounting
- accounting.report.read
- accounting.invoice.read
- accounting.invoice.propose
- accounting.transaction.propose

### Device
- device.screenshot
- device.clipboard.read
- device.keyboard.type
- device.application.launch

### Knowledge
- knowledge.search
- knowledge.read
- knowledge.write_proposal

---

## Provider Adapters

A capability may have multiple providers.

```text
calendar.create
      │
      ├── Google Calendar Adapter
      └── Microsoft Calendar Adapter
```

The agent should not need provider-specific planning logic for ordinary operations.

---

## Permission Separation

A provider being connected does not mean every capability is authorized.

Example:

```text
Gmail connected
  ├── email.read       ALLOWED
  ├── email.draft      ALLOWED
  └── email.send       APPROVAL REQUIRED
```

---

## Idempotency

Side-effecting capabilities should support idempotency wherever possible.

This is particularly important for:

- payments;
- email sending;
- document creation;
- CRM mutations;
- calendar creation.

---

## Verification

Capabilities should define how success can be independently confirmed.

Examples:

- retrieve created record;
- verify message ID;
- check file exists;
- compare expected state;
- query provider status.

---

## Health

Capability health should be queryable:

```text
AVAILABLE
DEGRADED
UNAVAILABLE
UNKNOWN
```

Agent planning should avoid unavailable capabilities where alternatives exist.

---

## Cost

Capabilities may expose estimated and actual costs.

Examples:

- model invocation;
- paid API;
- telephony;
- document generation;
- video/media generation.

Cost belongs in planning and governance.

---

## Plugin Evolution

Open.Jarvis-style plugins may seed this concept, but Trisphere capabilities should become richer than plugins.

A capability is not merely code loading.

It is:

> contract + permission + policy + cost + audit + health + verification.
