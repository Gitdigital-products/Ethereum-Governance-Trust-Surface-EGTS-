=== FILE: docs/adoption-playbook/01-dao-integration.md ===

EGTS Adoption Playbook – Chapter 01: DAO Integration

1. Scope

This chapter covers integrating the Ethereum Governance Trust Surface (EGTS) into DAOs (Decentralized Autonomous Organizations). It applies to DAOs using on‑chain voting (Governor contracts, Aragon, Zodiac) and off‑chain voting (Snapshot, Discord‑based).

2. Minimum Viable Adoption Checklist

Before starting, ensure these are complete:

· DAO has a published charter (even if minimal).
· DAO’s governance actions can be represented as EGTS proposals.
· At least one member can sign off‑chain EGTS artifacts.
· DAO agrees to publish all past and future proposals to the EGTS Registry.

3. Step‑by‑Step Integration

3.1. Registry Setup

Deploy or use an existing EGTS Registry contract. For most DAOs, the public EGTS Registry (address 0xEGTS...) is sufficient.

```solidity
// Example: Wrapping existing Governor contract to emit EGTS events
contract EGTSAwareGovernor is Governor {
    function propose(...) public override returns (uint256) {
        uint256 proposalId = super.propose(...);
        emit EGTSProposalCreated(proposalId, msg.sender, ...);
        return proposalId;
    }
}
```

3.2. Artifact Publishing

For each proposal, publish the following to IPFS and register the CID in the EGTS Registry:

· proposal_draft.json – Proposal metadata.
· vote_tally.json – Final vote results (after voting ends).
· resolution.json – Outcome and execution data.

Use the EGTS CLI tool:

```bash
egts publish --proposal-id 42 --artifact-type draft --file proposal.md
```

3.3. Vote Integration

If the DAO uses on‑chain voting, the EGTS Registry automatically reads vote events. For off‑chain voting (Snapshot), run a relayer that submits signed votes to the EGTS off‑chain endpoint.

3.4. Timelock Enforcement

For on‑chain DAOs, ensure that all executable proposals pass through a timelock controller that respects EGTS minimum delay (48 hours). If the DAO lacks a timelock, wrap execution:

```solidity
function executeWithEGTSDelay(uint256 proposalId) external {
    require(block.timestamp >= voteEndTime + 48 hours, "EGTS: delay not met");
    _execute(proposalId);
}
```

4. Observable Level (Basic)

At Observable level, the DAO only publishes registry entries. No further changes to voting mechanics are required.

Deliverables:

· A script that posts proposal artifacts to IPFS and calls EGTS Registry.
· A public page listing all EGTS‑registered proposals.

5. Interoperable Level

The DAO’s votes are accepted by other EGTS‑aware systems. Requires:

· Implementing the EGTS vote submission endpoint (or using a standard relayer).
· Publishing a trust_assumptions.json file.
· Adding a cross‑DAI delegate mechanism (optional).

Example trust assumptions:

```json
{
  "voting_system": "Snapshot with 1 token = 1 vote",
  "trusted_relayers": ["0xRelayer1", "0xRelayer2"],
  "failure_mitigation": "Manual override by multisig"
}
```

6. Trust Surface Level

Full alignment with EGTS Charter. The DAO must:

· Adopt the complete EGTS governance lifecycle (Initiation → Refinement → Review → Voting → Execution → Review).
· Have a formal review committee (or delegate that role to an EGTS‑recognized body).
· Publish quarterly alignment monitoring reports.

7. Example: Integrating a Snapshot‑Based DAO

8. Registry – Create an EGTS Registry entry for the DAO’s ENS name.
9. Proposal creation – When a Snapshot proposal is created, a bot calls egts publish with the Snapshot IPFS hash.
10. Vote tally – After voting ends, the bot fetches the final tally and publishes vote_tally.json.
11. Execution – The DAO’s multisig signs a resolution referencing the EGTS proposal ID before executing.

12. Common Pitfalls

· Forgetting to update registry after off‑chain voting – Automate with webhooks.
· Using different proposal IDs – Map Snapshot IDs to EGTS IDs in a public table.
· Missing timelock – Add a wrapper contract; do not rely on social delay alone.

9. Migration from Legacy DAO Framework

If your DAO uses Aragon, DAOstack, or Colony:

· Use the EGTS adapter plugin (see adoption-playbook/06-migration-paths.md).
· The adapter translates native proposals into EGTS events without modifying core contracts.

10. Success Metrics

A successful DAO integration has:

· 100% of new proposals registered within 1 hour of creation.
· At least 80% of votes submitted via EGTS‑compatible interface after 3 months.
· Zero unresolved registry discrepancies older than 7 days.
