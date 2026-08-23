# Trisphere Enterprise Agent — Roadmap

## Roadmap Philosophy

Do not attempt full autonomy first.

Progress from safe visibility to bounded execution.

---

## Phase 0 — Architecture and Intake

Deliver:

- Open.Jarvis source-level intake;
- dependency/license inventory;
- architecture map;
- retain/replace/generalize matrix;
- Trisphere capability contract draft;
- governance model;
- security threat model;
- product naming exploration.

Gate:
**GO / CONDITIONAL GO / NO-GO**

---

## Phase 1 — Local Assistant Chassis

Goal:
Prove a rebranded Trisphere desktop runtime.

Capabilities:

- local UI;
- voice/text input;
- runtime states;
- one local model/provider;
- one cloud provider adapter;
- read-only desktop capability;
- structured events;
- local configuration.

No sensitive side effects.

---

## Phase 2 — Identity + Organization

Add:

- organization;
- human users;
- agent identity;
- membership;
- roles;
- permissions;
- session context;
- audit attribution.

Gate:
No capability runs without explicit organization identity.

---

## Phase 3 — Capability Runtime

Add:

- registry;
- manifests;
- permission declarations;
- risk levels;
- execution adapters;
- verification;
- health;
- cost metadata.

Pilot capabilities:

- calendar read;
- email search;
- document search;
- CRM read.

---

## Phase 4 — Governance Engine

Add:

- deterministic policy;
- approvals;
- step-up;
- policy packs;
- bounded delegation;
- L0-L5 risk model.

Introduce selected mutating capabilities.

---

## Phase 5 — Company Knowledge

Add:

- approved knowledge ingestion;
- provenance;
- authority ranking;
- retrieval;
- source citations;
- classification;
- write governance.

---

## Phase 6 — Agent Operations

Integrate:

- health;
- execution events;
- provider cost;
- failures;
- latency;
- capability health;
- reconciliation.

---

## Phase 7 — First Company Pilot

Pilot with one controlled business environment.

Start read-only.

Measure:

- usefulness;
- accuracy;
- policy friction;
- false approvals;
- missed approvals;
- latency;
- model cost;
- capability reliability.

---

## Phase 8 — Trisphere Node Prototype

Only if local-first demand justifies it.

Prove:

- local knowledge;
- local model;
- secure local credentials;
- LAN integrations;
- device runtime;
- offline mode.

---

## Phase 9 — Vertical Edition

Choose one vertical based on real customer demand.

Do not build multiple editions simultaneously.

---

## Phase 10 — Commercialization

Define:

- packaging;
- onboarding;
- support;
- pricing;
- licensing;
- update process;
- security review;
- deployment SLA;
- documentation.

---

## Hard Stops

Stop or redesign if:

- authority cannot be reliably enforced;
- tenant isolation is weak;
- audit cannot reconstruct actions;
- critical actions can bypass approval;
- capability execution is non-deterministically routed;
- provider cost is unbounded;
- local device access becomes unrestricted;
- inherited licensing cannot be tracked.

---

## Success Metrics

Potential metrics:

- task completion rate;
- approval rate;
- denied-action correctness;
- execution verification rate;
- mean cost/task;
- mean latency;
- user intervention rate;
- capability failure rate;
- unsafe-action escapes;
- audit completeness;
- customer time saved.
