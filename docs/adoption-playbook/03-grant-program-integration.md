=== FILE: docs/adoption-playbook/03-grant-program-integration.md ===

EGTS Adoption Playbook – Chapter 03: Grant Program Integration

1. Scope

This chapter explains how grant programs (crypto‑native and traditional) can adopt the Ethereum Governance Trust Surface (EGTS) to make funding decisions transparent, auditable, and composable with other governance systems.

2. Minimum Viable Adoption Checklist

· Grant program has a public list of decision‑makers (addresses or legal entities).
· Each grant round produces a final allocation list.
· Program can publish decision artifacts to IPFS.
· At least one EGTS‑compatible signer exists (multi‑sig or EOA).

3. Adoption Levels for Grant Programs

3.1. Observable (Reporting Only)

· After each grant round, publish an EGTS resolution containing all awarded grants.
· Resolution must be signed by the grant program’s EGTS signer.
· No changes to internal selection process.

Output: grant_round_resolution.json

```json
{
  "round_id": "QmRound123",
  "program": "0xGrantProgramMultisig",
  "start_date": "2026-04-01",
  "end_date": "2026-04-30",
  "allocations": [
    {"recipient": "0xAlice", "amount": "5000", "token": "DAI", "memo": "L2 research"},
    {"recipient": "0xBob", "amount": "3000", "token": "DAI", "memo": "Documentation"}
  ],
  "decision_process": "Committee vote, threshold 4/5",
  "signatures": ["0xSig1", "0xSig2"]
}
```

3.2. Interoperable (EGTS Voting)

Grant decisions are made using EGTS vote tracks:

· Proposals for grants are submitted as EGTS proposals.
· Committee members vote using EGTS‑compatible interface (on‑chain or off‑chain).
· Final tally is published automatically by the EGTS registry.

Benefit: Third parties can verify that the allocation matches the vote.

3.3. Trust Surface (Full Lifecycle)

· Grant program follows the entire EGTS governance lifecycle (Initiation → Refinement → Review → Voting → Execution → Review).
· Refinement period allows public comment on grant applications.
· Formal review by an independent committee assesses technical merit.
· Execution includes on‑chain streaming or milestone‑based vesting.

4. Step‑by‑Step Integration

4.1. Register the Grant Program

Create an EGTS registry entry for the program:

```bash
egts register --name "Ethereum Foundation Grants" --address 0xEFMultisig --chain-id 1
```

4.2. Define Grant Round as EGTS Proposal

For each round, create a meta‑proposal that references all individual grant applications.

4.3. Automate Artifact Publishing

Use a webhook or cron job to:

· Listen for new grant applications (from a GitHub repo or Google Form).
· Convert each application to an EGTS draft proposal.
· After committee vote, publish the final resolution.

4.4. Link to Payouts

If the grant program uses on‑chain payouts, the payout transaction MUST reference the EGTS resolution IPFS hash.

Example:

```solidity
function payout(address recipient, uint256 amount, bytes32 resolutionHash) external onlyRole(PAYER) {
    require(egtsRegistry.isValidResolution(resolutionHash), "Invalid resolution");
    token.transfer(recipient, amount);
}
```

5. Handling Confidential Applications

Some grant applications may be confidential. For those:

· Publish a hash of the application instead of full content.
· Include a confidentiality_note in the artifact.
· Make the full application available to reviewers via a secure channel.

EGTS does not require public disclosure of sensitive information, but the hash provides integrity.

6. Example: Gitcoin Grants Round Integration

Gitcoin Grants (quadratic funding) can adopt EGTS at Observable level:

1. After a round ends, a script fetches final allocations from the Gitcoin contract.
2. The script generates an EGTS resolution.
3. The resolution is signed by the Gitcoin multisig.
4. The resolution is published to IPFS and registered in EGTS.

External parties can now verify that the published allocation matches the on‑chain payouts.

7. Compliance with Legal Requirements

Grant programs subject to legal jurisdiction may need to:

· Redact personal information from public artifacts (use hash commitment).
· Keep a separate private log for regulatory review.
· Use the EGTS “limited alignment” declaration to avoid claiming full decentralization.

8. Metrics for Success

· Transparency score – Percentage of grant decisions with published EGTS resolution.
· Auditability – Time to reproduce a past round’s allocation from registry data (target <1 hour).
· Dispute resolution – Number of successfully resolved disputes using EGTS process.

9. Failure Modes (Grant‑Specific)

See governance-failure-modes.md for general failures. Grant‑specific mitigations:

Failure Mitigation
Reviewer collusion Public vote tallies, mandatory conflict disclosure
Incomplete payout Resolution includes vesting schedule; on‑chain clawback
Grant fraud Milestone reviews as post‑execution artifacts

10. Tools and Templates

· EGTS Grant CLI – egts grant publish-round
· Template: Grant Resolution – templates/grant_resolution.json
· Sample webhook – Python script in egts-examples/gitcoin-adapter.py
