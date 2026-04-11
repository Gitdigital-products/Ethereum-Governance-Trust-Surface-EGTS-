=== FILE: docs/badge-authority-spec.md ===

Badge Authority Specification – EGTS

1. Purpose

The Badge Authority issues, manages, and revokes governance badges that represent roles, permissions, delegations, or achievements within the EGTS ecosystem. Badges are bound to contributor registry entries and are publicly verifiable.

2. Badge Schema

Each badge is an ERC‑1155 (or equivalent) token with the following immutable properties:

· Badge ID – bytes32 (namespace + local identifier)
· Badge Type – e.g., VOTER, DELEGATE, AUDITOR, STEWARD, CONTRIBUTOR_L1
· Issuer – address of the Badge Authority contract or a designated minter role.
· Expiry – Unix timestamp after which the badge is invalid (0 = never expires).
· Revocation – boolean flag; if true, badge is permanently invalid.

Mutable properties stored in metadata URI:

· name, description, image, criteria (e.g., link to governance rule)
· scope (bitmask of applicable governance domains)

3. Issuance Rules

Badges can be issued only by:

· EGTS Validator Set (via on‑chain vote) – for system‑wide roles.
· Authorized Sub‑DAOs – each sub‑DAO has a designated badge minter contract.
· Self‑issuance with proof – e.g., “completed tutorial” badge using a ZK proof.

3.1 Issuance Flow

1. Issuer calls issueBadge(contributorId, badgeType, expiry, metadataURI).
2. Contract checks that caller is authorized for badgeType.
3. Badge is minted to an address derived from contributorId (e.g., address(keccak256(contributorId))).
4. Event BadgeIssued(contributorId, badgeId, expiry) is emitted.

5. Revocation & Expiry

· Expiry – After expiry timestamp, the badge is considered inactive. No on‑chain action needed.
· Revocation – Only the original issuer or EGTS governance can revoke a badge early. Revocation sets a revoked flag and emits BadgeRevoked(contributorId, badgeId).

A revoked badge cannot be re‑issued for the same contributor and badge type for 90 days (cooldown).

5. Delegation Badges

Special badge type DELEGATION allows a contributor to delegate their governance rights to another contributor. Delegation badges have additional fields:

· delegateTo – contributorId of the delegate.
· weight – fraction of original voting power (0–100%).
· delegationScope – which governance domains the delegation applies to.

Delegation badges can be revoked by the delegator at any time via a signed message.

6. Badge Aggregation

The Badge Authority provides an on‑chain view function:

```solidity
function getBadgesForContributor(bytes32 contributorId) external view returns (Badge[] memory);
```

Off‑chain indexers also provide aggregated views grouped by badge type, expiry, and issuer.

7. Privacy Considerations

Sensitive badges (e.g., “whistleblower”) can be issued with zero‑knowledge issuance: the contributor proves they satisfy criteria without revealing which badge they hold. The contract stores a commitment instead of plaintext badge type.

8. Governance of Badge Authority

Changes to badge types, issuance roles, or metadata schemas require an EGTS Improvement Proposal with a 7‑day review period and a validator supermajority vote (75%). Emergency parameter changes (e.g., pausing issuance) require 5-of-9 Security Council approval.
