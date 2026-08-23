# Trisphere Local Reflex / Task & Capability Triage

**Status:** Architecture preserved · Install gate defined · **Not installed · Not built**
**Owner:** Trisphere Ventures LLC
**Canonical role:** FIOS durable record of the architectural reasoning behind Lena's future local triage tier
**Candidate studied:** `cactus-compute/needle` (Needle 2) — **Apache-2.0 · 🟢 GREEN**
**Roadmap ownership:** **Unowned** — needs a milestone assignment

---

## Where the detail lives

Reasoning and decisions live here. The implementation-facing specification — the provider interface,
benchmark requirements, supply-chain gate, activation and rollback path — lives in the versioned
engineering authority and is not duplicated:

- **Engineering blueprint:** `Trisphere-Enterprise-Agent/docs/roadmap/LENA-UPGRADE-LOCAL-REFLEX-TRIAGE.md`
- **Provenance registry:** `Trisphere-Enterprise-Agent/docs/provenance/UPSTREAM-REGISTRY.md`
- **Licence notice:** `Trisphere-Enterprise-Agent/licenses/UPSTREAM-NOTICES.md`
- Companion record: [[15_PROGRESSIVE-CONTEXT-AND-CACHE-FABRIC]] · Router: [[05_MODEL-ROUTER]] · Capabilities: [[04_CAPABILITY-RUNTIME]]

---

## Provenance — pinned

| Field | Value |
|---|---|
| Repository | `cactus-compute/needle` |
| Commit | `571fcd68f48b6649c91f353eb91d0a0c9b9135ce` (tag **`v2.0.9`**) |
| Commit date | 2026-08-20 · **Studied** 2026-08-23 |
| Licence — code | **Apache-2.0** |
| Licence — **weights** | **Apache-2.0**, ungated (`huggingface.co/Cactus-Compute/needle2`) — **checked separately** |
| Classification | 🟢 **GREEN** |
| Disposition | **ASSESS** — direct use / wrap / adapt permitted in principle, **gated** on four findings |
| Transformation | **WRAP** behind a Trisphere-owned interface — never adopted under its own name |
| Code incorporated | **None** |
| Study clone | `C:\Development\Trisphere\references\needle` |

**Weights were licence-checked separately from code.** A permissive code licence over restricted
weights is the usual trap in this category; here both are permissive, and recording it means nobody
has to re-establish it.

---

## The problem this solves

> *"Find Carlos's phone number"* should not require a large cloud reasoning model merely to
> determine `task = CONTACT_LOOKUP`, `capability = contacts.search`.

Today it does. And as Lena's capability registry grows toward hundreds of entries, every request
carries a prompt describing capabilities it will never use.

---

## What was extracted

Four mechanisms, each independently useful:

1. **Confidence as the MINIMUM of two independent signals** — a calibrated post-hoc head and the
   decode probability of the produced call. A disagreement produces caution rather than a blended
   middle, so *the failure mode is escalation, not wrong execution*. Off-topic input returns empty
   rather than a guess.
2. **Capability shortlisting** — every schema embedded once, only the top few admitted per turn, and
   the decode grammar rebuilt over just that subset.
3. **Schema-constrained extraction** — a byte-level grammar compiled from the declared schemas, so
   malformed output is *unreachable* rather than unlikely.
4. **Bounded memory** — a fixed small footprint regardless of conversation length, which is what makes
   edge deployment plannable.

---

## The authority boundary — absolute, and the reason this is safe

> ## `Capability Suggestion ≠ Capability Grant`

The Local Reflex Provider **cannot** create capabilities, grant capabilities, grant permissions, lower
a risk tier, or widen a shortlist into an authorization. Its entire output is **untrusted input**,
identical in standing to text a user typed, and it enters the same canonical pipeline:
`validation → Capability → Policy → Authority → Execution`.

**This is the same rule Lena already enforces for a cloud model's proposed capability calls.** A
proposal is a proposal — and **a local model earns no additional trust for running on our own
hardware. Proximity is not authority.**

---

## Four findings that gate adoption

None is cosmetic, and none is a blocker on its own:

| | Finding | Consequence |
|---|---|---|
| **F1** | A **native binary is fetched from the model repository at runtime and executed in-process** | Must be pinned by digest and mirrored internally before any install. The most significant supply-chain finding. |
| **F2** | **Fine-tuning voids the confidence head** — tuned weights report no confidence | Confidence *is* the escalation mechanism, so domain adaptation is architecturally expensive, not the cheap win it appears to be |
| **F3** | Top-N tool retrieval is a **hard cut** — an unselected capability is *unreachable*, not merely unlikely | At Lena's catalogue size a recall miss becomes a task failure, not a worse answer. Needs a recall floor and a widening valve. |
| **F4** | Heavy ML framework stack for a very small model | The model is tiny; the runtime is not. Edge footprint is dominated by the framework. |

**F2 is the one most likely to be discovered too late.** "Fine-tune it on Familia data" reads as an
obvious improvement and would silently remove the signal the whole design depends on.

---

## Decisions recorded

| # | Decision | Status |
|---|---|---|
| D1 | Lena owns a generic `LocalReflexProvider`; **Needle never becomes the architectural name** | **Accepted** |
| D2 | Triage output is untrusted input, always re-entering validation | **Accepted, permanent invariant** |
| D3 | `Capability Suggestion ≠ Capability Grant` | **Accepted, permanent invariant** |
| D4 | Escalation thresholds are Trisphere-calibrated, never vendor-claimed | **Accepted** |
| D5 | Any tier qualifies **per task class**, never in general | **Accepted** — consistent with existing Model Lab doctrine |
| D6 | Preserve an intelligence ladder (deterministic → tiny local → larger local → economy cloud → premium) without traversing it automatically | **Accepted** |
| D7 | Install gated on F1–F4 | **Accepted** |

---

## Economic hypothesis

**No savings are claimed.** H1 a material share of requests never need cloud reasoning · H2
capability-schema tokens fall sharply · H3 latency falls for simple requests · H4 tool-selection
errors fall.

**The nulls, stated first-class:** H0 — triage adds latency and a failure mode without displacing
enough cloud calls. H0b — shortlist recall misses cause task failures costing more than the tokens
saved.

Before any activation, Model Lab must establish whether **confidence actually predicts correctness on
Lena business tasks** — a vendor's calibration is calibration against a vendor's task distribution.

---

## A strategic point worth remembering

The existing egress rule already forbids `CONFIDENTIAL` and above from leaving the deployment. That
means local tiers are the **only** tiers available for that work — **an argument for local
intelligence that is independent of cost**, and one that survives even if every economic hypothesis
above fails.
