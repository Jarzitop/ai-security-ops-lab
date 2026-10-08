# Tomorrow — Resume Phase 0

## Current state

Confirmed:

- Azure for Students subscription exists.
- Azure for Students role: **Owner**.
- Azure for Students credit: **USD 100**.
- Azure Académico is a separate subscription with limited permissions.
- Portal subscription filter includes both subscriptions.
- Resource-group creation still only offers **Azure Académico**.

## Working diagnosis

The subscription filter is not the blocker.

The most likely cause is that the Azure portal is currently operating in the **Universidad de los Andes directory/tenant**, while the **Azure for Students** subscription belongs to a different Microsoft Entra directory.

Do **not** disable, delete, or modify the Azure Académico subscription.

## First task tomorrow

1. In Azure Portal, click the account/avatar in the top-right.
2. Open **Switch directory** / **Cambiar directorio**.
3. Inspect all available directories.
4. Switch to the directory associated with **Azure for Students**.
5. Return to **Subscriptions** and verify that Azure for Students is still visible.
6. Open **Resource groups → Create**.
7. Confirm that the subscription dropdown now offers **Azure for Students**.

If there are multiple candidate directories, take a screenshot before changing anything.

## If Azure for Students still does not appear

Run one of these read-only checks:

### Azure CLI

```bash
az account list --output table
```

### Azure PowerShell

```powershell
Get-AzSubscription | Select-Object Name, Id, TenantId, State
```

The goal is to identify the TenantId associated with Azure for Students and compare it with the current directory.

## After subscription access works

Create:

- Resource group: `rg-soc-detection-lab`
- Region: `East US`

Then start VM creation but **do not deploy yet**:

- Name: `vm-wazuh`
- Image: Ubuntu Server 24.04 LTS x64
- Authentication: SSH public key
- Username: `azureuser`
- Initial public inbound port: SSH (22) only

Before deployment:

- compare 4 vCPU / 8 GiB VM sizes;
- review estimated price;
- choose smallest sensible disk;
- configure auto-shutdown;
- restrict network exposure;
- confirm cost controls.

## Cost objective

Target: keep this MVP lab below roughly **USD 5–8/month**, preferably lower, by deallocating the VM whenever it is not actively used.

The Wazuh MVP should be completed quickly and cloud compute should not stay running idle.
