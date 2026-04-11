📘 EGTS Governance Lifecycle Diagrams (Narrative Specification)
Version: 0.2.x  
Status: Active Draft  
Purpose: Provide narrative lifecycle diagrams for governance events, validation, trust signals, and audit logs  
Audience: DAOs, L2 ecosystems, governance platforms, auditors, grant programs  

---

1. Overview

EGTS defines four core governance lifecycles:

1. Proposal Lifecycle  
2. Validation Lifecycle  
3. Trust Signal Lifecycle  
4. Audit Log Lifecycle

Each lifecycle is described as a state machine in narrative form, suitable for:

- implementers  
- auditors  
- governance researchers  
- dashboard developers  
- grant reviewers  

---

2. Proposal Lifecycle (Narrative Diagram)

The proposal lifecycle consists of seven states:

---

State 1 — Drafted
A contributor creates a proposal draft.

Required EGTS elements:
- actor DID  
- role = Proposal Author  
- authority reference  
- context  

Transitions:
- → Submitted  

---

State 2 — Submitted
The proposal is formally submitted to governance.

Required EGTS event:
- proposal_submitted

Transitions:
- → Under Review  
- → Rejected (if invalid submission)  

---

State 3 — Under Review
Governance validators review the proposal.

Checks performed:
- identity  
- role  
- authority  
- context  
- completeness  

Transitions:
- → Voting  
- → Rejected  

---

State 4 — Voting
Eligible voters cast votes.

Required EGTS events:
- vote_cast (multiple)  
- vote_closed  

Transitions:
- → Approved  
- → Rejected  

---

State 5 — Approved
Proposal meets quorum and passes.

Required EGTS event:
- proposal_approved

Transitions:
- → Execution  

---

State 6 — Execution
Proposal is executed by authorized actors.

Required EGTS event:
- proposal_executed

Transitions:
- → Completed  
- → Failed Execution  

---

State 7 — Completed
Proposal lifecycle ends successfully.

Required EGTS event:
- proposal_completed

---

3. Validation Lifecycle (Narrative Diagram)

Validation is the process of verifying governance events.

It consists of five states:

---

State 1 — Event Emitted
A governance event is created.

Input:
- governance event object  

Transitions:
- → Pending Validation  

---

State 2 — Pending Validation
Validators receive the event.

Checks performed:
- identity  
- role  
- authority  
- action type  
- context  
- lifecycle consistency  

Transitions:
- → Validated  
- → Rejected  

---

State 3 — Validated
Validators approve the event.

Required EGTS output:
- trust signal  

Transitions:
- → Recorded in Audit Log  

---

State 4 — Rejected
Validators reject the event.

Required EGTS output:
- rejection metadata  
- validator signatures  

Transitions:
- → End  

---

State 5 — Recorded in Audit Log
Event and trust signal are appended to audit log.

Transitions:
- → End  

---

4. Trust Signal Lifecycle (Narrative Diagram)

Trust signals represent validated governance actions.

Lifecycle consists of four states:

---

State 1 — Generated
Validators generate a trust signal.

Includes:
- event reference  
- validator signatures  
- validation method  
- integrity hash  
- ecosystem context  

Transitions:
- → Published  

---

State 2 — Published
Trust signal is stored or broadcast.

Destinations:
- contributor registry  
- audit log  
- dashboards  
- cross‑chain adapter  

Transitions:
- → Imported (optional)  
- → Archived  

---

State 3 — Imported (Cross‑Ecosystem)
Another ecosystem imports the trust signal.

Checks performed:
- identity  
- authority  
- context  
- integrity  
- lineage  

Transitions:
- → Recognized  
- → Rejected  

---

State 4 — Recognized
Trust signal becomes part of local trust surface.

---

5. Audit Log Lifecycle (Narrative Diagram)

Audit logs preserve governance history.

Lifecycle consists of five states:

---

State 1 — Entry Created
A new audit log entry is created.

Includes:
- event reference  
- trust signal reference  
- validator list  
- hash lineage  

Transitions:
- → Appended  

---

State 2 — Appended
Entry is added to the audit log.

Guarantees:
- append‑only  
- hash‑linked  

Transitions:
- → Published  

---

State 3 — Published
Audit log is published for the period.

Destinations:
- IPFS  
- dashboards  
- public archives  

Transitions:
- → Verified  

---

State 4 — Verified
Audit log integrity is checked.

Checks:
- hash lineage  
- timestamp consistency  
- trust signal references  

Transitions:
- → Archived  

---

State 5 — Archived
Audit log becomes part of long‑term governance history.

---

