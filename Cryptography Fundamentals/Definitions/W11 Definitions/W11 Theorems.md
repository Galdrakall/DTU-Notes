
## Theorem: Shor Breaks RSA and DH
If a large-scale quantum computer exists, then:
- RSA
- Diffie–Hellman
- ECC

are all broken in polynomial time.

---

## Theorem: Grover’s Algorithm Quadratic Speedup
For any symmetric key of length $n$ bits, Grover reduces security to:
$$
2^{n/2}
$$

---

## Theorem: Hash-Based Signatures Are Quantum-Safe
If the underlying hash function is:
- Preimage resistant
- Collision resistant

Then hash-based signature schemes remain secure against quantum adversaries.

---

## Theorem: Lattice Problems Are Quantum-Hard (Conjectured)
No known quantum polynomial-time algorithm exists for:
- LWE
- SVP (approximate)

Thus lattice-based cryptography is believed to be post-quantum secure.

---

## Proof Idea (Grover)
Grover’s algorithm uses quantum amplitude amplification to:
- Increase probability of the correct key
- In only $O(2^{n/2})$ queries

This is optimal for generic search.
