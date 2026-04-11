📘 EGTS Validator Set Specification
Version: 0.2.x  
Status: Active Draft  
Purpose: Define validator roles, trust assumptions, rotation rules, performance metrics, and cross‑ecosystem legitimacy for EGTS trust‑surface validation  
Audience: DAOs, L2 ecosystems, governance platforms, contributor registries, auditors  

---

1. Overview

The EGTS Validator Set is the group of actors responsible for:

- validating governance events  
- generating trust signals  
- enforcing authority scopes  
- verifying contributor roles  
- maintaining governance integrity  
- ensuring audit log correctness  

Validators are the core trust anchors of the EGTS trust surface.

---

2. Validator Roles

EGTS defines three validator roles:

---

2.1 Governance Validator (Primary Role)

Responsibilities:

- validate governance events  
- verify identity, role, authority, and context  
- sign trust signals  
- ensure lifecycle correctness  
- detect governance anomalies  

---

2.2 Audit Validator

Responsibilities:

- verify audit log integrity  
- check hash lineage  
- detect missing or inconsistent entries  
- validate cross‑ecosystem audit sync  

---

2.3 Ecosystem Validator

Responsibilities:

- validate cross‑chain trust signals  
- verify ecosystem context  
- map external roles to local roles  
- prevent replay attacks  

---

3. Validator Identity Requirements

Validators must have:

- a DID  
- one or more public keys  
- a verifiable role assignment  
- an active status  
- a governance‑approved authority scope  

Validators may not be anonymous.

---

4. Validator Trust Assumptions

EGTS assumes:

4.1 Threshold Honesty
A threshold of validators behave honestly.

4.2 Role Integrity
Validators do not exceed their authority scope.

4.3 Identity Stability
Validator identities remain stable and verifiable.

4.4 Cross‑Ecosystem Consistency
Validators maintain consistent identity across ecosystems.

---

5. Validator Set Composition

Validator sets must be:

- diverse (multiple contributors)  
- role‑verified  
- authority‑scoped  
- ecosystem‑recognized  

Recommended minimum:

- 3 validators for small DAOs  
- 5 validators for medium ecosystems  
- 7+ validators for L2 ecosystems or public‑good systems  

---

6. Validator Rotation

Validator rotation ensures:

- decentralization  
- resilience  
- reduced collusion risk  
- long‑term governance integrity  

Rotation Triggers
- term expiration  
- inactivity  
- governance vote  
- role revocation  
- identity compromise  

Rotation Requirements
- new validators must be role‑verified  
- outgoing validators must be recorded in audit logs  
- rotation events must be EGTS‑validated  

---

7. Validator Performance Metrics

EGTS tracks validator performance across:

7.1 Validation Accuracy
- correct vs. incorrect validations  
- authority scope enforcement  

7.2 Validation Latency
- time between event emission and validation  

7.3 Participation Rate
- percentage of events validated  

7.4 Collusion Indicators
- correlated validation patterns  
- repeated rubber‑stamping  

7.5 Cross‑Ecosystem Reliability
- consistency across chains  

These metrics feed into the EGTS Governance Risk Model.

---

8. Validator Set Governance

Validator sets must be governed by:

- a governance charter  
- role assignment rules  
- authority scope definitions  
- rotation procedures  
- revocation procedures  

All validator governance actions must be:

- EGTS‑formatted  
- validated  
- recorded in audit logs  

---

9. Validator Signature Requirements

Validator signatures must:

- use secure signature schemes (ECDSA, Ed25519, etc.)  
- include validator DID  
- include validation timestamp  
- include validation method  
- be included in trust signals  

Signatures must be verifiable across ecosystems.

---

10. Validator Misbehavior Handling

Misbehavior includes:

- invalid validations  
- authority violations  
- collusion  
- identity compromise  
- audit log manipulation  

Consequences
- role revocation  
- badge revocation  
- contributor registry update  
- audit log entry  
- ecosystem‑wide trust downgrade  

---

