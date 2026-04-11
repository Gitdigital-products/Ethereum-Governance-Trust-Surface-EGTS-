📘 EGTS Contributor Registry Specification
Version: 0.2.x  
Status: Active Draft  
Purpose: Define a cross‑ecosystem registry for contributor identity, roles, authority, and governance legitimacy  
Scope: DAOs, L2 ecosystems, protocols, public‑good systems, grant programs  

---

1. Overview

The EGTS Contributor Registry is a chain‑agnostic, ecosystem‑wide legitimacy registry that records:

- contributor identities  
- governance roles  
- authority scopes  
- validated governance actions  
- trust‑signal lineage  
- audit log references  

It enables portable contributor legitimacy across DAOs, L2s, and public‑good ecosystems.

This registry is the backbone of a universal governance trust surface.

---

2. Registry Goals

2.1 Cross‑Ecosystem Legitimacy
A contributor validated in one ecosystem should be recognized in another.

2.2 Role & Authority Transparency
Anyone should be able to verify:

- who a contributor is  
- what role they hold  
- what authority they have  
- whether their authority is active  

2.3 Governance History
A contributor’s governance actions should be:

- verifiable  
- auditable  
- portable  

2.4 Public‑Good Alignment
The registry is designed as public infrastructure, not a proprietary system.

---

3. Registry Data Model

The registry consists of Contributor Records, each containing:

`json
{
  "contributor_id": "did:gitdigital:contributor-001",
  "identity": {
    "did": "did:gitdigital:contributor-001",
    "public_keys": ["0xabc123..."],
    "status": "active"
  },
  "roles": [
    {
      "role": "Core Contributor",
      "authority": "Governance Charter v1.2",
      "valid_from": "2026-01-01",
      "valid_to": null
    }
  ],
  "governance_actions": [
    {
      "event_id": "evt-2026-001",
      "actiontype": "proposalapproved",
      "timestamp": "2026-04-11T19:00:00Z",
      "trust_signal": "sig-2026-001",
      "auditlogref": "audit-2026-04"
    }
  ],
  "metadata": {
    "ecosystems": ["Ethereum", "L2-X", "DAO-Y"],
    "version": "0.2.0"
  }
}
`

---

4. Registry Components

4.1 Identity Layer
Stores:

- DID  
- public keys  
- identity status (active, revoked, suspended)  

4.2 Role Layer
Stores:

- role name  
- authority reference  
- validity period  
- governance charter source  

4.3 Authority Layer
Defines:

- what actions the contributor can perform  
- which governance rules apply  
- which ecosystems recognize the authority  

4.4 Governance Action Layer
Stores:

- event references  
- trust signal references  
- audit log references  
- timestamps  

4.5 Metadata Layer
Stores:

- ecosystem participation  
- versioning  
- optional contributor metadata  

---

5. Registry Operations

5.1 Add Contributor
Adds a new contributor identity.

5.2 Assign Role
Adds a role with authority scope.

5.3 Revoke Role
Marks a role as inactive.

5.4 Record Governance Action
Adds a governance event reference.

5.5 Record Trust Signal
Links a validated trust signal to the contributor.

5.6 Record Audit Log Entry
Links audit lineage to the contributor.

5.7 Cross‑Ecosystem Sync
Mirrors contributor legitimacy across ecosystems.

---

6. Security Considerations

6.1 Identity Integrity
DIDs must be verifiable and non‑spoofable.

6.2 Authority Verification
Roles must be validated against governance charters.

6.3 Audit Log Integrity
All governance actions must reference immutable audit logs.

6.4 Cross‑Ecosystem Trust
Trust signals must include ecosystem context to prevent replay attacks.

---

