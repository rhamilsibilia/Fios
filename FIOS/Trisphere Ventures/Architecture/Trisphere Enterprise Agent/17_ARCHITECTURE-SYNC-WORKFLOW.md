# Architecture Preservation & Auto-Sync Workflow

**Status:** Standing process · Binding on all Lena architecture work
**Owner:** Trisphere Ventures LLC
**Canonical role:** How completed architecture research reaches its durable homes

---

## Why this exists

A completed architecture study is **not complete** if its only durable copy lives in a chat
transcript, an unversioned directory, a scratch file, or one machine with no synchronization path.
**Preservation is part of the work, not an afterthought.**

This record does not replace the existing intake doctrine — it says where the output goes. The
*how to study an external repository* rules remain the Repository Intake Protocol in the Trisphere
Architecture Library and `CLAUDE.md` §4 in the Enterprise Agent repository.

---

## The three canonical homes, and what each is for

**Do not copy the same document into all three.** They serve different purposes; prefer backlinks
between canonical artifacts over divergent duplicates.

| Home | Location | Holds | Versioned |
|---|---|---|---|
| **FIOS vault** | `C:\Users\Rhamil Sibilia\Documents\FIOS` — vault id `081a499744cf886e` | durable knowledge: architectural reasoning, decisions/ADRs, provenance, rejected approaches, product principles, economic hypotheses, lessons, cross-project patterns | **git → `github.com/rhamilsibilia/Fios`** |
| **Enterprise Agent repo** | `C:\Development\Trisphere\Trisphere-Enterprise-Agent` | the engineering contract: interfaces, schemas, contracts, tests, roadmap files, upstream registry, licence notices, reproducible evidence | **git → `github.com/rhamilsibilia/Trisphere-Enterprise-Agent`** |
| **Architecture Library** | `C:\Development\Trisphere\trisphere-architecture` | discoverability: the registry, the Technology Radar, and **pointers** to the canonical detailed records | ⚠️ **not a git repository** — see Known gaps |

**Rule of thumb.** Reasoning → FIOS. Specification → GitHub. Pointer → Library.

---

## The workflow

Run on completion of any material external architecture study, Trisphere architectural decision,
upgrade blueprint, provenance record, or roadmap decision.

```text
 1  DISCOVER PRIOR STUDY      already studied? then analyze the DELTA only
 2  PIN THE SOURCE            exact commit SHA. "latest" is not a version
 3  LICENCE & PROVENANCE      code AND model weights, checked separately
 4  STUDY                     source inspection, not README-only
 5  EXTRACT THE PATTERN       what is worth having, and what is not
 6  COMPARE                   against existing Trisphere architecture
 7  ASSESS IMPACT             intelligence · capability · reliability · ROI
 8  DECIDE                    accept / adapt / independently implement / reject
 9  SAVE TO FIOS              the reasoning and the ADRs
10  UPDATE THE LIBRARY        registry + radar entry, pointing at the canonical records
11  UPDATE GITHUB             the implementation-facing artifact, when it affects implementation
12  COMMIT                    every version-controlled destination
13  PUSH                      where authorized and consistent with project practice
14  VERIFY THE REMOTE         confirm the commit exists on the remote, independently
15  REPORT GAPS               anything not durably synchronized, and where it IS preserved
```

### Preserve on every external source

repository · **pinned commit SHA** · tag/version · date studied · licence (code) · licence (model
weights, separately) · classification 🟢🟡🔴 · dependencies · disposition · **transformation
classification** (RETAIN · RETAIN + HARDEN · GENERALIZE · REPLACE · REMOVE · REFERENCE ONLY).

---

## The duplication gate

Before proposing any new component, architecture, document, framework, interface or study, ask:
**does Trisphere already have this?** Search the Lena repository, this vault, the Architecture
Library, the upstream registry, the roadmap, and prior decisions.

**If it exists: extend or reference it. Do not recreate it.**

This explicitly includes **Open.Jarvis**, which is Lena's architectural ancestry and already studied,
extracted and hardened — see [[12_OPEN-JARVIS-INTAKE]]. It is not a new research target unless a
future upstream version contains a materially new capability worth a delta review.

---

## Access discovery — before declaring anything unavailable

Absence from one shell proves nothing about the organization. Check, in order: canonical project
documentation · local filesystem · this vault · configured git remotes · authenticated CLI state ·
available connectors · existing repository configuration · Architecture Library paths.

**Do not ask a human to reconnect something before the existing paths have actually been checked, do
not invent credentials, and do not create a replacement repository because authentication failed.**

---

## Known gaps

| Gap | State | Effect | Remedy |
|---|---|---|---|
| **Architecture Library is not a git repository** | `C:\Development\Trisphere\trisphere-architecture` has no `.git`, and neither does its parent | Radar and registry entries persist on one machine with **no remote and no history** | Needs a repository created and a remote added. Blocked from this session: the GitHub CLI is not installed, and a replacement repository must not be invented. **Content is not at risk** — every Library entry is a pointer to records held in the two git-backed homes. |
| **GitHub CLI absent** | `gh` is not installed on this machine | Cannot create repositories or manage GitHub resources from a shell | Git push works via Git Credential Manager, so all *existing* repositories stay synchronizable. Install `gh` only if repository creation is needed. |
| **Vault carried uncommitted content** | Pre-existing untracked directories at the time this workflow was established | Content existed only on local disk | Being brought under version control incrementally; see the vault history. |
