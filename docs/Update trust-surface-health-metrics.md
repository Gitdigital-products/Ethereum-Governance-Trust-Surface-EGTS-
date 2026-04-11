=== FILE: docs/trust-surface-health-metrics.md ===

Trust Surface Health Metrics – EGTS

1. Purpose

Defines the standard set of health metrics used to evaluate the quality, reliability, and security of EGTS trust surfaces. These metrics are computed per governance domain (e.g., by chain, by badge type) and aggregated.

2. Metric Categories

Category Metrics
Liveness Proof submission latency, validator uptime, adapter availability
Correctness Invalid proof rate, challenge success rate
Decentralization Validator stake distribution, badge Gini coefficient
Freshness Age of latest snapshot, median proof age
Adoption Number of active contributors, number of badges issued

3. Detailed Metrics

3.1 Proof Submission Latency

L_sub = (timestamp_egts_finalized - timestamp_source_block)
Goal: median < 1 hour, 95th percentile < 4 hours.

3.2 Validator Uptime

U_val = (signed_slots / total_slots) over 7 days.
Alert if any validator < 80%.

3.3 Adapter Availability

A_ad = (successful_batches / expected_batches) per adapter.
Unhealthy if < 95%.

3.4 Invalid Proof Rate

R_inv = (rejected_proofs / total_submitted_proofs) over 30 days.
Target < 0.5%. Above 2% triggers automatic adapter review.

3.5 Challenge Success Rate

R_ch = (successful_challenges / total_challenges).
High rate (>10%) indicates systemic issues in proof generation.

3.6 Badge Gini Coefficient

Computed per badge type and governance domain. See governance-risk-model.md for formula.

3.7 Snapshot Freshness

F_snap = current_time - last_snapshot_time.
Must be ≤ 6 hours. If > 6h, dashboard shows critical alert.

3.8 Active Contributors

Number of contributor entries with status = active and last proof within 90 days.
Trend over time indicates ecosystem health.

4. Metric Aggregation

Metrics are aggregated into a Health Score (0–100):

HealthScore = 100 * (1 - normalized_risk_score), where normalized_risk_score is the composite risk score defined in governance-risk-model.md clipped to [0,1].

Score Range Status
90–100 Excellent
70–89 Good
50–69 Warning
0–49 Critical

5. Reporting

· On‑chain: every snapshot includes a signed summary of the 10 most important metrics.
· Off‑chain: real‑time streaming via WebSocket to dashboards.
· Archival: historical metrics stored in a data lake (CSV + Parquet) for at least 2 years.

6. Alert Thresholds

EGTS defines three alert levels:

Level Trigger Example Action
Info Validator uptime < 95% Log, notify operator
Warning Snapshot age > 4h Display on dashboard, notify Security Council
Critical Invalid proof rate > 5% Pause adapter, initiate emergency validator meeting

7. Metric Oracles

Most metrics are derived directly from on‑chain data. For metrics requiring off‑chain data (e.g., geographic distribution), a set of trusted oracles (reputation‑based) submits signed reports. Any user can challenge an oracle report with a bond; successful challenges slash the oracle.
