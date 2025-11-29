
## Intuition: Digital Fingerprints
A hash function is like a **digital fingerprint**:
- Small change in message → completely different hash
- Same message → always same hash

---

## Example: Avalanche Effect
If:
$$
H(\text{"hello"}) = x
$$
Then:
$$
H(\text{"Hello"}) \neq x
$$
Even one bit change flips about half the output bits.

---

## Intuition: Why Collisions Must Exist
There are infinitely many inputs but only $2^n$ outputs.
By the pigeonhole principle, collisions must exist.

Cryptography only requires that they be **hard to find**.

---

## Example: Birthday Paradox
In a room of 23 people, probability of shared birthday > 50%.
Similarly, collisions appear after only $2^{n/2}$ hashes.

---

## Intuition: Length Extension as Continued Hashing
Merkle–Damgård hashes just keep chaining forward.
If you know a chaining value, you can continue hashing.

---

## Example: Salted Password Hash
Instead of:
$$
H(p)
$$
store:
$$
H(s \parallel p)
$$
Each user has a different $s$, so precomputed attacks fail.

---

## Intuition: Why HMAC Is Safe
HMAC hides the internal state behind two layers of hashing.
Length extension cannot be applied to break it.
