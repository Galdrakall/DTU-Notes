

This sheet compares the three most important **public-key cryptography families**:
- RSA
- Diffie–Hellman (DH)
- Elliptic Curve Cryptography (ECC)

Use this for:
- Fast exam revision
- Choosing the right primitive
- Understanding performance and security trade-offs

---

## 1. One-Line Purpose

| Scheme | Main Purpose |
|--------|--------------|
| RSA | Public-key encryption and digital signatures |
| Diffie–Hellman | Key exchange |
| ECC | Encryption, signatures, and key exchange with smaller keys |

---

## 2. Underlying Mathematical Problem

| Scheme | Hard Problem |
|--------|--------------|
| RSA | Integer factorization |
| Diffie–Hellman | Discrete logarithm problem |
| ECC | Elliptic curve discrete logarithm problem (ECDLP) |

---

## 3. Core Mathematical Setting

### RSA
Arithmetic in:
$$
\mathbb{Z}_N \quad \text{where } N = p \cdot q
$$

### Diffie–Hellman
Exponentiation in a finite cyclic group:
$$
\mathbb{Z}_p^*
$$

### ECC
Point addition and scalar multiplication on elliptic curves over finite fields.

---

## 4. Core Algorithms (Formulas)

### RSA
Encryption:
$$
c = m^e \bmod N
$$

Decryption:
$$
m = c^d \bmod N
$$

---

### Diffie–Hellman Key Exchange
Public generator $g$, prime $p$.

Alice:
$$
a \leftarrow \mathbb{Z}_p, \quad A = g^a \bmod p
$$

Bob:
$$
b \leftarrow \mathbb{Z}_p, \quad B = g^b \bmod p
$$

Shared secret:
$$
K = g^{ab} \bmod p
$$

---

### Elliptic Curve Diffie–Hellman (ECDH)

Alice:
$$
a \leftarrow \mathbb{Z}_n, \quad A = a \cdot G
$$

Bob:
$$
b \leftarrow \mathbb{Z}_n, \quad B = b \cdot G
$$

Shared secret:
$$
K = ab \cdot G
$$

---

## 5. What Each One Is Used For

| Task | RSA | DH | ECC |
|------|-----|----|-----|
| Key exchange | Yes (indirect) | Yes (native) | Yes (ECDH) |
| Encryption | Yes | No | Yes (ECIES) |
| Digital signatures | Yes | No | Yes (ECDSA, EdDSA) |
| TLS handshakes | Legacy | Yes | Yes |
| Mobile / IoT | Rare | Rare | Common |

---

## 6. Deterministic vs Randomized

| Scheme | Deterministic by Default? |
|--------|----------------------------|
| RSA | Yes (unsafe without padding) |
| Diffie–Hellman | No (uses random exponents) |
| ECC | No (uses random scalars) |

Raw RSA must use **random padding** to achieve IND-CPA security.

---

## 7. Typical Key Sizes for ~128-Bit Security

| Scheme | Key Size |
|--------|----------|
| RSA | 3072 bits |
| Diffie–Hellman | 3072 bits |
| ECC | 256 bits |

ECC provides **equal security with far smaller keys**.

---

## 8. Efficiency Comparison

| Property | RSA | DH | ECC |
|----------|-----|----|-----|
| Key generation | Slow | Fast | Fast |
| Encryption | Moderate | N/A | Fast |
| Decryption | Very slow | N/A | Fast |
| Key exchange | Indirect | Fast | Very fast |
| Bandwidth | Large keys | Large keys | Small keys |

ECC is the most efficient in:
- CPU usage
- Memory
- Network bandwidth

---

## 9. Security Properties

| Property | RSA | DH | ECC |
|----------|-----|----|-----|
| Based on well-studied math | Yes | Yes | Yes |
| Forward secrecy | No (by itself) | Yes | Yes |
| Vulnerable to deterministic attacks | Yes (without padding) | No | No |
| Efficient at scale | No | Moderate | Yes |

---

## 10. Forward Secrecy

### Diffie–Hellman and ECC
Provide **forward secrecy** because session keys are derived from fresh randomness:
$$
a, b \text{ are ephemeral}
$$

Compromise of long-term keys does **not** reveal past session keys.

---

### RSA
Standard RSA key exchange:
$$
c = k^e \bmod N
$$
does **not** give forward secrecy if the RSA private key is later leaked.

---

## 11. Main Attacks

| Scheme | Main Attacks |
|--------|--------------|
| RSA | Factoring, low-exponent attacks, padding oracles |
| DH | Small subgroup attacks, man-in-the-middle |
| ECC | Weak curves, implementation flaws |

---

## 12. Quantum Threat

Shor’s algorithm breaks:
- RSA
- Diffie–Hellman
- ECC

All three are **not quantum-safe**.

---

## 13. Real-World Usage Today

| Area | Dominant Choice |
|------|-----------------|
| Web (TLS 1.3) | ECDHE + ECDSA |
| Secure messaging | ECC |
| Smart cards | ECC |
| Legacy PKI | RSA |
| Embedded systems | ECC |

RSA is slowly being phased out in favor of ECC.

---

## 14. Strengths and Weaknesses Summary

### RSA
**Strengths**
- Simple mathematics
- Widely deployed
- Supports encryption and signatures

**Weaknesses**
- Large keys
- Slow decryption
- No forward secrecy
- Requires padding for security

---

### Diffie–Hellman
**Strengths**
- Natural key exchange
- Forward secrecy
- Simple protocol

**Weaknesses**
- Needs authentication
- Large parameters
- MITM without signatures

---

### ECC
**Strengths**
- Small keys
- Fast operations
- Forward secrecy
- Ideal for modern systems

**Weaknesses**
- More complex math
- Curve choice is critical
- Harder to implement correctly

---

## 15. Exam-Critical Comparisons

| Question | Correct Answer |
|----------|----------------|
| Which gives forward secrecy? | DH and ECC |
| Which needs padding to be secure? | RSA |
| Which is fastest per bit of security? | ECC |
| Which is deterministic by default? | RSA |
| Which is best for mobile devices? | ECC |
| Which is broken by factoring? | RSA |
| Which is broken by ECDLP? | ECC |

---

## 16. Quick Memory Hooks

- **RSA** → factoring, encryption & signatures, big keys  
- **DH** → shared secret, forward secrecy  
- **ECC** → same security, tiny keys, fastest in practice  
