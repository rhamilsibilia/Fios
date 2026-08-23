# Trisphere Enterprise Agent — Deployment

## Purpose

The platform should support different customer risk, privacy, budget, and infrastructure profiles without rewriting the product.

---

## Deployment Profiles

### Profile A — Cloud-Connected Desktop
A desktop client with cloud intelligence and cloud integrations.

Best for:
- small businesses;
- quick deployment;
- lower local infrastructure.

### Profile B — Hybrid Desktop
Desktop client plus local knowledge/runtime and approved cloud models.

Best for:
- privacy-conscious SMBs;
- offices with local files and devices.

### Profile C — Trisphere Node
Dedicated local appliance with company knowledge, policy, local model options, and device services.

Best for:
- sensitive environments;
- multi-device offices;
- local-first requirements.

### Profile D — Central Enterprise Runtime
Managed server runtime for larger organizations with multiple users/agents.

---

## Installation Components

A customer deployment may include:

- agent client;
- local runtime;
- organization config;
- capability adapters;
- identity integration;
- knowledge connectors;
- policy pack;
- audit transport;
- health monitor;
- optional local model;
- optional Trisphere Node.

---

## Company Onboarding

Recommended flow:

```text
Organization
   ↓
Identity
   ↓
Users / Roles
   ↓
Approved Data Sources
   ↓
Capabilities
   ↓
Policy
   ↓
Knowledge
   ↓
Model Policy
   ↓
Pilot
   ↓
Production
```

---

## Pilot Gate

Never give a new organization unrestricted execution from day one.

Recommended stages:

1. read-only;
2. draft-only;
3. controlled internal mutations;
4. selected external side effects;
5. expanded capabilities after evidence.

---

## Configuration

Configuration should be declarative where practical.

Examples:

- approved capabilities;
- providers;
- risk thresholds;
- allowed hours;
- approval chains;
- storage mode;
- local-only data classes.

---

## Update Strategy

Updates must preserve:

- customer configuration;
- identity;
- local knowledge;
- secrets;
- audit continuity.

Update packages should be signed where feasible.

---

## Health Checks

Deployment health should cover:

- model providers;
- integrations;
- knowledge index;
- capability adapters;
- local runtime;
- storage;
- policy engine;
- audit transport.

---

## Disaster Recovery

Design for:

- provider outage;
- device failure;
- corrupted cache;
- local-node replacement;
- credential rotation;
- knowledge reindex;
- rollback.

---

## Multi-Organization Hosting

Where cloud infrastructure is shared, tenant boundaries must remain explicit and testable.

---

## Deployment Success

A deployment is not complete merely because the assistant answers.

It must demonstrate:

- correct identity;
- correct organization scope;
- policy enforcement;
- audit;
- safe failure;
- capability health;
- user understanding of approval behavior.
