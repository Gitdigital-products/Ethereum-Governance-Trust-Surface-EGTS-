=== FILE: docs/security-model.md ===

Security Model – Ethereum Governance Trust Surface (EGTS)

1. Trust Assumptions

EGTS assumes the following minimal trust anchors:

· Ethereum L1 consensus – Finality and reorg resilience.
· Cryptographic primitives – SHA‑256, secp256k1, and zero‑knowledge proofs are secure.
· Validator honesty – At least 2/3 of stake-weighted validators behave correctly for liveness and safety.

No additional trusted third parties are required for core verification.

2. Threat Model

2.1 Active Threats

· Registry poisoning – Insertion of false contributor identities.
· Badge forgery – Issuance of badges without proper authority.
· Proof equivocation – Validators signing conflicting proofs.
· Cross‑chain censorship – Adapter missing or withholding governance events.
· Sybil attacks – One entity controlling many contributor entries.

2.2 Passive Threats

· Metadata surveillance – Leakage of contributor metadata URIs.
· Governance fingerprinting – Correlation of badge sets to real identities.

3. Mitigations

Threat Mitigation
Registry poisoning Proof‑of‑contribution + dispute period + social slashing
Badge forgery Badge Authority contract with role‑based permissions and timelocks
Proof equivocation On‑chain slashing for double‑signing, fork detection
Cross‑chain censorship Multiple independent adapters + challenge mechanism
Sybil attacks Staking requirement or proof‑of‑humanity integration
Metadata surveillance Optional off‑chain encryption with viewer‑specific keys

4. Cryptographic Guarantees

· Integrity proofs – Computational binding to source chain state.
· Signature aggregation – BLS multi‑signatures reduce validator overhead.
· Zero‑knowledge proofs – Hide contributor‑specific data while proving membership.

5. Validator Security

Validators must:

· Run redundant nodes with tamper‑proof hardware (optional TEE).
· Publish their public keys and identity statements on‑chain.
· Maintain a backup recovery procedure with multi‑sig.

Failure to produce proofs for more than 2 epochs results in automatic removal from the validator set.

6. Formal Verification Scope

The following components are formally verified:

· Integrity proof verification circuit.
· Cross‑chain adapter state transition function.
· Badge revocation logic.

Other components rely on extensive testing and audit.

7. Emergency Procedures

A Security Council (multi‑sig with 7-of-11) can:

· Pause badge issuance for 48 hours.
· Freeze cross‑chain adapters.
· Initiate a validator set emergency rotation.

All emergency actions must be followed by a community review and a retrospective report within 14 days.
