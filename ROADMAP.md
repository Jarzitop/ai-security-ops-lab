# Project Roadmap

The roadmap is evidence-driven. A phase is complete only when its acceptance criteria are met.

## Phase 0 — Environment Inventory and Architecture

**Goal:** design a lab that fits the available hardware and budget.

### Tasks
- document host OS, CPU, RAM, and available storage;
- inventory existing VMs;
- define virtual networking;
- decide which components can run simultaneously;
- define the first Wazuh + endpoint architecture;
- document cost constraints before using cloud resources.

### Acceptance criteria
- architecture diagram exists;
- resource allocation is documented;
- networking plan is documented;
- no component is deployed without understanding its purpose.

## Phase 1 — Wazuh Core

**Goal:** deploy Wazuh and verify real telemetry ingestion.

### Acceptance criteria
- Wazuh components are running;
- at least one endpoint is enrolled;
- events are searchable;
- the data flow from endpoint to alert can be explained.

## Phase 2 — Windows Telemetry

**Goal:** collect and investigate useful Windows endpoint telemetry.

### Planned topics
- Windows Event Logs;
- Sysmon;
- process creation;
- authentication;
- PowerShell;
- services;
- scheduled tasks;
- network connections.

### Acceptance criteria
- telemetry is visible in Wazuh;
- at least three controlled behaviors are generated and traced;
- relevant evidence can be correlated into a timeline.

## Phase 3 — Linux Telemetry

**Goal:** investigate Linux authentication, privilege, process, and service activity.

### Acceptance criteria
- Linux endpoint is enrolled;
- authentication and sudo-related events are searchable;
- at least two controlled scenarios are investigated.

## Phase 4 — Detection and Triage

**Goal:** create, validate, and tune useful detections.

Each detection must document:
- behavior;
- data source;
- detection logic;
- validation method;
- MITRE ATT&CK mapping when justified;
- false-positive considerations;
- tuning notes;
- observed result.

## Phase 5 — Investigations

**Goal:** complete 5–8 evidence-based investigations.

Each investigation should include:
- alert summary;
- initial triage;
- evidence;
- timeline;
- entities;
- analysis;
- verdict;
- severity;
- recommended response;
- investigation notes.

## Phase 6 — Microsoft Sentinel

**Goal:** transfer the workflow to an enterprise cloud SIEM.

Planned scope:
- Log Analytics;
- controlled ingestion;
- KQL;
- analytics rules;
- incidents;
- hunting;
- ATT&CK mapping.

Cloud resources must not be enabled before cost and trial conditions are verified.

## Phase 7 — Portfolio Review

**Goal:** make the repository useful to a technical reviewer.

### Acceptance criteria
- README reflects only completed work;
- architecture is understandable;
- detections are reproducible;
- investigations contain evidence and reasoning;
- screenshots support explanations rather than replace them;
- no secrets or sensitive data are committed.
