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

---

## ADR-EA-024 — Browser Identity: Provider-Neutral Core, Supabase Auth for Customer Zero

**Status:** Accepted · 2026-08-23 · Register: **D27**

Lena's operator dashboard had no way for a human to reach it. This decides how a browser proves a
Principal — and nothing else.

```text
AUTHENTICATION ≠ AUTHORIZATION
Identity Provider Role ≠ Lena Effective Authority
Valid Login ≠ Dashboard Access
```

**Discovery changed the answer.** Lena already has durable sessions — open, check, refresh, revoke,
idle and absolute expiry, device-bound per D3. The gap was never "sessions"; it was how a browser
proves a Principal. And Supabase Auth is **already live** on the same Staging project Lena already
uses as its durable store.

**Decision.** Core stays provider-neutral behind an `IdentityProvider` port. Customer Zero uses
Supabase Auth: no new vendor, no new invoice, no new data-processor relationship, no additional cost,
and the user table stays in the client's own Postgres.

**Alternatives.** Logto remains ASSESS — a completeness checklist, not a dependency, per the
standing Identity Blueprint decision. Auth0/Clerk/WorkOS rejected for Customer Zero as new vendors
solving a problem owned infrastructure already solves; they stay cheap adapter choices later.
Proprietary password authentication: refused.

**The boundary is structural, not documentary.** `VerifiedIdentity` has no field for a role,
permission, scope, group, entitlement, clearance, office or capability — a provider asserting
`roles: ["admin"]` finds nowhere to put it. Same discipline as `OverrideGrant` in ADR-EA-022.

**An unmapped subject is refused, never auto-provisioned.** Creating a Principal on first login would
let the identity provider decide who exists inside Lena, which is the whole boundary.

**Customer Zero ≠ Core.** The same Core must serve a client on Entra ID, Okta or Google Workspace
with a second adapter and no source fork.

**Open risk:** passkeys/WebAuthn are not first-class in the chosen provider today. The `aal3` tier
and the factor kinds exist, so the model is ready — a deployment needing phishing-resistant step-up
now would need a different adapter.

Engineering contract: `Trisphere-Enterprise-Agent/docs/contracts/BROWSER-IDENTITY.md`.

### ADR-EA-024 — addendum: proven live, 2026-08-23

The decision is no longer only architectural. The Supabase adapter is implemented and the whole
chain was driven **in a real browser against Familia Business Formation — Staging**: real login,
real verified identity, real Lena session, real dashboard with real evidence.

**The finding that justifies the design.** A **deleted user's still-valid signed JWT stops
authenticating immediately**, because the adapter asks Supabase rather than decoding the token
locally. A locally-decoded JWT would have sailed straight through — "valid signature" and "still
allowed to be here" are different questions, and only one of them matters.

**`Valid Login ≠ Dashboard Access` observed, not asserted.** An authenticated operator holding no
Lena permissions logged in successfully and every protected panel refused.

**Economics, measured rather than inferred.** No additional vendor; **$0 incremental today** — no new
SKU, contract or line item, and Customer Zero's handful of operators sits far inside the bundled
plan tier. **It is not $0 forever**: per-MAU pricing across many clients' staff is a real future
question, deferred to the pricing work rather than waved away.

Full live evidence: `Trisphere-Enterprise-Agent/docs/contracts/BROWSER-IDENTITY.md`.

---

## ADR-EA-025 — Wake Detection Is a Discriminative Local Model, and Its Weights Must Be Commercially Clean

**Status:** Accepted · 2026-08-23 · closes deferred decision **D5**

**Decision.** Lena's wake word is a **discriminative local model** — Apache-2.0 runtime code plus a
**Trisphere-trained `Hey Lena` classifier head**. Detection runs entirely on-device with no network
path. **No CC-BY-NC-SA weight ever ships.**

**Why not the elegant option.** Grammar-constrained ASR would have served as wake word *and*
transcriber under a single Apache-2.0 licence with no training step. It was measured and rejected:
**30% false positives**, firing on `Hey Elena` and `Hey Lisa` — and decisively, **true and false
wakes both scored 1.000**. A constrained decoder is forced choice, so a near-miss becomes a confident
match. **No threshold separates them.** A confidence score that cannot discriminate is not a safety
control; shipping one would be worse than shipping none, because it would look like a control.

The discriminative model scored **0/20 false positives with +0.9946 separation** on the identical
negatives, for **1.9% of a 4-core N95 and 196 MB**, with zero egress while idle.

**Why not Porcupine.** Rejected on economics, not quality: $6,000–$30,000/yr plus a runtime AccessKey
— the vendor dependence the pilot exists to avoid — to beat a $0 option that measured zero false
positives. **Retained as a pre-qualified fallback adapter**, which is what the provider seam is for.

**The consequential part is the licence.** openWakeWord is a **mixed repository**: Apache-2.0 code,
**CC-BY-NC-SA-4.0 pre-trained weights**, and a feature extractor described as Google's under
Apache-2.0 by a README that also declares *all* included pre-trained models non-commercial. Both
statements cover the same files. **The ambiguity was escalated, not resolved by adopting the
convenient reading** — the moment the useful interpretation wins, the licence firewall has stopped
working.

**Therefore this ADR selects an engine and deliberately does not produce a shippable artifact.** Two
conditions gate one: a written licence determination on the feature extractor, and training the head
on data that is not CC-BY-NC-SA — a deliberate deviation, since the upstream training path defaults
to exactly the contaminated feature set. **Until both close, Lena has no shippable wake word**, and
recording that is more useful than recording that an engine was chosen.

`Always available ≠ always transmitting` is now enforced structurally: the detector is a local graph
with no network path, measured at zero egress and zero marginal cost while idle.

---

## ADR-EA-026 — Speaker Identification Is Rejected; Speaker *Change* Detection Is Sufficient

**Status:** Accepted · 2026-08-23 · closes deferred decision **D6**

**Decision.** Speaker identification is **rejected for the pilot**. The architecture's only actual
requirement is met by **in-session speaker-change detection**, which stores no voiceprint.

**The insight.** Lena's architecture never asks *"who is speaking?"* The Voice Standard uses speaker
attribution in exactly one direction — **to withhold authority, never to grant it.** A question that
only ever needs a negative answer does not require identification:

```text
what the architecture needs    "is this the same voice that opened this session?"
what identification provides   "which enrolled human is this?" — plus a biometric database
```

Change detection compares within the session and discards at session end. Identification would create
a **retained biometric identifier whose output the architecture forbids acting on** — near-zero
benefit against BIPA/CUBI exposure, consent and retention duties, a new breach surface, and poor
accuracy on a laptop microphone in a busy office.

**Closing it in this direction retires a recorded drift risk.** The register warned that *an
unresolved question tends to resolve toward the only available implementation*. Leaving D6 open was
itself the hazard. `Speaker Recognition ≠ Authority` is now enforced by the **absence of the
capability**, not by restraint.

Preserved as a future optional capability requiring its own authorization and a legal review.
**Never an authenticator.**

Reasoning: [[18_VOICE-PREFLIGHT-L8.2]] · Evidence:
`Trisphere-Enterprise-Agent/docs/engineering/L8.2-VOICE-PREFLIGHT-RECORD.md`
