
## Why We Need Post-Quantum Cryptography
Quantum computers break:
- RSA (factoring)
- Diffie–Hellman (discrete log)
- ECC (elliptic curve discrete log)

All widely deployed public-key systems become insecure.

---

## What Quantum Computers Do NOT Break
Quantum computers do NOT efficiently break:
- Symmetric encryption
- Hash functions

They only weaken them via Grover’s algorithm.

---

## Impact of Shor’s Algorithm
Shor’s algorithm solves in polynomial time:
- Integer factorization
- Discrete logarithms

This completely breaks:
- RSA
- Diffie–Hellman
- ECC

---

## Impact of Grover’s Algorithm
Grover’s algorithm speeds up brute-force search:

Instead of:
$$
2^n \text{ steps}
$$
it requires:
$$
2^{n/2} \text{ steps}
$$

Thus we **double symmetric key lengths** for safety.

---

## Families of Post-Quantum Cryptography

Main PQC families:
- Lattice-based
- Code-based
- Multivariate polynomial-based
- Hash-based signatures
- Isogeny-based (partially broken)

---

## Lattice-Based Cryptography (High-Level)
Security is based on problems such as:
- Learning With Errors (LWE)
- Shortest Vector Problem (SVP)

These are believed to be hard even for quantum computers.

---

## Hash-Based Signatures
Built only from:
- One-way functions
- Collision-resistant hash functions

Security relies purely on hash security.

---

## Key Encapsulation Mechanisms (KEMs)
Instead of encrypting messages directly:
- Public key encrypts a random key $k$
- $k$ is used with symmetric crypto

This is the dominant model in PQC.
