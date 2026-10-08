# SOC Detection Lab

Hands-on cybersecurity lab focused on security monitoring, endpoint telemetry, detection, triage, and incident investigation.

## Project Goal

Build practical, defensible experience with the workflow used in entry-level SOC / MDR environments:

```text
Endpoint activity
      ↓
Telemetry and logs
      ↓
SIEM / XDR collection
      ↓
Detection
      ↓
Alert
      ↓
Triage
      ↓
Investigation
      ↓
MITRE ATT&CK mapping
      ↓
Incident documentation
```

The lab will start with Wazuh and Windows/Linux telemetry. Microsoft Sentinel will be introduced later, after the local detection and investigation workflow is working reliably.

## Planned Scope

- Wazuh SIEM/XDR
- Windows Event Logs
- Sysmon
- Linux system/authentication logs
- Endpoint agents
- Alert triage
- Detection engineering fundamentals
- MITRE ATT&CK mapping
- Investigation timelines
- Incident notes and reports
- Microsoft Sentinel
- KQL fundamentals
- Controlled attack / behavior simulations

## Current Status

**Phase 0 — Environment inventory and architecture design.**

No detections, investigations, or incident results are claimed yet. This repository will be updated only with work actually completed and validated in the lab.

## Planned Repository Structure

```text
.
├── README.md
├── architecture/
├── setup/
├── detections/
│   ├── wazuh/
│   └── sentinel-kql/
├── investigations/
├── incident-reports/
├── scripts/
├── samples/
└── lessons-learned/
```

## Learning Objectives

By the end of the project, I should be able to:

- explain the lab architecture and telemetry flow;
- collect and search Windows/Linux security telemetry;
- investigate alerts using evidence rather than assumptions;
- build and validate basic detections;
- identify likely false positives and tuning opportunities;
- correlate users, processes, hosts, and network activity;
- build investigation timelines;
- map observed behavior to MITRE ATT&CK when justified;
- document findings and escalation decisions;
- write and explain basic/intermediate KQL queries in Microsoft Sentinel;
- clearly distinguish a controlled lab from production SOC experience.

## Portfolio Positioning

This repository is a **personal hands-on security lab in a controlled environment**.

It is not professional SOC experience and should not be represented as such.

## Safety and Scope

All simulations will be performed only on systems I own or am explicitly authorized to test.

The project is intended for defensive security learning, detection validation, and incident-investigation practice.
