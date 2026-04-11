=== FILE: docs/charter-alignment-templates.md ===

Charter Alignment Templates

1. Purpose

These templates help projects, DAOs, and governance systems align their governing charters with the Ethereum Governance Trust Surface (EGTS) Charter. Alignment ensures that a system’s internal rules are compatible with EGTS trust assumptions and lifecycle requirements.

2. Charter Alignment Statement Template

Every system seeking EGTS alignment MUST publish a Charter Alignment Statement using the following structure:

```markdown
# Charter Alignment Statement for [System Name]

## 1. Reference Documents
- EGTS Charter v[version] (canonical URI: ipfs://...)
- System Charter v[version] (local URI)

## 2. Alignment Scope
We declare alignment for the following governance actions:
- [ ] Parameter changes
- [ ] Treasury allocations
- [ ] Membership management
- [ ] Protocol upgrades
- [ ] Other: ___________

## 3. Divergences
List any provisions in the system charter that conflict with EGTS:

| EGTS Clause | System Provision | Divergence Type | Mitigation |
|-------------|------------------|----------------|-------------|
| (e.g., 3.2 Quorum) | (e.g., 15% quorum) | numeric | (e.g., dynamic quorum override) |

## 4. Trust Assumption Overrides
If the system uses different trust assumptions than EGTS default, list them:

| EGTS Default | System Override | Justification |
|--------------|----------------|----------------|
| 48h timelock | 12h timelock | Faster iteration for L2 |

## 5. Signatures
- Authorized signer (address): _______
- Timestamp: _______
```

3. Governance Action Template

For each governance action (proposal, vote, execution) that claims EGTS alignment, include the following metadata in the action’s artifact:

```json
{
  "charter_alignment": {
    "aligned": true,
    "divergence_log": [],
    "charter_version": "ipfs://...",
    "reviewer_address": "0x..."
  }
}
```

4. Amendment Compatibility Template

When a system amends its own charter, it MUST produce an Amendment Compatibility Report:

```markdown
# Amendment Compatibility Report for [System Name]

## Proposed Amendment
- Section: _______
- Old text: _______
- New text: _______

## EGTS Impact Analysis
- [ ] Does this change affect any EGTS lifecycle stage? (Y/N)
- [ ] Does it alter trust assumptions? (Y/N)
- [ ] Does it change vote threshold or quorum? (Y/N)

## Remedial Actions
If any impact is Yes, describe actions to remain EGTS‑aligned:
1. _______
2. _______

## Transition Period
Number of days until amendment takes effect: _______
During transition, both old and new rules are accepted.

## Signatures
- [ ] System representative
- [ ] EGTS Charter Guardian (if major change)
```

5. Conflict Resolution Alignment

EGTS requires a defined conflict resolution process. The template below must be included in any aligned charter:

```markdown
## Conflict Resolution (EGTS‑Aligned)

1. **Internal Disputes** – Resolved via the system’s own governance as defined in Section [X].
2. **Inter‑System Disputes** – Referred to the EGTS Dispute Resolution Forum within 7 days.
3. **EGTS Charter Violation** – Reported to the EGTS Oversight Committee. A violation may result in revocation of alignment status.
4. **Emergency Conflicts** – The Guardian Multi‑sig may issue a binding advisory opinion within 48 hours.
```

6. Monitoring and Reporting Template

Aligned systems MUST provide a quarterly Alignment Monitoring Report:

```markdown
# Alignment Monitoring Report – [System Name] – Q[Y]

## Adherence Metrics
- Total governance actions: _____
- EGTS‑compliant actions: _____ (_____%)
- Divergences logged: _____
- Remediations completed: _____

## Failure Modes Encountered
List any failure modes from `governance-failure-modes.md` observed during the quarter.

## Upcoming Charter Changes
Describe any planned amendments and their expected EGTS impact.

## Attestation
I attest that this report is accurate and complete.
- Name: _______
- Address: _______
- Date: _______
```

7. Integration Without Full Alignment

Systems that cannot fully align may still achieve EGTS Observable status by implementing only the registry and event emission templates. Observable systems must publish a Limited Alignment Declaration:

```markdown
# Limited Alignment Declaration

[System Name] does not fully align with the EGTS Charter but commits to:
- Publishing all governance actions to the EGTS Registry.
- Emitting EGTS events for state changes.
- Not using EGTS branding or claiming alignment.

Signed: _______
```

8. Example: DAO Charter Alignment

A DAO charter fragment aligned with EGTS:

Article 12 – Governance Lifecycle
All proposals shall follow the EGTS lifecycle narrative (Initiation → Refinement → Review → Voting → Execution → Review). The DAO’s voting period is 7 days, which exceeds EGTS minimum (3 days). Quorum is dynamic based on past 5 votes’ average turnout. In case of conflict with EGTS, the DAO shall follow EGTS rules unless a supermajority (66%) votes to diverge.

9. Validation Checklist

Before submitting a charter alignment statement, verify:

· The system’s charter explicitly references EGTS Charter version.
· All divergences are documented with mitigations.
· Trust assumptions are declared and justified.
· A conflict resolution clause is present.
· The alignment statement is signed by an authorized on‑chain address.
· The statement is published on IPFS and registered in the EGTS Registry.
