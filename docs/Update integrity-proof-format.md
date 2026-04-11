=== FILE: docs/integrity-proof-format.md ===

Integrity Proof Format – EGTS

1. Overview

All governance data presented by EGTS must be accompanied by a cryptographic integrity proof that binds the data to a canonical source. This specification defines the proof format, verification procedure, and encoding.

2. Proof Types

EGTS supports three proof types:

Type Identifier Use Case
Merkle inclusion 0x01 Batch inclusion of events in a root
ZK‑SNARK 0x02 Zero‑knowledge proof of event validity
Signature aggregation 0x03 Validator multi‑signature over a root

3. Common Header

Every proof begins with a fixed 64‑byte header:

```
0                   1                   2                   3
0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|   version     |   type        |          chain_id             |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         root_hash (32 bytes)                   |
|                                                               |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
|                         timestamp                              |
+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

4. Merkle Inclusion Proof (type 0x01)

After header, the proof contains:

· leaf_index (uint64)
· sibling_hashes (variable length array of 32‑byte hashes)

Verification: recompute root from leaf hash and siblings, compare to header root.

5. ZK‑SNARK Proof (type 0x02)

Proves that a given contributorId and eventPayload are included in a batch with root R without revealing other events.

Proof structure:

· proof_data (variable, circuit‑specific)
· public_inputs array (including root_hash and leaf_hash)

Verification uses the EGTS verification key stored on‑chain for the given circuit ID (derived from chain_id and batch height).

6. Signature Aggregation Proof (type 0x03)

Used when a batch is attested by a supermajority of EGTS validators.

Structure:

· validator_set_root (32 bytes)
· aggregated_signature (96 bytes, BLS12‑381)
· participation_bitfield (bitset of validators who signed)

Verification: compute message = root_hash || chain_id || timestamp, verify aggregated signature against the current validator set.

7. Encoding & Wrapping

All proofs are encoded as a CBOR byte string for transport. The top‑level envelope:

```cbor
{
  "proof_type": int,
  "header": bytes,
  "body": bytes,
  "metadata": {
    "source_chain": int,
    "block_number": int,
    "adapter_id": bytes
  }
}
```

8. On‑Chain Verification

The EGTS core contract exposes:

```solidity
function verifyProof(bytes calldata proof, bytes32 expectedRoot) external returns (bool);
```

It decodes the header, dispatches to the appropriate verifier, and returns true iff the proof is valid and the root matches the expected root for that epoch.

9. Proof Expiry

Proofs older than 2 weeks are considered stale and must be refreshed. Clients should reject stale proofs unless explicitly configured to accept historical data.
