===== FILE: / adapters/v0.3.x/offchain-governance-adapter.md =====

---

version: "0.3.x"
status: "Active Draft"
spec: "EGTS"
file: "/adapters/v0.3.x/offchain-governance-adapter.md"

---

Offchain Governance Adapter

Purpose

Enables EGTS to ingest governance events from off‑chain systems – forums, voting platforms, decision logs, or legal documents – that do not use a blockchain. The adapter normalises these events into the same JSON‑LD GovernanceEvent schema as on‑chain adapters.

Supported Source Types

· Forum threads (e.g., Discourse, GitHub Discussions) – converted to ProposalCreated.
· Polling platforms without token voting (e.g., anonymous straw polls, ranked choice) – converted to VoteCast.
· Off‑chain decision registers (e.g., executive minutes) – converted to DecisionExecuted.
· Role assignment documents (e.g., org chart) – converted to RoleAssigned.

Data Collection Methods

The adapter does not prescribe APIs. Implementers MAY:

· Scrape HTML (with proper terms of service).
· Read structured files (CSV, JSON) from a shared drive.
· Accept manual entry via a UI (no endpoint defined).

Mapping Rules

From Forum Thread to ProposalCreated

Off‑chain field EGTS property
Thread URL id (minted as URN or DID)
Author username/email role.governingEntity (DID after mapping)
Post timestamp timestamp
Title + body details (free object)
Forum rules authority.source (URL to guidelines)

From Poll to VoteCast

Off‑chain field EGTS property
Voter identifier (pseudonym) role.governingEntity
Option selected details.choice
Timestamp of vote timestamp
Poll reference proposalId

Authority & Context

Because off‑chain systems lack native on‑chain authorities, the adapter MUST reference a documented authority (e.g., a constitution PDF, a terms of service) and a governance context defined in the EGTS registry.

Example (fictional Discourse thread)

Source: Discourse topic #456 titled “Increase validator bond to 32 ETH” – Note: This is an example; EGTS does not endorse staking. The example uses a fictional protocol that discusses bonds as a parameter.

Adapter output (after normalisation):

```json
{
"id": "urn:uuid:11111111-2222-3333-4444-555555555555",
"type": "GovernanceEvent",
"eventType": "ProposalCreated",
"timestamp": "2026-04-01T09:00:00Z",
"role": {
  "roleId": "did:example:offchain:role/forum-user-jdoe",
  "roleType": "Proposer",
  "governingEntity": "did:example:person/jdoe"
},
"authority": {
  "authorityId": "did:example:offchain:authority/forum-code-of-conduct",
  "source": "https://forum.example.org/tos",
  "scope": "all parameter change proposals"
},
"context": {
  "governanceScope": "ExampleProtocol Forum",
  "chainReference": null,
  "temporalBounds": {
    "start": "2026-04-01T09:00:00Z",
    "end": "2026-04-15T09:00:00Z"
  }
},
"proposalId": "forum-topic-456",
"details": {
  "title": "Increase validator bond to 32 ETH",
  "body": "This proposal suggests raising the bond requirement...",
  "author_handle": "@jdoe"
}
}
```

Trust Implications

Off‑chain events lack cryptographic finality. The EGTS trust‑signal pipeline SHOULD be used to attach Validity or Accuracy signals from trusted observers. The adapter itself does not assign trust.

Non‑Requirements

· No on‑chain verification.
· No KYC or real‑identity linking (pseudonyms allowed).
· No reputation scoring or badge system.
· No token weighting – votes are treated as unit votes or as recorded in the source.

