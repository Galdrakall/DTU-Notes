
## Intuition: Publicly Verifiable Seal
A digital signature is like:
- A tamper-proof seal
- That anyone can verify
- But only one person can create

---

## Example: Contract Signing
Alice signs a contract.
Anyone with Alice’s public key can verify:
- Alice approved the exact contents
- The contract was not modified

---

## Intuition: Why Hashing Is Required
Signing the whole message is inefficient.
Hashing compresses the message into a short fingerprint.

Only the fingerprint is signed.

---

## Example: Hash-and-Sign Flow
1. Compute:
   $$
   h = H(m)
   $$
2. Compute:
   $$
   \sigma = h^d \bmod N
   $$
3. Verify:
   $$
   h \stackrel{?}{=} \sigma^e \bmod N
   $$

---

## Intuition: Why MACs and Signatures Are Different
A MAC proves:
- “Someone with the shared secret sent this.”

A signature proves:
- “This specific person signed this.”
