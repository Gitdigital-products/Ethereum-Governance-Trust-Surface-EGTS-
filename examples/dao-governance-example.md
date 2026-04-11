File: examples/dao-governance-example.md


[PLACEHOLDER: CRYPTOGRAPHIC BANNER]

# DAO Governance Example (Conceptual)

## Scenario
A DAO uses ETSC Core to record a proposal lifecycle.

## Steps
1. **Create Proposal**  
   Member A submits a proposal to fund a working group.  
   A `GovernanceEvent` of type `proposal_created` is emitted with event_id `prop_001`.

2. **Voting Period**  
   Members B and C cast votes.  
   Two `GovernanceEvent` objects of type `vote_cast` reference `prop_001`.

3. **Execution**  
   Proposal passes.  
   `GovernanceEvent` of type `proposal_executed` is emitted.

## Trust Signal Integration
- Member D issues an `endorsement` trust signal for Member B’s voting history.
- Member E issues a `challenge` signal on the proposal’s financial calculation.

## Audit Trail
Every action (create, vote, execute, signal issuance) is recorded as an `AuditLog` with before/after state where applicable.

[PLACEHOLDER: SUPPORT LINKS]

