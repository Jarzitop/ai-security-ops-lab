# Architecture

This directory records the lab architecture and its evolution.

## Current design

The active target architecture is [v2.md](v2.md).

It expands the original SOC-only design into an AI-Augmented Security Operations Lab while preserving the same infrastructure principle: the physical laptop should not host the complete Wazuh central stack because of its limited memory.

## Documents

- [host-inventory.md](host-inventory.md) — validated host constraints.
- [v1.md](v1.md) — historical initial SOC architecture; superseded by v2.
- [v2.md](v2.md) — current target architecture.
- [azure-cost-guardrails.md](azure-cost-guardrails.md) — cloud cost controls.

Architecture documents distinguish proposed design from implemented and validated components.
