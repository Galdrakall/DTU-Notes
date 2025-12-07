---
title: "Week 9 Notes – Digital Signatures"
week: 9
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/9, signatures]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_8_Notes]]"
  next: "[[Week_10_Notes]]"
---
---

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_8_Notes]] · Next: [[Week_10_Notes]]

---

## What Is a Digital Signature?

A **digital signature scheme (DSS)** consists of:

- $\textbf{KeyGen}() \to (vk, sk)$  
- $\textbf{Sign}(sk, m) \to \sigma$  
- $\textbf{Ver}(vk, m, \sigma) \in \{0,1\}$  

Security goals:

- **Authenticity** – Only the holder of $sk$ can produce valid signatures.
- **Integrity** – Any change to $m$ invalidates $\sigma$.
- **Non-repudiation** – Signer cannot deny producing $\sigma$ (given identity-bound $vk$).

Digital signatures are the **public-key analogue of MACs**.

---

### ✅ Practical Signature Meaning

**Question:**  
What is the key difference between a MAC and a signature?

**Solution:**  
A MAC requires a **shared secret key**, while a signature can be verified using a **public key**.

---

## Use Cases of Digital Signatures

- Software updates
- TLS certificates
- Cryptocurrency transactions
- Electronic contracts
- Authenticated communication channels

---

## Replay Attacks vs Forgeries

### Replay Attack
Attacker records $(m,\sigma)$ and resends it later.

- This is **not a cryptographic break**
- It is a **protocol-level freshness problem**

Mitigations:
- Nonces
- Timestamps
- Sequence numbers
- Context binding: sign $(m \parallel \text{nonce})$

### Forgery
Attacker produces a **new valid** $(m^*,\sigma^*)$ without knowing $sk$.

This is the **real cryptographic threat**.

---

### ✅ Practical Replay Question

**Question:**  
Is replaying a previously signed banking transaction a forgery?

**Solution:**  
No. It is a replay attack. The signature is valid, but the protocol must prevent reuse.

---

## Formal DSS Security – EUF-CMA

**Existential Unforgeability under Chosen-Message Attack (EUF-CMA):**

1. Challenger runs $\text{KeyGen}$ and gives $vk$ to the adversary.
2. Adversary queries a **signing oracle** on messages $m_1,\dots,m_q$.
3. Adversary outputs $(m^*,\sigma^*)$.
4. Adversary wins if:
   - $\text{Ver}(vk,m^*,\sigma^*)=1$, and
   - $m^*$ was **never queried** before.

Security condition:
$$
\Pr[\text{forge}] \text{ is negligible}.
$$

---

## Strong Unforgeability (SUF-CMA)

Stronger notion:

- Even producing a **new signature on an old message** counts as forgery.

Used in:
- TLS
- Blockchain protocols

---

## DSS Real vs Fake Libraries

### Real Library $L_{\text{sig-real}}$

- $GetSig(m)$ returns $\text{Sign}(sk,m)$.
- $VerSig(m,\sigma)$ runs $\text{Ver}(vk,m,\sigma)$.

### Fake Library $L_{\text{sig-fake}}$

- Stores all issued pairs $(m,\sigma)$.
- $VerSig(m,\sigma)$ returns **1 only if $(m,\sigma)$ was previously issued**.

Security:
$$
L_{\text{sig-real}} \approx L_{\text{sig-fake}}
$$

This guarantees adversaries **cannot create new valid pairs**.

---

## Non-Repudiation

If:
- $vk$ is tied to a real-world identity,
- DSS is EUF-CMA secure,

then:
> Only the owner of $sk$ could have generated $(m,\sigma)$.

Thus **non-repudiation follows from unforgeability**.

---

## Textbook RSA Signatures (INSECURE)

Let:
- Public key: $(N,e)$
- Secret key: $d$

Signature:
$$
\sigma = m^d \bmod N
$$

Verification:
$$
m \equiv \sigma^e \bmod N
$$

---

## Why Textbook RSA Signatures Fail

### ❌ Attack 1 – Random Signature Forgery

1. Pick random $\sigma \in \mathbb{Z}_N^*$  
2. Compute $m = \sigma^e \bmod N$  
3. Output $(m,\sigma)$

This is a **valid signature without $sk$**.

---

### ❌ Attack 2 – Multiplicative Forgery

Given:
$$
\sigma_1 = m_1^d,\quad \sigma_2 = m_2^d
$$

Then:
$$
\sigma = \sigma_1\cdot\sigma_2 = (m_1m_2)^d
$$

Attacker forges a signature on $m=m_1m_2$.

---

### ✅ Practical RSA Forgery Question

**Question:**  
Why is algebraic structure dangerous in signatures?

**Solution:**  
Because it lets attackers combine known signatures to forge new ones.

---

## RSA-FDH (Full Domain Hash) Signatures

Let:
- $H:\{0,1\}^* \to \mathbb{Z}_N^*$ be a cryptographic hash
- Modeled as a **random oracle**

### Signing
$$
h = H(m), \quad \sigma = h^d \bmod N
$$

### Verification
$$
\text{Accept if } \sigma^e \equiv H(m)\pmod N
$$

Here:
- Signatures are over **hash outputs**, not raw messages
- Message space is uniformly spread over $\mathbb{Z}_N^*$

---

### ✅ Practical FDH Benefit

**Question:**  
Why does hashing before signing stop algebraic forgeries?

**Solution:**  
Because attackers can no longer exploit multiplicative structure of raw messages.

---

## Random Oracle Model (ROM)

In proofs of RSA-FDH:

- $H$ is treated as a **truly random function**
- Everyone gets black-box access
- Proofs become simpler and stronger (but idealized)

Used in security proofs of:
- RSA-FDH
- RSA-PSS
- OAEP

---

## Trapdoor One-Way Function View

RSA function:
$$
f(x)=x^e \bmod N
$$

Properties:
- Easy to compute
- Hard to invert without $d$
- Easy to invert with $d$

Thus RSA is a **trapdoor one-way permutation**, enabling signatures.

---

## RSA-FDH Security Proof – Intuition

Key steps:

1. Replace $H$ with a truly random oracle.
2. Track all $(m,H(m))$ queries.
3. Eliminate events where $H(m)$ collides.
4. If a forgery occurs:
   - Adversary has **inverted RSA on a random value**
   - This breaks the trapdoor OWF assumption.

Thus:
$$
\text{RSA-FDH is EUF-CMA secure assuming RSA is hard in the ROM.}
$$

---

### ✅ Practical Proof Intuition Question

**Question:**  
What hard problem would a successful FDH forger solve?

**Solution:**  
They would invert RSA without knowing the private key.

---

## One-Time Signatures (High Level)

Example: **Lamport signatures**

- Based only on hash functions
- Post-quantum secure
- Large keys and signatures
- Secure for **one message only**

Used as building blocks for:
- Merkle signature trees
- Some post-quantum signature schemes

---

## Final Exam Key Takeaways (Week 9)

You must be able to:

- Define a digital signature scheme formally
- Explain EUF-CMA and SUF-CMA
- Distinguish **replay attacks vs cryptographic forgeries**
- Explain why textbook RSA signatures are insecure
- Describe both RSA forgery attacks
- Write RSA-FDH Sign & Verify equations
- Explain the role of the Random Oracle Model
- Explain why RSA-FDH is secure at a high level
- Explain how unforgeability implies non-repudiation

---

## Week 9 Cheat Sheet

**Digital signatures:**
- Algorithms: $KeyGen \to (vk,sk)$, $Sign(sk,m)\to\sigma$, $Ver(vk,m,\sigma)\in\{0,1\}$.
- Goals: authenticity, integrity, non-repudiation.
- Public-key analogue of MACs (no shared secret needed for verification).

**Security notions:**
- EUF-CMA: no adversary with signing oracle can produce a valid signature on a **new message**.
- SUF-CMA: even producing a new signature on an old message counts as a forgery.

**Real vs fake signature libraries:**
- Real: returns true for any valid $(m,\sigma)$ under $vk$.
- Fake: returns true **only** for signatures it previously issued.
- Security: $L_{sig-real} \approx L_{sig-fake}$.

**Replay vs forgery:**
- Replay: resending $(m,\sigma)$ — protocol-level issue (fix with nonces/timestamps).
- Forgery: creating new valid $(m^*,\sigma^*)$ without oracle query on $m^*$.

**Textbook RSA signatures (insecure):**
- $\sigma = m^d \bmod N$, verify by $\sigma^e \equiv m$.
- Attack 1: pick random $\sigma$, set $m=\sigma^e \bmod N$ (trivial forgery).
- Attack 2: multiplicative combination of known signatures yields new signed message.

**RSA-FDH signatures:**
- Use hash $H:\{0,1\}^*\to\mathbb Z_N^*$ modeled as random oracle.
- Sign: $\sigma = H(m)^d \bmod N$; verify: $\sigma^e \stackrel{?}{=} H(m)$.
- Removes algebraic structure on messages; security reduces to inverting RSA on random values.

**Random Oracle Model (ROM):**
- Treat $H$ as a truly random function in proofs.
- Lets us link any successful forger to breaking the underlying trapdoor permutation.

**One-time / hash-based signatures:**
- Lamport OTS: hash-based, one-use, post-quantum secure; large keys and signatures.
- Building block for Merkle-tree and PQ signature schemes.

### Extra Mini Questions (Practice)

1. **EUF-CMA vs SUF-CMA:** Give a concrete example where a scheme is EUF-CMA secure but not SUF-CMA secure.
2. **Textbook RSA forgery:** Explicitly write the steps of the “random $\sigma$” forgery attack and explain why it always succeeds.
3. **FDH intuition:** In your own words, explain how hashing before signing helps to “randomize” the messages modulo $N$.
4. **Non-repudiation:** Under what conditions (cryptographic + external) does EUF-CMA security imply that a signer cannot plausibly deny a valid signature?

### True/False Practice – Week 9 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. A digital signature scheme provides authenticity, integrity, and non-repudiation under the assumption that the verification key is correctly bound to the signer’s identity. (**True**)
2. In EUF-CMA security for signatures, replaying a previously seen valid pair $(m,\sigma)$ counts as a successful forgery. (**False**)
3. Strong unforgeability (SUF-CMA) forbids an attacker from producing a new valid signature on any message, including ones they have already asked the signing oracle to sign. (**True**)
4. Textbook RSA signatures are EUF-CMA secure as long as RSA is hard to invert. (**False**)
5. The “random $\sigma$” attack on textbook RSA shows that any $(m,\sigma)$ pair with $m\equiv \sigma^e \pmod N$ is accepted, even if the signer never saw $m$. (**True**)
6. Multiplicative structure of textbook RSA signatures allows combining signatures on $m_1$ and $m_2$ to forge a signature on $m_1m_2$. (**True**)
7. RSA-FDH prevents multiplicative forgeries by hashing the message into $\mathbb{Z}_N^*$ before signing. (**True**)
8. In RSA-FDH, the security proof models $H$ as a random oracle and reduces any EUF-CMA forgery to inverting RSA on a random target. (**True**)
9. Digital signatures are the public-key analogue of MACs, in that anyone with the verification key can generate valid signatures. (**False**)
10. Replay attacks against signed messages are generally prevented by including nonces, timestamps, or sequence numbers inside the signed data. (**True**)
11. A scheme that is SUF-CMA secure is automatically EUF-CMA secure. (**True**)
12. Hash-based one-time signatures like Lamport OTS can be quantum-secure, but they can safely sign an unlimited number of messages. (**False**)
13. The RSA function $f(x)=x^e \bmod N$ is a trapdoor one-way permutation when RSA is hard: easy to compute, hard to invert without $d$. (**True**)
14. If RSA is easy to invert, then RSA-FDH signatures are still secure in the random oracle model because the hash hides structure. (**False**)
15. In the real vs fake DSS libraries, the fake library $VerSig$ oracle always returns 0 on any pair $(m,\sigma)$ that has not previously been output by $GetSig$. (**True**)
16. EUF-CMA secure signatures, together with a trusted binding between $vk$ and a real-world identity, give a strong cryptographic basis for non-repudiation. (**True**)
17. A scheme may be EUF-CMA secure but not SUF-CMA secure if it allows the adversary to generate new signatures on previously signed messages. (**True**)
18. Applying FDH-style hashing to textbook RSA automatically provides post-quantum security for the signature scheme. (**False**)
19. In practice, standardized RSA signatures (like RSA-PSS) are designed to be secure under EUF-CMA in the ROM or similar models. (**True**)
20. Using deterministic signing without any hashing (signing raw messages directly) is generally recommended to improve performance and security. (**False**)
