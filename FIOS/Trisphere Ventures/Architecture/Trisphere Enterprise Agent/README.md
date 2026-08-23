# Trisphere Enterprise Agent — FIOS Pack

This folder contains the initial FIOS architecture pack for **Trisphere Enterprise Agent**.

## Recommended FIOS location

```text
FIOS/
└── Trisphere Ventures/
    └── Architecture/
        └── Trisphere Enterprise Agent/
```

## Files

1. `00_TRISPHERE-ENTERPRISE-AGENT.md` — canonical master definition
2. `01_ARCHITECTURE.md` — logical architecture
3. `02_GOVERNANCE.md` — policy, approvals, risk
4. `03_IDENTITY-AND-PERMISSIONS.md` — humans, agents, organizations, delegation
5. `04_CAPABILITY-RUNTIME.md` — capability contracts and execution
6. `05_MODEL-ROUTER.md` — local/cloud model routing
7. `06_COMPANY-KNOWLEDGE.md` — company knowledge and provenance
8. `07_DEVICE-RUNTIME.md` — desktop/device control
9. `08_SECURITY.md` — security and threat boundaries
10. `09_DEPLOYMENT.md` — deployment profiles and onboarding
11. `10_TRISPHERE-NODE.md` — local appliance/runtime concept
12. `11_VERTICAL-EDITIONS.md` — vertical product strategy
13. `12_OPEN-JARVIS-INTAKE.md` — upstream provenance and transformation strategy
14. `13_ROADMAP.md` — phased roadmap
15. `14_ARCHITECTURE-DECISIONS.md` — initial architecture decisions
16. `15_PROGRESSIVE-CONTEXT-AND-CACHE-FABRIC.md` — future context layer; OpenViking study (AGPL-3.0, reference only)
17. `16_LOCAL-REFLEX-TRIAGE.md` — future local triage tier; Needle study (Apache-2.0, assess)
18. `17_ARCHITECTURE-SYNC-WORKFLOW.md` — how completed architecture reaches its durable homes

## Canonical Rule

The master file governs the product concept. Supporting documents may evolve independently but should not contradict accepted architecture decisions without an explicit ADR update.

## Implementation Boundary

FIOS stores durable architecture, product intent, decisions, provenance, and reusable knowledge.

Source code belongs in the Trisphere development repositories, not in this FIOS folder.
