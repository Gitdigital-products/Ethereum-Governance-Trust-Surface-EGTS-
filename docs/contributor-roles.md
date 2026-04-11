📘 EGTS Contributor Role Definitions (Spec‑Grade)

`
File: docs/contributor-roles.md
Version: 0.2.x
Status: Active Draft
`

---

EGTS Contributor Roles

The Ethereum Governance Trust Surface (EGTS) defines a set of contributor roles that establish authority, responsibility, and validation scope within governance systems.  
These roles are chain‑agnostic and compatible with DAOs, protocols, L2 ecosystems, and public‑good infrastructure.

Each role contributes to the trust surface by generating, validating, or auditing governance events.

---

1. Proposal Author

Purpose:  
Creates governance proposals and initiates governance actions.

Responsibilities:  
- Draft proposals with clear intent and context  
- Provide rationale and supporting documentation  
- Submit proposals to the governance process  
- Respond to reviewer questions and clarifications  

Authority Scope:  
- Limited to proposal creation  
- No inherent validation or approval authority  

Trust‑Surface Impact:  
Proposal Authors generate the initial governance event that enters the EGTS lifecycle.

---

2. Core Contributor

Purpose:  
Executes governance‑approved actions and maintains protocol or ecosystem operations.

Responsibilities:  
- Implement approved proposals  
- Maintain protocol components, documentation, and governance artifacts  
- Provide operational transparency  
- Participate in governance discussions  

Authority Scope:  
- Execution authority derived from governance charters  
- May generate governance events requiring validation  

Trust‑Surface Impact:  
Core Contributors produce high‑integrity governance events that require validation and audit logging.

---

3. Governance Validator

Purpose:  
Validates governance events, contributor authority, and trust signals.

Responsibilities:  
- Verify actor identity and role legitimacy  
- Confirm authority scope for each governance action  
- Apply validation rules (multi‑sig, quorum, role checks)  
- Approve or reject trust signals  

Authority Scope:  
- High authority within governance trust‑surface  
- Can approve or invalidate governance events  

Trust‑Surface Impact:  
Governance Validators are the primary trust‑surface gatekeepers.

---

4. Auditor

Purpose:  
Ensures the integrity, completeness, and correctness of governance audit logs.

Responsibilities:  
- Review audit log entries for accuracy  
- Confirm event lineage and hash integrity  
- Detect inconsistencies or missing validations  
- Produce audit reports for governance transparency  

Authority Scope:  
- No authority to approve governance events  
- Full authority to flag inconsistencies  

Trust‑Surface Impact:  
Auditors maintain the long‑term integrity of the trust surface.

---

5. Trust‑Surface Maintainer

Purpose:  
Maintains the EGTS implementation, schemas, and validation logic.

Responsibilities:  
- Update schemas and validation rules  
- Maintain documentation and examples  
- Ensure backward compatibility  
- Coordinate with governance bodies and ecosystem partners  

Authority Scope:  
- Authority over the EGTS specification  
- No authority over governance decisions  

Trust‑Surface Impact:  
Maintainers ensure the spec remains stable, interoperable, and future‑proof.

---

6. Ecosystem Integrator

Purpose:  
Implements EGTS within DAOs, protocols, L2s, or public‑good systems.

Responsibilities:  
- Map local governance processes to EGTS schemas  
- Integrate trust‑surface outputs into dashboards and tooling  
- Provide feedback to maintainers  
- Ensure compatibility with local governance rules  

Authority Scope:  
- Implementation authority only  
- No governance decision authority  

Trust‑Surface Impact:  
Integrators expand the reach and adoption of EGTS across the ecosystem.

---

Role Interaction Matrix

`
┌───────────────────────────┬──────────────┬──────────────┬────────────┬───────────┬──────────────┐
│ Role                      │ Creates Event │ Validates     │ Audits     │ Executes  │ Maintains    │
├───────────────────────────┼──────────────┼──────────────┼────────────┼───────────┼──────────────┤
│ Proposal Author           │ Yes          │ No           │ No         │ No        │ No           │
│ Core Contributor          │ Yes          │ No           │ No         │ Yes       │ No           │
│ Governance Validator      │ No           │ Yes          │ No         │ No        │ No           │
│ Auditor                   │ No           │ No           │ Yes        │ No        │ No           │
│ Trust‑Surface Maintainer  │ No           │ No           │ No         │ No        │ Yes          │
│ Ecosystem Integrator      │ No           │ No           │ No         │ No        │ Partial      │
└───────────────────────────┴──────────────┴──────────────┴────────────┴───────────┴──────────────┘
`

---

