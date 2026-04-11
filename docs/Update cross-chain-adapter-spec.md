=== FILE: docs/cross-chain-adapter-spec.md ===

Cross‑Chain Adapter Specification – EGTS

1. Purpose

The Cross‑Chain Adapter imports governance signals from Ethereum L2s, sidechains, and non‑Ethereum ecosystems, normalizes them into EGTS canonical events, and produces verifiable proofs of their inclusion on the source chain.

2. Architecture

Each adapter is specific to a source chain (e.g., Arbitrum, Optimism, Polygon, Solana). Adapters consist of:

· Listener – Indexes governance contracts and events.
· Normalizer – Maps source events to EGTS event schema.
· Proof generator – Creates Merkle or zk‑proofs of event inclusion.
· Relayer – Submits proofs and normalized events to EGTS L1 contracts.

3. Canonical Event Schema

All normalized events share the same JSON structure:

```json
{
  "eventType": "VOTE" | "BADGE_ISSUE" | "REGISTRATION" | "PROPOSAL",
  "sourceChainId": 42161,
  "sourceTxHash": "0x...",
  "sourceBlockNumber": 12345678,
  "timestamp": 1698765432,
  "contributorId": "0x...",
  "payload": { ... }  // type-specific fields
}
```

4. Verification Mechanism

For each batch of events (e.g., per 1000 blocks), the adapter produces:

· A Merkle tree of the normalized event hashes.
· A light‑client proof (or ZK proof) that the source chain’s block header is canonical.
· A signature from the adapter operator over the root.

EGTS validators verify:

1. The source chain block header is valid (via light client).
2. The Merkle root matches the batch.
3. The normalized event hashes are correctly derived.

4. Adapter Security Tiers

Tier Verification Method Trust Assumption
1 Light client + Merkle None (fully trustless)
2 Oracle + ZK proof Oracle honesty for headers
3 Relayed signatures Adapter honesty (audited)

EGTS recommends Tier 1 for high‑value governance (e.g., treasury decisions). Tier 3 may be used for informational metrics.

6. Adapter Lifecycle

· Registration – Adapter operator posts bond and registers chain ID, verification contract, and endpoint.
· Monitoring – EGTS validators challenge invalid proofs; successful challenges slash the bond.
· Upgrade – Adapter logic changes require a 14‑day timelock and on‑chain approval by EGTS governance.

7. Example: Arbitrum Adapter

· Listens to ArbitrumGovernor contract events.
· Normalizes VoteCast to EGTS VOTE event.
· Uses Arbitrum’s nitro light client (already verified on L1) to prove block inclusion.
· Submits proof every 512 blocks to L1 EGTS contract.

8. Failure Handling

If an adapter fails to submit proofs for 2 consecutive epochs:

· The adapter is flagged as unhealthy.
· Events are no longer accepted from that adapter.
· A replacement adapter can be registered by any operator after 7 days.
