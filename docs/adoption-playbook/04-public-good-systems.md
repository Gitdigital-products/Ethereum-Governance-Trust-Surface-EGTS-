=== FILE: docs/adoption-playbook/04-public-good-systems.md ===

EGTS Adoption Playbook – Chapter 04: Public Good Systems

1. Scope

Public good systems include Ethereum protocol clients (Geth, Nethermind, etc.), core dev teams, EIP editors, and infrastructure providers (e.g., RPC nodes, relayers). These systems often lack formal on‑chain governance. This chapter adapts EGTS for off‑chain, social‑layer governance while maintaining verifiability.

2. Minimum Viable Adoption Checklist

· System has a publicly documented decision‑making process (e.g., “All Core Devs calls”).
· Decisions are recorded in a public archive (GitHub, Discourse, or similar).
· At least one maintainer can sign EGTS artifacts.
· System agrees to publish decision artifacts to the EGTS Registry.

3. Characteristics of Public Good Governance

· No binding on‑chain votes – Decisions are social consensus.
· Rotating participants – Different individuals attend different calls.
· Long feedback periods – Changes may take months.
· Minimal tooling – Often using spreadsheets and mailing lists.

EGTS provides structure without forcing on‑chain voting.

4. Adoption Levels

4.1. Observable

For each decision (e.g., “include EIP-XXXX in next hard fork”), publish an EGTS resolution signed by at least two maintainers.

Resolution example:

```json
{
  "decision_id": "ACD-152-decision-3",
  "system": "Ethereum Core Protocol",
  "decision": "Include EIP-XXXX in Prague upgrade",
  "supporting_material": "https://github.com/ethereum/pm/issues/123",
  "vote_tally": "For: 8, Against: 2, Abstain: 1",
  "timestamp": "2026-04-11T14:00:00Z",
  "signers": ["0xMaintainer1", "0xMaintainer2"]
}
```

4.2. Interoperable

The system accepts EGTS proposals from external parties (e.g., a grant program suggesting a protocol change). Process:

1. External party submits an EGTS proposal.
2. The system’s maintainers review it during a public call.
3. A decision resolution is published, referencing the external proposal ID.

4.3. Trust Surface

Full lifecycle with formal review and off‑chain voting:

· Initiation: Anyone creates an EGTS draft.
· Refinement: Discussed on GitHub / forum for at least 14 days.
· Formal review: A standing committee (e.g., EIP editors) assesses.
· Voting: Maintainers vote using EIP‑712 signatures aggregated by a trusted coordinator.
· Execution: Decision is implemented (code merged, call notes updated).
· Review: Retrospective published after 30 days.

5. Step‑by‑Step Integration for Client Teams

5.1. Create an EGTS Identity

Register the client team’s GitHub team or multi‑sig as an EGTS actor:

```bash
egts register --name "Geth Team" --type team --verification github:ethereum/go-ethereum
```

5.2. Automate Decision Capture

Write a GitHub Action that:

· Watches for new issues labeled decision.
· When an issue is closed, prompts the assignee to sign a decision resolution.
· Publishes the resolution to IPFS and EGTS Registry.

5.3. Integrate with All Core Devs Calls

· Before each call, a bot creates an EGTS draft proposal for each agenda item.
· During the call, the chair marks decisions on the draft.
· After the call, the bot publishes resolutions for items where rough consensus was reached.

6. Example: EIP Editor Decision

EIP editors decide to reject an EIP. Observable level:

1. Editor writes rejection reason in GitHub comment.
2. Editor runs egts publish with decision type eip-rejection.
3. The published artifact includes the EIP number, reason, and editor’s signature.
4. Anyone can verify that the decision was made by a recognized editor.

5. Handling Disputes

Because public good systems lack on‑chain slashing, disputes rely on social reputation:

· Any community member can file an EGTS dispute against a resolution.
· The dispute is published in the registry.
· If the dispute is unresolved after 30 days, the system’s alignment status is suspended.

8. Tools for Low‑Tech Environments

· EGTS Email Gateway – Submit proposals via email; gateway publishes to registry.
· Discourse Plugin – Adds “Publish to EGTS” button on forum posts.
· Google Sheets Add‑on – Export rows as EGTS resolutions.

9. Success Metrics

· Decision latency – Time between rough consensus and published resolution (target <48 hours).
· Adoption coverage – Percentage of decisions (by importance) that produce EGTS artifacts.
· External proposals – Number of non‑maintainer submissions using EGTS.

10. Limitations and Caveats

· EGTS does not enforce social consensus – it only records it.
· The “Trust Surface” level requires significant process change; most public goods start at Observable.
· Signer key management: Use hardware wallets or threshold signatures for team decisions.
