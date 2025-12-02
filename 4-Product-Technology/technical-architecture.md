# Technical Architecture

## System Overview

ReFi.Trading is built as a modern, cloud-native platform with a clear separation between the user-facing application layer, the broker integration layer, and the reinforcement learning engine. The architecture is designed for scalability, security, and regulatory compliance from day one.

```
┌─────────────────────────────────────────────────────────────┐
│                     USER LAYER                               │
│  React Web App + Mobile-Responsive PWA                       │
│  Authentication: Auth0 + EIP-4361 (Wallet Connect)          │
└─────────────────────────────────────────────────────────────┘
                           ↓ HTTPS
┌─────────────────────────────────────────────────────────────┐
│                   APPLICATION LAYER                          │
│  FastAPI Backend (Python 3.11+)                             │
│  - User Management + RBAC                                    │
│  - Agent Configuration + Deployment                          │
│  - Risk Management Engine                                    │
│  - Performance Analytics + Reporting                         │
│  Database: PostgreSQL (primary) + Redis (cache)             │
└─────────────────────────────────────────────────────────────┘
                           ↓ 
┌─────────────────────────────────────────────────────────────┐
│                 BROKER INTEGRATION LAYER                     │
│  SnapTrade API (Primary)                                    │
│  - OAuth 2.0 authentication                                  │
│  - Multi-broker support (IBKR, Questrade, Kraken, etc.)    │
│  - Real-time market data                                     │
│  - Order execution + confirmation                            │
│  Direct API Fallback: IBKR API, Kraken API                  │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                      RL ENGINE LAYER                         │
│  PyTorch (2.0+) + Stable Baselines3                        │
│  - RVI-Q Algorithm (Average-Reward RL)                      │
│  - Model training + inference pipeline                       │
│  - Feature engineering (technical indicators, sentiment)     │
│  - Model versioning (IPFS CIDs)                             │
│  Compute: AWS EC2 (training), Lambda (inference)            │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                   COMPLIANCE & AUDIT LAYER                   │
│  Phase 2+: Chainlink CCID (KYC), ACE (Compliance)          │
│  Phase 3+: ReFIN L2 (Audit Trail), zk-VaR Proofs           │
│  Phase 4+: On-chain execution, DePIN staking               │
└─────────────────────────────────────────────────────────────┘
```

## Core Technology Stack

### Frontend
- **Framework:** React 18+ with TypeScript
- **UI Library:** Tailwind CSS + shadcn/ui components
- **State Management:** Zustand (lightweight, performant)
- **Charts:** TradingView Lightweight Charts + Recharts
- **Authentication:** Auth0 + WalletConnect (EIP-4361)
- **Deployment:** Vercel (CDN, edge functions, auto-scaling)

**Key Features:**
- Real-time performance dashboard with WebSocket updates
- Interactive agent configuration wizard
- Risk controls interface (VaR limits, position sizing, kill switches)
- Paper trading simulator with live market data
- Community features (Discord integration, leaderboards)

### Backend
- **Framework:** FastAPI (Python 3.11+) - async, type-safe, auto-documented
- **ORM:** SQLAlchemy 2.0 (async) + Alembic (migrations)
- **Task Queue:** Celery + Redis (async background jobs)
- **API Documentation:** OpenAPI 3.0 (auto-generated via FastAPI)
- **Authentication:** Auth0 (managed OAuth 2.0 + JWT)
- **Deployment:** AWS ECS Fargate (auto-scaling containers)

**Key Services:**
1. **User Service:** Authentication, profile management, RBAC
2. **Agent Service:** RL agent configuration, deployment, lifecycle management
3. **Trading Service:** Order generation, execution, confirmation
4. **Risk Service:** Real-time VaR calculation, circuit breakers, progressive caps
5. **Analytics Service:** Performance metrics, backtesting, reporting

### Database Architecture
- **Primary DB:** PostgreSQL 15+ (RDS Multi-AZ for HA)
  - Users, agents, orders, performance history
  - Partitioned by time for efficient querying (monthly partitions)
  
- **Cache Layer:** Redis 7+ (ElastiCache)
  - Real-time market data (15-min TTL)
  - User session state
  - Agent inference results

- **Time-Series DB:** TimescaleDB (extension of PostgreSQL)
  - Tick data, order book snapshots
  - Performance metrics (second-level granularity)

- **Document Store (Phase 3+):** IPFS + Filecoin
  - Model checkpoints (TorchScript exports)
  - Audit logs, compliance records

### RL Engine

**Core Algorithm: Average-Reward RVI-Q Learning**

Unlike traditional RL approaches that use discounted rewards (γ < 1), our RVI-Q algorithm uses average rewards, which is mathematically optimal for infinite-horizon environments like financial markets.

**Key Advantages:**
1. **No artificial time horizon:** Doesn't penalize future rewards arbitrarily
2. **Stationary optimal policy:** Converges to stable strategies even in non-stationary markets
3. **Better risk-adjusted returns:** Empirically outperforms Q-learning, SARSA, and Actor-Critic in backtests

**Implementation:**
- **Framework:** PyTorch 2.0+ (dynamic computation graphs, JIT compilation)
- **Training Library:** Stable Baselines3 (extended with custom RVI-Q policy)
- **State Space:** 47 features (technical indicators, volume, order book depth, sentiment)
- **Action Space:** Discrete (buy, sell, hold) + continuous (position sizing 0-100%)
- **Reward Function:** Risk-adjusted return (Sharpe ratio × 100 - drawdown penalty)
- **Training Data:** 3.1 years of minute-level OHLCV + order book data
- **Retraining Frequency:** Weekly (incremental updates), monthly (full retrain)

**Model Versioning:**
- Each trained model is exported as TorchScript (deterministic inference)
- Model hash stored on-chain (Phase 3: IPFS CID on ReFIN L2)
- Governance vote required for production deployment (Phase 4: DAO)

**Inference Pipeline:**
1. Feature extraction: 40ms (real-time indicators from tick data)
2. Model inference: 5ms (TorchScript on CPU, <1ms on GPU)
3. Risk check: 10ms (VaR calculation + limit verification)
4. Order generation: 5ms (position sizing + order book analysis)
5. **Total latency:** <100ms end-to-end (P95 < 150ms)

### Broker Integration

**Primary: SnapTrade API**

SnapTrade provides a unified API across 20+ brokers, abstracting the complexity of individual broker APIs.

**Key Features:**
- OAuth 2.0 flow for secure user authorization
- Read/write access to user accounts (with explicit consent)
- Real-time market data + streaming quotes
- Order placement, modification, cancellation
- Portfolio sync + transaction history

**Supported Brokers (Phase 1):**
- Interactive Brokers (equities, options, futures, forex)
- Questrade (Canadian equities, options)
- Kraken (crypto spot + futures)
- Coinbase (crypto spot)

**Fallback: Direct API Integration**

For brokers not yet supported by SnapTrade, we maintain direct API clients:
- **IBKR TWS API:** Native Python client for low-latency execution
- **Kraken WebSocket API:** Real-time order book + trades
- **Coinbase Advanced Trade API:** Institutional-grade crypto execution

**Risk Controls at Integration Layer:**
1. **Order validation:** Reject orders violating user-defined limits
2. **Rate limiting:** Respect broker API quotas (e.g., 50 req/sec for IBKR)
3. **Circuit breakers:** Auto-pause trading on detected anomalies
4. **Reconciliation:** Daily position/balance sync with broker

### Risk Management Engine

**Value-at-Risk (VaR) Calculation:**
- **Method:** Historical simulation (95% confidence level)
- **Window:** 90-day rolling window
- **Frequency:** Real-time (recalculated on every order)
- **Enhancements (Phase 3):** Cornish-Fisher approximation for fat tails, zk-SNARK proof generation

**Progressive Caps (Phase 1):**
- Day 1: $100 max notional
- Day 3: $500 max notional
- Day 7: $5,000 max notional
- Day 14: $50,000 max notional (with 2-week performance review)

**Kill Switches:**
1. **User-triggered:** Manual pause via web app or API
2. **Automated:**
   - VaR breach (99% threshold)
   - Drawdown > 15% in 24 hours
   - API connectivity loss > 5 minutes
   - Broker account balance mismatch

### Security Architecture

**Authentication:**
- Auth0 (managed OAuth 2.0)
- Multi-factor authentication (TOTP, SMS, email)
- Wallet Connect (EIP-4361) for Phase 2+ (Ethereum wallet sign-in)

**Authorization:**
- Role-Based Access Control (RBAC)
- JWT tokens (15-min expiry, refresh tokens 7-day expiry)
- API key rotation every 90 days

**Data Encryption:**
- TLS 1.3 for all traffic (Let's Encrypt certs)
- At-rest encryption: AES-256 (RDS, S3)
- Secrets management: AWS Secrets Manager (API keys, DB credentials)

**Broker Credentials:**
- **Never stored:** SnapTrade handles OAuth tokens, we only store userSecret (encrypted)
- **Scope-limited:** Read account data + place orders (no withdrawal permissions)
- **Revocable:** Users can disconnect broker at any time

**SOC 2 Type II Compliance (Target: Q2 2026):**
- Automated security scanning (Snyk, OWASP ZAP)
- Penetration testing (bi-annual)
- Audit logging (CloudTrail, DataDog)
- Incident response plan + runbooks

### Scalability & Performance

**Auto-Scaling Strategy:**
- **Frontend:** Edge-cached via Vercel CDN (sub-50ms global latency)
- **Backend:** ECS Fargate auto-scales based on CPU/memory (2-50 containers)
- **RL Inference:** Lambda functions (millisecond cold starts, infinite horizontal scale)
- **Database:** RDS read replicas (up to 15 for read-heavy workloads)

**Performance Targets:**
- **Web App Load:** <1s (LCP, 95th percentile)
- **API Latency:** <200ms (P95)
- **Order Execution:** <500ms end-to-end (user click → broker confirmation)
- **RL Inference:** <100ms (P95)

**Monitoring & Observability:**
- **APM:** DataDog (distributed tracing, real-user monitoring)
- **Logging:** CloudWatch Logs + Elasticsearch (centralized, searchable)
- **Alerting:** PagerDuty (on-call rotation for critical issues)
- **Dashboards:** Grafana (real-time metrics, SLA tracking)

## Phase-Specific Architecture Evolution

### Phase 1 (Q4 2025): Barebones Prototype
**Focus:** Paper trading MVP, SnapTrade integration, basic RL agents

**Tech Stack:**
- React + FastAPI + PostgreSQL + Redis
- SnapTrade API (IBKR, Questrade, Kraken, Coinbase)
- PyTorch + Stable Baselines3 (RVI-Q)
- Deployed on AWS (ECS + RDS + ElastiCache)

**Deliverables:**
- Paper trading with simulated execution
- Weekly agent retraining
- Basic risk controls (VaR, progressive caps)
- Performance dashboard

### Phase 2 (Q1 2026): Identity & Compliance
**Focus:** KYC/AML, wallet authentication, UAE ADGM licensing

**New Components:**
- Chainlink CCID (KYC attestation)
- Chainlink ACE (compliance rules engine)
- Auth0 + WalletConnect (EIP-4361)
- Compliance adapter (cache ACE responses)

**Deliverables:**
- Wallet-based authentication
- Automated KYC/KYB onboarding
- Compliance checks (sanctions screening, PEP)
- Regulatory readiness for live trading

### Phase 3 (Q2-Q3 2026): Audit & ReFIN L2 Anchoring
**Focus:** Transparent audit trail, on-chain proof-of-risk

**New Components:**
- ReFIN Layer 2 (OP-stack rollup)
- zk-SNARK proof generation (Circom/SnarkJS)
- IPFS/Filecoin (model + proof storage)
- Chainlink CCIP (cross-chain messaging)

**Deliverables:**
- On-chain trade + performance proofs
- L1 anchor (Ethereum mainnet)
- Public dashboard for regulators
- Cryptographic VaR attestation

### Phase 4 (Q4 2026+): On-Chain Execution & DePIN
**Focus:** Decentralized infrastructure, tokenized execution

**New Components:**
- On-chain order execution (ReFIN L2 native)
- DePIN node network (GPU/data staking)
- $REFI token (utility + governance)
- DAO governance (Snapshot + on-chain execution)

**Deliverables:**
- Token-gated features (staking for rebates)
- Decentralized compute network
- Marketplace for third-party strategies
- Reduced infrastructure costs (40% via DePIN)

## Infrastructure Costs

**Phase 1 (Month 1-12):**
- AWS: $2,000/month (ECS, RDS, ElastiCache, bandwidth)
- Auth0: $200/month (up to 10K users)
- SnapTrade: $1.50/user/month × 5,000 = $7,500/month (Year-end)
- Vercel: $20/month (Pro plan)
- DataDog: $500/month (monitoring)
- **Total:** ~$10,220/month at 5,000 users

**Phase 4 (DePIN Maturity):**
- Traditional cloud: $50,000/month (50K users)
- DePIN-subsidized: $30,000/month (40% reduction via token emissions)
- Net savings: $20,000/month = $240K/year

## Technology Risks & Mitigation

| Risk | Mitigation |
|------|-----------|
| **SnapTrade downtime** | Direct API fallback for IBKR, Kraken |
| **RL model drift** | Weekly retraining, anomaly detection, automatic rollback |
| **Broker API changes** | Version pinning, integration tests, monthly API review |
| **Scaling bottlenecks** | Auto-scaling, caching, read replicas, async processing |
| **Security vulnerabilities** | Automated scanning, pen testing, bug bounty program |
| **Regulatory compliance** | Compliance-by-design, legal review of all features |

---

**Last Updated:** December 2025  
**Maintained By:** Daniel Oosthuyzen (CTO)  
**Review Cadence:** Quarterly architecture review, monthly tech debt assessment
