# Trisphere Enterprise Agent — Governance

## Purpose

Governance determines what the agent may do, under what conditions, and with whose authority.

The model is advisory. Governance is authoritative.

---

## Governance Principles

1. No action authority comes from model confidence.
2. Permissions are explicit.
3. High-risk operations require stronger controls.
4. Agent authority cannot silently exceed the principal's authority.
5. Organization policy may restrict otherwise available capabilities.
6. Financial, legal, security, and destructive actions receive elevated controls.
7. Every policy decision should be explainable.
8. Denial is a valid and expected runtime outcome.

---

## Policy Evaluation Inputs

A policy decision may consider:

- organization;
- human principal;
- agent identity;
- role;
- capability;
- resource;
- requested action;
- data classification;
- risk level;
- amount/value;
- destination;
- time;
- device;
- network;
- provider;
- prior approval;
- session assurance;
- recent authentication;
- business rules;
- cost limits.

---

## Recommended Decision Types

```text
ALLOW
ALLOW_WITH_LOGGING
ALLOW_WITH_STEP_UP
REQUIRE_APPROVAL
REQUIRE_DUAL_APPROVAL
DENY
HARD_BLOCK
```

---

## Example Policy

```yaml
capability: email.send
risk: L3
rules:
  - if recipient_domain in approved_domains:
      decision: ALLOW_WITH_LOGGING
  - if contains_sensitive_attachment:
      decision: REQUIRE_APPROVAL
  - if external_recipient and principal_role == "staff":
      decision: REQUIRE_APPROVAL
```

---

## Agent Delegation

When an agent acts on behalf of a human:

```text
Effective Authority =
Agent Permissions
INTERSECT
Principal Permissions
INTERSECT
Organization Policy
```

Never use a union.

Audit should record:

- `actor = agent`;
- `principal = human`;
- `organization`;
- `delegation_context`.

---

## Risk Classes

### L0 — Read
Examples:
- search internal knowledge;
- read calendar;
- read CRM record.

### L1 — Reversible preparation
Examples:
- draft message;
- create draft document;
- prepare report.

### L2 — Internal mutation
Examples:
- add internal note;
- update low-risk internal status.

### L3 — External side effect
Examples:
- send email;
- create external meeting;
- publish approved content.

### L4 — Sensitive operational action
Examples:
- modify accounting record;
- alter customer contract data;
- change system configuration.

### L5 — Critical
Examples:
- transfer money;
- delete critical data;
- grant permissions;
- execute destructive system command;
- release regulated information.

---

## Approval Objects

Approvals should be explicit objects, not informal chat state.

Recommended fields:

```yaml
approval_id:
organization_id:
request_id:
capability_id:
requested_by:
agent_id:
risk_level:
summary:
parameters_hash:
created_at:
expires_at:
approved_by:
decision:
decision_at:
```

An approval should apply to the exact bounded action that was reviewed.

---

## Step-Up Authentication

Sensitive work may require fresh authentication.

Examples:

- elevated admin action;
- financial transaction;
- disclosure of sensitive records;
- changing security policy.

Step-up should be separate from ordinary session authentication.

---

## Cost Governance

Model and provider cost should be evaluated alongside security.

Policies may include:

- per-task ceiling;
- per-agent ceiling;
- daily organization ceiling;
- premium-model approval;
- local-first requirement;
- provider fallback order.

---

## Policy Packs

Each organization may have a policy pack.

Vertical editions may provide templates, but customer policy remains explicit.

Examples:

- accounting office;
- contractor;
- law office;
- retail operation.

---

## Governance Is Not Prompting

System prompts may communicate policy to the model, but prompts are not enforcement.

Real enforcement must occur in deterministic code before side effects.

---

## Audit Requirement

Every governed action should record:

- requested action;
- policy inputs;
- policy outcome;
- approval requirement;
- approval evidence;
- execution result;
- verification result;
- policy version.

This allows later reconstruction of why the agent acted.
