# AI-Augmented Security Operations Lab

A hands-on cybersecurity project for building, testing, and evaluating an end-to-end security operations workflow with SIEM/XDR telemetry, detection engineering, incident investigation, and an evidence-grounded AI analyst assistant.

> This is a controlled personal lab. It is not production SOC experience, and planned capabilities are not presented as implemented until they are validated.

## Project Objective

Build a reproducible lab that connects:

```text
Controlled activity / adversary simulation
                ↓
        Windows / Linux endpoints
                ↓
         Security telemetry
                ↓
              Wazuh
                ↓
      Detections and alerts
                ↓
       SOC investigations
                ↓
       AI-assisted triage
                ↓
        Evaluation harness
```

A later phase will reproduce selected detections and investigations in Microsoft Sentinel using KQL.

## What Makes This Project Different

The goal is not to attach an LLM to a SIEM dashboard.

The AI component will be evaluated against known ground truth and must distinguish:

- observed evidence;
- analyst/model inference;
- missing information;
- recommendations.

The project will measure failure modes such as:

- hallucinated evidence;
- false positives;
- false negatives;
- incorrect MITRE ATT&CK mappings;
- incorrect escalation decisions;
- indirect prompt injection through untrusted log data.

## Current Status

**Phase 0 — Architecture and environment foundation: in progress.**

Currently validated:

- host inventory documented;
- local hardware constraints documented;
- Azure for Students access validated through Azure CLI;
- Azure resource group `rg-soc-detection-lab` created in `East US`;
- remote Wazuh architecture selected because the physical host has limited RAM.

Not yet implemented or validated:

- Wazuh VM;
- Wazuh server/indexer/dashboard;
- Windows Wazuh agent;
- Sysmon ingestion;
- Linux telemetry;
- custom detections;
- SOC investigations;
- AI SOC Copilot;
- RAG;
- AI evaluation;
- Microsoft Sentinel/KQL.

See [STATUS.md](STATUS.md) for the evidence-based project state and [ROADMAP.md](ROADMAP.md) for planned phases.

## Target Architecture

```text
                    CONTROLLED ADVERSARY
                     Kali / scripts
                           │
                           ▼
              ┌────────────────────────┐
              │       ENDPOINTS        │
              │ Windows 11 / Linux VM  │
              │ Sysmon / system logs   │
              └───────────┬────────────┘
                          │ telemetry
                          ▼
              ┌────────────────────────┐
              │         WAZUH          │
              │ collection / indexing  │
              │ rules / alerts         │
              └───────────┬────────────┘
                          │
               ┌──────────┴──────────┐
               ▼                     ▼
       DETECTION ENGINEERING   SOC INVESTIGATION
       Wazuh / Sigma           evidence / timeline
       validation / tuning     ATT&CK / verdict
               └──────────┬──────────┘
                          ▼
                 ┌──────────────────┐
                 │  AI SOC COPILOT  │
                 │ normalization    │
                 │ correlation      │
                 │ retrieval        │
                 │ structured triage│
                 └─────────┬────────┘
                           ▼
                  EVALUATION HARNESS
                  grounding / FP / FN
                  hallucinations
                  prompt injection

                           ↓ later

                   MICROSOFT SENTINEL
                   KQL / detections
```

Detailed architecture: [architecture/v2.md](architecture/v2.md).

## Core v1.0 Scope

The first complete release targets:

- Wazuh central infrastructure;
- Windows + Sysmon telemetry;
- Linux telemetry;
- 5 controlled security scenarios;
- 5 validated detections;
- Sigma representation where appropriate;
- 5 evidence-based SOC investigations;
- Python AI SOC Copilot MVP;
- at least 15 ground-truth evaluation cases;
- hallucination / grounding / FP / FN evaluation;
- indirect prompt-injection tests;
- reproducible documentation;
- cost and security controls.

Microsoft Sentinel/KQL is a subsequent comparison phase, not a prerequisite for the first operational Wazuh workflow.

## Evidence Standard

A capability is tracked as one of:

- **PLANNED** — designed but not implemented;
- **IMPLEMENTED** — deployed or coded;
- **VALIDATED** — tested with observed evidence;
- **EXPERIMENTAL** — exploratory work without production-style claims;
- **FAILED/BLOCKED** — attempted but not currently working.

A detection is not considered validated because it triggers once. Positive/negative testing, evidence, limitations, and false-positive considerations are required.

## Repository Direction

As implementation progresses, the repository will contain:

```text
architecture/
setup/
telemetry/
simulations/
detections/
sigma/
investigations/
incident-reports/
ai-soc/
playbooks/
evaluation/
ai-security/
sentinel/
kql/
samples/
scripts/
lessons-learned/
```

Directories are added when they contain real work rather than as empty placeholders.

## Safety

- All simulations are limited to owned/authorized systems.
- No destructive experimentation is performed on the physical Windows host.
- Higher-risk activity belongs in isolated VMs.
- Cloud exposure is minimized.
- Secrets, credentials, private keys, tokens, and sensitive raw logs must not be committed.
- The AI layer starts read-only and will not perform autonomous remediation.

## Portfolio Positioning

This repository documents a personal hands-on security engineering project. It should demonstrate reproducible technical work, reasoning, validation, and limitations without representing lab activity as professional SOC experience.
