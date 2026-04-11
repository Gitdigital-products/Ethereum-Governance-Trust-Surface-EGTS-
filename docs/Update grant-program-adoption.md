=== FILE: docs/grant-program-adoption.md ===

Grant Program Adoption – EGTS

1. Overview

Grant programs (e.g., Ethereum Foundation, Gitcoin, DAO treasuries) can use EGTS to verify contributor track records, automate milestone verification, and reduce fraud. This document outlines adoption strategies.

2. Benefits for Grant Programs

· Verifiable history – No need to trust self‑reported resumes.
· Cross‑project reputation – Contributions to other projects are visible.
· Automated milestone checks – Proof‑of‑contribution triggers grant payouts.
· Slashing for misuse – Misbehaving contributors lose badges and future eligibility.

3. Integration Levels

Level Description Effort
Bronze Manual lookup of EGTS badges before awarding grant. Low
Silver Smart contract reads EGTS eligibility for automatic approval. Medium
Gold Grant payouts are conditional on continuous proof‑of‑contribution. High

4. Recommended Adoption Path

Phase 1: Pilot (1 month)

· Select 5–10 grant recipients.
· Ask them to register on EGTS.
· Manually check badge status before each milestone.

Phase 2: Light Integration (2 months)

· Deploy a grant contract that calls EGTS verifier.
· Require “GRANT_RECIPIENT” badge for application.
· Use EGTS to compute a risk score for each applicant.

Phase 3: Full Automation (3 months)

· Payouts are released only when contributor submits a fresh proof‑of‑contribution.
· Slashing conditions encoded: if contributor’s badge is revoked, remaining grant is clawed back.
· Publish anonymized grant data as EGTS metadata.

5. Grant Program Badge Types

EGTS defines standard badge types for grant programs:

Badge Issuer Meaning
GRANT_REVIEWER Grant program DAO Can review applications
GRANT_RECIPIENT Grant program DAO Received a grant
GRANT_MILESTONE_X Grant program DAO Completed milestone X
GRANT_FAILED EGTS slashing Contributor failed to deliver

6. Example: Automated Milestone Payout

```solidity
function claimMilestone(uint256 milestoneId, bytes calldata contributionProof) external {
    bytes32 contributorId = egts.getContributorId(msg.sender);
    require(egts.verifyProof(contributionProof, contributorId), "Invalid proof");
    require(hasBadge(contributorId, "GRANT_RECIPIENT"), "Not a grant recipient");
    // transfer milestone payment
}
```

7. Fraud Detection

EGTS validators run automated monitors that flag suspicious patterns:

· One contributor receiving many grants with low proof activity.
· Badge issuance concentrated in a small set of issuers.
· Rapid badge expiry and re‑issuance.

Suspicious reports are sent to grant program administrators.

8. Cost & Sustainability

EGTS does not charge grant programs for using the trust surface. However, programs are encouraged to:

· Allocate 0.5% of grant funds to EGTS public goods treasury (optional).
· Run an EGTS validator to support the network.
· Sponsor audits of new cross‑chain adapters.

9. Success Metrics

Grant programs adopting EGTS should track:

· Reduction in grant fraud incidents (target: 80%).
· Decrease in manual verification time (target: 90%).
· Increase in successful milestone completion (target: 20%).

Quarterly reports are shared with the EGTS community.
