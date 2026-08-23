# L8.2 Voice Preflight — Reasoning, Decisions, Rejected Approaches

**Status:** Preflight complete · full L8.2 implementation NOT authorized · **2026-08-23**
**Owner:** Trisphere Ventures LLC
**Canonical role:** why the voice decisions were made. The measurements, pinned hashes and
reproducible evidence live in the Enterprise Agent repository and are **not duplicated here.**

> Specification & evidence → `Trisphere-Enterprise-Agent/docs/engineering/L8.2-VOICE-PREFLIGHT-RECORD.md`
> Normative architecture → `docs/standards/LENA-VOICE-STANDARD-v1.0.md` (unchanged, plus a dated revision)
> Related: [[14_ARCHITECTURE-DECISIONS]] · [[17_ARCHITECTURE-SYNC-WORKFLOW]] · [[13_ROADMAP]]

---

## What the preflight actually discovered first

**Most of the voice architecture already existed and was better than expected.** The Lena Voice
Standard v1.0 — written at the Delta v1.3 pre-build gate — already settled barge-in, local stop,
the follow-up window, narration levels, language resolution, read-back, and classification-governed
speech. The preflight's job was therefore not to design voice. It was to **resolve two deferred
decisions with evidence and to find out whether the architecture survives contact with hardware.**

Two things the authorization assumed existed did **not**, and inventing them quietly would have been
the easy error:

- **There is no prior ElevenLabs decision anywhere in the canonical architecture.** Zero mentions.
  The standard deliberately names no provider, per AT-33. Nothing was re-decided, because nothing
  had been decided.
- **There was no recorded Lena voice persona.** `VoiceIdentity` was a defined shape with no value.
  "The Executive Leader" is therefore **new canonical input from the founder**, now recorded as
  `VoiceIdentity v1.0` rather than treated as pre-existing.

---

## D5 — Wake-word engine. The measurement that decided it

Three candidates were taken seriously: grammar-constrained ASR (one engine for wake *and* speech),
a purpose-built discriminative model, and Porcupine.

**The tempting option was the elegant one.** Vosk is Apache-2.0 in both code and model weights, needs
no training step, and would have served as wake word *and* transcriber — one dependency, one licence,
no training pipeline. On paper it wins.

**It failed on evidence, and the failure is structural rather than tunable.** Against twenty
adversarial negatives it produced a **30% false-positive rate**, firing on `Hey Elena` and `Hey
Lisa`. The decisive detail is not the rate:

```text
confidence of a TRUE  wake     1.000
confidence of a FALSE wake     1.000
```

A constrained decoder is *forced choice* — it must map audio onto its grammar, so a near-miss becomes
a confident match. **No threshold exists that separates them, because there is nothing to separate.**
A confidence score that cannot discriminate is not a safety control, and shipping one would have
been worse than shipping none, because it would look like a control.

The discriminative model, on the identical negatives, scored **0/20 false positives with +0.9946
separation** for essentially the same CPU cost.

**Porcupine was rejected on economics, not quality.** $6,000–$30,000/yr plus a runtime AccessKey — a
vendor dependence the pilot exists to avoid — against a $0 option that measured zero false positives.
It is retained as a pre-qualified fallback adapter, which is what the provider seam is *for*.

### The licence finding is the real content of D5

**openWakeWord is a mixed repository, and the halves are different colours** — the same pattern the
Kage precedent established, and the reason [[17_ARCHITECTURE-SYNC-WORKFLOW]] step 3 says to check
code and weights **separately**.

```text
code                Apache-2.0                     GREEN
pre-trained models  CC-BY-NC-SA-4.0                RED — non-commercial, and Lena is commercial
feature extractor   "Google, Apache-2.0" — while the same README says ALL included
                    pre-trained models are CC-BY-NC-SA.  Both sentences cover the same files.
```

**The ambiguity was escalated rather than resolved by choosing the convenient reading.** That is the
whole point of the licence firewall: the moment the permissive interpretation is adopted because it
is more useful, the firewall has stopped working.

So D5 answers the engine question and **explicitly does not produce a shippable artifact.** Two named
conditions gate one: a written licence determination, and training the head on data that is not
CC-BY-NC-SA — noting that the upstream training path defaults to exactly the contaminated feature
set, so cleanliness is a deliberate deviation, not a default.

**Recording that Lena has no shippable wake word today is more useful than recording that the
engine was chosen.**

---

## D6 — Speaker identification. Closed by rejecting it

The register had flagged the real hazard: *an unresolved question tends to resolve toward the only
available implementation.* Leaving D6 open **was** the risk. It is now closed in the safe direction.

The insight that decides it: **the architecture never asks "who is speaking?"** Voice Standard §5
uses speaker attribution in exactly one direction — to **withhold** authority, never to grant it. A
question that only ever needs a *negative* answer does not require identification:

```text
what the architecture needs      "is this the same voice that opened this session?"
what identification provides     "which enrolled human is this?"  — plus a biometric database
```

**In-session speaker-change detection** answers the needed question, stores no voiceprint, and
discards its comparison at session end. Full identification would create a retained biometric
identifier whose output the architecture *forbids acting on* — near-zero benefit against BIPA/CUBI
exposure, consent and retention obligations, a new breach surface, and poor accuracy on a laptop
microphone in a busy office.

**Rejected for the pilot. Preserved as a future optional capability behind its own authorization and
a legal review. Never an authenticator.**

---

## The finding that should shape L8.2 more than either decision

Local small-model transcription was measured on real Familia-domain business phrases, in English and
Spanish, on clean synthetic speech with no noise and no accent — **the most favourable possible
conditions**. Both engines destroyed the **action verb**:

```text
"Transfer eight hundred fifty dollars…"  ->  "france for 850 dollars…"
"Schedule a follow up…"                  ->  "casual a follow up…"
"Send the intake packet…"                ->  "then the intake packet…"
"Envía el paquete…"                      ->  "india el paquete…"
"Rodriguez"                              ->  "rodrigo s"
```

Critical-slot accuracy peaked at **92.3%** and fell as low as **69.2%**.

**This converts read-back from a prudent design choice into a load-bearing safety control.** The
Voice Standard already required it; the evidence is why it must never be softened into a prompt
instruction that a future implementer optimises away.

**Three of the four destroyed verbs were the first word of the utterance** — leading-audio
truncation, not model weakness. A ~500 ms pre-roll buffer is the fix, and finding this in preflight
rather than in a client office is the whole return on doing a preflight.

**Spanish was not the weak language.** Vosk transcribed Spanish *better* than English (2.5% vs 10.0%
WER) and roughly 3× faster. The bilingual assumption that Spanish would be the harder case is wrong
for this stack.

---

## What the hardware gave us for free

The pilot machine's audio DSP performs **speech-targeted acoustic echo cancellation** — 65–71 dB
suppression of speech versus 49 dB for a tone. **Lena's own voice cannot re-trigger Lena's own
microphone**, which removes the largest engineering risk in barge-in before any code is written.

It also **invalidated a test method**, which is worth recording as a lesson: the machine cancels its
own speaker, so the "a television says *Hey Lena*" attack cannot be staged through its own speakers.
That test needs a human voice or an external speaker. The limitation was reported rather than
worked around, and the through-the-air wake proof is recorded as **NOT PROVEN**.

**Stop was measured at max 20 ms against a 300 ms requirement**, with no network and no model in the
path — the human can stop Lena while the reasoning model is still thinking, which is the entire
requirement of Voice Standard §4.

---

## Economics — the ratio that matters

```text
TTS costs roughly 13x what STT costs
    STT  ~6 s command      ~$0.00077
    TTS  ~200 chars        ~$0.010
    wake word, local        $0.00       measured, zero egress while idle
```

**A completed voice task, including the read-back the accuracy evidence makes mandatory, is roughly
$0.026 excluding model cost** — 2 STT + 2 TTS, times a retry rate implied by the measured accuracy.

The useful consequence: **barge-in is a cost control, not only a courtesy.** Truncating an unwanted
spoken response truncates the dominant line item. The Voice Standard asserted this; it is now
quantified.

**No new accounting vocabulary is required.** `CostComponent` and `Amount = known | estimated |
unknown` already express wake, STT, TTS, failed transcription, retry and fallback. Nothing was built.

---

## Rejected, and why — so none of it is reopened by accident

| Rejected | Reason |
|---|---|
| Grammar-constrained ASR as the wake detector | 30% false positives with **no separable confidence**. Structural, not tunable |
| Porcupine for the pilot | $6k–30k/yr plus a runtime AccessKey, to beat a $0 option that scored 0/20 |
| Shipping any openWakeWord pre-trained weight | **CC-BY-NC-SA — non-commercial. Lena is commercial** |
| Importing Piper as a library | GPL-3.0-or-later. Separate process only |
| A Windows-native TTS fallback | **No Spanish voice on the platform** — English-only is a half-capability for a bilingual office |
| Speaker identification in the pilot | Biometric cost for a benefit the architecture forbids acting on |
| `WAKE_DETECTED` and `OFFLINE` as states | Vocabulary drift. One is a transition, one is a reason on `DEGRADED` |
| Extending the follow-up window to 30–60 s | Canonical 20 s is tighter and safer for uninitiated listening |
| Spending on the connected audio gateway for TTS benchmarking | It defaults to a different vendor and routes through an aggregator — **the latency and cost would not transfer to Lena's real path.** No evidentiary value, so no spend |

---

## Lessons worth carrying

1. **Check code and weights separately, every time.** The single most consequential finding of this
   preflight was a licence, not a benchmark — and it sat behind an Apache-2.0 badge.
2. **A confidence score that cannot separate is worse than no score**, because it will be trusted.
3. **Measure the first call, not the average.** Local synthesis is ~600 ms warm and **6.1 s cold**;
   only a cold-start measurement reveals that the first thing Lena ever says arrives six seconds late.
4. **Favourable hardware behaviour can invalidate your test rig.** The AEC that makes barge-in safe
   is the same AEC that made the through-the-air test impossible. Report the gap; do not narrate
   around it.
5. **Popularity was not consulted and would have chosen differently.** The elegant one-engine design
   lost to a measurement.

---

## Readiness closure — 2026-08-23

**The preflight said the engine was chosen and no shippable artifact existed. That is now closed, and
the way it closed is the lesson.**

The openWakeWord feature-extractor licence never came clean. A reasonable reading favoured us. **We
did not take it** — we removed the dependency instead, and built a wake model with **no third-party
weights at all**. A mel filterbank is arithmetic, and arithmetic carries no licence. The result is
also *twelve times cheaper* to run than the thing it replaced, which was not the goal and is worth
noticing: refusing the shortcut produced the better engineering outcome, not merely the safer one.

**The biggest risk did not materialise.** A model trained on two synthetic Windows voices detected a
real human saying "Hey Lena" **five times out of five**, through the air, on the pilot laptop.

**The second lesson is about what a confidence score is for.** The preflight found that
grammar-constrained wake detection scored true and false wakes identically at 1.000. The closure
found the *same* thing in the command path: every command whose verb had been destroyed still
reported 0.90–1.00 confidence. Twice now, the number that looked like a safety control carried no
information. **The control that works asks a different question: is this a verb Lena actually has?**
That caught 7 of 7 corrupted verbs, with none passing silently.

**The third lesson is that the live test found what no synthetic test could.** Not the wake word —
that worked. The *microphone*. Real speech arrived at −64 dBFS at conversational distance and −34 dBFS
close, and early runs produced **empty** transcripts purely from level. The wake model shrugged it off
because it was trained with gain augmentation; the recogniser did not. Every threshold in the harness
had to be recalibrated from measurement, including a barge-in threshold that had been sitting *above
the operator's own voice* — so a genuine interruption went undetected twice before anyone knew why.

**And the finding that decides readiness.** Spoken by a human: *"transfer eight hundred fifty dollars
to the operating account"* was heard as *"that's the a hundred fifty dollars to the operating
account"*. **A six-fold error in the amount of a money movement.** Lena refused correctly anyway,
because the governance layer does not depend on the transcript being right. That is the architecture
working — and it is also why Lena is **NOT READY**: an assistant that mis-hears most consequential
commands and has to ask again every time is not something to put in front of a client, even when it
is safe.

**Both remaining blockers close with one action:** retrain on consented Trisphere staff recordings.
That settles the training-data provenance question *and* attacks the accuracy gap, which is the kind
of coincidence worth spending the money on.
