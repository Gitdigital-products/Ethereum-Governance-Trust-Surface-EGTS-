=== FILE: docs/ecosystem-integration-templates.md ===

Ecosystem Integration Templates – EGTS

1. Purpose

This document provides ready‑to‑use templates for integrating EGTS into existing DAOs, L2s, grant programs, and public good systems. Templates are designed to be copied and minimally adapted.

2. DAO Integration Template

2.1 Smart Contract Modifications

Add to your DAO’s governor contract:

```solidity
import "./IEGTSVerifier.sol";

contract MyDAOGovernor {
    IEGTSVerifier public egts;
    
    function castVoteWithEGTS(uint256 proposalId, uint8 support) external {
        bytes32 contributorId = egts.getContributorId(msg.sender);
        require(egts.isActiveContributor(contributorId), "Not active EGTS contributor");
        uint256 weight = egts.getBadgeWeight(contributorId, "VOTER");
        _castVote(proposalId, msg.sender, support, weight);
    }
}
```

2.2 Off‑Chain Dashboard Widget

Embed EGTS health metrics using the provided iframe:

```html
<iframe src="https://dash.egts.eth/widget/dao/{dao_address}" width="400" height="300"></iframe>
```

3. L2 Integration Template

3.1 Adapter Configuration File

For a new L2, create adapter-config.json:

```json
{
  "chainId": 12345,
  "rpcUrl": "https://rpc.newl2.io",
  "governanceContracts": ["0xDAOGovernor", "0xBadgeIssuer"],
  "eventTopics": ["VoteCast(address,uint256,uint8)", "BadgeMinted(address,uint256)"],
  "proofTier": 1,
  "batchSize": 512,
  "submissionInterval": 3600
}
```

3.2 Deploy Adapter Script

```bash
npx egts-cli register-adapter --config adapter-config.json --bond 10 ETH
npx egts-cli start-adapter --chain-id 12345
```

4. Grant Program Integration Template

4.1 Grant Eligibility Check

```javascript
const { EGTSClient } = require("@egts/sdk");

async function checkGrantEligibility(contributorId) {
    const client = new EGTSClient("https://egts.l1.eth");
    const badges = await client.getBadges(contributorId);
    const hasRequired = badges.some(b => b.type === "GRANT_REVIEWER" && !b.expired);
    const active = await client.isActiveContributor(contributorId);
    return { eligible: hasRequired && active, badges };
}
```

4.2 Grant Reporting

Grant recipients must submit a proof‑of‑contribution every 30 days. Template email:

```
Subject: EGTS Proof of Contribution for Grant #[ID]

Dear [Name],

Please submit your contribution proof via https://egts.eth/contribute using your registered contributor ID.

If you have not yet registered, register at https://egts.eth/register.

Deadline: [Date]
```

5. Public Good System Template

5.1 Verifiable Public Good Badge

Issue a “Public Good Steward” badge automatically when a contributor:

· Has held any other badge for > 6 months.
· Has submitted > 10 contribution proofs.
· Is not slashed or suspended.

Smart contract snippet:

```solidity
function issuePublicGoodBadge(bytes32 contributorId) external {
    require(egts.getBadgeWeight(contributorId, "ANY") > 0, "No badges held");
    require(egts.getContributionCount(contributorId) >= 10, "Insufficient proofs");
    badgeAuthority.issue(contributorId, "PUBLIC_GOOD_STEWARD", expiry, metadataURI);
}
```

6. Cross‑Ecosystem Alignment Template

For projects that want to recognize EGTS badges from other ecosystems:

```solidity
function importBadge(bytes32 externalContributorId, uint256 externalChainId, bytes calldata proof) external {
    bytes32 root = egts.getSnapshotRoot(externalChainId);
    require(egts.verifyProof(proof, root), "Invalid cross-chain proof");
    // map externalContributorId to local contributor
    mapping_[msg.sender] = externalContributorId;
}
```

7. Migration Path Template

See docs/adoption-playbook/06-migration-paths.md for detailed migration steps from legacy governance systems.
