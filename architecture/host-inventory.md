# Host Inventory

## Confirmed hardware

| Component | Value |
| --- | --- |
| Host OS | Microsoft Windows 11 Home, 64-bit |
| OS version | 10.0.26200 |
| CPU | Intel Core i5-10300H @ 2.50 GHz |
| CPU cores | 4 physical / 8 logical |
| Installed RAM | 7.84 GB |
| System drive used | 391.21 GB |
| System drive free | 66.77 GB |
| Hypervisor | VMware Workstation Pro |
| Existing VM | Kali Linux |

## Constraints

The main constraint is memory, not CPU.

Wazuh's current all-in-one quickstart recommendation for 1–25 agents is 4 vCPU, 8 GiB RAM, and 50 GB storage. Running the complete Wazuh stack locally would therefore consume approximately the entire physical memory of this laptop and is not a sensible design.

The available 66.77 GB of free disk space is enough for a small number of lab VMs, but storage should be managed carefully.

## Design implications

- Do not run the Wazuh central stack on the laptop.
- Use the Windows 11 host as the first monitored endpoint for safe telemetry collection.
- Keep Kali as a controlled simulation/attacker system and power it on only when required.
- Add lightweight victim VMs only when a scenario requires isolation.
- Avoid running multiple heavy VMs simultaneously.
- Prefer a remotely hosted Wazuh server if a low-cost/student-cloud option is available.
- Do not enable paid cloud resources before verifying current pricing, credits, and spending limits.
