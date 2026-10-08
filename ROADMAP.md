# Project Roadmap

The roadmap is evidence-driven. A phase is complete only when its acceptance criteria are met.

## Phase 0 — Governance, Environment, and Architecture

**Goal:** establish a safe, realistic, low-cost foundation.

### Current work
- host inventory;
- Azure access and cost constraints;
- lab architecture;
- network exposure decisions;
- evidence/status conventions.

### Acceptance criteria
- architecture v2 documented;
- realistic resource allocation documented;
- cost guardrails documented;
- networking approach documented;
- current implementation state recorded;
- next deployment step is clear.

---

## Phase 1 — SOC Infrastructure

**Goal:** deploy Wazuh centrally and establish endpoint-to-SIEM data flow.

### Scope
- Azure Ubuntu VM;
- Wazuh server/indexer/dashboard;
- network restrictions;
- auto-shutdown/deallocation workflow;
- Windows Wazuh agent;
- Sysmon.

### Acceptance criteria
- Wazuh components operational;
- endpoint enrolled;
- expected Windows events searchable;
- data flow can be explained end-to-end;
- setup is documented reproducibly.

---

## Phase 2 — Telemetry Engineering

**Goal:** understand what evidence exists before writing detections.

### Windows telemetry
- process creation;
- authentication;
- PowerShell;
- user/group changes;
- scheduled tasks;
- services;
- network activity.

### Linux telemetry
- SSH/authentication;
- sudo;
- processes;
- services;
- relevant file/system activity.

### Acceptance criteria
- telemetry matrix records expected vs observed data;
- important fields and sources are documented;
- missing telemetry is explicitly identified.

---

## Phase 3 — Controlled Adversary Simulation

**Goal:** generate known, safe behavior with ground truth.

Initial target scenarios:
1. repeated authentication failures;
2. suspicious PowerShell activity;
3. privileged user/group modification;
4. SSH password guessing in an isolated target;
5. suspicious process + network sequence.

Each scenario must document preconditions, actions, expected telemetry, cleanup, ATT&CK hypothesis, and ground truth.

---

## Phase 4 — Detection Engineering

**Goal:** turn observed telemetry into tested detections.

### Scope
- Wazuh rules;
- Sigma where appropriate;
- positive tests;
- negative tests;
- tuning;
- ATT&CK mapping;
- FP/FN analysis.

### Acceptance criteria
Each completed detection includes:
- detection hypothesis;
- required telemetry;
- implementation;
- observed positive result;
- observed negative result;
- false-positive considerations;
- limitations;
- evidence.

---

## Phase 5 — SOC Investigations

**Goal:** complete evidence-based investigations from lab alerts.

Each investigation must separate:
- facts;
- inferences;
- unknowns;
- recommendations.

### Target for v1.0
At least 5 documented investigations with:
- alert context;
- evidence;
- timeline;
- entities/process/network context;
- alternative explanations;
- ATT&CK mapping when justified;
- verdict and confidence;
- escalation decision;
- lessons learned.

---

## Phase 6 — AI SOC Copilot MVP

**Goal:** build a small software system that assists triage without inventing evidence.

### Initial architecture
```text
alert / exported event
        ↓
parser
        ↓
normalized schema
        ↓
related-event correlation
        ↓
context builder
        ↓
LLM
        ↓
structured analyst output
```

### Principles
- Python-first;
- CLI/backend before UI;
- typed/validated schemas;
- provider abstraction;
- read-only operation;
- evidence traceability;
- explicit facts vs inferences.

---

## Phase 7 — Retrieval and Analyst Knowledge

**Goal:** add controlled retrieval over analyst knowledge.

Potential sources:
- SOC playbooks;
- project investigation methodology;
- detection documentation;
- selected MITRE ATT&CK metadata;
- validated lessons learned.

Retrieved documentation is context, not incident evidence.

---

## Phase 8 — AI Evaluation

**Goal:** measure whether the AI layer helps and where it fails.

### Initial metrics
- verdict accuracy;
- false positives;
- false negatives;
- evidence grounding;
- unsupported-claim rate;
- hallucinated-evidence rate;
- MITRE mapping accuracy;
- escalation precision/recall;
- schema validity.

### Target for v1.0
At least 15 cases with independently defined ground truth.

---

## Phase 9 — AI Security Testing

**Goal:** test the AI analyst against untrusted telemetry.

Scenarios may include:
- instruction-like filenames;
- malicious usernames;
- command-line prompt injection strings;
- poisoned knowledge documents;
- contradictory evidence;
- missing evidence;
- ambiguous incidents.

The model must not treat instructions embedded in logs as trusted instructions.

---

## Phase 10 — Microsoft Sentinel and KQL

**Goal:** reproduce selected validated cases using Microsoft's SIEM stack.

### Scope
- controlled Log Analytics ingestion;
- KQL;
- analytics/custom detections;
- alerts/incidents;
- comparison with Wazuh.

Compare implementation, telemetry, workflow, portability, operational complexity, and observed cost.

---

## Phase 11 — Optional Research Extensions

Only after the core workflow is complete:
- ML anomaly detection;
- application telemetry from `supportdesk-api`;
- larger evaluation dataset;
- automated enrichment;
- CI/CD for detections;
- infrastructure as code.

These are not required for the first complete release.

---

# Definition of Done — v1.0

v1.0 requires, at minimum:

- Wazuh operational;
- validated Windows telemetry;
- validated Linux telemetry;
- >= 5 controlled scenarios;
- >= 5 validated detections;
- >= 5 documented investigations;
- Sigma rules where appropriate;
- operational AI Copilot MVP;
- >= 15 ground-truth evaluation cases;
- measured hallucinations / grounding / FP / FN;
- prompt-injection testing;
- documented architecture, costs, limitations, and reproduction steps.

Anything not validated remains explicitly marked as planned or experimental.
