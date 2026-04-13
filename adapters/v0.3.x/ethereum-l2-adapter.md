  ===== FILE: / adapters/v0.3.x/ethereum-l2-adapter.md =====

---

version: "0.3.x"
status: "Active Draft"
spec: "EGTS"
file: "/adapters/v0.3.x/ethereum-l2-adapter.md"

---

Ethereum L2 Governance Adapter

Purpose

Extends the L1 adapter to support Ethereum Layer‑2 networks (Arbitrum, Optimism, Base, etc.). Handles differences in finality, block structure, and batch submission while producing identical EGTS GovernanceEvent objects.

Key Differences from L1

Feature L1 (Mainnet) L2 (Rollup)
Finality ~15 blocks (probabilistic) Defined by L2’s finalisation mechanism (e.g., after L1 batch inclusion)
Timestamp reliability block.timestamp Same, but may be subject to sequencer drift
Event emission Direct logs May be delayed until batch posted to L1
Chain identifier ethereum-mainnet arbitrum-one, optimism, base, etc.

Adapter Behaviour

Finality Handling

· The adapter MUST wait for L2 finality (e.g., an L2 block labelled “finalised” by the rollup node) OR for L1 inclusion of the batch containing the L2 transaction.
· A configurable minConfirmations parameter (default: 1) is allowed but does not imply staking or PBFT.

DID Mapping

Same as L1: convert L2 addresses (e.g., 0x... on Arbitrum) to DIDs with a chain‑specific namespace, e.g.:

· did:example:arbitrum:0x...
· did:example:optimism:0x...

Event Batching

If multiple governance events occur in one L2 block, the adapter MAY emit each as a separate EGTS event. Ordering MUST preserve the original transaction order.

Example (fictional Arbitrum event)

L2 log (Arbitrum):

```
event ProposalCreated(uint256 proposalId, address proposer, bytes32 descriptionHash)
```

Adapter output:

```json
{
  "id": "urn:uuid:550e8400-e29b-41d4-a716-446655440000",
  "type": "GovernanceEvent",
  "eventType": "ProposalCreated",
  "timestamp": "2026-04-12T10:15:00Z",
  "role": {
    "roleId": "did:example:arbitrum:role/0xDea...dBeef",
    "roleType": "Proposer",
    "governingEntity": "did:example:arbitrum:0xDea...dBeef"
  },
  "authority": {
    "authorityId": "did:example:l2:authority/arb-gov",
    "source": "arbitrum:0x123...",
    "scope": "treasury allocation"
  },
  "context": {
    "governanceScope": "ArbitrumTreasury",
    "chainReference": "arbitrum-one",
    "temporalBounds": {
      "start": "2026-04-12T10:15:00Z",
      "end": "2026-04-26T10:15:00Z"
    }
  },
  "proposalId": "prop-101",
  "details": {
    "descriptionHash": "0xabc...",
    "targets": ["0xTarget"]
  }
}
```

Cross‑L1/L2 Consistency

The EGTS standard expects that a governance event from an L2 adapter must not be treated as “less valid” than an L1 event. Both produce identical JSON‑LD schemas; only the chainReference differs. Trust signals (see trust‑signal schema) may later indicate finality confidence, but the base adapter does not assign trust scores.

Non‑Requirements

· No L2‑specific token or validator set.
· No assumption of fast finality (do not implement PBFT).
· No relayer incentives or staking bonds.

