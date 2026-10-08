# Security and Lab Safety

This repository documents a controlled defensive security lab.

## Authorization

All simulations must target systems owned by the project author or systems for which explicit authorization has been granted.

## Physical Host

The physical Windows host is used only for safe telemetry generation. Destructive, persistence-heavy, malware-like, or otherwise higher-risk testing belongs in isolated lab VMs.

## Secrets

Never commit:

- passwords;
- API keys;
- access tokens;
- Azure credentials;
- private SSH keys;
- certificates containing private material;
- Wazuh credentials;
- connection strings containing secrets;
- sensitive `.env` files.

If a secret is accidentally committed, assume it is compromised and rotate/revoke it. Removing it in a later commit is not sufficient because Git history persists.

## Sensitive Telemetry

Before committing logs, screenshots, or event samples, sanitize unnecessary:

- public IP addresses;
- email addresses;
- usernames when personally identifying;
- subscription/tenant identifiers;
- hostnames containing personal data;
- tokens, cookies, or credentials;
- unrelated personal files or command history.

Prefer small representative samples over bulk raw logs.

## Cloud Exposure

- expose only ports required for the current phase;
- restrict source IP ranges where practical;
- remove temporary access rules after use;
- deallocate Azure VMs when the lab is inactive.

## AI Safety Boundary

The planned AI SOC Copilot begins as read-only.

It must not autonomously:
- block users;
- disable accounts;
- delete files;
- isolate endpoints;
- change firewall rules;
- execute remediation.

Telemetry and retrieved content are treated as untrusted input because they may contain instruction-like or adversarial text.
