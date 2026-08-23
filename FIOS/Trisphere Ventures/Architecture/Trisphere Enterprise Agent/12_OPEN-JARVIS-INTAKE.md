# Open.Jarvis Intake — Trisphere Enterprise Agent

## Source

Repository: `https://github.com/dmrr35/Open.Jarvis`

## Role

Open.Jarvis is considered an upstream engineering seed and architectural reference for selected Trisphere Enterprise Agent capabilities.

It is not the Trisphere product identity.

---

## License Position

The reviewed repository presents an MIT license.

Implication:

Trisphere may use, copy, modify, merge, publish, distribute, sublicense, and sell MIT-covered software subject to the license terms, including preservation of the required copyright and permission notice.

Trisphere must preserve provenance for retained upstream code.

This document is not legal advice; any commercial release should preserve applicable notices and receive normal release review.

---

## Why This Repository Is Valuable

Useful concepts include:

- Windows-first local assistant runtime;
- local-first provider posture;
- optional cloud providers;
- runtime state model;
- desktop automation;
- permission-aware commands;
- plugin system;
- privacy-aware memory controls;
- diagnostics;
- voice pipeline;
- threat-model thinking.

---

## What Trisphere Should Not Do

Do not:

- rename Open.Jarvis and ship it unchanged;
- retain Jarvis branding;
- assume upstream security is enterprise-grade;
- expose unrestricted desktop/process execution;
- preserve provider-specific assumptions unnecessarily;
- treat simple assistant memory as the final Trisphere Knowledge Layer.

---

## Transformation Categories

Each upstream module should be classified:

### RETAIN
Useful, sufficiently clean, permissively licensed.

### RETAIN + HARDEN
Useful but needs security, tests, or governance.

### GENERALIZE
Useful concept but too Open.Jarvis-specific.

### REPLACE
Conflicts with Trisphere architecture.

### REMOVE
Not strategically useful.

---

## Expected Trisphere Additions

Open.Jarvis alone does not provide the full enterprise architecture.

Trisphere must add or substantially expand:

- organization model;
- multi-user identity;
- agent identity;
- delegation;
- capability contracts;
- deterministic policy engine;
- structured approvals;
- audit layer;
- central Agent Ops;
- company knowledge;
- cost governance;
- provider policy;
- vertical packs;
- deployment lifecycle;
- Trisphere Node integration.

---

## Proposed Source Intake Procedure

Claude Code should:

1. clone the exact upstream repository into a dedicated Trisphere reference area;
2. record commit SHA;
3. inventory licenses and third-party assets;
4. map package/module boundaries;
5. identify security-sensitive modules;
6. run existing tests;
7. run static analysis/type checks where available;
8. produce retain/replace/generalize decisions;
9. identify inherited code that would remain MIT-noticed;
10. propose the Trisphere target architecture before major refactoring.

---

## Provenance Rule

If a file contains substantial retained upstream code, preserve the required upstream notice and record provenance.

Independent Trisphere modules should carry Trisphere ownership notices appropriate to internal policy.

Do not intentionally blur the boundary.

---

## Target Outcome

The target is not:

> Open.Jarvis with a Trisphere logo.

The target is:

> A Trisphere enterprise platform that may use selected MIT-licensed Open.Jarvis infrastructure as one engineering foundation while replacing its product model with Trisphere identity, governance, capabilities, knowledge, audit, operations, and deployment architecture.

---

## Current Intake Decision

**License class:** GREEN / MIT  
**Strategic value:** High  
**Recommended action:** Source-level intake + fork/transformation design  
**Production approval:** Not granted by this document  
**Next gate:** Complete source audit and transformation plan
