===== FILE: / registry/v0.3.x/governance-role-registry.md =====

---

version: "0.3.x"
status: "Active Draft"
spec: "EGTS"
file: "/registry/v0.3.x/governance-role-registry.md"

---

Governance‑Role Registry (v0.3.x)

Purpose

The EGTS Governance‑Role Registry provides a standardised, DID‑based directory for roles, authorities, and contexts that appear in governance events. It enables cross‑system validation without requiring a centralised database or blockchain‑specific logic.

Architecture Overview

The registry is a logical mapping – not a smart contract or API. Implementations MAY store it as:

· A JSON‑LD document hosted at a well‑known URI.
· A distributed ledger’s state (e.g., a Ethereum contract reading a registry file).
· A versioned Git repository.

```
+----------------+     +-----------------+     +------------------+
|  Role Record   |<--- | Authority Record|---->|  Context Record  |
| (DID subject)  |     | (DID subject)   |     |  (DID subject)   |
+----------------+     +-----------------+     +------------------+
       ^                        ^                        ^
       |                        |                        |
       +------------------------+------------------------+
                            |
                  +---------------------+
                  |  Governance Event   |
                  | (references Triad)  |
                  +---------------------+
```

Record Types

1. Role Record

Describes a governance role. Identified by a DID (e.g., did:example:123#role/voter).

Field Type Description
id DID URL DID of the role
type Role Fixed
roleType string One of Proposer, Voter, Executor, etc.
governingEntity DID Entity (person, org, agent) holding the role
delegationChain DID[] Optional chain of delegations
validFrom timestamp Start of validity
validUntil timestamp End of validity
authorityId DID URL Link to the Authority that grants this role
contextId DID URL Link to the Context in which role is valid

Example (fictional):

```json
{
  "@context": "https://egts.standard/v0.3.x/context.jsonld",
  "id": "did:example:registry/role/proposer-abc",
  "type": "Role",
  "roleType": "Proposer",
  "governingEntity": "did:example:org/green-energy-collective",
  "validFrom": "2026-01-01T00:00:00Z",
  "validUntil": "2027-01-01T00:00:00Z",
  "authorityId": "did:example:registry/authority/constitution-v2",
  "contextId": "did:example:registry/context/mainnet-governance"
}
```

2. Authority Record

Defines a source of authority (constitution, law, by‑law, smart contract).

Field Type Description
id DID URL Authority identifier
type Authority Fixed
source URI Location of the rule set (document, code)
scope string Domain of authority
constraints string[] List of constraints (e.g., "quorum >= 4%")

3. Context Record

Defines a governance context (chain, temporal bounds, jurisdiction).

Field Type Description
id DID URL Context identifier
type Context Fixed
governanceScope string Name of governance system
chainReference string Optional blockchain identifier
temporalBounds object {start, end} timestamps

Operations (Logical)

The registry does not define an API. Instead, changes are expressed as Governance Events of type RoleAssigned, RoleRevoked, AuthorityGranted, ContextDefined. Any system that consumes EGTS events MUST apply those events to its local view of the registry.

Example Registry Snapshot (fictional)

```json
{
  "@context": "https://egts.standard/v0.3.x/context.jsonld",
  "roles": [
    {
      "id": "did:example:registry/role/voter-001",
      "type": "Role",
      "roleType": "Voter",
      "governingEntity": "did:example:person/alice",
      "authorityId": "did:example:registry/authority/charter",
      "contextId": "did:example:registry/context/arbitrum-governance"
    }
  ],
  "authorities": [
    {
      "id": "did:example:registry/authority/charter",
      "type": "Authority",
      "source": "https://example.org/charter.pdf",
      "scope": "All protocol parameter changes"
    }
  ],
  "contexts": [
    {
      "id": "did:example:registry/context/arbitrum-governance",
      "type": "Context",
      "governanceScope": "Arbitrum DAO",
      "chainReference": "arbitrum-one"
    }
  ]
}
```

Validation Rule

A governance event is valid if and only if:

· The role referenced in the event exists in the registry and is active (validFrom ≤ now ≤ validUntil).
· The authority exists and its scope covers the action described in eventType + details.
· The context exists and its temporal/chain bounds match the event’s timestamp and chain.

This is the Validation Triad check.


