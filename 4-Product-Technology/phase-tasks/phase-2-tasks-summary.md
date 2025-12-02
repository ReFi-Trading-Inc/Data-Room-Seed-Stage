# Phase 2 Development Tasks

This document summarises the key tasks planned for Phase 2 of the ReFi.Trading platform. The detailed task list is available in the attached CSV and PDF.

## Conventions and standards
- Extend Redis key conventions for new modules.
- Define API conventions: small, versioned REST APIs with separate public endpoints for wallet sign‑in and CCID; internal endpoints for the compliance adapter.
- Document API path structure and policies.

## Networking and security
- Set egress allow lists for KYC/KYB providers, Chainlink CCID and ACE per environment.
- Establish ingress controls with TLS, CORS and CSRF protections.

## Data and storage
- Configure Cloud Spanner tables and indexes for identity and compliance.
- Create policy documents in Firestore, including allowed domains, origins, URIs and chain IDs.
- Implement session caches, webhook caches and nonces in Redis.

## Artifact management
- Define repositories in Artifact Registry for wallet sign‑in, CCID/KYC and compliance adapter modules.

## Module development
- Plan to build and integrate small services for new modules into existing infrastructure.

Attachments: `phase-2-tasks.csv` and `phase-2-tasks.pdf` contain the full task list.