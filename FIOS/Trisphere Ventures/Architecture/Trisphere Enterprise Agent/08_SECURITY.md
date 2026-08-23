# Trisphere Enterprise Agent — Security

## Security Position

An enterprise agent is a privileged automation system.

The threat model must assume:

- prompts can be malicious;
- external content can contain instructions;
- plugins can be unsafe;
- providers can fail;
- credentials can leak;
- agents can misunderstand intent;
- users can over-delegate;
- device automation can cause real damage.

---

## Core Security Principles

1. Least privilege.
2. Explicit organization scope.
3. Separate human and machine identity.
4. Deny by default for sensitive capabilities.
5. No direct model-to-side-effect path.
6. Secret isolation.
7. Structured audit.
8. Verification after mutation.
9. Strong approval for critical actions.
10. Provider and plugin boundaries are trust boundaries.

---

## Threat Surfaces

### Prompt / content injection
Untrusted email, documents, websites, and messages may attempt to manipulate the agent.

### Capability abuse
An otherwise valid capability may be invoked with dangerous parameters.

### Credential theft
Integrations and providers require secrets.

### Plugin compromise
Third-party code may attempt unauthorized access.

### Cross-tenant access
Organization isolation failures are critical.

### Desktop automation
UI control can bypass application-level permission models.

### Model data leakage
Sensitive prompts may leave company infrastructure.

### Audit tampering
An attacker may attempt to erase evidence.

---

## Security Architecture

```text
Untrusted Input
      ↓
Context Classification
      ↓
Agent Planning
      ↓
Capability Validation
      ↓
Policy Enforcement
      ↓
Approval / Step-up
      ↓
Execution Sandbox / Adapter
      ↓
Verification
      ↓
Immutable-ish Audit Evidence
```

---

## Prompt Injection Principle

Retrieved content is data, not authority.

The assistant must not treat instructions embedded in:

- emails;
- PDFs;
- websites;
- documents;
- CRM notes

as higher priority than platform policy.

---

## Secret Management

Secrets should not be:

- placed in prompts unnecessarily;
- exposed to plugins;
- written to logs;
- stored in plaintext config files;
- passed to child processes unless required.

A future Trisphere Node may host a local credential broker.

---

## Plugin Security

A plugin being installed or signed does not mean it is authorized to perform all actions.

Plugin permissions should be explicit and capability-scoped.

---

## Data Classification

Recommended baseline:

```text
PUBLIC
INTERNAL
CONFIDENTIAL
RESTRICTED
```

Policy may determine:

- allowed models;
- storage location;
- retention;
- export;
- logging;
- human review.

---

## Tenant Isolation

Organization isolation should be enforced in more than one layer where practical:

- identity;
- API;
- database;
- capability request;
- audit.

Do not rely on prompt text such as "only access company A."

---

## Critical Actions

Examples requiring strongest controls:

- payments;
- credential changes;
- access grants;
- bulk deletion;
- destructive shell commands;
- regulated-data release.

For some capabilities, the correct design may be that the agent can prepare but never execute.

---

## Security Evidence

Security-sensitive changes require:

- tests;
- threat analysis;
- failure-path testing;
- audit validation;
- rollback plan.

---

## Upstream Code

MIT licensing permits reuse, but licensing does not establish security quality.

All inherited Open.Jarvis code should be audited before being trusted as an enterprise security boundary.
