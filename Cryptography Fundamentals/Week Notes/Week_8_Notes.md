---
title: "Week 8 Notes – DLP, DH & ElGamal"
week: 8
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/8, dlog, dh, elgamal]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_7_Notes]]"
  next: "[[Week_9_Notes]]"
---
---
# Week 8 – Discrete Logarithms, Diffie–Hellman & LWE Cryptography

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_7_Notes]] · Next: [[Week_9_Notes]]

---

## Cyclic Groups

A group $G$ with operation $\cdot$ is **cyclic** if there exists $g \in G$ such that

$$
G = \{g^0, g^1, g^2, \dots, g^{n-1}\}.
$$

- $g$ is called a **generator**.
- Example: $(\mathbb{Z}_p^*,\cdot)$ for prime $p$ is cyclic of order $p-1$.
- Elliptic curve groups are also commonly used.

---

### ✅ Practical Group Question

**Question:**  
Why do we prefer groups with large prime order in cryptography?

**Solution:**  
To avoid small-subgroup attacks, which leak partial information about secret exponents.

---

## Discrete Logarithm Problem (DLP)

Given:
- Cyclic group $G$ of order $n$ with generator $g$,
- $h = g^x$,

the **Discrete Logarithm Problem** is:

> Find $x$ given $(g,h)$.

Security of:
- Diffie–Hellman,
- ElGamal,
- many signatures,
relies on hardness of DLP or related problems.

---

## Computational & Decisional Diffie–Hellman

- **CDH:** Given $(g^a,g^b)$ compute $g^{ab}$.
- **DDH:** Distinguish $(g^a,g^b,g^{ab})$ from $(g^a,g^b,g^c)$.

Hierarchy (typical belief in good groups):

$$
\text{DLP} \Rightarrow \text{CDH} \Rightarrow \text{DDH}.
$$

---

### ✅ Practical Assumption Question

**Question:**  
Why does ElGamal require DDH and not just CDH?

**Solution:**  
Because security proof requires ciphertext components to be *indistinguishable from random*, not merely hard to compute.

---

## Diffie–Hellman Key Exchange

Public parameters: prime $p$, generator $g$.

1. Alice chooses $a$, sends $A=g^a$.
2. Bob chooses $b$, sends $B=g^b$.
3. Alice computes $K=B^a=g^{ab}$.
4. Bob computes $K=A^b=g^{ab}$.

Shared secret: $K=g^{ab}$.

Security based on **CDH**.

---

## Man-in-the-Middle Attack on DH

Unauthenticated DH is vulnerable:

- Attacker replaces $g^a$ and $g^b$ with own values.
- Two separate keys are formed with attacker.
- Alice and Bob never talk directly.

**Fix:** authenticated DH using:
- signatures,
- certificates,
- MACs with PSKs,
- or authenticated key-exchange protocols.

---

## ElGamal Encryption (Recap)

Keygen:
- Choose $x \in \mathbb{Z}_q$
- $pk = g^x$

Encryption of $m \in G$:
1. Choose $r$
2. $c_0 = g^r$
3. $c_1 = m\cdot pk^r$

Decryption:
$$
m = c_1 \cdot (c_0^x)^{-1}
$$

ElGamal is **IND-CPA secure under DDH**, but **malleable** and not IND-CCA secure.

---

### ✅ Practical ElGamal Attack

**Question:**  
If attacker multiplies $c_1$ by $g^t$, what happens?

**Solution:**  
Plaintext is multiplied by $g^t$ → malleability → not IND-CCA secure.

---

## Is Unconditional Security Possible for PKE?

For symmetric encryption: OTP gives unconditional security.  
For **public-key encryption**:

> **Unconditional security is impossible**, because the attacker can always encrypt messages with the public key and compare outputs.

Thus PKE necessarily relies on **computational hardness assumptions**.

---

## The Learning With Errors (LWE) Problem

Let:
- $A \in \mathbb{Z}_p^{\ell \times n}$
- $s \in \mathbb{Z}_p^n$
- $e \in \mathbb{Z}_p^\ell$ sampled from a **small noise distribution**

Define:
$$
b = As + e \pmod p.
$$

### Search-LWE
Given $(A,b)$, recover $s$.

### Decision-LWE
Distinguish:
- $(A,As+e)$ from
- $(A,u)$ with $u$ uniform.

LWE remains hard **even for quantum attackers** → basis of **post-quantum cryptography**.

---

### ✅ Practical LWE Intuition

**Question:**  
Why does Gaussian elimination fail for LWE?

**Solution:**  
Because the equations contain *noise*, so solutions are only approximate and classical linear algebra breaks.

---

## Discrete Gaussian Noise

Noise $e_i$ is sampled from a **discrete Gaussian**:

$$
q_\sigma(z) = \frac{e^{-z^2/2\sigma^2}}{\sum_{y \in \mathbb{Z}} e^{-y^2/2\sigma^2}}
$$

This keeps noise:
- small enough for correct decryption,
- large enough to hide linear structure.

---

## Regev’s Public-Key Encryption (LWE-Based)

### Key Generation
1. Sample:
   - $A \in \mathbb{Z}_p^{\ell \times n}$
   - $s \in \mathbb{Z}_p^n$
   - $e \in \mathbb{Z}_p^\ell$
2. Compute:
$$
b = As + e
$$
3. Output:
- $pk=(A,b)$
- $sk=s$

---

### Encryption of $m \in \{0,1\}$

1. Sample $r \in \{0,1\}^\ell$
2. Compute:
$$
c_0 = r^T A
$$
$$
c_1 = r^T b + m\cdot \frac{p-1}{2}
$$
3. Output:
$$
c=(c_0,c_1)
$$

---

### Decryption

Compute:
$$
m' = \left\lfloor \frac{2}{p}(c_1 - c_0 s) \right\rceil
$$

Correct because:
$$
c_1 - c_0 s = r^T e + m\cdot \frac{p-1}{2},
$$
and $r^T e$ is small.

---

### ✅ Practical Correctness Check

**Question:**  
Why does rounding recover $m$?

**Solution:**  
Noise term $r^T e$ is very small compared to $(p-1)/2$, so rounding correctly separates 0 from 1.

---

## IND-CPA Security of Regev PKE

Security is reduced to **Decision-LWE**:

Games:
- $L_{CPA-0}$ encrypts $m_0$
- $L_{CPA-1}$ encrypts $m_1$
- $L_{CPA-rand}$ outputs random ciphertexts

Using:
- Hybrid arguments,
- **Leftover Hash Lemma** (randomness extraction from $r^T A$),

we prove:
$$
L_{CPA-0} \approx L_{CPA-1}.
$$

Thus Regev PKE is **IND-CPA secure based solely on LWE**.

---

## Leftover Hash Lemma (Intuition)

For universal hash family $H_k(x)$:

If $x$ has **sufficient min-entropy**, then:
$$
(k,H_k(x)) \approx (k,U)
$$
where $U$ is uniform.

Used here to argue that $r^T A$ and $r^T b$ are statistically close to uniform.

---

## Efficiency Improvements

### 1. Encrypt Vectors

Instead of scalar $s$:
- Use matrix $S \in \mathbb{Z}_p^{n \times v}$
- Encrypt vector messages $m \in \{0,1\}^v$.

This amortizes cost per bit.

---

### 2. Larger Plaintext Alphabet

Instead of 1 bit:
$$
m \in \{0,\dots,t-1\}
$$
using:
$$
m \cdot \frac{p-1}{t}
$$

Improves bandwidth.

---

### 3. Algebraic Speedups (Ring-LWE)

Replace matrices by **polynomials**:
- Faster multiplication via FFT/NTT
- Smaller public keys
- Used in NIST post-quantum standards (e.g., Kyber)

---

## ElGamal vs Regev

| Property | ElGamal | Regev PKE |
|----------|---------|-----------|
| Assumption | DDH | LWE |
| Quantum-safe | ❌ | ✅ |
| IND-CPA | ✅ | ✅ |
| IND-CCA | ❌ | ❌ (needs wrapper) |
| Practical use | Classical crypto | Post-quantum crypto |

---

## Final Exam Key Takeaways (Week 8)

You must be able to:

- Define DLP, CDH, DDH
- Explain MITM attack on DH
- Describe ElGamal encryption and why it is only IND-CPA
- State Search-LWE vs Decision-LWE
- Write Regev KeyGen / Enc / Dec
- Explain why LWE hides linear equations
- Explain IND-CPA proof idea via LWE
- Understand efficiency improvements and Ring-LWE motivation
- Compare ElGamal vs Regev

---

## Week 8 Cheat Sheet

**Groups & assumptions:**
- Cyclic group $G=\langle g\rangle$ of (usually prime) order.
- DLP: given $(g,g^x)$, find $x$.
- CDH: given $(g^a,g^b)$, compute $g^{ab}$.
- DDH: distinguish $(g^a,g^b,g^{ab})$ from $(g^a,g^b,g^c)$.

**Diffie–Hellman key exchange:**
- Public $g$; Alice sends $g^a$, Bob sends $g^b$; both compute $g^{ab}$.
- Security: CDH/ DDH assumptions; vulnerable to MITM without authentication.

**ElGamal recap:**
- Keygen: secret $x$, public $g^x$.
- Enc: $(g^r, m\cdot g^{xr})$; Dec: divide by $(g^r)^x$.
- IND-CPA under DDH; malleable, not IND-CCA.

**LWE basics:**
- Given $(A,b=As+e)$ with small noise $e$, Search-LWE: recover $s$; Decision-LWE: distinguish $As+e$ from random.
- Remains hard even with quantum computers → post-quantum foundation.

**Regev PKE (high level):**
- Keygen: $pk=(A,As+e)$, $sk=s$.
- Enc: choose random $r$; compute $(c_0=r^TA, c_1=r^Tb + m\cdot (p-1)/2)$.
- Dec: compute $c_1 - c_0s = r^Te + m\cdot (p-1)/2$, then round to recover $m$.

**Why Regev is IND-CPA:**
- If Decision-LWE holds, $(A,As+e)$ is indistinguishable from $(A,u)$.
- Hybrids + leftover hash lemma show encryptions of $m_0$ and $m_1$ are indistinguishable.

**Efficiency tricks / Ring-LWE:**
- Encrypt vectors to amortize overhead.
- Use larger plaintext alphabets to reduce expansion.
- Use Ring-LWE to replace matrices with structured polynomials → faster and smaller keys.

### Extra Mini Questions (Practice)

1. **DH MITM:** Describe in 3–4 steps how a man-in-the-middle attacker can intercept a plain Diffie–Hellman key exchange and establish separate keys with Alice and Bob.
2. **DDH vs ElGamal:** Why would ElGamal’s security break if DDH became easy but CDH remained hard?
3. **LWE hardness:** Intuitively, why does adding small random noise $e$ to $As$ make solving for $s$ hard using linear algebra?
4. **Regev correctness:** For small $|r^Te|$, explain why $m=0$ and $m=1$ map to two disjoint intervals when you scale and round in decryption.

### True/False Practice – Week 8 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. In a standard Diffie–Hellman key exchange over a DDH-hard group, the resulting shared key is IND-CPA secure as a secret if it is only ever used as a symmetric key. (**True**)
2. Plain Diffie–Hellman key exchange without authentication is vulnerable to man-in-the-middle attacks, even if DLP, CDH, and DDH are all hard. (**True**)
3. ElGamal encryption is IND-CPA secure under the CDH assumption alone. (**False**)
4. ElGamal ciphertexts are longer than the plaintext because they include two group elements instead of one. (**True**)
5. ElGamal encryption is malleable: an attacker can modify a ciphertext to obtain a related plaintext without learning the original message. (**True**)
6. Because ElGamal is IND-CPA secure under DDH, it is also automatically IND-CCA secure. (**False**)
7. Regev’s LWE-based PKE has ciphertexts whose length does not depend on the message length, since messages are encoded into field elements. (**True**)
8. The IND-CPA security of Regev’s PKE is based on the decision-LWE assumption, not on CDH or DDH. (**True**)
9. Unconditional (information-theoretic) security is achievable for public-key encryption, just like the One-Time Pad for symmetric encryption. (**False**)
10. In Regev’s scheme, small noise is crucial to allow correct decryption while still hiding the linear relation between $A$, $s$, and $b=As+e$. (**True**)
11. If the LWE noise distribution were replaced by an all-zero vector, Regev’s encryption would become insecure. (**True**)
12. Both ElGamal and Regev’s schemes are malleable and therefore not IND-CCA secure without additional mechanisms like MACs or CCA transforms. (**True**)
13. LWE-based encryption can be made IND-CCA secure by wrapping Regev’s PKE inside a generic CCA transform such as Fujisaki–Okamoto. (**True**)
14. In the ElGamal scheme, the ciphertext length always equals the plaintext length. (**False**)
15. LWE remains hard even for quantum computers, which is why Regev’s PKE is considered post-quantum. (**True**)
16. The DDH assumption is known to hold in every cyclic group where DLP is hard. (**False**)
17. The Leftover Hash Lemma is used in the proof of Regev’s scheme to argue that certain inner products are statistically close to uniform. (**True**)
18. A man-in-the-middle attack on Diffie–Hellman can be prevented purely by switching from classical DH to LWE-based Regev encryption without any authentication. (**False**)
19. In both ElGamal and Regev encryption, encrypting the same plaintext twice with fresh randomness gives different ciphertexts with overwhelming probability. (**True**)
20. A KEM built from LWE (like Kyber) typically provides IND-CCA security when used with appropriate transformations, unlike basic Regev PKE. (**True**)
