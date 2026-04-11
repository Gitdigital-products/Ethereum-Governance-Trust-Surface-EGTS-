=== FILE: docs/adoption-playbook/06-migration-paths.md ===

EGTS Adoption Playbook – Chapter 06: Migration Paths

1. Scope

This chapter provides migration strategies for existing governance systems (legacy DAOs, off‑chain committees, custom voting contracts) to adopt EGTS without disrupting ongoing operations. Migration can be incremental, with parallel running and cutover phases.

2. Migration Principles

· No sudden changes – Existing governance continues to function during migration.
· Artifact backfilling – Historical decisions are published to EGTS Registry retroactively.
· Opt‑in adoption – New proposals use EGTS while old ones remain in legacy format.
· Reversible – A fallback path exists if EGTS integration causes issues.

3. Migration Paths by System Type

3.1. Legacy On‑Chain DAO (no EGTS)

Current state: Governor contract with direct voting, no registry.

Migration path:

1. Wrapper contract – Deploy an EGTS adapter that listens to the Governor’s events and forwards them to EGTS Registry.
2. Backfill – For past proposals, extract data from logs and publish as historical artifacts (marked historical=true).
3. Switchover – After 3 months, require new proposals to be submitted via the adapter.
4. Cutover – Replace Governor with EGTS‑aware version that enforces timelock.

3.2. Snapshot + Multisig

Current state: Off‑chain Snapshot votes, multisig executes.

Migration path:

1. Relayer – Deploy a bot that listens to Snapshot events and publishes EGTS proposals.
2. Signer integration – Require the multisig to reference EGTS proposal ID before signing.
3. Optional on‑chain – Replace Snapshot with EGTS off‑chain voting (same UX, but registry‑backed).

3.3. Off‑Chain Committee (no smart contracts)

Current state: Decisions made by email or chat, recorded in minutes.

Migration path:

1. Manual publishing – For each new decision, a committee member runs egts publish with the decision text.
2. Threshold signatures – Use a tool like egts sign to collect signatures from 3+ members.
3. Full automation – Integrate with the committee’s communication tool (e.g., Discord bot that posts resolutions).

4. Backfilling Historical Data

Backfilling is optional but recommended for transparency. Steps:

1. Export legacy governance actions to a structured format (CSV, JSON).
2. For each action, create an EGTS artifact with historical: true and original_timestamp.
3. Sign the artifact with a special “historical” key (or a multi‑sig representing the legacy system).
4. Publish to IPFS and register.

Limitation: Historical signatures cannot be verified if original signers are unavailable. Mark as unverifiable in that case.

5. Parallel Running Period

During migration (typically 30‑90 days), both legacy and EGTS processes run in parallel:

· Proposers may submit to either system.
· Voters may vote in both, but the official outcome is the legacy system’s until cutover.
· Registry contains both legacy‑wrapped and native EGTS proposals.

A consistency checker compares outcomes and alerts if they diverge.

6. Cutover Strategies

6.1. Hard Cutover

At a predetermined block height, the legacy system is paused and only EGTS proposals are accepted. Requires community coordination.

6.2. Soft Cutover

The legacy system remains but is deprecated. New proposals are strongly encouraged to use EGTS. After a period (e.g., 6 months) with no legacy activity, the legacy system is archived.

6.3. Threshold Cutover

New proposals must use EGTS once a certain percentage of active voters have registered EGTS addresses (e.g., 80%).

7. Rollback Plan

If EGTS integration causes critical issues:

1. Pause the EGTS adapter (via a pause switch in the wrapper contract).
2. Revert to legacy proposal submission (manual or old contract).
3. Announce rollback in EGTS Registry as a special emergency_rollback resolution.
4. Analyze and fix issues before re‑attempting migration.

5. Example: Migrating a Moloch DAO

Legacy: Moloch v2 with submitProposal, sponsorProposal, processProposal.

Migration:

1. Deploy MolochEGTSAdapter that:
   · Listens to SubmitProposal and calls EGTS Registry createProposal.
   · Listens to ProcessProposal and publishes vote tally.
2. Backfill 50 past proposals by reading Moloch events.
3. After 2 months of parallel running, upgrade Moloch to v3 (which natively supports EGTS).
4. Hard cutover at block 18,000,000.

5. Migration Cost Estimation

Activity Effort (person‑weeks) Gas cost (estimated)
Wrapper contract development 2 0.5 ETH (deployment)
Backfilling 1000 proposals 1 2 ETH (registration)
Relayer bot 1 Minimal
Testing & cutover 2 0.2 ETH
Total 6 ~2.7 ETH

10. Post‑Migration Verification

After cutover, verify:

· All new proposals appear in EGTS Registry within 5 minutes of submission.
· Vote tallies in registry match the actual outcome.
· Timelock delays are enforced.
· No legacy‑only proposals have been submitted for 30 consecutive days.

If any verification fails, execute rollback plan.

=== FILE: docs/adoption-playbook/07-governance-maturity-model.md ===

EGTS Adoption Playbook – Chapter 07: Governance Maturity Model

1. Purpose

The EGTS Governance Maturity Model (GMM) defines progressive levels of integration with the Ethereum Governance Trust Surface. Systems can use the GMM to plan their adoption roadmap, communicate their current state to stakeholders, and identify next steps.

2. Maturity Levels

Level Name Description Exit Criteria
0 Traditional No EGTS integration; governance is opaque or ad‑hoc. Publish first EGTS artifact.
1 Observable Governance actions are published to EGTS Registry after they occur. 100% of actions published within 7 days.
2 Interoperable EGTS lifecycle is used for new proposals; votes are machine‑readable and externally verifiable. External system can replay governance and reproduce outcomes.
3 Trust Surface Full alignment with EGTS Charter; formal review, timelock, dispute resolution. Pass annual EGTS compliance audit.
4 Federated Cross‑system alignment (see Chapter 05); participates in at least one federation. Active in federation for 3 months.

3. Level 0: Traditional

Characteristics:

· Decisions recorded in private channels or unstructured documents.
· No verifiable voting records.
· No timelock or execution delay.
· No public registry.

Transition to Level 1:

· Designate an EGTS signer.
· Create a process to publish decisions as EGTS resolutions.
· Backfill at least the last 3 months of decisions (optional but recommended).

4. Level 1: Observable

Requirements:

· Every governance action produces an EGTS artifact (proposal draft or resolution) within 7 days.
· Artifacts are signed by at least one authorized signer.
· Artifacts are pinned to IPFS and registered in EGTS Registry.
· A public page lists all artifacts.

Capabilities:

· External observers can see what decisions were made.
· Historical audit is possible (though signatures may be from a single party).

Limitations:

· No guarantee that published artifacts reflect actual decision process.
· No vote‑weight verification.
· No cross‑system acceptance.

5. Level 2: Interoperable

Requirements (in addition to Level 1):

· Governance follows the EGTS lifecycle (Initiation → Refinement → Review → Voting → Execution → Review).
· Voting uses EGTS vote tracks (on‑chain or off‑chain with EIP‑712).
· Vote tallies are published automatically, not manually.
· Timelock enforced for all executable actions (minimum 48 hours).
· System publishes trust assumptions.

Capabilities:

· External systems can trust the vote tally because it is cryptographically signed.
· Other EGTS‑aware systems may accept the system’s votes as input.
· Disputes can be raised against the tally.

Exit criteria:

· Three consecutive governance cycles completed without manual override of vote tally.
· At least two external systems have successfully read and acted upon the system’s votes.

6. Level 3: Trust Surface

Requirements (in addition to Level 2):

· Formal review committee (or equivalent) assesses proposals before voting.
· Charter alignment statement is published and kept up‑to‑date.
· Quarterly alignment monitoring reports are filed.
· Dispute resolution process (as defined in EGTS Charter) is implemented.
· Emergency override mechanisms are documented and limited.

Capabilities:

· The system is recognized as a reference implementation of EGTS.
· Other systems may rely on it without additional trust assumptions.
· The system can serve as a reviewer or guardian in federations.

Exit criteria:

· Passed an EGTS compliance audit by an independent third party.
· No unresolved disputes older than 90 days.
· Alignment reports for four consecutive quarters.

7. Level 4: Federated

Requirements (in addition to Level 3):

· System has joined at least one EGTS federation (see Chapter 05).
· Implements at least one cross‑system pattern (mirroring, sequencing, mutual veto).
· Publishes cross‑system vote normalization rules.
· Participates in mediation for cross‑system conflicts.

Capabilities:

· The system can engage in composable governance with other federated systems.
· A vote in one system can automatically trigger actions in another.
· Cross‑system disputes are resolved via EGTS mediation.

Exit criteria:

· Successfully completed three cross‑system proposals.
· Mediation mechanism has been tested (even if never used).

8. Maturity Assessment Matrix

Capability L0 L1 L2 L3 L4
Artifact publishing No Yes Yes Yes Yes
Signed artifacts No ≥1 signer ≥2 signers ≥3 signers ≥3 signers
EGTS lifecycle No Partial Full Full Full
Automatic vote tally No No Yes Yes Yes
Timelock No No Yes Yes Yes
Formal review No No No Yes Yes
Charter alignment No No No Yes Yes
Dispute resolution No No No Yes Yes
Cross‑system federation No No No No Yes

9. Self‑Assessment Tool

Systems can score themselves using the following rubric:

· Level 0 – 0‑3 points from matrix above.
· Level 1 – 4‑6 points.
· Level 2 – 7‑9 points.
· Level 3 – 10‑12 points.
· Level 4 – 13‑14 points.

10. Progression Roadmap

A typical roadmap from Level 0 to Level 3 takes 6‑12 months:

· Month 1 – Achieve Level 1 (Observable).
· Months 2‑3 – Automate vote tally, implement timelock → Level 2.
· Months 4‑6 – Establish review committee, publish charter alignment → Level 3.
· Months 7‑12 – Join federation, implement cross‑system pattern → Level 4 (optional).

11. Maintaining Maturity

Once a level is achieved, the system must:

· Submit an annual attestation that requirements are still met.
· Report any material changes to governance process within 30 days.
· Allow random spot checks by EGTS Oversight Committee.

Failure to maintain requirements results in downgrade to the highest level still satisfied, with public notice.
