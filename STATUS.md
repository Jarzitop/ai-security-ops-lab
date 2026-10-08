# Project Status

Last updated: 2026-10-08

This file is the concise source of truth for what has actually been implemented and validated.

## VALIDATED

- Physical host inventory documented.
- Host limitation identified: 7.84 GB installed RAM makes a local Wazuh all-in-one design impractical.
- Azure for Students subscription access verified with Azure CLI.
- Azure resource group `rg-soc-detection-lab` created successfully in `East US`.
- Repository renamed to `ai-security-ops-lab`.

## IMPLEMENTED BUT NOT YET VALIDATED

None currently.

## PLANNED

- Azure Ubuntu VM for Wazuh.
- Wazuh Server / Indexer / Dashboard.
- Windows Wazuh Agent.
- Sysmon telemetry.
- Linux endpoint telemetry.
- Controlled adversary scenarios.
- Wazuh/Sigma detections.
- SOC investigations.
- Python AI SOC Copilot.
- Retrieval/RAG over curated analyst knowledge.
- AI evaluation dataset and harness.
- Indirect prompt-injection tests.
- Microsoft Sentinel / KQL comparison.

## EXPERIMENTAL

None currently.

## FAILED / BLOCKED

- Azure Portal resource-group subscription selector did not expose Azure for Students correctly.
- Workaround validated: Azure CLI successfully selected the subscription and created the resource group.

This UI issue does not currently block the lab.

## Next Technical Step

Verify a suitable low-cost 4 vCPU / 8 GiB Azure VM SKU in `East US`, review expected cost, then create the Wazuh VM with minimal network exposure and auto-shutdown controls.

## Evidence Rule

A feature moves to **VALIDATED** only after observable evidence confirms the intended behavior.
