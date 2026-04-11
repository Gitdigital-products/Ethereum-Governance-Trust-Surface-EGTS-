=== FILE: docs/governance-failure-modes.md ===

Governance Failure Modes – EGTS

1. Purpose

Identifies potential failure modes in the EGTS governance process, their root causes, and recommended mitigations. Used for risk assessment and contingency planning.

2. Failure Mode Taxonomy

Category Examples
Coordination Validator disagreement, fork ambiguity
Liveness Adapter downtime, proof submission halt
Safety Double voting, invalid proofs accepted
Capture Badge authority takeover, validator collusion
External Bridge hack, L2 reorg, eclipse attack

3. Detailed Failure Modes

3.1 Validator Set Censorship

Description: A majority of validators refuse to include proofs from a specific adapter.
Mitigation: Rotating leader with forced inclusion; any validator can force‑submit a proof with a higher fee.
Recovery: Affected adapter can route proofs through a different validator set (backup).

3.2 Badge Authority Private Key Leak

Description: The multi‑sig controlling badge issuance is compromised.
Mitigation: Use a 7-of-11 multi‑sig with hardware wallets and key ceremony.
Recovery: Emergency rotation to a new multi‑sig; revoke all badges issued in the last 24h.

3.3 Cross‑Chain Adapter Double Inclusion

Description: Adapter submits the same event twice, inflating badge counts.
Mitigation: Adapter must include source transaction hash; duplicate detection in EGTS contract.
Recovery: Slash adapter, recalculate affected snapshots.

3.4 Long‑Range Reorg on Source Chain

Description: An L2 reorg invalidates previously submitted proofs.
Mitigation: EGTS only accepts proofs after source chain finality (e.g., 200 blocks).
Recovery: Validators revert snapshots to before the reorg, slash adapter if it submitted before finality.

3.5 Governance Capture via Stake Accumulation

Description: One entity acquires >33% of validator stake, enabling veto.
Mitigation: Stake上限 (max 20%) enforced by smart contract.
Recovery: Community‑initiated fork that enforces stake redistribution.

3.6 Proof Verification Bug

Description: A bug in the proof verifier accepts invalid proofs.
Mitigation: Formal verification + bug bounty.
Recovery: Pause the verifier, upgrade contract, re‑verify all snapshots since the bug was introduced.

4. Failure Detection

EGTS includes an automated watchtower that runs every 15 minutes and checks:

· Validator equivocation (via fork detection).
· Adapter submission liveness.
· Unusual badge issuance patterns.

Alerts are sent to a public Discord channel and on‑chain via Alert event.

5. Emergency Response Procedures

Failure Response Owner
1/3 validators down Emergency validator rotation Security Council
Adapter hack Freeze adapter, redeploy EGTS core devs
Badge authority compromise Revoke all badges, rotate keys Security Council
Long‑range reorg Revert snapshots, notify users Validator set

All emergency actions must be followed by a post‑mortem within 7 days.

6. Testing & Simulation

EGTS maintains a failure simulation environment (testnet) where each failure mode is injected quarterly. Results are published and used to update the mitigation playbook.
