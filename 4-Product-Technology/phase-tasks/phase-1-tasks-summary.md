# Phase 1 Development Tasks

This document summarises the major tasks planned for Phase 1 of the ReFi.Trading platform. The detailed task list is in the attached CSV and PDF.

## Conventions and foundations
- Set standards for environment names (dev, staging, prod) and resource naming.
- Define event attributes: correlation ID, emitter, emittedAt and natural keys (asset, template_id, account_id, intent_id, plan_id, client_order_id).
- Establish ordering keys and define lock keys for risk, orders and positions in Redis caches.

## Infrastructure and networking
- Create separate Google Cloud projects for each environment with strict isolation.
- Configure Cloud Run, Pub/Sub and MemoryStore networks and set up VPC connectors.
- Choose a primary region and enforce regional restrictions.

## Data stores
- Define Cloud Spanner tables and indexes for identity and compliance data.
- Create policy documents in Firestore.
- Implement nonces, session caches and position caches in Redis.

## Security & IAM
- Create service accounts for each component with least privilege.
- Configure Secrets Manager and set network access controls.

## Development and operations
- Build a training scheduler and data loader with Cloud Run jobs.
- Set up CI/CD pipelines using GitLab and Cloud Build.
- Install observability tools: dashboards, alerts and logs.

Attachments: `phase-1-tasks.csv` and `phase-1-tasks.pdf` provide the full task list.