
## Intuition: Quantum as a “Lock-Picking Machine”
Quantum computers do not break all cryptography.
They only break cryptography whose security relies on:
- Factoring
- Discrete logarithms

---

## Example: Why AES Still Works
AES-128 security against quantum becomes:
$$
2^{64}
$$
which is borderline.

AES-256 becomes:
$$
2^{128}
$$
which is safe.

---

## Intuition: Why Lattices Are Different
Lattice problems involve:
- High-dimensional geometry
- Approximation problems

Quantum computers do not currently speed these up efficiently.

---

## Example: Hybrid TLS
Modern systems may use:
- Classical key exchange (e.g., ECDH)
- Plus a PQC KEM

Both keys are combined to form the session key.

Even if one breaks, security remains.

---

## Intuition: Hash-Based Signatures
If you trust hash functions today,
you can trust hash-based signatures after quantum.
