---
title: "Week 12 Notes – Quantum & Post-Quantum Crypto"
week: 12
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/12, quantum, post-quantum]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_11_Notes]]"
---

---
# Week 12 – Quantum Computing & Post-Quantum Cryptography

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_11_Notes]] Next: [[Week_13_Notes]]
---
## Why Quantum Computing Matters for Cryptography

Quantum computers:
- Exploit **superposition** and **entanglement**
- Can solve certain mathematical problems **exponentially faster** than classical computers
- Directly threaten:
  - RSA
  - Diffie–Hellman
  - Elliptic-curve cryptography

Key message:
> Once large-scale quantum computers exist, **all widely used public-key crypto breaks**.

---

## Classical Bits vs Quantum Bits

### Classical Bit
$$
b \in \{0,1\}
$$

### Quantum Bit (Qubit)
$$
|\psi\rangle = \alpha|0\rangle + \beta|1\rangle, \quad |\alpha|^2 + |\beta|^2 = 1
$$

Measurement:
- Outputs 0 with probability $|\alpha|^2$
- Outputs 1 with probability $|\beta|^2$

Key quantum effects:
- **Superposition** – many states at once
- **Entanglement** – correlated particles
- **Interference** – probability amplitudes add/cancel

---

## Quantum Gates

Quantum gates are **unitary matrices** (reversible):

Common gates:
- Hadamard (H): creates superposition  
- Pauli-X, Y, Z  
- CNOT: creates entanglement

Quantum circuits:
- Sequences of quantum gates
- Operate on $|\psi\rangle \in \mathbb{C}^{2^n}$

---

### ✅ Practical Qubit Question

**Question:**  
Why must quantum gates be reversible?

**Solution:**  
Because quantum operations are unitary transformations, which always have inverses to preserve total probability.

---

## Shor’s Algorithm

Peter Shor (1994) discovered a quantum algorithm that:

- Factors integers in **polynomial time**
- Computes **discrete logarithms** in polynomial time

This breaks:

- RSA (factoring $N$)
- Diffie–Hellman (DLP)
- Elliptic-curve crypto (ECDLP)

Thus:
> RSA, DH, ECDH, ECDSA, ElGamal → **all broken by large quantum computers**

---

## How Shor Breaks RSA (High Level)

Problem:
$$
N = pq \quad \text{(find } p,q\text{)}
$$

Classical:
- Factoring is believed to take exponential time

Quantum:
1. Reduce factoring to **period finding**
2. Use **Quantum Fourier Transform (QFT)**
3. Recover period $r$
4. Compute:
$$
\gcd(a^{r/2} - 1, N)
$$
to obtain a factor

Thus factoring becomes efficient.

---

## Grover’s Algorithm (Symmetric Crypto)

Grover gives a **quadratic speedup** for brute force:

- Search space size: $2^n$
- Classical: $2^n$ guesses
- Quantum: $2^{n/2}$ guesses

Implication:

| Key Size | Classical Security | Quantum Security |
|----------|---------------------|------------------|
| 128-bit  | $2^{128}$ | $2^{64}$ |
| 256-bit  | $2^{256}$ | $2^{128}$ |

Design rule:
> **Double symmetric key sizes for post-quantum security (e.g. AES-256).**

---

### ✅ Practical Grover Question

**Question:**  
What AES key size gives ~128-bit security against a quantum attacker?

**Solution:**  
AES-256.

---

## What Is Quantum-Vulnerable?

Broken by Shor:
- ✅ RSA
- ✅ Diffie–Hellman
- ✅ Elliptic-curve crypto

Unaffected in principle:
- ✅ Symmetric crypto (AES, ChaCha20)
- ✅ Hash functions (SHA-256, SHA-3)
  - Only weakened by Grover, not broken

---

## Store-Now-Decrypt-Later Threat

Attack model:

1. Adversary records **encrypted traffic today**
2. Stores it for 10–20 years
3. Decrypts it later using a quantum computer

Even if:
- We upgrade security **after** quantum computers exist,
- **Old recorded data becomes readable**.

This is why **long-term confidentiality must migrate early**.

---

## Example: TLS Under Quantum Attack

Typical modern TLS stack (as seen in slides):

- Key exchange: **ECDHE (P-256)** → quantum-broken
- Authentication: **RSA signatures** → quantum-broken
- Symmetric encryption: **AES-128-GCM** → quantum-weakened only
- Hashing: **SHA-384 (Merkle–Damgård)** → Grover-weakened only

Thus:
- **Handshake is completely broken**
- **Session encryption remains mostly safe**

---

## When Must We Migrate? (Mosca’s Theorem)

Let:
- $x$ = how long data must stay secret
- $y$ = how long migration takes
- $z$ = time until large-scale quantum computer exists

**Mosca’s Theorem:**
> If $x + y > z$, then we must migrate **now**.

Corollary:
- If $x > z$ → long-term secrets already doomed
- If $y > z$ → transition itself too slow

---

### ✅ Practical Mosca Example

**Question:**  
If data must stay secret for 30 years and migration takes 10 years, what quantum timeline is dangerous?

**Solution:**  
If $z < 40$ years, migration must start immediately.

---

## Post-Quantum Cryptography (PQC)

Goal:
> Cryptography secure against **both classical and quantum attackers**.

Main families:

- **Lattice-based** (LWE, Ring-LWE)
- **Code-based** (McEliece)
- **Hash-based** (XMSS, SPHINCS+)
- Multivariate polynomial
- Isogeny-based (many now broken)

These are used for:
- Key exchange (KEMs)
- Digital signatures

---

## Lattice-Based Cryptography (Recap)

Based on **Learning With Errors (LWE)**:

Given:
$$
(A, As + e)
$$

Goal:
- Recover $s$, or
- Distinguish from random

Properties:
- Based on **worst-case lattice hardness**
- Believed secure against **quantum attacks**
- Enables advanced crypto:
  - Fully homomorphic encryption
  - Functional encryption

---

## NIST Post-Quantum Standardization

NIST selected PQ standards:

- **FIPS 203**: Module-Lattice KEM (Kyber-like)
- **FIPS 204**: Module-Lattice Signatures (Dilithium)
- **FIPS 205**: Stateless Hash-Based Signatures (SPHINCS+)
- Upcoming:
  - FIPS 206: FN-DSA
  - Code-based KEMs (e.g., HQC)

Adoption:
- Ongoing but **slow**
- Requires:
  - Software updates
  - Hardware updates
  - Protocol redesigns

---

## Signal & Post-Quantum Security

Modern Signal protocol now includes:
- **Post-quantum KEM** inside key agreement (PQXDH / PQ3DH variants)
- Combined with classical X3DH

Provides:
- Forward secrecy
- Post-compromise security
- **Post-quantum confidentiality**

This is called a **hybrid key exchange**:
> Classical DH + PQ KEM together.

---

## Quantum Cryptography (Different Concept)

Not post-quantum crypto, but **physical quantum protocols**:

Example:
- **Quantum Key Distribution (QKD)**

Security:
- Guaranteed by physics
- Detects eavesdropping

Downsides:
- Expensive
- Short distance
- Specialized hardware
- Not widely scalable

Thus:
- PQ cryptography is the **practical solution**
- QKD is niche research / infrastructure tech

---

## Summary – Quantum Threat Model

- Quantum computers:
  - Are fundamentally different from classical computers
  - Excel at discovering **structure in mathematical problems**
- Break:
  - RSA, DH, ECC
- Weaken:
  - Symmetric crypto and hashes via Grover
- Transition must consider:
  - Long-term secrecy
  - Migration timelines
- We are:
  - **Partially prepared**
  - But real-world transition will take **many years**

---

## Final Exam Key Takeaways (Week 12)

You must be able to:

- Explain what a qubit is
- Describe superposition and entanglement at a high level
- Explain Shor’s algorithm and what it breaks
- Explain Grover’s algorithm and its effect on AES
- Identify which cryptosystems are quantum-vulnerable
- Explain “store-now-decrypt-later”
- State and apply Mosca’s Theorem
- List major families of post-quantum crypto
- Explain why lattice-based crypto is important
- Explain the role of NIST PQC standardization
- Distinguish:
  - Post-quantum cryptography vs quantum cryptography

---

## Week 12 Cheat Sheet

**Quantum basics:**
- Qubit: $|\psi\rangle = \alpha|0\rangle + \beta|1\rangle$ with $|\alpha|^2+|\beta|^2=1$.
- Key phenomena: superposition, entanglement, interference.
- Quantum gates are unitary (reversible) operations.

**Quantum algorithms:**
- Shor: polynomial-time factoring and discrete log → breaks RSA, DH, ECC.
- Grover: quadratic speedup for brute-force search → reduces effective key length by factor 2.

**Impact on crypto:**
- Public-key: RSA, DH, ECDH, ECDSA, ElGamal become insecure once large quantum computers exist.
- Symmetric/hash: AES/SHA weakened only by Grover; doubling key size restores ~same security.

**Store-now-decrypt-later:**
- Adversary records ciphertexts today and decrypts them in the future when quantum computers exist.
- Threatens long-lived secrets (medical data, government secrets, etc.).

**Mosca’s rule:**
- If data must be secret for $x$ years and migration takes $y$ years, and quantum computer arrives in $z$ years, then if $x + y > z$ you must start migration **now**.

**Post-quantum crypto (PQC):**
- Goal: schemes secure against classical and quantum adversaries.
- Main families: lattice-based (LWE, Ring-LWE), code-based, hash-based, multivariate, isogeny-based (many broken).

**NIST PQC standardization:**
- KEMs: Kyber-like (lattice) for key exchange.
- Signatures: Dilithium (lattice), SPHINCS+ (hash-based), etc.
- Driving real-world adoption and parameter choices.

**Hybrid key exchange:**
- Combine classical DH/ECDH with PQ KEM (e.g. in Signal, TLS experiments).
- Security relies on both; attacker must break **both** schemes.

**Quantum vs post-quantum crypto:**
- Quantum crypto: uses quantum channels (e.g., QKD) and physics-level guarantees.
- Post-quantum crypto: classical algorithms resistant to quantum attacks; deployable on today’s networks.

### Extra Mini Questions (Practice)

1. **Shor vs Grover:** In one or two sentences, contrast what types of problems Shor’s and Grover’s algorithms speed up.
2. **AES key sizing:** Explain why AES‑256 is recommended instead of AES‑128 in a long-term post‑quantum setting.
3. **Migration timing:** Given $x=25$ years confidentiality, $y=8$ years migration time, and an estimate $z=30$ years for quantum computers, decide if migration needs to start now and justify using Mosca’s rule.
4. **PQC family match:** For each use case (KEM, signatures), name one lattice-based and one non-lattice-based candidate.

### True/False Practice – Week 12 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. Shor’s algorithm gives a polynomial-time quantum algorithm for factoring integers and computing discrete logarithms. (**True**)
2. Grover’s algorithm gives an exponential speedup for brute-force key search against symmetric ciphers. (**False**)
3. Under Shor’s algorithm, RSA, Diffie–Hellman, and elliptic-curve cryptosystems are considered broken in the presence of a sufficiently large quantum computer. (**True**)
4. Grover’s algorithm effectively halves the security level of symmetric keys, so a 128-bit key behaves like roughly a 64-bit key against a large quantum adversary. (**True**)
5. Hash functions are completely insecure against quantum adversaries because of Shor’s algorithm. (**False**)
6. Post-quantum cryptography refers to schemes that run on classical computers but resist attacks by quantum computers. (**True**)
7. A scheme is called quantum-safe if there is a proof that no quantum algorithm can break it. (**False**)
8. Lattice-based cryptography (e.g., LWE-based schemes) is one of the main families of post-quantum cryptography. (**True**)
9. The “store now, decrypt later” threat assumes attackers may record ciphertexts today and decrypt them in the future once quantum computers become powerful enough. (**True**)
10. Mosca’s rule combines the time to build large quantum computers, the time you need confidentiality, and the time to migrate to PQC when planning deployments. (**True**)
11. If your system needs data confidentiality for 30 years, and you expect PQC deployment to take 5 years, you can safely delay thinking about PQC until quantum computers exist. (**False**)
12. Code-based, lattice-based, hash-based, and multivariate polynomial-based schemes are among the main PQC families considered by NIST. (**True**)
13. NIST’s PQC standardization process focuses only on key encapsulation mechanisms (KEMs) and does not include digital signatures. (**False**)
14. Hybrid key exchange sends both a classical and a PQ key exchange in parallel and derives the final session key from both secrets. (**True**)
15. In a well-designed hybrid KEX, the session key remains secure if at least one of the underlying key exchanges remains secure. (**True**)
16. Replacing RSA with a lattice-based KEM in a protocol automatically provides forward secrecy even if the original protocol did not use ephemeral keys. (**False**)
17. Post-quantum digital signatures (e.g., lattice-based or hash-based) aim to provide EUF-CMA security even against quantum adversaries. (**True**)
18. Symmetric-key lengths and hash output sizes may need to be increased slightly to account for Grover’s algorithm in long-term security analyses. (**True**)
19. A “quantum” cryptosystem always requires quantum communication channels between parties. (**False**)
20. Post-quantum secure messaging protocols may use classical constructions (like Double Ratchet) but replace underlying key exchanges and signatures with PQC alternatives. (**True**)

#### Extra True/False – Week 12 Interactions

21. Shor’s algorithm breaks both RSA and symmetric-key primitives like AES at roughly the same complexity. (**False**)
22. Grover’s algorithm can be largely mitigated in symmetric cryptography by doubling key sizes, which is why AES-256 is preferred over AES-128 in long-term post-quantum settings. (**True**)
23. A hybrid TLS key exchange that combines ECDHE with a lattice-based KEM can remain confidential even if either classical or post-quantum assumptions fail, as long as at least one side remains secure. (**True**)
24. Deploying post-quantum signatures in a system immediately guarantees protection against store-now-decrypt-later attacks on past traffic. (**False**)
25. Mosca’s rule suggests starting PQC migration when the sum of required confidentiality lifetime and migration time is greater than the estimated time to large-scale quantum computers. (**True**)