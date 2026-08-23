# Trisphere Enterprise Agent — Architecture Decisions

This file captures initial architecture decisions. Formal ADR files may later split these into separate documents.

---

## ADR-EA-001 — Enterprise Agent Is a Platform, Not a Chatbot

**Status:** Accepted

The product is defined around governed work execution, not conversation alone.

---

## ADR-EA-002 — Trisphere Owns the Core Abstractions

**Status:** Accepted

Where strategically useful, Trisphere owns canonical contracts for:

- identity;
- capabilities;
- policy;
- models;
- knowledge;
- audit;
- agent operations.

Providers remain replaceable implementations.

---

## ADR-EA-003 — Open.Jarvis May Be Used as MIT Upstream

**Status:** Accepted with provenance requirement

Selected upstream code may be retained or adapted subject to MIT obligations.

The Trisphere product must not erase upstream attribution.

---

## ADR-EA-004 — No Model-to-Side-Effect Direct Path

**Status:** Accepted

All side effects go through capability validation and governance.

---

## ADR-EA-005 — Human and Agent Identity Are Separate

**Status:** Accepted

Agent actions are audited as agent actions, including delegated human principal where applicable.

---

## ADR-EA-006 — Effective Delegated Authority Uses Intersection

**Status:** Accepted

Effective authority must not exceed:

- agent permission;
- principal permission;
- organization policy.

---

## ADR-EA-007 — Provider Independence

**Status:** Accepted

No single model provider defines the product architecture.

---

## ADR-EA-008 — Local-First Is a Supported Deployment Posture

**Status:** Accepted

The platform must permit local/hybrid operation where feasible.

It does not require every deployment to be fully offline.

---

## ADR-EA-009 — Capability Is More Than a Plugin

**Status:** Accepted

A capability includes:

- contract;
- permissions;
- risk;
- policy;
- cost;
- audit;
- health;
- verification.

---

## ADR-EA-010 — Execution Must Be Verified

**Status:** Accepted

A provider success response is not always enough to declare task completion.

---

## ADR-EA-011 — Company Knowledge Requires Provenance

**Status:** Accepted

Retrieved information must retain source and organization context.

Generated text does not automatically become authoritative knowledge.

---

## ADR-EA-012 — Trisphere Node Is Optional

**Status:** Accepted

The core runtime cannot require a dedicated appliance.

The Node is an optional deployment mode.

---

## ADR-EA-013 — Vertical Editions Extend the Core

**Status:** Accepted

Vertical products should not fork the entire runtime.

---

## ADR-EA-014 — High-Risk Financial and Security Actions Require Hard Gates

**Status:** Accepted

Some actions must require explicit approval or may remain permanently non-executable by the agent.

---

## ADR-EA-015 — Audit Is a Product Requirement

**Status:** Accepted

Audit is not merely developer logging. It is necessary for enterprise trust and accountability.

---

## Open Decisions

The following remain unresolved:

- final customer-facing product name;
- initial implementation language/runtime;
- exact desktop framework;
- first model-provider set;
- secrets backend;
- first vertical;
- Trisphere Node hardware tiers;
- commercial packaging;
- multi-tenant hosting architecture;
- signed update mechanism.

These should not be guessed into permanence without evidence.

---

## ADR-EA-020 — Context Relevance Is Not Authority

**Status:** Accepted · 2026-08-23

A Principal who is not authorized to know that a resource **exists** must not receive even its
abstract. Authorization computes the searchable surface **before** discovery; an unauthorized
resource is never a candidate rather than being filtered from results, and the outcome must be
indistinguishable from one where the resource does not exist.

Otherwise a short abstract becomes an existence oracle for confidential work. Detail:
[[15_PROGRESSIVE-CONTEXT-AND-CACHE-FABRIC]].

---

## ADR-EA-021 — Cache Knowledge and Computation; Never Cache Authority

**Status:** Accepted · 2026-08-23

A cache hit never grants permission. Current authority is evaluated on every read of protected cached
information, against live state. Authority is **not a cacheable quantity** — caching it turns every
revocation into a race with an expiry timer.

Retrieval-result caches must be keyed by authorized-surface identity, never by query alone.

---

## ADR-EA-022 — Capability Suggestion Is Not Capability Grant

**Status:** Accepted · 2026-08-23

A local triage model may suggest a task class, a capability shortlist and structured arguments. Its
output is **untrusted input** and re-enters `validation → Capability → Policy → Authority →
Execution` exactly as user-typed text would.

**A local model earns no additional trust for running on our own hardware.** Detail:
[[16_LOCAL-REFLEX-TRIAGE]].

---

## ADR-EA-023 — External Architecture Is Studied, Not Copied

**Status:** Accepted · 2026-08-23

AGPL-3.0 upstreams are **reference only**: the architecture may be learned from and must be
independently implemented. Every external study pins a commit, classifies the licence, records a
disposition and a transformation classification, and is registered in both the FIOS vault and the
versioned engineering repository before the study is considered complete.

Model weights are licence-checked **separately from code**.
