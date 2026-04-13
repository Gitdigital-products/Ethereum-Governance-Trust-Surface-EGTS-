===== FILE: / adapters/v0.3.x/ethereum-l1-adapter.md =====

---

version: "0.3.x"
status: "Active Draft"
spec: "EGTS"
file: "/adapters/v0.3.x/ethereum-l1-adapter.md"

---

Ethereum L1 Governance Adapter

Purpose

The Ethereum L1 adapter translates on‑chain governance events (logs, transactions) from Ethereum mainnet into EGTS‑compliant JSON‑LD GovernanceEvent objects. It enables cross‑chain interoperability by normalising Ethereum‑specific data into the EGTS Validation Triad model.

Scope

· Event sources: Contract logs (EIP‑1155, EIP‑20 votes, custom governor logs).
· Finality consideration: Adapters SHOULD wait for a safe number of confirmations (e.g., 15 blocks) before emitting an event.
· Chain identifier: ethereum-mainnet

Mapping Rules

1. Role Extraction

On‑chain data EGTS Role field
msg.sender of transaction role.governingEntity (DID mapping)
voter parameter in VoteCast log role.governingEntity
Delegate mapping role.delegationChain

The adapter maintains a local DID mapping table (off‑chain configuration) that converts Ethereum addresses to DIDs (e.g., 0xAbC…123 → did:example:l1:0xAbC…123). No on‑chain registry is required.

2. Authority Extraction

On‑chain source EGTS Authority field
Contract address + ABI authority.source = ethereum:0x…
Governance parameters (quorum, delay) authority.constraints
Hard‑coded rules (e.g., constitution) authority.scope (configured off‑chain)

3. Context Extraction

Data EGTS Context field
block.number and block.timestamp context.temporalBounds
Chain ID (1) context.chainReference
Governance contract name context.governanceScope (e.g., "CompoundGovernor")

4. Event Type Mapping

Ethereum log event EGTS eventType
ProposalCreated ProposalCreated
VoteCast / VoteCastWithParams VoteCast
ProposalExecuted DecisionExecuted
RoleGranted (OpenZeppelin) RoleAssigned
RoleRevoked RoleRevoked

Adapter Lifecycle

```
[Ethereum Node] → (getLogs) → [Adapter] → (normalise) → [EGTS Event] → (store/forward)
```

Example (fictional)

Ethereum log (raw):

```
event VoteCast(address indexed voter, uint256 proposalId, uint8 support, uint256 votes)
```

Adapter output (EGTS GovernanceEvent):

```json
{
  "id": "urn:uuid:f81d4fae-7dec-11d0-a765-00a0c91e6bf6",
  "type": "GovernanceEvent",
  "eventType": "VoteCast",
  "timestamp": "2026-04-12T15:30:00Z",
  "role": {
    "roleId": "did:example:l1:role/0xAbC...123",
    "roleType": "Voter",
    "governingEntity": "did:example:l1:0xAbC...123"
  },
  "authority": {
    "authorityId": "did:example:l1:authority/governor-alpha",
    "source": "ethereum:0xGovernorAddress",
    "scope": "proposal execution"
  },
  "context": {
    "governanceScope": "ExampleProtocolGovernor",
    "chainReference": "ethereum-mainnet",
    "temporalBounds": {
      "start": "2026-04-12T15:30:00Z",
      "end": null
    }
  },
  "proposalId": "prop-42",
  "details": {
    "support": 1,
    "votes": "1000000000000000000"
  }
}
```

Validation Constraints

· The adapter MUST NOT create or assume a native token, staking, or validator set.
· The adapter MUST NOT inject APY or economic incentives.
· The adapter MUST respect EGTS’s rule: no on‑chain scoring of reputation.


