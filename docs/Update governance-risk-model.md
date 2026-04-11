=== FILE: docs/governance-risk-model.md ===

Governance Risk Model – EGTS

1. Introduction

The EGTS Governance Risk Model provides quantitative and qualitative metrics to assess the health and security of governance trust surfaces. It is used by dashboards, risk analysts, and automated insurance or slashing mechanisms.

2. Risk Dimensions

Dimension Description
Concentration Distribution of badges / voting power among contributors
Validator Decentralization Distribution of stake across validators
Cross‑Chain Dependency Reliance on bridge or adapter security
Proof Freshness Age of integrity proofs
Badge Authority Capture Control over badge issuance
Contributor Liveness Recent proof‑of‑contribution activity

3. Quantitative Metrics

3.1 Gini Coefficient (Badge Distribution)

G = Σ (2i - n - 1) * v_i / (n Σ v_j), where v_i is sorted badge weights.
Thresholds:

· < 0.3 → Low risk
· 0.3–0.6 → Moderate
· 0.6 → High risk

3.2 Validator Participation Rate

P = (signing_validators / total_validators) over last 1024 blocks.
Risk level:

· P > 90% → Low
· 70%–90% → Medium
· < 70% → High

3.3 Cross‑Chain Adapter Latency

L = (block_time_source - block_time_egts) in seconds.
Alert if L > 2 * target block time for > 1 hour.

3.4 Badge Authority Decay

Fraction of badges expired or revoked in last 30 days.
Decay > 20% indicates possible governance instability.

4. Composite Risk Score

RiskScore = w1*G + w2*(1-P) + w3*(L/60) + w4*Decay
Default weights: w1=0.4, w2=0.3, w3=0.2, w4=0.1

Score range 0–1:

· 0.00–0.25 → Low risk
· 0.26–0.60 → Medium risk
· 0.61–1.00 → High risk

5. Qualitative Overrides

Certain events override the quantitative score:

· Emergency pause of badge authority → RiskScore = 1.0 immediately.
· Validator slashing event → RiskScore increases by +0.2 for 7 days.
· Cross‑chain adapter replacement → RiskScore = 0.8 for 14 days.

6. Reporting Frequency

Risk metrics are computed every epoch (1 hour). A summary is posted on‑chain as a signed message from the EGTS risk oracle (a deterministic contract). Off‑chain dashboards may compute more frequent updates.

7. Integration with Slashing & Insurance

· If RiskScore > 0.8 for 3 consecutive epochs, validators may vote to pause governance actions.
· Insurance protocols (e.g., Nexus Mutual) can use RiskScore to adjust premiums.
· A risk‑adjusted badge weight is exposed for voting: adjustedWeight = rawWeight * (1 - RiskScore).
