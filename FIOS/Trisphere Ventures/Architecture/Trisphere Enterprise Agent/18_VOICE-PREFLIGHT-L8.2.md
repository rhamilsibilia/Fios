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

---

## Voice quality gate — 2026-08-23

**Three lessons, and two of them are about not believing your own earlier evidence.**

**The wake model failed on real human speech — 0 out of 10.** Part 2 recorded five live detections
in five attempts and treated that as evidence of general capability. It was not. Those five were
spoken deliberately, close to the microphone, one at a time; the corpus covers varied manner,
distance and level, and the model detected none of it while happily waking to "Hello Lena" and
"Lena will be back Monday". **A small favourable sample is not a capability claim**, and it took a
proper corpus to show it. The result was verified two independent ways before it was believed.

**Retraining on consented data fixed detection and broke everything else.** v2 and v3 both reach
10/10, and both wake to "Hey Elena", "Hey Lisa" and "Hey Layla" — the exact phrases that
disqualified grammar-constrained keyword spotting in the preflight. Seven human positives
oversampled 180–260× teaches "a human voice → fire", and rebalancing the negatives did not rescue it
because **augmentation cannot invent phonetic diversity that is not in the data.** The corpus
specification had already said a retrain needs five or more speakers; the evidence simply confirmed
its own prediction. **No model was promoted.** Picking a winner would have meant shipping a wake word
that answers to a colleague named Elena.

**The recogniser benchmark was voided by its own control, and that is the most useful thing in this
increment.** Three recognisers — including one several times larger — all scored zero action verbs
with word error rates above 100%. Three engines failing identically is not three engines failing.
The control took one minute: the same engine, the same session, transcribed known-good synthetic
audio perfectly and the human recording of the same sentence as **"only"**. **The microphone was
broken, not the models.** Publishing "whisper-medium achieves 18.8%" would have been a false claim
about a vendor's product, derived from our own faulty capture path, and it would have been very hard
to retract later.

**So the binding blocker moved, and got cheaper.** It is no longer "which speech recogniser should
Lena use" — a research question with a licensing tail — but "the built-in laptop microphone drops to
digital silence mid-sentence and swings seventy decibels between identical prompts". That is a
twenty-dollar headset, and it gates every recognition number we would otherwise have spent weeks
arguing about. It also contradicts the pilot's stated goal of requiring no special audio hardware,
which is worth saying plainly rather than engineering around.

**One thing did close cleanly.** Lena's voice was chosen by a human listening to real Lena lines:
**Helena**, over Nadine, on identical English and Spanish passages including a money-movement
read-back. `Lena Voice Identity ≠ TTS Provider` holds — Helena is an adapter choice behind
`SpeechSynthesisProvider`, and a later swap is deployment configuration, not a code change. Local
Piper is now explicitly the **offline fallback**, not Lena's normal voice; the founder heard the
difference unprompted and asked which one was which, which is about as direct a naturalness result
as this gate could produce.

**Consent was built before capture, not reconstructed after.** Benchmarking permission and
model-training permission are asked separately and enforced in code, because deleting a recording
does not unlearn it.

---

## Audio capture root cause — 2026-08-23. A correction.

**The previous entry recommended a USB microphone on the strength of "exact digital silence" in the
recordings. That recommendation was wrong, and the mistake is worth more than the finding.**

The audio was around −96.7 dBFS. **−96.7 dBFS quantises to exactly zero in 16-bit.** Our own save
routine created the zeros; we then read them back, called them a dead stream, and concluded the
hardware had failed. One instrumented run ended it: `stream active=True, zero-blocks=0/114`. The
stream had been alive and delivering non-zero float data the entire time.

**The microphone is healthy.** Three hundred seconds of continuous capture, zero dropouts, zero
device changes, no other application holding the device, and the Bluetooth headset endpoint
unplugged throughout. Speech reaches −34 dBFS with roughly thirteen hundred distinct values, which
is clean, usable audio.

**What is actually happening is a noise gate, and an unusually violent one.** In a silent room the
Intel DSP emits **three distinct sample values across eight seconds**. No analogue front end does
that; only software does. The gate slams shut between utterances instead of settling to a noise
floor, so a nine-second window holding a four-word command contains two seconds of speech and seven
of exact zeros. It is working precisely as designed — and **working correctly is not the same as
being suitable.**

**Three lessons, and the middle one is the expensive one.**

*Measure in the domain where the signal lives.* Every level check in this milestone ran on 16-bit
files after a lossy conversion. The conversion was the defect. Had one measurement been taken in
float before the cast, the wrong diagnosis would never have been written down.

*A smoking gun that does not survive a repeat is not evidence.* Mid-investigation a probe showed the
native sample rate producing 1507 distinct values against 3 for the requested rate — a decisive
sample-rate fault, apparently. Re-measured simultaneously, both read −96.7 dBFS with 3 values. The
probes had been taken minutes apart in a room whose sound had changed. It was written up as a
finding before it was re-run, and re-running it is what saved the conclusion.

*The cheap wrong answer was available and was nearly taken.* "Buy a USB microphone" explains the
symptom, costs little, and would have quietly become a permanent product requirement — a pilot that
promised no special audio hardware, abandoning that promise because of an integer conversion. The
instruction not to declare hardware failure from a single capture implementation is what forced the
second look.

**Nothing about the system settings was changed.** Disabling audio enhancements on the endpoint is
the highest-value remaining experiment and it belongs to the operator, not to the agent that
diagnosed it.

---

## Audio pipeline correction — 2026-08-24

**The pipeline is fixed and proven. The microphone still cannot be relied on, and the reason is
specific to this laptop rather than to Lena.**

Three corrections, each demonstrated rather than asserted. Audio stays **float32** until the
provider boundary — on identical samples a naive cast produces 100% zeros while the normalised
conversion produces 13.4% on real speech, which is the entire original misdiagnosis in one line.
Capture health became **three states** instead of two, and a unit test now proves a quiet room reads
`GATE_CLOSED` with the stream alive while a genuinely closed stream reads `STREAM_DEAD` **on the
same samples** — the old binary logic could not separate those, which is exactly why its liveness
check never fired when it mattered. And capture became **gate-aware**: a continuously-filled
pre-roll ring, onset decided against an adaptive floor measured in float, and an ending bounded by
silence rather than a timer. It captured cleanly end to end at −29 dBFS with 9503 distinct values.

**The sixty-second human gate still did not pass** — zero seconds of signal in sixty, with zero
stream deaths and zero liveness failures. The pipeline behaved correctly and had almost nothing to
capture. Across arm's length, thirty centimetres and fifteen, at normal and raised volume, the gate
opened for one second out of thirty-two.

**What settled the attribution was the one check that shares none of our code:** Windows' own input
meter moves when the operator speaks. The hardware is fine. This machine's DSP simply demands far
more acoustic energy than ordinary conversation delivers, and that is a property of the laptop's
audio tuning, not of Lena. It is recorded as a **hardware-specific limitation**, with a USB headset
**recommended for this machine** — deliberately not promoted to a universal requirement, because
neither another machine nor this one with enhancements disabled has been measured. The operator
chose not to disable enhancements this round, so that question is **not tested**, not answered.

**The methodological lesson outranks the audio result.** The same hypothesis — that requesting a
non-native sample rate was destroying the signal — was raised, refuted, resurrected on new evidence,
and refuted again. The first refutation was itself worthless: it re-measured in a silent room, where
both configurations correctly report the noise floor and look identical. **It compared nothing
against nothing and called that a result.** Only a paired, interleaved test taken during continuous
speech separated a real configuration difference from a gate opening and closing on its own
schedule — and that test showed the supposedly broken configuration performing better.

So the standing rule now has a corollary. Instrumentation integrity must be proven before blaming
the product, the model, the provider or the hardware — **and a comparison run against silence proves
nothing about either side of it.**

The earlier wrong conclusions are kept and marked superseded. The sequence — dead stream, then
sample-rate fault, then sample-rate fault again — is the useful record. Each was plausible, each was
measured, and each was wrong for a different reason.

---

## The gate was a Windows checkbox — 2026-08-24

**Every gating behaviour recorded across five parts of this investigation was the Windows Audio
Enhancements setting, which had been ON by default the whole time.**

Turning it off removed the gate outright. Silence went from −96.7 dBFS with **three** distinct sample
values and 77% exact zeros, to −24.9 dBFS with **ten thousand six hundred** distinct values and no
zeros at all. The failed sixty-second gate, the distance envelope that opened one second in
thirty-two, the "exact digital silence" that started the whole misdiagnosis — all one checkbox.

**And the gate was hiding the real problem.** Underneath it the microphone delivers about **four
decibels of signal above its own noise**, where normal capture sits at twenty to forty. Speech is
finally captured — a twenty-five second read came back correctly as *"testing testing one two
three"*, the first correct transcript this microphone has produced in the entire milestone — but
business commands still fail badly. Four of thirteen critical tokens survived. *"Transfer eight
hundred fifty dollars"* became *"trains were a hundred and fifty dollars"*: the verb destroyed and a
six-fold error in the amount. *"Do not send the invoice to Martinez"* became *"the next in the voice
to my teenage kids"*, losing the negation entirely.

**Two mitigations were tried and both were insufficient, one instructively so.** Spectral subtraction
raised the signal-to-noise ratio by fourteen decibels and made the transcript *worse* — the
artifacts it introduces are harder for a recogniser than the noise it removes. **A mitigation that
improves the metric and worsens the outcome is not a mitigation**, and this is the third time in this
milestone that an attractive number turned out to carry no information, after the wake-word
confidence score and the recogniser confidence score. The pattern is worth naming: *whenever a number
improves and the thing it supposedly measures does not, believe the thing.*

**A genuine defect in our own pipeline surfaced on the way.** The speech-onset threshold was a fixed
twelve decibels above the noise floor — trivially cleared while the gate parked that floor at −96
dBFS, and *mathematically unreachable* once the floor rose to −25 dBFS with speech at −21. Every
utterance was reported as a closed gate. It is now proportional to the available headroom. Fixing it
was necessary and nowhere near sufficient.

**So the conclusion is narrow and deliberately so.** This laptop is a **hardware-specific degraded
voice endpoint** — a noisy analogue front end whose deficiency was masked by an aggressive gate. Lena
gets an external-microphone recommendation *for this machine*. The no-special-hardware promise is
**not** abandoned, because no second machine has been measured and the pipeline that failed here is
now demonstrably correct rather than suspect. The cheapest next experiment is to run the identical
protocol somewhere else; if the token accuracy jumps, the promise survives everywhere but here.

---

## The control that did not exist — 2026-08-24

**Part 6 ended with a USB recommendation. That was one claim too many.** The evidence proved this
laptop's built-in path is degraded; it never proved an external microphone fixes it. The correction
was right, and the gate that would have settled it **could not be run at all.**

The zero-cost candidate turned out to have no microphone: the USB camera is a composite device with a
single interface, video only, and the machine has **no USB audio device of any kind**. The only other
capture endpoint is a Bluetooth headset, and when it was connected it enumerated on all four Windows
audio APIs and delivered **absolute digital silence — one distinct sample value — on every one of
them, at every rate.** An endpoint exists. A working microphone does not.

**So the honest verdict is NOT CONFIRMED, not "confirmed by absence".** The instruction anticipated
the case where the control performs badly and said: do not blame both microphones, reopen the
upstream path. That branch was never reached, because a device producing one distinct value is not a
poorly-performing microphone — **it is an absent measurement.** Whether the fault lies in this
laptop's microphone or somewhere upstream of it cannot be decided on a machine with exactly one
working capture device, because a single-device comparison is not a comparison.

**Two things went wrong on my side and both are worth keeping.**

The first comparison recorded **both microphones simultaneously**, which is genuinely better science —
identical acoustic event, no drift possible, half the operator's effort. It also broke the
measurement: opening a Bluetooth Hands-Free stream puts Windows into communications mode, and the
laptop microphone collapsed to two decibels of dynamic range. **A paired design is worthless when the
pairing perturbs the thing being paired.** The elegant experiment was the wrong experiment.

And the built-in microphone then measured progressively worse than its own earlier baseline while the
headset stayed attached — eleven decibels of dynamic range, then seven, then two. The endpoint never
released, so it was **never re-measured clean**. Every number from that session is confounded and
**none of it supersedes the earlier baseline**; the thirty-one percent figure stands. Recording that
the later, worse numbers are unusable matters more than recording the numbers.

**The closing experiment costs nothing:** run the committed protocol on any second machine that
already exists. If accuracy jumps, this laptop is the outlier and the no-special-hardware promise
survives everywhere else. If it does not, the cause is upstream of the microphone and the capture
path reopens rather than the hardware being blamed. **Nothing should be purchased on the strength of
this record.**
