# FIOS Project Preflight & Delivery Framework

**Status:** Canonical Operating Methodology  
**Applies to:** All projects, ventures, client work, internal products, websites, applications, AI systems, automation, media, and infrastructure initiatives  
**Origin:** Formalized from the Famil-IA 2.0 restart and Delivery Harness lessons  

## Core Principle

Do not begin with **“Can we build it?”**

Begin with:

> **Should we build it this way, with this platform, under these constraints, and can we prove the critical parts before committing to the full project?**

The purpose of this framework is to prevent avoidable rework, uncontrolled spending, unsuitable platform choices, fragile implementations, false PASS results, and launches that do not meet the intended experience.

A NO-GO is a successful Preflight outcome when it prevents us from building the wrong thing.

---

## Universal Project Lifecycle

**DISCOVER → DEFINE → PREFLIGHT → PROVE → DESIGN → BUILD → REVIEW → VALIDATE → HUMAN GATE → RELEASE → MONITOR → LEARN**

No project advances merely because work has been completed. It advances when the exit criteria for the current milestone are supported by valid evidence.

---

## 1. DISCOVER — Understand the Real Problem

Before selecting tools or architecture:

- Understand the business objective.
- Identify the actual user/customer problem.
- Capture the desired experience and outcome.
- Identify stakeholders.
- Review existing infrastructure and prior work.
- Preserve useful intellectual capital instead of automatically rebuilding.
- Identify assumptions that are being treated as facts.

**Output:** Problem statement, context, existing assets, initial constraints.

---

## 2. DEFINE — Lock Success and Non-Negotiables

Define what success actually means before implementation.

Capture:

- Primary objective.
- User experience requirements.
- Business requirements.
- Functional requirements.
- Brand/design requirements.
- Performance expectations.
- Accessibility expectations.
- Security/privacy requirements.
- Cost boundaries.
- Schedule/deadline requirements.
- Non-negotiable acceptance criteria.
- Explicit failure conditions.

A technically functioning product may still be a failure if it violates the intended experience.

**Output:** Project Charter + Acceptance Criteria.

---

## 3. PREFLIGHT — Determine Whether the Project Should Proceed

Preflight occurs before full implementation.

Evaluate:

### Feasibility
- Can the required experience actually be produced?
- Are mandatory dependencies technically possible?
- Are there known platform limitations?
- Are required APIs/features available?
- Are browser/runtime constraints understood?

### Architecture
- What is the simplest reliable architecture that supports the experience?
- Which components are truly necessary?
- Are we introducing unnecessary coupling or complexity?

### Platform Selection
Never choose a platform merely because we already use it.

Evaluate whether the proposed platform is optimal for this specific project based on:

- Required capabilities.
- Reliability.
- API limitations.
- Integration support.
- Runtime/browser support.
- Scalability.
- Maintainability.
- Vendor lock-in.
- Cost.
- Development speed.
- Deployment environment.
- Evidence from prior use.

If the platform cannot reliably support a mandatory requirement, change the platform or architecture before building.

### Economics
- Expected implementation cost.
- Paid API/model usage.
- Recurring costs.
- Infrastructure costs.
- Cost of experimentation.
- Cost of failure/rework.
- Available budget/margin.

### Risk
Maintain a scored risk register covering technical, product, business, cost, schedule, dependency, platform, security, and experience risks.

### Preflight Verdict
Use:

- **GO** — major requirements and critical dependencies are sufficiently proven.
- **CONDITIONAL GO / PROTOTYPE FIRST** — one or more critical assumptions require proof before implementation.
- **NO-GO** — current architecture/platform/concept cannot responsibly satisfy mandatory requirements.

**Output:** Preflight Report, Architecture Decision, Risk Register, Cost Strategy, Evidence Plan.

---

## 4. PROVE — Test the Hardest Assumption First

Do not prototype the easy parts.

Prototype the uncertainty.

Identify the dependency most capable of killing the project and test it before building around it.

Use the smallest/cheapest experiment capable of falsifying the hypothesis.

For each experiment record:

- Hypothesis.
- Candidate/test ID.
- Inputs.
- Platform/model/tool/version.
- Settings.
- Cost.
- Environment.
- Output.
- Measurements.
- Result: PASS / FAIL / INCONCLUSIVE / ENVIRONMENT_INVALID / INSTRUMENTATION_INVALID.
- What was learned.
- Whether another experiment is justified.

Do not repeatedly spend money without producing new information.

**Output:** Proof-of-Capability evidence.

---

## 5. DESIGN — Prove the Design Language Before Scaling It

Do not build an entire product before establishing that the visual and UX direction is correct.

For design-heavy projects:

- Establish creative north star.
- Preserve brand philosophy.
- Reject generic/template output.
- Prototype the most representative portion first.
- Test responsive implications early.
- Review information architecture.
- Review accessibility and usability.

A useful design test:

> If the branding were removed, would the experience still feel specifically designed for this company/product?

If not, refine before scaling implementation.

**Output:** Approved design system/direction and representative prototype.

---

## 6. BUILD — Sophisticated Experience, Simplest Reliable Architecture

Implementation begins only after relevant gates pass.

Principles:

- Prefer simple, predictable architecture.
- Avoid unnecessary state and coupling.
- Preserve graceful degradation.
- Build accessible semantics from the beginning.
- Keep media/animation enhancements from controlling core usability.
- Preserve maintainability.
- Avoid architecture that exists only to support visual spectacle.
- Track material scope/architecture changes through decisions.

**Output:** Implementation candidate.

---

## 7. REVIEW — The Builder Does Not Self-Certify

Run an independent review layer after implementation.

Review:

- Architecture.
- Code quality.
- Component design.
- Duplication.
- Unnecessary complexity.
- State management.
- Accessibility.
- Semantic structure.
- Performance.
- Media handling.
- Browser compatibility.
- Error handling.
- Security-relevant concerns.
- Maintainability.
- Dead code.
- Console/runtime errors.

Material findings reopen the affected gate.

**Output:** Independent Review Report + repair actions.

---

## 8. VALIDATE — Evidence, Not Assumptions

The Delivery Harness validates the actual candidate.

### Evidence Integrity Rules

- Temporal overlap does not prove causality.
- Prove that the causal path was actually activated before attributing a result to a repair.
- Counterfactual analysis is evidence only when the trace contains the target event; otherwise classify it INCONCLUSIVE.
- Internally inconsistent release-critical samples invalidate downstream conclusions until instrumentation integrity is proven.
- Instrument evidence with coherent identity metadata: asset/entity, requestId, generation/version, source, timestamp/sampleId, environment, etc.
- Instrumentation integrity is a gate before product conclusions.
- Invalidated/questioned evidence reopens the decision for revalidation.

### Shipping Artifact Rule

Evidence applies to the exact artifact tested.

If an asset, encode, build, configuration, dependency, or production candidate changes materially, affected evidence must be rerun.

Do not transfer PASS status to an untested replacement.

### Environment Integrity

Before interpreting browser/media evidence, verify that the environment represents the phenomenon being tested.

If automation changes the environment — for example, hidden/background browser behavior invalidating visible media/compositing conclusions — classify the result as **ENVIRONMENT_INVALID** or **INCONCLUSIVE**, not product PASS/FAIL.

**Output:** Harness evidence package and gate status.

---

## 9. HUMAN GATE — Reality Is the Final Perceptual Test

Automated evidence is necessary but not always sufficient.

For visual, interactive, audio, video, workflow, or experiential requirements, conduct a human reality review using the actual integrated candidate in a representative foreground/runtime environment.

Ask whether the experience actually meets the original intent — not merely whether the system reports success.

A technical PASS with an unacceptable real-world experience is a FAIL.

**Output:** Human Reality Gate PASS/FAIL.

---

## 10. RELEASE — Deployment Is a Separate Authorization

Completion does not imply permission to deploy.

Explicit authorization is required for:

- Paid actions beyond approved limits.
- Material architecture changes.
- Preview deployment where required by project rules.
- Production deployment.

Before production:

- Required gates must be green.
- Shipping artifact must match validated artifact.
- Rollback path must exist.
- Known risks must be documented.
- Production authorization must be explicit.

**Output:** Release decision + production record.

---

## 11. MONITOR — Verify Production Reality

After release:

- Confirm production health.
- Monitor critical flows.
- Review performance/error evidence.
- Validate important integrations.
- Confirm production behavior matches the approved candidate.
- Preserve incident evidence when problems occur.

Do not confuse successful deployment with successful operation.

---

## 12. LEARN — Preserve Intellectual Capital

Every project should improve the operating system.

Capture:

- What worked.
- What failed.
- Platform limitations discovered.
- Cost/performance observations.
- Reusable scripts/tools.
- Architecture lessons.
- Design lessons.
- Harness refinements.
- New risks.
- New acceptance patterns.

Do not automatically change the active Harness mid-project. Record lessons in the refinement backlog and promote them deliberately after the pilot/project review.

---

# Four Persistent Project Records

Every project should continuously maintain four categories of truth:

## Decisions
What did we decide, when, and why?

## Evidence
What proves or challenges that decision?

## Risks
What remains uncertain or capable of harming delivery?

## Authorization
What is the team/agent actually permitted to do next?

These four records prevent chat history, assumptions, or momentum from becoming the source of truth.

---

# Economic Control Rule

Exceeding an expected cost or ETA does not automatically stop an approved project.

For minor/moderate variance, perform a variance review containing:

- Spend to date.
- Revised remaining cost.
- Revised ETA.
- Margin/business impact.
- Scope impact.
- Architecture impact.
- Probability of successful completion.
- Recommended action.

Continue only after explicit approval when required.

Hard stops apply to:

- Unapproved spending.
- Unbounded/unknown cost.
- Unacceptable margin/deadline exposure.
- Material business risk.
- Failure of a non-negotiable gate.

---

# Milestone Rule

A milestone is not complete because an agent says it completed the work.

A milestone is complete when:

1. Required deliverables exist.
2. Acceptance criteria are evaluated.
3. Evidence is valid.
4. Blocking findings are resolved or explicitly accepted.
5. Required human/authorization gates are satisfied.

**Work completed ≠ Gate passed.**

---

# Universal Stop Rules

Stop and reassess when:

- A mandatory requirement is proven infeasible.
- The chosen platform cannot support a non-negotiable requirement.
- Instrumentation cannot be trusted.
- Evidence contradicts the working assumption.
- Cost becomes unbounded.
- Repeated experiments stop producing new information.
- A material architecture change is required without approval.
- The validated artifact and shipping artifact diverge.
- Human reality contradicts automated PASS results.

Stopping is not project failure. Continuing blindly is.

---

# Canonical Operating Principle

> **We do not earn confidence by spending more time on a project. We earn confidence by reducing uncertainty with valid evidence.**

The goal of FIOS project delivery is not simply to finish projects. It is to make deliberate, evidence-based decisions that result in products we are willing to stand behind.

---

## Reference Implementation: Famil-IA 2.0

Famil-IA 2.0 established the practical model for this methodology:

- The prior architecture was reconsidered instead of blindly reused.
- Platform/model behavior was measured from previous evidence.
- The highest-risk dependency — the seamless canonical droid hero loop — was tested before the website was rebuilt.
- Cheap proof-of-capability experiments preceded expensive production generation.
- Failed instrumentation was invalidated and repaired rather than rationalized.
- Internal-loop feasibility was proven before advancing.
- Human torture-test review remained a release gate.
- Identity drift became a hard filter after evidence exposed it as the next dominant risk.
- Generic Figma output was rejected rather than accepted because work had already been spent on it.
- Front-end design was redirected toward specialized design skills, followed by Delivery Harness validation and independent code review.

The lesson is broader than the individual project:

> **Prove the risky thing first. Choose platforms from requirements, not habit. Build only after the evidence justifies building.**

---

**FIOS Classification:** Operating System / Project Delivery / Preflight & Harness  
**Recommended canonical filename:** `FIOS-Project-Preflight-Delivery-Framework.md`
