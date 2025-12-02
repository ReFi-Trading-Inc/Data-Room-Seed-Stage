# KYC / AML Framework

- **Identity Verification:** Users will undergo Know Your Customer (KYC) checks via third‑party providers (e.g., Jumio or Sumsub). Chainlink CCID attestations will be used in Phase 2 to verify digital identities.
- **AML Screening:** All users are screened against sanctions lists (OFAC, UN, EU) and politically exposed person databases.
- **Ongoing Monitoring:** The compliance adapter caches ACE verdicts and refreshes them based on triggers such as profile updates, large orders or jurisdiction changes.
- **Data Protection:** User data is stored in compliance with GDPR and ADGM data protection rules.