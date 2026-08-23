# Trisphere Enterprise Agent — Device Runtime

## Purpose

The Device Runtime enables controlled interaction with local computers and approved devices.

Desktop control is powerful and high-risk. It must be governed like an enterprise capability, not treated as unrestricted automation.

---

## Device Adapter Model

```text
Action Runtime
      ↓
Device Contract
      ↓
┌─────────────┬─────────────┬─────────────┐
│ Windows     │ Linux       │ Trisphere   │
│ Adapter     │ Adapter     │ Node        │
└─────────────┴─────────────┴─────────────┘
```

---

## Capability Examples

- application launch;
- browser open;
- screenshot;
- clipboard read/write;
- keyboard input;
- mouse interaction;
- file read;
- file write;
- print;
- scan;
- local network command;
- smart-device control.

---

## Permission Boundaries

Examples:

```text
device.application.launch
device.screenshot
device.clipboard.read
device.keyboard.type
filesystem.read
filesystem.write
process.execute
device.smart.control
```

Permissions should be narrower than a generic `desktop_access`.

---

## Filesystem Safety

File capabilities should support:

- allowed roots;
- denied roots;
- extension filters;
- size limits;
- read/write separation;
- path normalization;
- symlink/junction handling;
- traversal protection.

---

## Process Execution

Process execution should default to disabled or strongly controlled.

Where allowed:

- avoid shell expansion by default;
- use allowlists where practical;
- set timeouts;
- sanitize environment;
- capture output;
- record command metadata;
- prevent credential leakage.

---

## Human Visibility

For sensitive desktop actions, the user should be able to see or review what the agent intends to do.

Possible patterns:

- preview;
- ghost cursor;
- action queue;
- approval card;
- execution transcript.

---

## Browser Automation

Browser interaction should prefer structured browser/API integrations over fragile coordinate automation when available.

UI automation is a fallback, not the first choice.

---

## Device Identity

Every managed device should have:

- device ID;
- organization;
- owner;
- platform;
- capabilities;
- trust level;
- last health check;
- agent access policy.

---

## Offline Operation

Local capabilities may continue when cloud models are unavailable if policy allows and required local intelligence exists.

---

## Home/Facility Extension

The same contract can evolve toward:

- lighting;
- climate;
- access control;
- sensors;
- smart appliances;
- office devices.

This creates a conceptual bridge to Trisphere's smart-environment architecture without coupling the enterprise agent to any one home-automation platform.
