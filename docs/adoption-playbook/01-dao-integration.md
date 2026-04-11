=== FILE: docs/adoption-playbook/01-dao-integration.md ===

Adoption Playbook – DAO Integration

1. Overview

This chapter guides DAOs through integrating EGTS to verify contributor identities, use badge‑based voting, and publish governance events to the trust surface.

2. Integration Levels

Level Features
Bronze Manual badge checks before voting or proposals.
Silver Smart contract reads EGTS badge weights for voting.
Gold Full bidirectional trust: DAO events published to EGTS, EGTS badges accepted as voting power.

3. Bronze Integration (1‑2 weeks)

Steps:

1. Register your DAO on the EGTS dashboard.
2. Encourage members to register as EGTS contributors.
3. Manual check: Before a vote, query https://dash.egts.eth/contributor/{address} to see badges.
4. Record badge snapshots in your DAO’s off‑chain records.

Cost: None (only manual labor).

4. Silver Integration (2‑4 weeks)

4.1 Modify Voting Contract

Add EGTS verifier as shown in ecosystem-integration-templates.md (DAO Integration Template).

4.2 Define Badge Weights

Decide which EGTS badge types grant voting power. Example:

Badge Weight
VOTER 1
DELEGATE 2
STEWARD 5

4.3 Deploy & Test

· Deploy modified governor on testnet.
· Run a small vote with EGTS‑based weights.
· Monitor gas costs (typically +50k gas per vote).

4.4 Migration

After testing, propose a DAO vote to upgrade the governor contract.

5. Gold Integration (1‑2 months)

5.1 Publish DAO Events to EGTS

Deploy a cross‑chain adapter that listens to your DAO’s events and submits them to EGTS. Use the L2 integration template as a base (even if your DAO is on L1).

5.2 Accept Incoming Badges

Allow external EGTS badges to delegate voting power to your DAO’s members. Example:

```solidity
function acceptExternalBadge(bytes32 contributorId, bytes32 badgeType) external {
    require(egts.verifyBadge(contributorId, badgeType), "Invalid badge");
    externalBadgeWeight[contributorId][badgeType] = getWeightForBadge(badgeType);
}
```

5.3 Participate in EGTS Governance

Your DAO should designate a representative to vote on EGTS improvement proposals that affect your integration.

6. Common Pitfalls

· Gas overhead – Batch badge checks using Merkle proofs.
· Stale badges – Cache badge data with expiry (max 1 hour).
· Sybil resistance – Combine EGTS badges with a minimal stake.

7. Success Metrics

Metric Target
% of members registered on EGTS 80%
Vote participation rate +20% increase
Time to verify voter eligibility < 1 second

8. Checklist

· DAO registered on EGTS dashboard.
· At least one member has an EGTS badge.
· Voting contract modified (Silver) or full adapter deployed (Gold).
· Documentation updated.
· Announcement to community.
