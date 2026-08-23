# Trisphere Enterprise Agent — Vertical Editions

## Principle

Vertical editions should extend one shared platform rather than fork it.

---

## Edition Formula

```text
Enterprise Agent Core
       +
Vertical Capabilities
       +
Vertical Policies
       +
Vertical Knowledge
       +
Vertical UX / Terminology
```

---

## Accounting / Bookkeeping Edition

Potential capabilities:

- client briefing;
- invoice lookup;
- transaction review;
- categorization proposals;
- reconciliation support;
- document collection;
- missing-document follow-up;
- financial report retrieval.

High-risk accounting changes should remain governed and deterministic.

---

## Tax Office Edition

Potential capabilities:

- client intake;
- document checklist;
- appointment preparation;
- tax-research support;
- workflow status;
- secure follow-up;
- missing-document detection.

Tax conclusions must rely on authoritative sources and professional review where required.

---

## Contractor Edition

Potential capabilities:

- lead intake;
- estimate preparation;
- job scheduling;
- change-order drafting;
- material tracking;
- customer communication;
- project documentation.

---

## Professional Services Edition

Potential capabilities:

- meeting prep;
- CRM;
- email;
- calendar;
- documents;
- research;
- proposals;
- client follow-up.

---

## Retail / Distribution Edition

Potential capabilities:

- customer lookup;
- inventory context;
- purchase/order status;
- vendor communications;
- document generation;
- operational alerts.

---

## Legal-Office Support Edition

Potential capabilities:

- matter retrieval;
- document organization;
- calendar/deadline support;
- intake;
- research assistance;
- draft preparation.

The agent must not imply unauthorized legal practice.

---

## Vertical Policy Packs

Each vertical should include default risk classifications.

Example:

```text
Accounting:
  read financial report → L0
  propose journal entry → L2
  post journal entry → L4
  initiate payment → L5
```

---

## Vertical Knowledge Packs

May include:

- terminology;
- workflow templates;
- SOP structures;
- forms;
- integration mappings;
- compliance references.

Do not bundle copyrighted or restricted materials without rights.

---

## Product Strategy

A vertical edition should be deployable without modifying the core runtime.

If an edition requires core changes, determine whether the missing functionality is truly generic before adding it to the platform.
