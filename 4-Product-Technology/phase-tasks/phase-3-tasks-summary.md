# Phase 3 Development Tasks

This document summarises the planned tasks for Phase 3 of the ReFi.Trading platform. The detailed task list can be found in the attached CSV and PDF.

## Conventions and hashing
- Update Redis key conventions for Phase 3 modules.
- Define the on‑chain strategy: select a target EVM L2 (Base, Optimism or Arbitrum) and Ethereum L1 for anchoring.
- Standardise the hashing algorithm (default SHA‑256 with optional Keccak‑256 via configuration).

## Networking and storage
- Update egress allow lists and create additional Cloud Storage buckets for audit pre‑images; enable versioning and retention.
- Apply Cloud Spanner DDL for Phase 3 tables (audit leaves, Merkle intervals, refinement roots, anchor queues) and verify table existence.
- Create indexes and service accounts for audit writers, Merkle builders, anchor jobs and refinement processes.

## Security & IAM
- Define new service accounts and assign appropriate roles.

Attachments: `phase-3-tasks.csv` and `phase-3-tasks.pdf` include the full task details.