# Trisphere Enterprise Agent — Model Router

## Purpose

The Model Router decouples the enterprise agent from any single AI provider.

Trisphere should own the routing contract. Providers supply implementations.

---

## Goals

- local-first options;
- provider independence;
- privacy-aware routing;
- cost-aware routing;
- health-aware failover;
- task-quality matching;
- company policy enforcement.

---

## Routing Inputs

A routing decision may consider:

- organization policy;
- task type;
- data sensitivity;
- model capability;
- maximum cost;
- latency requirement;
- local availability;
- provider availability;
- context size;
- modality;
- tool-use requirement;
- residency requirement.

---

## Example

```text
Request
  ↓
Contains confidential payroll data?
  ├── YES → local-approved model only
  └── NO
       ↓
Needs complex reasoning?
       ├── YES → premium cloud model
       └── NO → economical model
```

---

## Canonical Provider Contract

A provider adapter should expose standardized operations such as:

- completion;
- structured output;
- tool/capability planning;
- vision;
- embeddings;
- transcription;
- speech generation.

The exact contract may vary by runtime language but should remain provider-neutral.

---

## Candidate Adapter Categories

```text
Local
  ├── Ollama-compatible
  ├── llama.cpp-compatible
  └── future local engines

Cloud
  ├── Anthropic
  ├── OpenAI
  ├── Gemini
  ├── Groq
  └── future providers
```

This document does not approve any specific provider.

---

## Data Classification

Recommended data classes:

- PUBLIC
- INTERNAL
- CONFIDENTIAL
- RESTRICTED

Provider policies can restrict which classes may leave local infrastructure.

---

## Cost Controls

Track:

- request count;
- input tokens;
- output tokens;
- estimated cost;
- actual cost;
- model;
- provider;
- organization;
- agent;
- task.

Policies may cap:

- per request;
- per day;
- per user;
- per agent;
- per organization.

---

## Fallback

Fallback should be explicit.

Example:

```text
Preferred: local model
Fallback 1: approved cloud model
Fallback 2: lower-capability model
Final: fail closed / ask user
```

Do not silently move confidential tasks to a cloud model because the local provider is unavailable.

---

## Quality Routing

Examples:

- extraction → economical structured-output model;
- high-stakes reasoning → stronger model;
- simple classification → local/small model;
- vision → vision-capable provider;
- speech → dedicated speech provider.

---

## Audit

Model calls should emit enough metadata to reconstruct:

- provider;
- model;
- policy;
- reason for routing;
- data classification;
- cost;
- latency;
- failure/fallback.

Do not log confidential prompts indiscriminately.
