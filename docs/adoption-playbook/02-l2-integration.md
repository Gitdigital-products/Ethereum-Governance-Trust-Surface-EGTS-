=== FILE: docs/adoption-playbook/02-l2-integration.md ===

EGTS Adoption Playbook – Chapter 02: L2 Integration

1. Scope

This chapter guides Layer‑2 networks (optimistic rollups, zk‑rollups, sidechains, validiums) in integrating the Ethereum Governance Trust Surface (EGTS). L2 integration focuses on cross‑domain vote bridging, fast finality for off‑chain governance, and fraud proof compatibility.

2. Minimum Viable Adoption Checklist

· L2 can produce a canonical EGTS Registry mirror (either native or bridged).
· L2’s governance actions (e.g., upgrade transactions) are published as EGTS artifacts.
· A bridge exists to relay votes from L2 to L1 (or vice versa) with verifiable inclusion proofs.
· L2’s fraud proof window (if applicable) accommodates EGTS dispute periods.

3. Architecture Patterns

3.1. Native L2 Registry

The L2 deploys its own EGTS Registry contract. L2 proposals and votes are recorded directly on L2. L1 systems trust the L2’s state root.

3.2. Bridged Registry

The L2 reads from and writes to the L1 EGTS Registry via a canonical bridge. Votes cast on L2 are batched and submitted to L1 periodically.

3.3. Hybrid Registry

Critical proposals (e.g., upgrades) are recorded on both L1 and L2; non‑critical ones live only on L2.

4. Step‑by‑Step Integration

4.1. Deploy Registry Endpoints

Implement the EGTS JSON‑RPC endpoints on the L2’s native RPC:

· egts_getProposal
· egts_simulateVote
· egts_relayVote

For zk‑rollups, these endpoints may query a local indexer that tracks L2 state.

4.2. Configure Vote Bridging

Choose a bridging mechanism:

Mechanism Latency Trust Assumption Use Case
Native L2 → L1 message 1‑7 days Rollup’s honest sequencer Optimistic rollups
ZK proof relay 1‑2 hours Prover correctness zk‑rollups
Light client bridge minutes Relayer set Sidechains

Implement a vote aggregator that collects L2 votes and submits a Merkle root to L1.

4.3. Fraud Proof Integration (Optimistic Rollups)

Ensure that EGTS vote inclusion can be challenged during the fraud proof window:

· Store vote commitments in a pre‑image oracle.
· Allow challengers to submit a fraud proof showing that an L2 vote was incorrectly included.

4.4. Fast Finality for Off‑Chain Votes

If the L2 uses off‑chain voting (e.g., for sequencer selection), the L2 must provide a finality oracle that signs vote results. The oracle’s public key is registered in the EGTS Registry.

5. Observable Level

The L2 publishes all governance actions (including upgrade proposals) to the EGTS Registry on L1, but does not accept votes from other domains.

Implementation:

· A trusted relayer reads L2 governance events and calls L1 EGTS Registry.
· No cross‑domain vote acceptance.

6. Interoperable Level

The L2 accepts votes from L1 and other L2s via the bridged registry. Requirements:

· L2’s voting contracts respect the timelock delay of bridged votes.
· A canonical message pass is used for vote submission.
· The L2 maintains a slashing mechanism for invalid vote inclusion.

7. Trust Surface Level

Full EGTS alignment requires:

· The L2’s governance lifecycle matches the EGTS narrative (including formal review).
· L2’s trust assumptions are fully declared and audited.
· Dispute resolution can pause L2 upgrades if an EGTS challenge succeeds.

8. Example: Optimistic Rollup with EGTS

· Registry – L1 EGTS Registry used as source of truth.
· Vote submission – Users vote on L2; votes are batched and submitted to L1 every 6 hours.
· Challenge – A 7‑day fraud proof window. If an L2 vote batch is challenged and proven invalid, the batch is discarded and the sequencer is slashed.
· Execution – After the window passes, the proposal can be executed on L1 via timelock.

9. Security Considerations

· Replay attacks – Include chainId in every vote signature.
· Bridge censorship – Use multiple relayers with rotation.
· Fast finality without proof – Do not accept L2 votes as final on L1 until the challenge window expires.

10. Testing Checklist

Before mainnet deployment, test:

· L2 → L1 vote bridging with at least 100 simulated votes.
· Fraud proof challenge of an invalid batch.
· Timelock delay enforcement for bridged proposals.
· Recovery after bridge downtime (e.g., manual resubmission).

11. Resources

· EGTS L2 Adapter smart contract: contracts/adapters/L2EGTSBridge.sol
· Reference implementation: Optimism EGTS fork (tag egts-v1)
