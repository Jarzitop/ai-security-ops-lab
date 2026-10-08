# Azure Wazuh — preflight and deployment gate

**Document status:** PLANNED / READ-ONLY PREFLIGHT. Updated: 2026-10-08.
**Scope:** Infrastructure and telemetry only. This file does not attest to a deployed VM or working ingestion.

## Previously reported facts (not rechecked in this session)

- Azure for Students subscription accessed using Azure CLI and Cloud Shell.
- Resource group: `rg-soc-detection-lab`; region: `eastus`.
- No evidence of a Wazuh VM deployed yet.
- Proposed Wazuh VM: `vm-wazuh`.
- Physical Windows 11 host has approximately 8 GiB RAM. Do not run the central Wazuh stack there.

Repository inspection on 2026-10-08 found the README still labeled Phase 0. This document adds no claim of Azure deployment.

## Architecture gate

`Windows 11 -> Sysmon / Windows Event Logs -> Wazuh agent -> Wazuh server -> indexer -> searchable event (dashboard)`.

Proposed single-node all-in-one Wazuh deployment on Ubuntu 24.04 LTS, sized at **4 vCPU, 8 GiB** and **at least 50 GB** usable storage. Prefer a managed 64-GiB OS disk after checking image, pricing and quotas. Initial candidate SKU: `Standard_B4als_v2`; not yet proven available or affordable in this subscription.

Wazuh quickstart: https://documentation.wazuh.com/current/quickstart.html
Azure Basv2 SKU details: https://learn.microsoft.com/en-us/azure/virtual-machines/sizes/general-purpose/basv2-series

## Phase A: run in **Azure Cloud Shell Bash** (READ ONLY)

These commands do not deploy resources. Review their output before any `az vm create` call.

```bash
set -euo pipefail
RG="rg-soc-detection-lab"
REGION="eastus"
SKU="Standard_B4als_v2"

az account show --query "{name:name,id:id,state:state}" -o table
az group show --name "$RG" --query "{name:name,location:location,provisioningState:properties.provisioningState}" -o json
az resource list --resource-group "$RG" --output table
az vm list --resource-group "$RG" --show-details --output table
az vm list-skus --location "$REGION" --size "$SKU" --all --output table
az vm list-usage --location "$REGION" --output table
az vm image list-skus --location "$REGION" --publisher Canonical --offer ubuntu-24_04-lts --output table
```

**Interpretation:** An unlisted or restricted SKU, unavailable 4-core quota, insufficient remaining student credit, or an unexpected existing Wazuh deployment is a STOP condition. SKU listing does not guarantee regional capacity at creation time.

### Retail compute estimate (READ ONLY)

The public Azure Retail Prices API estimates on-demand compute, **not** the final contracted Azure for Students bill. Check matching Linux/non-Spot entries; obtain disk and public IP pricing separately. Validate with the Azure Pricing Calculator and remaining student credit.

```bash
FILTER="armRegionName eq 'eastus' and armSkuName eq 'Standard_B4als_v2' and priceType eq 'Consumption'"
curl -fsSG 'https://prices.azure.com/api/retail/prices' \
  --data-urlencode "\$filter=$FILTER" \
  | jq -r '.Items[] | select((.productName | contains("Windows")) | not) | select((.meterName | contains("Spot")) | not) | [.armSkuName, .productName, .retailPrice, .unitOfMeasure, .currencyCode] | @tsv'
```

Estimate `compute = hourly_price * planned_running_hours`; add persistent OS disk, charged IPv4 public IP, disk transactions (where applicable), and network egress. No exact monetary estimate is validated as of this writing.

Documentation: https://learn.microsoft.com/en-us/rest/api/cost-management/retail-prices/azure-retail-prices

## Phase B: safety/cost gates BEFORE resource creation

1. Inspect the resource group for existing resources and names to avoid duplicate spending.
2. Check SKU restrictions, quota, capacity limitations, Ubuntu image availability, Azure for Students credit and total projected consumption.
3. Set a monthly budget with alerts in Azure Cost Management, recognizing that **alerts are not a spending cap**. Check it is active.
4. Plan auto-shutdown in the correct time zone after VM creation. Manually `az vm deallocate -g rg-soc-detection-lab -n vm-wazuh` at the end of each session and verify `VM deallocated`.
5. Account for the **persistent** cost of OS disks and Standard IPv4 public IPs even when compute is deallocated. Review and remove unused resources, after backing up only what is necessary.
6. Use SSH public-key authentication only. Never commit private keys, installation-generated credential archives, passwords, tokens or public/private personal IPs.
7. Use NSG deny-by-default inbound; do not provision a VM with default public SSH exposure. Grant only the minimum inbound CIDR(s) required for the endpoint and dashboard; re-evaluate if the home public IP changes. Access from Cloud Shell will not necessarily share the endpoint's public IP.
8. For Wazuh, allow `1514/TCP` from a known endpoint IP; allow `1515/TCP` only during enrollment; allow `443/TCP` to a known administrator IP. Restrict `22/TCP` to authorized admin IP only if actually needed. Do not expose `9200/TCP`, `55000/TCP`, `1516/TCP` to the internet.
9. Keep Wazuh server and indexer as internal processes on the same VM. No load balancer, VPN gateway, separate database or Sentinel workspace without a demonstrated need.
10. Do not install or run simulations on the physical Windows endpoint until scope and safety have been reviewed.

Wazuh ports: https://documentation.wazuh.com/current/getting-started/architecture.html

## Phase C: implementation and validation gates (NOT EXECUTED)

- Provision `vm-wazuh` only after phase A and B evidence is recorded.
- Verify OS, RAM, CPU, storage and NSG; confirm auto-shutdown and manual deallocation procedure.
- Install Wazuh assisted all-in-one from the official *versioned* installation guide only after VM resources pass checks; never publish installation-generated passwords.
- Verify manager, indexer, dashboard, Filebeat and access via restricted HTTPS; measure CPU, RAM and disk.
- Deploy Wazuh Windows agent, then Sysmon with a documented benign configuration. Enable the `Microsoft-Windows-Sysmon/Operational` channel explicitly in agent configuration if not present.
- Prove real Windows events at the endpoint, collection by the agent, ingestion by Wazuh and indexed search. **Connected agent alone does not satisfy the gate.**
- Index patterns `wazuh-alerts-*` contain alerts only. Raw non-alert events require explicitly enabling `wazuh-archives-*` ingestion; test this in a short controlled window because it increases storage.

Wazuh archive guide: https://documentation.wazuh.com/current/user-manual/wazuh-indexer/wazuh-indexer-indices.html

## Deployment status checklist

- [ ] Subscription/resource inventory captured in this session
- [ ] SKU/quota/image/retail pricing/credit checked
- [ ] Cost alerts configured and evidenced
- [ ] Safe NSG design reviewed
- [ ] Azure VM provisioned and measured
- [ ] Shutdown/deallocate validated
- [ ] Wazuh installed and services healthy
- [ ] Agent connected
- [ ] At least one event from each agreed telemetry source queried with timestamp, host, field evidence and source

**Sources of truth:** sanitized Azure CLI output, Wazuh console queries, date-stamped local evidence; keep secrets out of version control.
