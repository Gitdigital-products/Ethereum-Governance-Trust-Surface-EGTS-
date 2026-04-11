=== FILE: docs/adoption-playbook/05-cross-ecosystem-alignment.md ===

EGTS Adoption Playbook – Chapter 05: Cross‑Ecosystem Alignment

1. Scope

This chapter addresses aligning governance across multiple independent systems (e.g., multiple DAOs, L1 and L2 networks, grant programs and protocol clients) using EGTS as a common trust surface. Cross‑ecosystem alignment enables composable governance, where a decision in one system can automatically affect or be recognized by another.

2. Minimum Viable Adoption Checklist

· At least two distinct systems have achieved EGTS Interoperable level (per previous chapters).
· Each system has published its trust assumptions and alignment statement.
· A cross‑system dispute resolution mechanism exists (or participants agree to use EGTS default).
· A common registry of aligned systems is maintained.

3. Alignment Patterns

3.1. Vote Mirroring

System A’s vote outcome is automatically accepted as a binding vote in System B. Use case: A DAO votes to allocate funds to a grant program, and the grant program automatically processes the allocation.

Implementation: System B’s governance contract reads vote tally from System A’s EGTS registry entry.

3.2. Delegate Cross‑System Voting

An address delegates its voting weight to a representative in multiple systems simultaneously. The delegate votes once, and the vote is replicated across all systems that accept the delegation.

Implementation: A delegation registry that maps a master delegate to system‑specific delegates.

3.3. Sequenced Governance

System A must vote before System B can act. For example, an L2 upgrade requires approval from both the L2 DAO and the L1 community.

Implementation: System B’s proposal references System A’s EGTS proposal ID and verifies that System A’s state is executed before proceeding.

3.4. Mutual Veto

Two systems can veto each other’s proposals within a defined window. Use case: A grant program can veto a protocol change that would break its funded projects.

Implementation: A veto registry where each system registers a veto contract. Before execution, any registered system may call veto(proposalId).

4. Step‑by‑Step Alignment

4.1. Establish a Cross‑System Trust Anchor

Deploy an EGTS Federation Contract that lists all aligned systems and their registry addresses. Systems must opt in by calling joinFederation().

```solidity
interface IEGTSFederation {
    function joinFederation(bytes32 systemId, address registry) external;
    function isAligned(address registry) external view returns (bool);
    function getTrustAssumptions(bytes32 systemId) external view returns (bytes32);
}
```

4.2. Define Cross‑System Proposal Schema

A cross‑system proposal must include a cross_domain field:

```json
{
  "proposal_id": "egts:0x...",
  "cross_domain": {
    "target_systems": ["L2 Optimism", "Grant Program EF"],
    "required_approvals": 2,
    "execution_order": ["parallel", "sequential"],
    "timeout": 604800
  }
}
```

4.3. Implement Vote Aggregation Across Systems

Use a cross‑system vote aggregator that collects vote tallies from each system’s EGTS registry, normalizes weights, and determines the final outcome.

Normalization rules must be declared in the proposal.

4.4. Handle Conflicting Outcomes

If systems return conflicting vote results, the proposal enters a mediation period (default 7 days). During mediation:

· Any aligned system may propose a compromise.
· If no compromise is reached, the proposal fails.
· The dispute is logged in the EGTS Registry.

5. Example: L1 / L2 Mutual Alignment

· L1 (Ethereum mainnet) and L2 (Arbitrum) both achieve EGTS Interoperable level.
· They deploy a federation contract.
· L1 proposes a fee market change that would affect L2’s bridge costs.
· L2 automatically mirrors the vote: if L1 passes, L2’s governance considers the same proposal with a reduced quorum (50% of L1’s quorum).
· L2 can veto the L1 proposal within 48 hours by submitting a veto resolution.

6. Failure Modes Specific to Cross‑System Alignment

Failure Mode EGTS Mitigation
Inconsistent vote normalization Proposals must declare normalization; federation validates
One system fails to submit tally Timeout: proposal proceeds without that system’s vote
Malicious federation contract Rotation of federation signers; on‑chain upgrade delay
Cross‑system governance attack Mutual veto and dispute mediation

7. Alignment Maturity Levels

· Level 1 – Ad Hoc – Manual coordination; no automated cross‑system voting.
· Level 2 – Mirrored – Votes are automatically mirrored one‑way.
· Level 3 – Reciprocal – Two‑way mirroring and mutual veto.
· Level 4 – Federated – Multiple systems with sequenced governance and dispute mediation.

8. Tools

· EGTS Federation UI – Dashboard to join federations and monitor cross‑system proposals.
· Cross‑system relayer – Automatically submits votes from one system’s registry to another.
· Mediation bot – Escalates conflicting outcomes to a human‑readable dispute board.

9. Success Metrics

· Alignment coverage – Number of systems in federation / total eligible systems.
· Cross‑system proposal latency – Time from proposal creation to final execution across all systems.
· Mediation success rate – Percentage of conflicts resolved without hard failure.

10. Getting Started

11. Identify two or more systems that already trust each other informally.
12. Help each system reach EGTS Interoperable level (Chapters 01‑04).
13. Deploy the EGTS Federation contract on a neutral chain (or L1).
14. Run a pilot cross‑system proposal (e.g., joint funding of a public good).
15. Document lessons learned and expand.
