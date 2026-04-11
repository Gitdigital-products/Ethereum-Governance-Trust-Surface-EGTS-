=== FILE: docs/validator-set-spec.md ===

Validator Set Specification – EGTS

1. Overview

The EGTS validator set is responsible for producing integrity proofs, attesting to cross‑chain events, and participating in governance of the EGTS system. Validators stake EGTS native tokens (or ETH) and are subject to slashing for misbehavior.

2. Validator Lifecycle

2.1 Registration

· Candidate submits transaction with:
  · Public key (BLS12‑381 for signature aggregation)
  · ETH address for rewards
  · Bond amount (minimum 1000 EGTS tokens)
· After 7 days of monitoring, candidate enters the active set if bond is above the dynamic threshold (top N by stake).

2.2 Active Set Size

Target size: 100 validators.
Minimum size: 25.
If below minimum, EGTS governance activates a recruitment period with reduced bond requirement.

2.3 Rotation

Every 28 days (one epoch), the bottom 10% of validators by stake are rotated out; the top 10% of candidates rotate in.

2.4 Exit

Validator may voluntarily unstake with a 21‑day cooldown. During cooldown, they must still perform duties or face slashing.

3. Duties

Each validator must:

· Run an integrity proof generator for each registered cross‑chain adapter.
· Submit signed proofs every 6 hours (or every 512 blocks, whichever is longer).
· Participate in on‑chain governance votes (EGTS improvement proposals).
· Maintain an open RPC endpoint for proof verification.

4. Incentives

· Rewards – Issued per submitted proof (fixed amount + share of transaction fees). Rewards are distributed proportionally to stake.
· Penalties – Missing 2 consecutive proof submissions → 10% of bond slashed. Missing 4 → removal and full bond slashed.
· Bonus – Validators that discover and report a faulty cross‑chain adapter receive 5% of the adapter’s bond.

5. Slashing Conditions

Condition Slashing Amount
Double signing (two conflicting proofs) 100% of bond
Failure to submit proof for 2 epochs 10%
Invalid proof (rejected by verifier) 50%
Voting on both sides of a governance proposal 25%

Slashing proceeds are burned (50%) and sent to a treasury (50%).

6. Validator Communication

Validators communicate over a gossip network (libp2p). The network uses a deterministic DHT for proof propagation. All messages are signed and sequenced using a round‑robin leader model.

7. Light Client Verification

External systems can verify validator signatures using the on‑chain validator set root (updated every epoch). A Merkle proof of inclusion of a given validator’s public key can be generated from the root.

8. Emergency Rotation

If more than 1/3 of validators are unresponsive for 12 hours, the Security Council can trigger an emergency rotation, selecting new validators from the candidate pool without the normal rotation delay.
