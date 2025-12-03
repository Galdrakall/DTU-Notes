---
title: "Week 12 Notes – Quantum & Post-Quantum Crypto"
week: 12
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/12, quantum, post-quantum]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_11_Notes]]"
---

# Week 12 – Quantum Computing & Post-Quantum Crypto

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_11_Notes]]

## Quantum bits & gates

Classical bit: 0 or 1.

Quantum bit (**qubit**):

- State $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$ with complex amplitudes $\alpha, \beta$.
- Measurement collapses to 0 or 1 with probabilities $|\alpha|^2$ and $|\beta|^2$.

Quantum gates:

- Reversible linear operations on qubits (unitary matrices).
- Common gates:
  - Hadamard (H): creates superposition.
  - Pauli-X, Y, Z.
  - CNOT (controlled-NOT) for entanglement.

Quantum circuits:

- Sequences of gates applied to qubits.
- Computation exploits **superposition** and **entanglement**.

---

## Shor’s Algorithm

Peter Shor’s algorithm (1994):

- Quantum algorithm that factors integers and computes discrete logarithms in **polynomial time**.
- On a large enough quantum computer, it breaks:
  - RSA (factoring modulus $N$).
  - DH and ECC (discrete logarithms).

Implication:

- Most currently deployed public-key cryptography (RSA, DH, ECDH, ECDSA) is insecure against large-scale quantum computers.

Symmetric-key impact:

- Grover’s algorithm speeds up brute-force search quadratically.
- Effective security of $n$-bit key becomes roughly $n/2$ bits.
- Doubling key sizes (e.g. AES-256) mitigates this for realistic quantum models.

---

## Quantum attacks on RSA, DH, ECC

- **RSA**: Shor’s algorithm factors modulus $N$, allowing attacker to recover private key.
- **DH** (finite field) and **ECC**:
  - Shor’s algorithm solves discrete log problems efficiently.
  - Breaks Diffie–Hellman key exchange and signatures based on DLP/ECDLP.

So post-quantum transition requires new public-key primitives.

---

## Post-quantum cryptography

**Post-quantum (PQ) cryptography** aims to design schemes secure against both classical and quantum adversaries.

Desirable:

- Security based on problems with no known efficient quantum algorithms.
- Efficient enough for deployment (keys, ciphertexts, signatures, computation time).

Major families:

- Lattice-based.
- Code-based.
- Multivariate polynomial.
- Hash-based.
- Isogeny-based (though some have been broken).

Standardization efforts (e.g. NIST PQC) select practical schemes for KEMs and signatures.

---

## Lattice-based crypto (LWE, Regev)

Lattice-based cryptography relies on hard problems about high-dimensional lattices.

**Learning With Errors (LWE)**:

- Goal: recover hidden vector $s$ from noisy linear equations:

$$
(A, As + e)
$$

where:

- $A$ is random matrix,
- $s$ secret vector,
- $e$ is “small” error vector.

Problem: Given pairs $(A, As + e)$, distinguish them from random or recover $s$.

**Regev’s cryptosystem**:

- A public-key encryption scheme based on LWE.
- Provides a quantum-resistant alternative to RSA/DH.

Advantages of lattice-based crypto:

- Believed secure against quantum attacks (no Shor-like algorithm known).
- Supports advanced primitives:
  - Fully homomorphic encryption (FHE).
  - Identity-based encryption.
  - Functional encryption.

Downsides:

- Larger key sizes and ciphertexts compared to classical RSA/ECC (but still practical in many settings).
