# Phase 4 Development Tasks

This document summarises the main tasks planned for Phase 4 of the ReFi.Trading platform. The detailed task list is in the attached CSV and PDF.

## Conventions and on‑chain components
- Add Redis key conventions for asset routing, token policy, on‑chain driver, DePIN job locks and zk‑VaR circuits.
- Implement asset routing read‑through caches and token policy nonces for short‑lived claims.
- Define per‑wallet on‑chain nonces for the on‑chain driver and token reconciliation.

## On‑chain strategy (ReFIN L2)
- Deploy all DePIN contracts (Node Registry, Staking, QoS Oracle, Rewards, Slashing) and optional registries (Policy Registry, Attestation Registry) to the same L2 as the Phase 3 audit registry.
- Integrate zk‑VaR circuits for proving risk metrics.

Attachments: `phase-4-tasks.csv` and `phase-4-tasks.pdf` provide the detailed task list.