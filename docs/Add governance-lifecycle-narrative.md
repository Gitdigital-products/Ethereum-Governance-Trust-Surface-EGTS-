=== FILE: docs/governance-lifecycle-narrative.md ===

Governance Lifecycle Narrative – EGTS

1. Introduction

This document describes the end‑to‑end lifecycle of governance signals within the EGTS, from raw on‑chain activity to final verified trust surface consumption.

2. Phase 1: Signal Emission

A contributor interacts with a governance contract on any supported chain. Example events:

· Voting on a DAO proposal (Ethereum mainnet).
· Delegating voting power (Arbitrum).
· Issuing a badge (Optimism).
· Registering as a contributor (EGTS native registry).

Each event is emitted as a log with the contributor’s public key or contributor ID.

3. Phase 2: Collection & Normalization

Cross‑chain adapters listen to these events. For each batch of blocks:

1. The adapter fetches logs from the source chain.
2. It normalizes each event into the EGTS canonical event schema.
3. It constructs a Merkle tree of the normalized event hashes.
4. It generates a light‑client proof (or ZK proof) that the events were included in the source chain.

The adapter submits the Merkle root and proof to the EGTS L1 contract.

4. Phase 3: Validation & Attestation

EGTS validators:

· Retrieve the submitted root and proof.
· Verify the proof against the source chain’s latest finalized header.
· If valid, they sign the root using BLS aggregation.
· Once signatures from >2/3 of validators are collected, the root is finalized on‑chain as a “trust snapshot”.

5. Phase 4: Badge & Registry Integration

The Badge Authority and Contributor Registry query the latest trust snapshot to:

· Update badge balances based on newly issued badges.
· Mark contributors as inactive if proof‑of‑contribution is missing.
· Apply slashing or rewards based on validator performance.

These updates are themselves recorded as EGTS internal events and included in future snapshots.

6. Phase 5: Consumption

External systems (dashboards, risk models, insurance protocols) read from the EGTS contracts:

· They request a Merkle proof for a specific contributor’s badges.
· The EGTS contract returns the proof against the latest snapshot root.
· The consumer verifies the proof locally and trusts the data if valid.

7. Phase 6: Dispute & Recovery

If a consumer detects an inconsistency:

· They submit a challenge transaction with proof of error.
· Validators examine the challenge within 24 hours.
· If valid, the offending adapter or validator is slashed, and a new snapshot is produced correcting the error.
· The challenger receives a 10% reward from the slashed bond.

8. Cycle Repetition

The entire lifecycle repeats every epoch (6 hours). Historical snapshots are stored on‑chain for at least 3 months, then pruned but available via off‑chain archive nodes.

9. Example Walkthrough

Step Actor Action
1 Alice (contributor) Votes “Yes” on L2 proposal.
2 L2 adapter Detects event, normalizes, submits Merkle proof.
3 Validators Verify, sign, finalize snapshot.
4 Badge Authority Mints “Active Voter” badge to Alice.
5 Dashboard Queries Alice’s badges, verifies proof, displays.
6 Bob (challenger) Sees mismatch, challenges, wins reward.
