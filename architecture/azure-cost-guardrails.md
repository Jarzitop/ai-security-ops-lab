# Azure Cost Guardrails

Azure for Students is available with **USD 100 credit**.

The goal is to preserve as much of that credit as possible for future labs.

## Rules

- Do not create cloud resources without checking whether they are covered by a free tier or will consume credit.
- Prefer burstable development/test VM sizes.
- Deallocate the Wazuh VM whenever the lab is not in use.
- Configure Azure VM auto-shutdown.
- Keep persistent disks as small as the lab safely allows.
- Avoid optional managed services unless they directly support a learning objective.
- Do not enable Microsoft Sentinel until the local/Wazuh workflow is already understood.
- Review Cost Management regularly.
- Create a budget and cost alerts before sustained usage.

## Important billing behavior

A VM in **Stopped (Deallocated)** state does not incur compute charges, but attached disks and some networking resources can continue to incur charges.

## Wazuh sizing

Wazuh currently recommends, for an all-in-one quickstart monitoring 1–25 agents:

- 4 vCPU
- 8 GiB RAM
- 50 GB storage

Free Azure VM allocations such as B1s/B2pts v2/B2ats v2 are useful for other projects, but their memory is below Wazuh's all-in-one recommendation.

For this project, use a paid-from-credit VM only during active lab sessions and deallocate it afterward.
