# Trisphere Progressive Context & Cache Fabric

**Status:** Architecture preserved · Install gate defined · **Not installed · Not built**
**Owner:** Trisphere Ventures LLC
**Canonical role:** FIOS durable record of the architectural reasoning behind Lena's future context layer
**Source studied:** `volcengine/OpenViking` — **AGPL-3.0 · 🔴 RED · reference only**
**Roadmap ownership:** L8.4 (Progressive Context) · Cache Fabric **unowned**

---

## Where the detail lives

This record holds the **reasoning and the decisions**. The implementation-facing specification —
interfaces, gates, benchmark plan, compatibility matrix — lives in the versioned engineering
authority and is not duplicated here:

- **Engineering blueprint:** `Trisphere-Enterprise-Agent/docs/roadmap/LENA-UPGRADE-PROGRESSIVE-CONTEXT-AND-CACHE-FABRIC.md`
- **Provenance registry:** `Trisphere-Enterprise-Agent/docs/provenance/UPSTREAM-REGISTRY.md`
- **Licence notice:** `Trisphere-Enterprise-Agent/licenses/UPSTREAM-NOTICES.md`
- Companion record: [[16_LOCAL-REFLEX-TRIAGE]] · Ancestry: [[12_OPEN-JARVIS-INTAKE]] · Roadmap: [[13_ROADMAP]]

---

## Provenance — pinned

| Field | Value |
|---|---|
| Repository | `volcengine/OpenViking` |
| Commit | `6e944cc3e14872ec7e7a80edec9265397f367894` (`v0.4.16-9-g6e944cc3`) |
| Commit date | 2026-08-22 · **Studied** 2026-08-23 |
| Licence | **AGPL-3.0** |
| Classification | 🔴 **RED** |
| Disposition | **REFERENCE / INDEPENDENT IMPLEMENTATION ONLY** |
| Transformation | **GENERALIZE + REPLACE** — the idea is adopted, the implementation is not |
| Code incorporated | **None. Zero lines.** |
| Study clone | `C:\Development\Trisphere\references\OpenViking` (outside any production tree) |

---

## The problem this solves

Lena currently sends a reasoning model whatever context it is given. As deployments grow, the
expensive input is not the question — it is everything shipped alongside the question.

**The governing test:** *does this make Lena measurably smarter, more reliable, more capable, faster,
or cheaper to operate?* Progressive context is a candidate only for the last three.

---

## What was extracted, and why it is worth having

**The three layers** — a short abstract for filtering, a structured overview for navigation, and the
full source loaded only on demand. That much is obvious. Three details are not:

1. **Summaries describe a CONTAINER, not every item.** Item summaries are inputs aggregated upward.
   This is what keeps the searchable index small enough to be cheap.

2. **The abstract is EXTRACTED FROM the overview**, not generated beside it. One pass produces both,
   and they cannot drift apart. Generating them independently doubles the cost *and* introduces
   disagreement between two descriptions of the same thing.

3. **Refresh propagation stops when a summary's text did not actually change.** A parent consumes its
   children's abstracts; if a regeneration changed only the overview or the metadata, the parent's
   real input is unchanged and refreshing it is pure waste. Every level halts independently.

Point 3 is the most transferable idea in the repository, and it replaces the usual approach —
a TTL — with something better: **a TTL is a guess about how long something stays true; a dependency
graph is a statement of what depends on what.** Only the second can tell an operator *why* something
refreshed.

Also adopted: metadata excluded from relevance embedding via an explicit whitelist, so a source URL
or a generator version can never perturb ranking.

---

## Where OpenViking is not enough — the Trisphere improvement

OpenViking is a context engine for a **trusted local workspace**. It has no principal, no
organization, no clearance and no audit. Retrieval is unauthenticated by construction, and its
storage model puts hidden summary files beside the content — which does not survive contact with CRM
records, mailboxes and databases.

> ## `Context Relevance ≠ Authority`
>
> **A Principal who is not authorized to know that a resource EXISTS must not receive even its
> abstract.**

This is stronger than filtering results, and the distinction is the entire point. A 256-character
abstract is a description of something. Returning "you have no access to *Project Halcyon*" — or
returning a ranked list whose *length* varies with what exists — is an **existence oracle**: a
Principal learns the shape of the organization's confidential work without reading a document.

**Therefore authorization computes the searchable surface BEFORE discovery.** An unauthorized
resource is never a candidate, rather than being removed from results afterwards, and the outcome
must be indistinguishable from one where the resource does not exist.

Two further invariants Lena adds, both absent upstream:

- **A summary inherits the classification of what it summarises.** A summary of `CONFIDENTIAL`
  material is `CONFIDENTIAL`, so the existing egress gate refuses to send it to an uncontrolled cloud
  model. **Summarising is not declassifying.**
- **A container's summary is classified at the MAXIMUM of its children**, or one restricted document
  inside an ordinary project silently downgrades through aggregation.

---

## Decisions recorded

| # | Decision | Status |
|---|---|---|
| D1 | Adopt three-layer progressive loading as Lena's context strategy | **Accepted in principle**, not built |
| D2 | Implement independently; incorporate no OpenViking code, ever | **Accepted** — AGPL-3.0 reaches a hosted Lena |
| D3 | Reject filesystem sidecars as the storage model; storage is a port | **Accepted** |
| D4 | `Context Relevance ≠ Authority` — authorize before discovery | **Accepted, permanent invariant** |
| D5 | Summaries inherit classification; containers take the maximum | **Accepted, permanent invariant** |
| D6 | Content-dependency invalidation, not TTL timers | **Accepted** |
| D7 | `Cache knowledge and computation; never cache authority` | **Accepted, permanent invariant** |
| D8 | Retrieval-result caches keyed by authorized-surface identity, never by query alone | **Accepted** |
| D9 | Ports specified now, coded later | **Accepted** — an unused interface drifts from its first real implementation |

---

## Cache Fabric — the one invariant that cannot be retrofitted

> ## `Cache knowledge and computation; never cache authority.`
>
> **A cache hit never grants permission.**

A cache that stores "principal X may see Y" turns every revocation into a race with a TTL, and a
revoked Principal keeps their access until an entry expires. **Authority is not a cacheable quantity
in this architecture.**

The dangerous layer is the retrieval-result cache: a ranked list computed for one Principal is a map
of what that Principal may see. Keyed by query alone, it serves one person's authorized surface to
another.

---

## Economic hypothesis

**No savings are claimed. These are hypotheses to be benchmarked, and the null is stated first-class:**

- H1 context tokens fall materially · H2 cost per successful task falls · H3 a cheaper qualified model
  becomes viable because prompts shrink · H4 latency falls · H5 caching removes repeated computation
- **H0 — progressive walking costs MORE than it saves** on tasks that always need the full source.
  A benchmark that cannot report a loss is not a benchmark.

**The measurement infrastructure already exists.** Context strategy and version are already an
evidence axis on every Model Lab record, every task economics record and every routing decision, and
aggregation already refuses to pool across a strategy change. Nothing needs building to measure this
— only to make it exist.

---

## Lessons worth keeping regardless of whether this ships

- **A summary of protected material is protected material.** The most likely leak in any hierarchical
  summarisation scheme is an aggregate that quietly loses its parents' sensitivity.
- **Stop-on-unchanged-input beats scheduling.** Most refresh machinery exists because nobody checked
  whether the input actually changed.
- **Honesty in the upstream design paper was worth as much as the mechanism.** OpenViking's own
  document says its strategy is *"a pragmatic cost-control strategy, not a strict real-time
  consistency protocol."* Adopting that framing prevents someone later treating staleness as a bug.
