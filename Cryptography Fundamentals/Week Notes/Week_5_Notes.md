---
title: "Week 5 Notes – MACs & Authenticated Encryption"
week: 5
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/5, mac, ae]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_4_Notes]]"
  next: "[[Week_6_Notes]]"
---
---
# Week 5 – MACs & Authenticated Encryption

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_4_Notes]] · Next: [[Week_6_Notes]]

---

## Message Authentication Codes (MACs)

A **MAC** provides **integrity** and **authenticity**.

Algorithms:

- $KeyGen(): \emptyset \to K$
- $Tag_k(m) = MAC(k,m) \to t$
- $Verify_k(m,t) \in \{\top,\bot\}$

Security goal:
> It should be infeasible to produce a valid tag on a *new* message.

MACs are the **symmetric-key analogue of digital signatures**.

---

### ✅ Practical MAC Use

**Question:**  
What security goals does a MAC provide?

**Solution:**  
Integrity and authenticity only — *not confidentiality*.

---

## Why CPA Security Is Not Enough

IND-CPA only protects **confidentiality**.

CTR and CBC are **malleable**:

An attacker can transform:

$$
c = Enc(k,m) \to c' = Enc(k,m')
$$

without knowing $k$.

Goal:
> Make **any modification detectable**.

Solution:
> Add a **MAC**.

---

### ✅ Malleability Example

**Question:**  
If an attacker flips a bit in CTR ciphertext, what happens?

**Solution:**  
The exact same bit flips in the plaintext.

---

## MAC Semantics

A MAC scheme consists of:

- Key space $K$
- Message space $M$
- Tag space $T$
- $KeyGen()$
- $MAC(k,m)$
- $Verify(k,m,t)$

Verification accepts if:

$$
Verify(k,m,t) = (MAC(k,m) = t)
$$

---

## MAC Security – EUF-CMA

**Existential Unforgeability under Chosen-Message Attack (EUF-CMA)**.

Game:

1. Challenger samples $k$.
2. Adversary queries $Tag(m_i)$.
3. Adversary outputs $(m^*,t^*)$.
4. Adversary wins if:
   - $Verify(k,m^*,t^*)=\top$
   - $m^*$ was never queried.

Secure if win probability is negligible.

---

### ✅ Practical Forgery Example

**Question:**  
If the attacker replays a known $(m,t)$ pair, is that a forgery?

**Solution:**  
No. A forgery must be on a *new* message.

---

## MAC Libraries (Real vs Fake)

### Real Library $L_{MAC-real}$

- $Tag(m) \to MAC(k,m)$
- $Check(m,t) \to (MAC(k,m)=t)$

### Fake Library $L_{MAC-fake}$

- Stores queried pairs in a set $S$
- $Tag(m)$ returns real tag and stores $(m,t)$
- $Check(m,t)$ only accepts if $(m,t)\in S$

Security:
$$
L_{MAC-real} \approx L_{MAC-fake}
$$

---

## What MACs Do and Do Not Do

MACs:
- ✅ Integrity
- ✅ Authenticity
- ✅ Shared-key verification

MACs do **NOT**:
- ❌ Provide confidentiality
- ❌ Provide non-repudiation
- ❌ Work with public verification

---

## MACs from PRFs

Let $F$ be a secure PRF:

$$
F: \{0,1\}^\lambda \times \{0,1\}^n \to \{0,1\}^n
$$

Construction:

- $KeyGen(): k \leftarrow F.KeyGen()$
- $MAC(k,m) = F(k,m)$

This is a **secure MAC**.

---

### ✅ PRF→MAC Question

**Question:**  
Why is unpredictability enough for MAC security?

**Solution:**  
Because forging a new valid tag requires predicting PRF output.

---

## MACs Are Not Necessarily PRFs

Example:

$$
MAC(k,m) = F(k,m) \parallel 00\ldots0
$$

This is:
- Still a valid MAC
- **Not pseudorandom**

MAC security does **not** require pseudorandom outputs.

---

## MACs for Longer Messages

We must authenticate variable-length messages.

---

## ❌ ECB-MAC (INSECURE)

Construction:
- Tag each block independently.

Attack:
- $(t_2,t_1)$ is valid tag for $(m_2,m_1)$.

**Never use ECB-MAC.**

---

### ✅ Practical ECB-MAC Attack

**Question:**  
Why does ECB-MAC fail for $(m_1,m_2)$?

**Solution:**  
Block order can be swapped without detection.

---

## ✅ CBC-MAC (Fixed-Length Only)

Let $F$ be a PRF.

Initialization:
$$
t_0 = 0^\lambda
$$

For $i=1\ldots \ell$:
$$
t_i = F(k, m_i \oplus t_{i-1})
$$

Tag:
$$
t = t_\ell
$$

Security:
- Secure **only for fixed message lengths**
- Breaks if $\ell$ is variable

---

### ✅ CBC-MAC Length-Extension Attack

**Question:**  
Why is CBC-MAC insecure for variable lengths?

**Solution:**  
An attacker can extend a valid tag into a longer valid tag using chaining.

---

## ECBC-MAC (Variable-Length Secure)

Uses two keys $k_1,k_2$:

- First $\ell-1$ blocks with $k_1$
- Final block with $k_2$

This fixes the variable-length problem.

---

## Encrypt-then-MAC (EtM)

Builds **IND-CCA secure encryption**.

Construction:

1. $c \leftarrow Enc_{k_e}(m)$
2. $t \leftarrow MAC_{k_m}(c)$
3. Output $(c,t)$

Decryption:

1. Verify tag
2. If valid → decrypt
3. Else → abort

---

### ✅ Practical EtM Check

**Question:**  
Why must MAC verification happen *before* decryption?

**Solution:**  
To prevent decryption oracle and padding oracle attacks.

---

## IND-CCA Security (Formal)

Challenger chooses $b$.

Encryption:
$$
c^* = Enc(k,m_b)
$$

Decryption oracle:
- Available on all ciphertexts except $c^*$

Adversary wins if:

$$
|P[b'=b] - 1/2|
$$

is non-negligible.

---

## Why Do We Allow Decryption Queries?

Because real systems:
- Servers decrypt incoming data
- Attackers can submit malformed ciphertexts
- Error messages leak information

Example:
- TLS padding oracle
- iMessage IND-CCA failure

Worst-case attacker → strongest model.

---

## Why Not MAC-then-Encrypt?

Construction:

1. $t = MAC(m)$
2. $c = Enc(m \parallel t)$

Problem:
- Padding errors leak MAC validity
- Decryption oracle emerges

Thus:
> **Encrypt-then-MAC is the only generically safe composition.**

---

## Final Exam Key Takeaways (Week 5)

You must be able to:

- Define EUF-CMA
- Explain why MACs are needed beyond CPA security
- Distinguish:
  - ECB-MAC ❌
  - CBC-MAC ✅ (fixed length only)
  - ECBC-MAC ✅✅
- Construct a MAC from a PRF
- Explain Encrypt-then-MAC
- Explain why EtM ⇒ IND-CCA
- Explain why MAC-then-Encrypt is dangerous

---

## Week 5 Cheat Sheet

**MAC basics:**
- Algorithms: $KeyGen, MAC(k,m), Verify(k,m,t)$.
- Goals: integrity + authenticity (no confidentiality / no non-repudiation).
- Security notion: EUF-CMA (existential unforgeability under chosen-message attack).

**EUF-CMA game:**
- Adversary gets tags on chosen messages.
- Wins if it outputs $(m^*, t^*)$ with $m^*$ new and $Verify(k,m^*,t^*)=\top$.

**Real vs Fake MAC libraries:**
- Real: computes $MAC(k,m)$ and checks via recomputation.
- Fake: accepts only previously output $(m,t)$ pairs.
- Security: $L_{MAC-real} \approx L_{MAC-fake}$.

**MAC constructions:**
- PRF-based MAC: $MAC(k,m) = F(k,m)$ (secure if $F$ is PRF and domain fixed/managed).
- Not all MACs are PRFs (e.g. append zeros to tag).

**Block-MAC schemes:**
- ECB-MAC: insecure (block reordering/swapping attacks).
- CBC-MAC: secure only for **fixed** message lengths.
- ECBC-MAC: variant with two keys, secure for variable lengths.

**Encrypt-then-MAC (EtM):**
- Compute $c = Enc_{k_e}(m)$, then $t = MAC_{k_m}(c)$.
- Verify tag **before** decryption.
- Gives IND-CCA security under usual assumptions.

**Danger of MAC-then-Encrypt:**
- $t = MAC(m)$, then encrypt $(m \parallel t)$.
- Padding / format errors may leak MAC validity → decryption oracle.

### Extra Mini Questions (Practice)

1. **EUF-CMA vs replay:** Why is replaying an old valid $(m,t)$ pair not considered a successful forgery in the EUF-CMA game?
2. **ECB-MAC attack:** Construct an explicit example where a valid tag for $(m_1,m_2)$ yields a valid tag for a different message of your choice.
3. **CBC-MAC length issue:** Briefly sketch how an attacker could use a valid tag on a short message to forge a valid tag on a longer message when lengths are not fixed.
4. **EtM reasoning:** Explain in 2–3 sentences why verifying the MAC **before** decryption blocks padding oracle attacks.

### True/False Practice – Week 5 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. A MAC provides confidentiality, integrity, and authenticity for messages under a shared secret key. (**False**)
2. The EUF-CMA security notion for MACs allows the adversary to obtain tags on chosen messages before attempting to forge a new valid pair. (**True**)
3. Replaying an already-seen valid $(m,t)$ pair is considered a successful forgery in the EUF-CMA game. (**False**)
4. In the real MAC library, the $Check$ oracle recomputes $MAC(k,m)$ and compares it to the provided tag $t$. (**True**)
5. In the fake MAC library, $Check(m,t)$ accepts if and only if $(m,t)$ was previously returned by $Tag(m)$. (**True**)
6. If a MAC is EUF-CMA secure, then $L_{MAC-real}$ and $L_{MAC-fake}$ are computationally indistinguishable to any PPT adversary. (**True**)
7. Any PRF can be turned into a secure MAC for fixed-length messages by defining $MAC(k,m)=F(k,m)$. (**True**)
8. Every secure MAC is automatically a secure PRF when used with its tag-generation algorithm. (**False**)
9. ECB-MAC, which tags each block independently, is secure for arbitrary-length messages. (**False**)
10. CBC-MAC with a single key is secure for variable-length messages as long as the underlying function is a secure PRF. (**False**)
11. CBC-MAC is provably secure as a MAC when all messages have the same fixed length. (**True**)
12. ECBC-MAC uses two keys to fix the variable-length insecurity of CBC-MAC. (**True**)
13. Encrypt-then-MAC (EtM) means you first compute a MAC on the plaintext and then encrypt the pair $(m,t)$. (**False**)
14. In Encrypt-then-MAC, it is crucial to verify the MAC on the ciphertext **before** decrypting, to avoid oracle-based attacks. (**True**)
15. A scheme that is IND-CPA secure and uses EtM with an EUF-CMA secure MAC can achieve IND-CCA security. (**True**)
16. MAC-then-Encrypt can leak MAC validity information via padding or format errors, making it dangerous in general. (**True**)
17. A MAC alone (without encryption) is susceptible to chosen-ciphertext attacks on confidentiality. (**False**)
18. AEAD schemes can often be seen as a careful integration of encryption and MAC-like integrity checks into a single algorithm. (**True**)
19. A MAC key can safely be reused indefinitely across arbitrarily many messages, as long as the MAC scheme is EUF-CMA secure. (**True**)
20. If a MAC is not collision resistant as a hash function, then it cannot be EUF-CMA secure. (**False**)

#### Extra True/False – Week 5 Interactions

21. A scheme that is IND-CCA secure for encryption already guarantees integrity of ciphertexts, making a separate MAC unnecessary. (**True**)
22. In Encrypt-then-MAC, using the same key for both encryption and MAC is always safe as long as both primitives are individually secure. (**False**)
23. An AEAD construction can be viewed as a specific way of doing Encrypt-then-MAC internally, even if the interface exposes only a single combined API. (**True**)
24. CBC-MAC and CBC encryption, although both using chains of blocks, have different security goals: one for integrity/authentication, the other for confidentiality. (**True**)
25. If a MAC is EUF-CMA secure, an attacker who does not know the key should not be able to modify a ciphertext-tag pair without being detected, assuming the encryption itself is at least IND-CPA secure. (**True**)

##### Extra True/False – Week 5 Advanced

26. If you use Encrypt-then-MAC with an IND-CPA secure encryption scheme and an EUF-CMA secure MAC, you can safely reuse the same MAC key across different encryption keys without losing MAC security. (**True**)
27. A MAC that is secure for fixed-length messages under EUF-CMA might become insecure if used directly on variable-length messages without additional domain separation or length-encoding. (**True**)
28. AEAD schemes are typically designed so that failing verification does not leak whether a ciphertext was “almost” valid, to avoid creating side-channel style decryption oracles. (**True**)
29. Using MAC-then-Encrypt is always safe as long as the underlying encryption scheme is IND-CCA secure. (**False**)
30. In a hybrid encryption system (PKE + AEAD), the MAC or AEAD integrity property is what prevents an attacker from turning the PKE decryption interface into a powerful chosen-ciphertext oracle. (**True**)
