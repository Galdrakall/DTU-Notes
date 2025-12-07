---
title: "Week 1 Notes – Introduction & Security Mindset"
week: 1
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/1, intro]
links:
  index: "[[Cryptography Fundamentals]]"
  next: "[[Week_2_Notes]]"
---
---

> Linked index: [[Cryptography Fundamentals]]  
> Next week: [[Week_2_Notes]]

---

## What is Cryptography?

Cryptography is the study of techniques for **secure communication** in the presence of adversaries.  
It provides mathematical tools to protect information and ensure that systems behave securely even under attack.

### Core Security Goals (CIA +)

- **Confidentiality** – Only authorized parties can read the data.
- **Integrity** – Data cannot be modified without detection.
- **Authenticity** – The identity of communicating parties can be verified.
- **Non-repudiation** – A sender cannot deny having sent a message.

Cryptography provides **primitives** such as:
- Symmetric encryption
- Public-key encryption
- Hash functions
- Message authentication codes (MACs)
- Digital signatures

These are combined into **protocols** such as TLS, secure messaging, encrypted storage, and cryptocurrency systems.

---

### ✅ Practical Example – Security Goals

**Question:**  
A student submits an assignment online. What cryptographic goals are required?

**Solution:**
- Confidentiality: hide content from others.
- Integrity: prevent modification.
- Authenticity: verify the student.
- Non-repudiation: student cannot deny submission.

---

## Threat Models and Adversaries

A **threat model** formally specifies:
- What the attacker can do.
- What the attacker wants to achieve.
- What assumptions are made about honest parties.

### Typical Adversary Types

- **Passive attacker:** Only listens.
- **Active attacker:** Intercepts and modifies.
- **Chosen-plaintext attacker (CPA):** Requests encryptions.
- **Chosen-ciphertext attacker (CCA):** Requests decryptions (with limits).

Security claims are meaningless unless the **threat model is explicitly stated**.

---

### ✅ Practical Example – Identifying Attacker Type

**Question:**  
An attacker can modify encrypted network traffic but cannot decrypt it. What type?

**Solution:**  
This is an **active attacker**, not passive, because modification is allowed.

---

## Kerckhoffs’ Principle

> A cryptosystem must remain secure even if everything about the system, except the key, is public.

Implications:
- Attackers know the algorithm.
- Security relies **only on the secrecy of the key**.
- “Security by obscurity” is fundamentally flawed.

---

### ✅ Practical Example – Security by Obscurity

**Question:**  
A company hides their encryption algorithm instead of using a standard one. Is this secure?

**Solution:**  
No. Once the algorithm leaks, the entire system breaks. Security must rely only on secret keys.

---

## Perfect Security vs Computational Security

### Perfect Security

A scheme has **perfect secrecy** if the ciphertext gives **no information** about the plaintext.

$$
P[C = c \mid M = m_0] = P[C = c \mid M = m_1]
$$

Achieved only by the **One-Time Pad (OTP)** if:
- Key is random
- Key length ≥ message
- Key is never reused

**Intuition:** Seeing the ciphertext does **not change** the attacker's belief about which plaintext was sent. Formally, for any prior distribution on $M$, we have

$$
P[M=m \mid C=c] = P[M=m]
$$

so observing $C=c$ gives zero advantage.

---

### ✅ Practical OTP Example

**Question:**  
Message $m = 101101$, key $k = 011011$. Compute the ciphertext.

**Solution:**

$$
c = m \oplus k = 101101 \oplus 011011 = 110110
$$

---

### Computational Security

A scheme is secure if no **efficient attacker** can break it with more than negligible probability.

Security relies on hardness assumptions like:
- Factoring
- Discrete logarithm

**Intuition:** If an attacker had unlimited time, they could always brute force. We instead restrict to **probabilistic polynomial-time (PPT)** adversaries and only require their success probability to be **negligibly** better than random guessing.

---

## Game-Based Security Definitions

Security is defined using games.

Adversary advantage:

$$
\text{Adv}(\mathcal{A}) = |P[\text{Real}=1] - P[\text{Random}=1]|
$$

A scheme is secure if this value is negligible.

**Example:** If an attacker wins the game with probability $P[\text{Real}=1]=0.75$ and $P[\text{Random}=1]=0.5$, then

$$
	ext{Adv}(\mathcal{A}) = |0.75 - 0.5| = 0.25
$$

which is **not** negligible. To be secure we want something like $0.5 + 2^{-\lambda}$, where $2^{-\lambda}$ is negligible in the security parameter $\lambda$.

---

### ✅ Practical Game Interpretation

**Question:**  
If an attacker wins with probability 0.51, is the scheme secure?

**Solution:**  
Yes, because advantage = $0.51 - 0.5 = 0.01$, which is negligible for large security parameters.

---

## Security Parameter & Negligible Functions

Security parameter $n$ controls:
- Key sizes
- Difficulty of attacks

A function is negligible if:

$$
\mu(n) < \frac{1}{n^c}
$$

for every $c > 0$.

**Standard equivalent definition:** $\mu$ is negligible if for every polynomial $p(\cdot)$,

$$
\lim_{n\to\infty} p(n)\mu(n)=0.
$$

Examples:
- $2^{-n}$, $2^{-\sqrt{n}}$ are negligible.
- $1/n$, $1/n^{10}$, $1/\log n$ are **not** negligible.

**Exam tip:** Distinguish “very small” from “negligible”. A probability like $1/2^{128}$ is both extremely small **and** negligible, while $1/n^{10}$ is very small for large $n$ but still **not negligible** in the formal sense above.

---

# ✅ ADDED LECTURE MATERIAL + PRACTICE

---

## Professional Paranoia & Formal Security Thinking

Assume:
- Attacker knows everything except keys.
- Attacker adapts.
- Attacker uses best known techniques.
- Worst-case behavior only.

Formal security always requires:
1. Exact algorithms
2. Exact attacker power
3. Exact success condition

---

## Historical Ciphers & Failures

### Caesar Cipher

$$
E_k(x_i)=x_{(i+k) \bmod \ell}, \quad D_k(x_i)=x_{(i-k) \bmod \ell}
$$

Weak key space ≈ 26.

---

### ✅ Practical Caesar Example

**Question:**  
Encrypt HELLO with shift $k = 3$.

**Solution:**  
H→K, E→H, L→O, L→O, O→R  
Ciphertext = **KHOOR**

---

### Vigenère Cipher

Repeating key encryption:

$$
E(m_i)=m_i+k_{i \bmod r}
$$

---

### ✅ Practical Vigenère Example

**Question:**  
Encrypt ATTACK with key KEY.

**Solution:**  
KEYKEY  
ATTACK  
→ Encrypted via modular shifts (shown stepwise in exam).

---

### One-Time Pad (OTP)

Perfect secrecy only if key never reused.

If reused:

$$
c_1 \oplus c_2 = m_1 \oplus m_2
$$

---

### ✅ Practical OTP Reuse Attack

**Question:**  
If $c_1 \oplus c_2 = 010101$, what does the attacker learn?

**Solution:**  
The XOR of the two messages — strong structural leakage enabling message recovery.

---

## Formal Definition of Symmetric-Key Encryption (SKE)

- $Keygen(): \emptyset → K$
- $Enc: K × M → C$
- $Dec: K × C → M$

Correctness:

$$
Dec(k, Enc(k,m)) = m
$$

---

### ✅ Practical Correctness Check

**Question:**  
If decryption sometimes outputs the wrong message, is it a valid encryption scheme?

**Solution:**  
No. Correctness is mandatory.

---

## IND-CPA Security

Attacker:
1. Queries encryption oracle.
2. Sends $(m_0, m_1)$.
3. Gets encryption of $m_b$.
4. Guesses $b$.

Secure if advantage negligible.

---

### ✅ Practical IND-CPA Example

**Question:**  
If encrypting the same message twice gives the same ciphertext, is IND-CPA satisfied?

**Solution:**  
No. Deterministic encryption leaks equality information.

---

## IND-CCA Security

Stronger than IND-CPA.
Adds decryption oracle (except challenge ciphertext).

---

### ✅ Practical IND-CCA Example

**Question:**  
If an error message reveals whether padding is correct, what security breaks?

**Solution:**  
IND-CCA via padding oracle attack.

---

## Final Exam Key Takeaways

- Always define:
  - Algorithms
  - Attacker model
  - Advantage
- Be able to:
  - Compute OTP encryptions
  - Identify attacker types
  - Distinguish IND-CPA vs IND-CCA
  - Explain key reuse failures

---

## Week 1 Cheat Sheet

**Core goals (CIA+):**
- Confidentiality: hide content.
- Integrity: detect modification.
- Authenticity: verify identity.
- Non-repudiation: prevent sender denial.

**Attacker types:**
- Passive: only listens.
- Active: inject/modify.
- CPA: can obtain encryptions of chosen messages.
- CCA: can obtain decryptions (except challenge).

**Security notions:**
- Perfect secrecy: ciphertext gives no info about plaintext.
- Computational security: no PPT adversary has non-negligible advantage.
- Negligible: smaller than $1/p(n)$ for every polynomial $p$.

### Extra Mini Questions (Practice)

1. **Classify attacker:** A rogue Wi-Fi AP can both read and modify packets. What type of attacker is this, and which security goals are at risk?
2. **Perfect vs computational:** Suppose an attacker can break a scheme with probability $2^{-40}$ using $2^{40}$ operations. Is this failure probability negligible in the formal sense? Why might it still be unacceptable in practice?
3. **Kerckhoffs’ principle:** Give one modern system where relying on “secret algorithms” instead of secret keys would be dangerous. Explain in 2–3 sentences.

### True/False Practice – Week 1 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. Perfect secrecy implies that for any prior distribution on messages, observing the ciphertext does not change the attacker’s posterior belief about which message was sent. (**True**)
2. A scheme can be perfectly secret even if the key is shorter than the message, as long as the encryption algorithm is complicated enough. (**False**)
3. The One-Time Pad is IND-CPA secure against computationally bounded adversaries when the key is reused on multiple messages. (**False**)
4. Under the perfect secrecy definition, ciphertext length must be at least the message length, but it is allowed to be longer. (**True**)
5. Computational security allows a scheme that can be broken with probability $1/2$ by an unbounded adversary, as long as every PPT adversary only has negligible advantage. (**True**)
6. In game-based definitions, an adversary advantage of $0.25$ is always considered negligible for sufficiently large security parameter. (**False**)
7. In an IND-CPA game, the attacker is modeled as a passive eavesdropper who never interacts with any oracle. (**False**)
8. A chosen-ciphertext adversary (CCA) is strictly more powerful than a chosen-plaintext adversary (CPA). (**True**)
9. A passive attacker can violate confidentiality but not integrity. (**True**)
10. Kerckhoffs’ principle assumes that the encryption algorithm, key generation procedure, and protocol are all known to the attacker. (**True**)
11. Security by obscurity means that the system remains secure even if the attacker learns the key, but not the algorithm. (**False**)
12. If an encryption scheme is IND-CCA secure, then it is also IND-CPA secure. (**True**)
13. Negligible functions must go to zero faster than $1/n^c$ for some fixed $c>0$. (**False**)
14. A function $\\mu(n)=2^{-\\sqrt{n}}$ is considered negligible in the security parameter. (**True**)
15. A function $\\mu(n)=1/n^{10}$ is negligible because it is extremely small for $n=2^{128}$. (**False**)
16. In the real-or-random (RoR) security game for symmetric encryption, the adversary distinguishes encryptions of chosen messages from random strings of the same length. (**True**)
17. If an encryption scheme always outputs a ciphertext of fixed length independent of the message length, it trivially satisfies IND-CPA security. (**False**)
18. Perfect secrecy protects against both passive and active attackers, as long as key reuse does not occur. (**True**)
19. Computational security relies on hardness assumptions such as factoring or discrete logarithms, which may fail if quantum computers become practical. (**True**)
20. In a well-defined threat model, it is acceptable to leave the attacker’s capabilities informal, as long as the advantage function is clearly defined. (**False**)

#### Extra True/False – Combining Week 1 Concepts

21. If a scheme is perfectly secret, then it automatically also satisfies any game-based indistinguishability notion like IND-CPA and IND-CCA (ignoring efficiency). (**True**)
22. A scheme can be IND-CPA secure but still completely fail to provide integrity or authenticity. (**True**)
23. Non-repudiation is usually provided by symmetric-key MACs, since both parties share the same key. (**False**)
24. Stating “our scheme is secure” on an exam without specifying a threat model and security game is considered incomplete. (**True**)
25. A negligible function may be larger than $1/p(n)$ for infinitely many $n$ as long as it is eventually smaller beyond some threshold $N$. (**True**)
