  ===== FILE: / lifecycle/v0.3.x/audit-log-lifecycle.md =====

---

version: "0.3.x"
status: "Active Draft"
spec: "EGTS"
file: "/lifecycle/v0.3.x/audit-log-lifecycle.md"

---

Audit Log Lifecycle (v0.3.x)

Purpose

Defines the end‑to‑end lifecycle of a cryptographically chained audit log entry according to EGTS. The audit log provides non‑repudiable evidence of governance events, linking them via hash chaining and digital signatures.

Lifecycle Phases

```
+-----------+     +------------+     +------------+     +-------------+     +------------+
| 1. CREATE | --> | 2. SIGN    | --> | 3. CHAIN   | --> | 4. VERIFY   | --> | 5. STORE   |
+-----------+     +------------+     +------------+     +-------------+     +------------+
                                                              |
                                                              v
                                                    +-------------+
                                                    | 6. EXPOSE   |
                                                    +-------------+
```

Phase 1 – Create

A governance event (see governance-event.schema.json) is produced by an adapter or internal process. The event is serialised to canonical JSON (RFC 8785) and a SHA‑256 hash (eventHash) is computed.

Input: GovernanceEvent object
Output: eventHash (hex string)

Phase 2 – Sign

The logging entity (e.g., an organisation, a validator without staking) creates an AuditLogEntry:

· id: new UUID URN.
· timestamp: current UTC time (ISO 8601).
· eventHash: from Phase 1.
· previousHash: hash of the last log entry (or 0x00...00 for genesis).
· payload: the full governance event.

The entity signs the concatenation timestamp || eventHash || previousHash using its Ed25519 private key. The signature is placed in the signature field.

Security note: The private key MUST be kept secure. No on‑chain key registry is required.

Phase 3 – Chain

The new entry’s previousHash links it to the prior entry. This forms an append‑only hash chain. Any modification to an earlier entry would break all subsequent previousHash links.

```
Entry N-1                    Entry N
+----------------+           +----------------+
| previousHash   |---------->| previousHash = |
| eventHash_N-1  |           |   hash(Entry N-1)
| signature_N-1  |           | eventHash_N    |
+----------------+           | signature_N    |
                             +----------------+
```

Phase 4 – Verify

A verifier can check:

1. Hash integrity: Recompute eventHash from the payload; compare.
2. Chain continuity: Compute hash of previous entry and compare to previousHash.
3. Signature validity: Use the logging entity’s public key to verify the signature over (timestamp, eventHash, previousHash).

If any check fails, the log is considered tampered.

Phase 5 – Store

The audit log MAY be stored in any durable medium:

· Filesystem (JSON lines)
· Append‑only database
· Distributed ledger (as raw bytes, not as a smart contract)
· IPFS / Arweave (no endorsement of tokens)

No specific storage protocol is mandated.

Phase 6 – Expose

For interoperability, the log SHOULD be made available via a well‑known URL or a content‑addressable identifier. The format is a JSON array of AuditLogEntry objects, or newline‑delimited JSON.

Example Genesis Entry (fictional)

```json
{
  "id": "urn:uuid:genesis-0000-0000-0000-000000000001",
  "type": "AuditLogEntry",
  "timestamp": "2026-01-01T00:00:00Z",
  "eventHash": "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855",
  "previousHash": "0000000000000000000000000000000000000000000000000000000000000000",
  "signature": "abc123...signature",
  "payload": {
    "id": "urn:uuid:genesis-event",
    "type": "GovernanceEvent",
    "eventType": "ContextDefined",
    "timestamp": "2026-01-01T00:00:00Z",
    "role": {...},
    "authority": {...},
    "context": {...},
    "proposalId": null
  }
}
```

Relationship to Trust Signal

The audit log provides integrity. The trust‑signal pipeline (see trust-signal-lifecycle.md) provides confidence. A trust signal may reference an audit log entry ID as evidence of authenticity.

Non‑Requirements

· No staking or slashing to enforce honest logging.
· No PBFT or consensus on log order.
· No token reward for providing logs.


