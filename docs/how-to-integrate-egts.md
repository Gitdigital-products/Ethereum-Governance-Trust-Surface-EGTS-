📘 How to Integrate EGTS
Version: 0.2.x  
Status: Active Draft  
Audience: DAOs, Protocol Teams, L2 Ecosystems, Public‑Good Infrastructure  
Purpose: Provide a practical, step‑by‑step integration guide for adopting the Ethereum Governance Trust Surface (EGTS)  

---

1. Overview

EGTS is designed to be incrementally adoptable, chain‑agnostic, and compatible with existing governance systems.  
Integration does not require contract upgrades, protocol changes, or migration of existing governance processes.

This guide provides a clear, actionable path for integrating EGTS into any governance environment.

---

2. Integration Levels

EGTS supports three levels of integration:

Level 1 — Event Standardization (Minimum Viable Integration)
Adopt the EGTS governance event schema.

Level 2 — Trust‑Surface Validation (Full Governance Legitimacy)
Add validation rules and trust signal generation.

Level 3 — Audit Log Integration (Complete Trust Surface)
Implement portable audit logs for long‑term governance integrity.

Organizations can adopt EGTS at any level and upgrade over time.

---

3. Level 1: Event Standardization

This is the simplest and most common entry point.

Step 1 — Map Governance Actions to EGTS Events
Identify governance actions such as:

- proposal creation  
- voting  
- approvals  
- executions  
- role assignments  
- milestone completions  

Map each action to the EGTS governance event schema.

Step 2 — Emit EGTS‑Formatted Events
Your system should generate JSON objects matching:

`
schemas/governance-event.schema.json
`

Step 3 — Store or Publish Events
Events can be stored:

- on‑chain  
- off‑chain  
- in IPFS  
- in a database  
- in a governance dashboard  

At this level, validation is optional.

---

4. Level 2: Trust‑Surface Validation

This is where EGTS becomes a governance legitimacy layer.

Step 1 — Implement Role Verification
Map your governance roles to EGTS roles:

- Proposal Author  
- Core Contributor  
- Governance Validator  
- Auditor  
- etc.

Step 2 — Implement Authority Scope Checks
Define which roles can perform which actions.

Step 3 — Implement Validation Methods
Choose one or more:

- multi‑sig  
- attestation  
- quorum  
- role‑based validation  
- hybrid models  

Step 4 — Generate Trust Signals
When validation passes, generate trust signals matching:

`
schemas/trust-signal.schema.json
`

Step 5 — Publish Trust Signals
Trust signals can be:

- stored locally  
- published to IPFS  
- included in dashboards  
- shared with other ecosystems  

At this level, governance legitimacy becomes portable.

---

5. Level 3: Audit Log Integration

This is the full EGTS trust surface.

Step 1 — Create Audit Log Files
Use:

`
schemas/audit-log.schema.json
`

Step 2 — Append Trust Signals to Logs
Each validated event produces an audit entry.

Step 3 — Compute Hash Lineage
Each entry includes:

- event hash  
- trust signal hash  
- validator list  
- timestamp  

Step 4 — Publish Audit Logs
Audit logs can be:

- versioned  
- published monthly  
- stored in IPFS  
- mirrored across ecosystems  

Step 5 — Provide Audit Interfaces
Dashboards can display:

- event lineage  
- validator signatures  
- proposal lifecycle  
- governance integrity metrics  

This creates a complete, portable trust surface.

---

6. Integration Examples

DAO Example
A DAO integrates EGTS by:

- emitting EGTS events for proposals and votes  
- validating approvals via multi‑sig  
- generating trust signals  
- publishing monthly audit logs  

Protocol Example
A protocol integrates EGTS by:

- representing upgrades as governance events  
- validating upgrade authority  
- generating trust signals for upgrade legitimacy  
- maintaining long‑term audit logs  

L2 Ecosystem Example
An L2 integrates EGTS by:

- standardizing governance metadata  
- enabling cross‑rollup contributor legitimacy  
- publishing ecosystem‑wide audit logs  

---

7. Integration Checklist

A system is EGTS‑compatible if it:

- emits EGTS‑formatted governance events  
- validates events using EGTS rules  
- generates trust signals  
- maintains audit logs  
- supports chain‑agnostic trust surfaces  

---

