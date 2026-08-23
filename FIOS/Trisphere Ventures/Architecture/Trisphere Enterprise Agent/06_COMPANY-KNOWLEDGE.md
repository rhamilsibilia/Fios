# Trisphere Enterprise Agent — Company Knowledge

## Purpose

The assistant should know the company without treating every accessible file as trusted knowledge.

Company Knowledge must be scoped, governed, attributable, and updateable.

---

## Knowledge Sources

Potential sources include:

- policies;
- SOPs;
- product documentation;
- CRM records;
- approved emails;
- shared-drive documents;
- internal databases;
- websites;
- manuals;
- contracts;
- prior decisions;
- structured FIOS-style notes.

---

## Knowledge Layers

```text
Source Data
    ↓
Ingestion
    ↓
Normalization
    ↓
Classification
    ↓
Index / Retrieval
    ↓
Evidence
    ↓
Agent Context
```

---

## Source Trust

Every knowledge object should retain provenance.

Suggested metadata:

```yaml
source_id:
source_type:
organization_id:
owner:
created_at:
updated_at:
classification:
authority_level:
effective_date:
expires_at:
citation_pointer:
```

---

## Authority Hierarchy

Not all sources are equally authoritative.

Example:

```text
Approved company policy
        >
Signed contract
        >
Current CRM record
        >
Internal note
        >
Unverified conversation
```

The exact hierarchy should be configurable.

---

## Retrieval

Retrieval should consider:

- organization;
- user scope;
- capability;
- source permissions;
- freshness;
- authority;
- relevance;
- confidentiality.

---

## Memory vs Knowledge

Separate:

### Short-term task context
Temporary conversation/task state.

### User preferences
Stable preferences where appropriate.

### Organization knowledge
Company-owned authoritative information.

### Agent memory
Operational memory about prior work.

### Audit history
Immutable evidence of actions.

These should not collapse into one database concept.

---

## Write Governance

The agent should not silently convert generated text into company truth.

Writes should be categorized:

```text
PROPOSAL
DRAFT
APPROVED_KNOWLEDGE
SYSTEM_RECORD
AUDIT_RECORD
```

Only approved flows should elevate generated content into authoritative company knowledge.

---

## Privacy

Knowledge access must obey:

- organization boundaries;
- role permissions;
- source permissions;
- sensitivity classifications;
- retention policies.

---

## Local Knowledge Option

The Trisphere Node may host:

- local index;
- embeddings;
- cached documents;
- retrieval service;
- document parsing.

This can reduce exposure of sensitive company data.

---

## Knowledge Success Criteria

The assistant should be able to answer with:

- the information;
- the source;
- the freshness;
- the confidence;
- the applicable organization context.

Where evidence is insufficient, it should say so rather than inventing organizational facts.
