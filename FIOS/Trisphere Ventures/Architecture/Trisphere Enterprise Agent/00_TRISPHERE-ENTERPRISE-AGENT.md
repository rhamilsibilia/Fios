# Trisphere Enterprise Agent

**Status:** Product Platform Concept / Architecture Definition  
**Owner:** Trisphere Ventures LLC  
**Canonical role:** FIOS source of truth for the Trisphere Enterprise Agent platform  
**Audience:** Trisphere founders, architects, developers, AI coding agents, future product teams  
**Upstream engineering seed:** `dmrr35/Open.Jarvis` (MIT-licensed; see `12_OPEN-JARVIS-INTAKE.md`)  

---

## 1. Executive Definition

**Trisphere Enterprise Agent** is a Trisphere Ventures-owned enterprise AI assistant platform designed to be installed inside a business as a governed, organization-aware, multimodal AI employee.

It is not a generic chatbot.

It is intended to combine:

- conversational AI;
- company context;
- approved company knowledge;
- local and cloud model routing;
- desktop and device interaction;
- business-system integrations;
- permission-aware capabilities;
- action governance;
- human approvals;
- machine identity;
- auditable execution;
- operational health;
- local-first deployment options;
- reusable vertical capability packs.

The long-term objective is to let a business deploy an assistant that can understand the organization, interact with approved systems, complete bounded operational work, and escalate sensitive actions to humans without surrendering control of business data, permissions, or auditability.

---

## 2. Product Thesis

Most AI assistants are optimized around conversation.

Trisphere Enterprise Agent is optimized around **controlled work execution inside an organization**.

The platform should answer five questions before acting:

1. **Who is asking?**
2. **Which organization and scope are active?**
3. **What capability is being requested?**
4. **Is the agent allowed to perform it?**
5. **What evidence, approval, and audit record are required?**

This changes the architecture from:

```text
User
  ↓
LLM
  ↓
Tool
```

to:

```text
User / Event / Agent
        │
        ▼
   Identity Context
        │
        ▼
    Agent Runtime
        │
        ▼
  Capability Planner
        │
        ▼
 Governance / Policy
        │
        ▼
 Approval if required
        │
        ▼
  Controlled Execution
        │
        ▼
 Verification + Evidence
        │
        ▼
 Audit + Agent Operations
```

---

## 3. Strategic Ownership

Trisphere Ventures owns the reusable enterprise-assistant architecture developed around this product, including independently developed:

- governance models;
- capability contracts;
- identity integration;
- organization model;
- provider abstractions;
- model routing;
- company knowledge architecture;
- policy engine;
- audit contracts;
- agent operations integration;
- deployment model;
- vertical capability packs;
- Trisphere Node architecture;
- integration interfaces;
- original Trisphere UI/UX;
- original Trisphere brand and product experience.

Where MIT-licensed upstream Open.Jarvis source is retained or adapted, applicable copyright and MIT license notices must be preserved. Trisphere does not claim original authorship of upstream code it did not create.

The strategic objective is therefore not to erase provenance. It is to use permissively licensed infrastructure responsibly while building a substantially broader Trisphere-owned enterprise platform around it.

---

## 4. Relationship to Trisphere Platform

```text
                     TRISPHERE PLATFORM

              ┌────────────────────────┐
              │   Identity Core        │
              └───────────┬────────────┘
                          │
              ┌────────────────────────┐
              │   Audit Layer          │
              └───────────┬────────────┘
                          │
              ┌────────────────────────┐
              │   Agent Operations     │
              └───────────┬────────────┘
                          │
              ┌────────────────────────┐
              │   Knowledge Layer      │
              └───────────┬────────────┘
                          │
                          ▼
              ┌────────────────────────┐
              │ TRISPHERE ENTERPRISE   │
              │        AGENT           │
              └───────────┬────────────┘
                          │
          ┌───────────────┼────────────────┐
          ▼               ▼                ▼
      Company A        Company B        Company C
     Assistant         Assistant         Assistant
```

The enterprise agent should consume shared Trisphere capabilities rather than rebuilding them privately inside one product.

---

## 5. Core Architecture

```text
                    TRISPHERE ENTERPRISE AGENT
                              │
                              ▼
                         Agent Core
                              │
          ┌───────────────────┼───────────────────┐
          │                   │                   │
          ▼                   ▼                   ▼
     Model Router       Company Context      Memory Layer
          │                                       │
          └───────────────────┬───────────────────┘
                              ▼
                      Capability Runtime
                              │
                      Governance Engine
                              │
         ┌────────────────────┼────────────────────┐
         ▼                    ▼                    ▼
       Email                 CRM               Documents
     Calendar            Accounting              Files
       Phone               Browser              Devices
         │                    │                    │
         └────────────────────┼────────────────────┘
                              ▼
                       Action Runtime
                              │
                 ┌────────────┼────────────┐
                 ▼            ▼            ▼
               Audit       Agent Ops     Evidence
```

---

## 6. Core Platform Components

### 6.1 Agent Core
Responsible for:

- receiving requests;
- maintaining task context;
- planning bounded actions;
- coordinating capabilities;
- handling interruptions;
- waiting for approval;
- verifying results;
- reporting final outcomes.

The Agent Core must not directly own provider-specific integrations.

### 6.2 Model Router
Selects the appropriate intelligence provider based on:

- privacy;
- task type;
- capability;
- latency;
- cost;
- model quality;
- organization policy;
- local availability;
- provider health.

See `05_MODEL-ROUTER.md`.

### 6.3 Capability Runtime
Provides standardized contracts for business actions.

Examples:

- `email.read`
- `email.send`
- `calendar.read`
- `calendar.create`
- `crm.customer.read`
- `crm.customer.update`
- `document.generate`
- `accounting.invoice.read`
- `accounting.transaction.propose`
- `device.desktop.screenshot`

See `04_CAPABILITY-RUNTIME.md`.

### 6.4 Governance Engine
Evaluates whether an action:

- is permitted;
- requires human approval;
- is restricted;
- needs step-up authentication;
- exceeds financial or operational limits;
- requires stronger evidence;
- is forbidden entirely.

See `02_GOVERNANCE.md`.

### 6.5 Company Knowledge Layer
Provides controlled access to:

- policies;
- SOPs;
- client information;
- internal documents;
- product knowledge;
- approved reference materials;
- prior decisions;
- business data.

See `06_COMPANY-KNOWLEDGE.md`.

### 6.6 Device Runtime
Provides controlled interaction with:

- desktop OS;
- browser;
- local files;
- printers/scanners;
- local network services;
- smart devices;
- future Trisphere Node services.

See `07_DEVICE-RUNTIME.md`.

### 6.7 Identity & Permissions
Separates:

- human identity;
- agent identity;
- service accounts;
- integrations;
- API clients;
- organization membership;
- roles;
- permissions;
- delegation.

See `03_IDENTITY-AND-PERMISSIONS.md`.

### 6.8 Audit & Agent Operations
Every meaningful action should be observable and attributable.

The agent must produce structured events covering:

- actor;
- principal;
- organization;
- capability;
- requested action;
- policy decision;
- approval;
- execution;
- verification;
- cost;
- provider;
- evidence;
- result;
- error state.

---

## 7. Enterprise Assistant Behavior

The assistant should support:

### Informational work
- summarize;
- research;
- retrieve;
- compare;
- explain;
- prepare briefs;
- answer company questions.

### Administrative work
- draft emails;
- draft documents;
- prepare reports;
- update approved records;
- schedule meetings;
- assemble customer context.

### Operational work
- invoke CRM workflows;
- generate documents;
- initiate approved automations;
- execute desktop tasks;
- interact with company software.

### Governed high-risk work
- send external communications;
- modify financial records;
- delete data;
- change permissions;
- execute system commands;
- trigger payments;
- perform legal/compliance-sensitive operations.

High-risk work must never rely only on model confidence.

---

## 8. Risk-Based Action Model

Suggested default levels:

| Level | Meaning | Example | Default |
|---|---|---|---|
| L0 | Read-only | Search knowledge | Auto |
| L1 | Low-risk creation | Draft email | Auto |
| L2 | Controlled mutation | Create CRM note | Policy |
| L3 | External effect | Send email | Approval/policy |
| L4 | Sensitive business action | Modify financial data | Strong approval |
| L5 | Critical | Payment, permission escalation, destructive system action | Hard gate |

Risk level is only one input. Organization policy may increase or decrease handling requirements within allowed boundaries.

---

## 9. Runtime State Model

Recommended canonical states:

```text
INITIALIZING
READY
RECEIVING
PLANNING
WAITING_APPROVAL
EXECUTING
VERIFYING
COMPLETED
FAILED
DEGRADED
OFFLINE
```

Transitions should be emitted as structured operational events.

---

## 10. Local-First + Cloud-Hybrid Philosophy

Trisphere Enterprise Agent should support multiple deployment postures.

### Cloud-forward
Best for lightweight deployments and broad SaaS integrations.

### Local-first
Sensitive context and selected inference remain on company-controlled hardware.

### Hybrid
Local policy, memory, retrieval, and device control combined with cloud models where authorized.

The architecture must not assume that every organization is comfortable sending all business data to cloud models.

---

## 11. Trisphere Node

A future local appliance may provide:

- local model execution;
- local knowledge index;
- policy engine;
- secrets broker;
- device runtime;
- local integrations;
- event queue;
- offline operation;
- LAN discovery;
- encrypted cache.

See `10_TRISPHERE-NODE.md`.

---

## 12. Enterprise Deployment Model

A deployed assistant should have an explicit company configuration:

```text
Organization
  ├── Identity policy
  ├── Approved models
  ├── Approved capabilities
  ├── Data boundaries
  ├── Risk thresholds
  ├── Approval rules
  ├── Knowledge sources
  ├── Integrations
  ├── Local devices
  ├── Audit retention
  └── Cost policy
```

No enterprise installation should rely on hidden hard-coded assumptions.

---

## 13. Vertical Editions

The core should support vertical editions without forking the platform.

Examples:

- Accounting / bookkeeping;
- professional services;
- contractor;
- tax office;
- retail;
- property services;
- legal-office support;
- hospitality;
- home-services operations.

Vertical editions should be built as:

```text
Core Runtime
   +
Vertical Policy Pack
   +
Vertical Capabilities
   +
Vertical Knowledge
   +
Vertical UX
```

See `11_VERTICAL-EDITIONS.md`.

---

## 14. Upstream Open.Jarvis Relationship

Open.Jarvis provides a useful MIT-licensed engineering seed for selected areas such as:

- local-first assistant structure;
- provider routing;
- desktop automation;
- permission-aware actions;
- plugins;
- runtime states;
- diagnostics;
- local voice;
- privacy-aware memory controls.

Trisphere should not merely reskin it.

The transformation target is:

```text
Open.Jarvis baseline
       ↓
Source-level audit
       ↓
Retain / Replace / Generalize
       ↓
Trisphere governance
       ↓
Trisphere identity
       ↓
Trisphere capability contracts
       ↓
Trisphere knowledge
       ↓
Trisphere audit + Agent Ops
       ↓
Enterprise deployment model
       ↓
Trisphere Enterprise Agent
```

See `12_OPEN-JARVIS-INTAKE.md`.

---

## 15. Non-Negotiable Principles

1. **Identity before authority.**
2. **Capability before execution.**
3. **Policy before side effects.**
4. **Evidence before completion.**
5. **Human approval for critical actions.**
6. **Agent identity is never silently collapsed into human identity.**
7. **Model providers are replaceable.**
8. **The deterministic business system remains authoritative.**
9. **Local execution must have explicit permissions.**
10. **Every meaningful action must be auditable.**
11. **Company data remains company-scoped.**
12. **Trisphere-owned abstractions should prevent unnecessary vendor lock-in.**
13. **The assistant may recommend beyond its authority but may not execute beyond it.**
14. **Security and cost are runtime concerns, not post-build concerns.**
15. **Upstream license obligations must be preserved.**

---

## 16. Product Success Definition

The product succeeds when a company can deploy an assistant that:

- understands the organization;
- accesses only authorized context;
- performs useful work;
- distinguishes recommendation from execution;
- asks for approval only when necessary;
- provides evidence of what it did;
- can be audited;
- can operate with local-first options;
- can add or remove capabilities without rewriting the core;
- can change AI providers without redesigning the product;
- can evolve across multiple industries.

---

## 17. Related FIOS Documents

- `01_ARCHITECTURE.md`
- `02_GOVERNANCE.md`
- `03_IDENTITY-AND-PERMISSIONS.md`
- `04_CAPABILITY-RUNTIME.md`
- `05_MODEL-ROUTER.md`
- `06_COMPANY-KNOWLEDGE.md`
- `07_DEVICE-RUNTIME.md`
- `08_SECURITY.md`
- `09_DEPLOYMENT.md`
- `10_TRISPHERE-NODE.md`
- `11_VERTICAL-EDITIONS.md`
- `12_OPEN-JARVIS-INTAKE.md`
- `13_ROADMAP.md`
- `14_ARCHITECTURE-DECISIONS.md`

---

## 18. Current Status

**Architecture status:** Conceptual foundation established.  
**Source strategy:** Open.Jarvis eligible for MIT-based fork/adaptation subject to preservation of required notices.  
**Implementation status:** Not defined by this document.  
**Next gate:** Source-level Claude Code intake, dependency/license inventory, architecture comparison, and fork-to-Trisphere transformation plan.

This FIOS document governs the product concept even if implementation details evolve.
