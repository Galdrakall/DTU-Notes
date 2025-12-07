---
title: "Week 3 Notes – Pseudorandomness & PRFs"
week: 3
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/3, pseudorandomness]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_2_Notes]]"
  next: "[[Week_4_Notes]]"
---
---

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_2_Notes]] · Next: [[Week_4_Notes]]

---

## Negligible Functions

A function $\mu : \mathbb{N} \to \mathbb{R}$ is **negligible** if:

> For every polynomial $p(n)$, there exists $N$ such that for all $n > N$:
> $$
> |\mu(n)| < \frac{1}{p(n)}.
> $$
- $2^{-n}$, $2^{-\sqrt{n}}$ are negligible.
- $1/n$, $1/n^{10}$, $1/\log n$ are **not** negligible.
- $2^{-n}$, $2^{-\sqrt{n}}$ are negligible.
- $1/n$, $1/\log n$ are **not** negligible.

Negligible functions formalize “so small it can be ignored for large $n$”.

---

### ✅ Practical Negligibility Check
**Question:** Is $f(n)=1/n^{10}$ negligible?  
**Solution:** No. It goes to zero, but not faster than the inverse of every polynomial, so it is **not** negligible under the standard definition used here.

---

## Computational Security

We restrict attackers to **probabilistic polynomial-time (PPT)** algorithms.

A scheme is **computationally secure** if:
- Every PPT adversary has only **negligible advantage**.

Typical structure:
1. Define a game.
2. Measure adversary advantage.
3. Show it is negligible.

---

## The OTP Key Expansion Problem

In the One-Time Pad:
- $K = M = C$
- Perfect secrecy requires a key as long as the message.

If we had a function:

$$
F: \{0,1\}^\lambda \to \{0,1\}^n
$$

that expanded short keys into long **random-looking** strings, we could:
- Use $F(k,1)$ to encrypt message 1
- Use $F(k,2)$ to encrypt message 2
- Avoid key reuse

This motivates **pseudorandom functions (PRFs)**.

---

## Pseudorandom Functions (PRFs)

A deterministic function:

$$
F: \{0,1\}^\lambda \times \{0,1\}^n \to \{0,1\}^n
$$

is a **PRF** if no PPT adversary can distinguish it from a truly random function.

### PRF Security Game

Hidden bit $b$:
- If $b=0$: oracle returns $F_k(\cdot)$
- If $b=1$: oracle returns random function $R(\cdot)$

Adversary outputs guess $b'$.

Advantage:

$$
\text{Adv}_{\text{PRF}}(\mathcal{A}) =
\left| P[b'=b] - \frac12 \right|.
$$

$F$ is a secure PRF if this is negligible.

---

### Crucial Notes on PRFs

1. Security only holds if **$k$ is secret**.
2. Encryption defined as:
   $$
   c = F(k,m)
   $$
   is **not secure**, because:
   - Deterministic
   - Leaks equality of plaintexts
   - Breaks IND-CPA

**Toy insecure "PRF":** $G_k(x) = x \oplus k$ is easy to distinguish from random: query $x=0^n$ to obtain $k$, then you can predict $G_k(x)$ for any $x$.

---

### ✅ Practical PRF Misuse Example

**Question:** If $Enc(k,m)=F(k,m)$ and $Enc(k,m_1)=Enc(k,m_2)$, what leaks?  
**Solution:** $m_1 = m_2$. Equality leakage violates IND-CPA.

---

## From PRFs to PRPs

Problems of PRFs:
1. Not injective
2. Not invertible

We therefore need **Pseudorandom Permutations (PRPs)**.

---

## Pseudorandom Permutations (PRPs)

A function:

$$
F: \{0,1\}^\lambda \times \{0,1\}^n \to \{0,1\}^n
$$

is a **PRP** if:

1. For each $k$, $F_k$ is a permutation.
2. There exists efficient inverse $F_k^{-1}$.
3. It is indistinguishable from a random permutation.

Block ciphers (AES, DES) are modeled as PRPs.

---

## Building SKE from a PRP

Let $F$ be a PRP.

- $Keygen(): k \leftarrow \{0,1\}^\lambda$
- $Enc(k,m) = F(k,m)$
- $Dec(k,c) = F^{-1}(k,c)$

This forms a valid symmetric encryption scheme.

---

### ✅ Practical PRP Question

**Question:** Why must a block cipher be invertible?  
**Solution:** Otherwise decryption is impossible.

---

## Birthday Bound & Collision Probability

If we sample $q$ values from a set of size $N$, the probability of **at least one collision** is:

$$
\text{BirthdayProb}(q,N) = 1 - \prod_{i=0}^{q-1} \left(1 - \frac{i}{N} \right)
$$

Upper bound:

$$
\text{BirthdayProb}(q,N) \le \frac{q(q-1)}{2N}
$$

This explains why:
- After about $q \approx \sqrt{N}$ queries, collisions become likely.

---

### ✅ Practical Birthday Attack Example

**Question:** For a 64-bit block cipher ($N=2^{64}$), when do collisions appear?  
**Solution:** Around $2^{32}$ queries.

---

## PRF–PRP Switching Lemma

**Lemma:**  
If $F$ is a secure PRF with domain and range size $2^n$, then:

$$
\text{PRF}_F \approx \text{PRP}_P
$$

for a uniformly random permutation $P$, as long as:
- The adversary makes **polynomially many queries**
- Collisions are negligible

Thus PRFs and PRPs are indistinguishable in practice under standard limits.

---

## The Feistel Construction

Turns a PRF into a PRP.

Given PRF:

$$
F: \{0,1\}^\lambda \times \{0,1\}^{n/2} \to \{0,1\}^{n/2}
$$

Define one Feistel round:

$$
F^*(k,x,y) = (y,\; x \oplus F(k,y))
$$

Properties:
- Always invertible
- Does not require $F$ to be invertible

---

## Iterated Feistel & Luby–Rackoff Theorem

Apply multiple Feistel rounds with independent keys:

$$
(x_1,y_1) \to (x_2,y_2) \to \cdots
$$

**Theorem (Luby–Rackoff):**
> 3 rounds of Feistel with independent PRF keys yields a secure PRP.

---

### ✅ Practical Feistel Question

**Question:** Why are multiple rounds needed?  
**Solution:** One round is invertible but not pseudorandom. Multiple rounds mix input sufficiently.

---

## DES and AES Context

- **DES** (1975):
  - 56-bit key
  - 64-bit block
  - 16 Feistel rounds
  - Publicly insecure since 1990s
- **Triple-DES**: transitional fix
- **AES (2001)**:
  - Key sizes: 128, 192, 256 bits
  - Not Feistel-based
  - Secure by modern standards

---

## Final Exam Key Takeaways (Week 3)

You must be able to:

- Define negligible functions
- Explain PRF security games
- Explain why PRF ≠ secure encryption
- Define PRP and distinguish from PRF
- State and apply Birthday Bound
- State PRF–PRP Switching Lemma
- Explain Feistel construction
- Explain why multiple Feistel rounds are needed
- Identify why DES is insecure and AES is secure

---

## Week 3 Cheat Sheet

**Negligible functions:**
- Negligible $\mu(n)$: smaller than $1/p(n)$ for every polynomial $p$.
- Examples: $2^{-n}$, $2^{-\sqrt n}$; non-examples: $1/n$, $1/n^{10}$, $1/\log n$.

**PRFs:**
- $F: \{0,1\}^\lambda \times \{0,1\}^n \to \{0,1\}^n$.
- Indistinguishable from a random function for any PPT distinguisher.
- Must keep key secret; $Enc(k,m)=F(k,m)$ is insecure (deterministic, equality leakage, no inverse).

**PRPs / block ciphers:**
- PRP: family of permutations with efficient inverse and PRF-like security.
- Block cipher (e.g. AES) is modeled as a PRP.

**Birthday bound:**
- Collision prob with $q$ samples from $N$-sized set: $\le q(q-1)/(2N)$.
- Rough rule: collisions become likely around $q \approx \sqrt N$.

**PRF–PRP switching lemma:**
- A secure PRF over domain/range $\{0,1\}^n$ is indistinguishable from a random permutation, as long as the adversary makes polynomially many queries (so collisions are unlikely).

**Feistel construction:**
- Round: $(L,R) \mapsto (R, L \oplus F(k,R))$.
- Always invertible even if $F$ is not.
- Luby–Rackoff: 3 Feistel rounds with PRF round functions yield a secure PRP.

### Extra Mini Questions (Practice)

1. **PRF vs PRP:** Give one example of a property that a PRP has but a PRF does not need to have. Why is this property crucial for encryption?
2. **Birthday attack:** For a 128-bit block cipher, estimate how many blocks can be safely encrypted under a single key before birthday-bound attacks become a concern. Explain your reasoning.
3. **Feistel reasoning:** Why does a Feistel round stay invertible even when $F$ itself is not? Write the inverse transformation explicitly for one round.

### True/False Practice – Week 3 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. A negligible function must eventually be smaller than $1/p(n)$ for **every** polynomial $p(n)$. (**True**)
2. The function $\mu(n)=1/n^{10}$ is negligible because it goes to zero as $n$ grows. (**False**)
3. Computational security allows an adversary with unlimited running time, as long as its success probability is negligible. (**False**)
4. A scheme is computationally secure if every PPT adversary has only negligible advantage in the corresponding security game. (**True**)
5. PRFs are used to conceptually solve the One-Time Pad key expansion problem by generating long pseudorandom strings from short keys. (**True**)
6. In the PRF security game, the adversary can adaptively choose inputs to the oracle before guessing the hidden bit. (**True**)
7. If a distinguisher can predict $F_k(x)$ for all $x$ after seeing $F_k(0^n)$ once, then $F$ is still a secure PRF as long as $k$ is secret. (**False**)
8. The construction $Enc(k,m)=F(k,m)$ is IND-CPA secure as long as $F$ is a secure PRF. (**False**)
9. A PRP is a family of permutations with efficient inverse, and is usually modeled as a block cipher. (**True**)
10. Every PRP is also a PRF when we ignore its invertibility and only consider its forward direction. (**True**)
11. The birthday bound says that with about $2^{n/2}$ random samples from an $n$-bit space, we expect collisions with non-negligible probability. (**True**)
12. For a 64-bit block cipher, collisions become likely only after about $2^{64}$ blocks are encrypted under the same key. (**False**)
13. The PRF–PRP switching lemma informally states that a secure PRF over $n$-bit strings is indistinguishable from a random permutation as long as the number of queries is much smaller than $2^{n/2}$. (**True**)
14. A single-round Feistel network with a secure PRF as round function is already indistinguishable from a random permutation. (**False**)
15. In a Feistel round $(L,R)\mapsto (R,L\oplus F(k,R))$, knowing $F$ is invertible is essential for the overall permutation to be invertible. (**False**)
16. DES is considered insecure today primarily because its block size is too small, while its key size is still adequate. (**False**)
17. Triple-DES was introduced as a transitional solution to increase effective key size while reusing the DES block cipher structure. (**True**)
18. AES is not Feistel-based but is still modeled as a PRP on 128-bit blocks. (**True**)
19. A distinguisher that succeeds with probability $0.51$ in the PRF game (over random guessing) has non-negligible advantage. (**True**)
20. If $F$ is a secure PRF, then using three independent keys in a 3-round Feistel network yields a construction that is *provably* a secure PRP (Luby–Rackoff). (**True**)

#### Extra True/False – Week 3 Interactions

21. If $F$ is a secure PRP, then it is also a secure PRF when the adversary never queries the inverse. (**True**)
22. The PRF–PRP switching lemma relies on the birthday bound to argue that collisions in the PRF outputs remain unlikely under polynomially many queries. (**True**)
23. From the standpoint of IND-CPA security for block-cipher modes, it usually does not matter whether the underlying primitive is modeled as a PRF or a PRP. (**True**)
24. A Feistel construction can turn any arbitrary function into a PRP, regardless of whether that function is a PRF. (**False**)
25. For a block cipher with $n$-bit blocks, encrypting significantly more than $2^{n/2}$ blocks under one key may start to leak information via birthday-type effects. (**True**)

