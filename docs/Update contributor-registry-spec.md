=== FILE: docs/contributor-registry-spec.md ===

Contributor Registry Specification – EGTS

1. Purpose

The Contributor Registry stores canonical, verifiable identities of governance participants across Ethereum ecosystems. It enables badges, voting power aggregation, and cross‑project contribution tracking.

2. Data Model

2.1 Contributor Entry

```typescript
interface ContributorEntry {
  id: bytes32;                // keccak256(publicKey || nonce)
  publicKey: bytes;           // secp256k1 or BLS12-381
  metadataURI: string;        // IPFS or Arweave URI
  scope: uint256;             // bitmask of governance domains
  status: uint8;              // 0=active, 1=suspended, 2=revoked
  registeredAt: uint64;       // Unix timestamp
  lastProofTimestamp: uint64; // last proof-of-contribution
}
```

2.2 Contribution Claim

```typescript
interface ContributionClaim {
  contributorId: bytes32;
  claimType: uint16;          // e.g., 1=vote, 2=proposal, 3=badge
  sourceChain: uint16;        // chain ID
  sourceTxHash: bytes32;
  proof: bytes;               // Merkle proof or zk-proof
}
```

3. Operations

3.1 Registration

· Any address may register a contributor entry by submitting a signed message and a small stake (0.01 ETH equivalent).
· The registry rejects duplicate public keys.
· Registration is pending for 7 days (dispute window) before becoming active.

3.2 Update & Revocation

· Contributor can update metadataURI and scope by signing the update.
· Revocation requires either:
  · Contributor’s own signature, or
  · 2/3 validator vote after a governance failure proof.

3.3 Proof of Contribution

· At least once every 90 days, a contributor must submit a fresh ContributionClaim.
· Failure to do so transitions status to “suspended”.
· Suspended entries are ignored by dashboards and badge issuers.

4. Dispute Resolution

Any user can challenge a registry entry by depositing a bond (0.1 ETH). Challenges are resolved by a decentralized court (e.g., Kleros or EGTS internal jury) within 14 days. If the entry is invalid, it is revoked and the challenger receives a reward.

5. Interface

5.1 Solidity Interface (simplified)

```solidity
interface IContributorRegistry {
  function register(bytes calldata publicKey, string calldata metadataURI, uint256 scope) external payable;
  function update(bytes32 id, string calldata newURI, uint256 newScope, bytes calldata signature) external;
  function revoke(bytes32 id, bytes calldata proof) external;
  function submitClaim(bytes32 id, ContributionClaim calldata claim) external;
  function resolveDispute(bytes32 id, bool isInvalid) external;
  function isActive(bytes32 id) external view returns (bool);
}
```

6. Off‑Chain Indexing

A registry indexer (run by EGTS validators) provides:

· Full text search over metadataURI fields.
· Historical lookup of status changes.
· Batch queries for dashboard pagination.

The indexer must be reproducible from on‑chain events.

7. Gas & Scalability

· Registrations and claims are batched using recursive Merkle proofs.
· L2 native registry (e.g., on Arbitrum) with L1 finality anchor is recommended for high volume.
