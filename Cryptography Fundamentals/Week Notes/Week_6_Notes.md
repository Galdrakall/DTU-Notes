---
title: "Week 6 Notes – Hash Functions"
week: 6
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/6, hash]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_5_Notes]]"
  next: "[[Week_7_Notes]]"
---
---
# Week 6 – Hash Functions

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_5_Notes]] · Next: [[Week_7_Notes]]

---

## Cryptographic Hash Functions

A cryptographic hash function is a function:

$$
H: \{0,1\}^* \to \{0,1\}^{\lambda}
$$

such that it is:
- Efficient to compute
- Deterministic
- Compressing (arbitrary length → fixed length)
- Output looks random

Naming convention:
- $m$ = message
- $h$ = hash value / digest

---

## Preimage Resistance

Given output $y$, it should be **computationally infeasible** to find $x$ such that:

$$
H(x) = y.
$$

Hash function acts like a one-way function.

---

### ✅ Practical Preimage Example

**Question:**  
If SHA-256 is preimage-secure, how many guesses on average are needed to invert one hash?

**Solution:**  
About $2^{255}$ guesses on average (assuming ideal behavior).

---

## Second Preimage Resistance

Given $x$, it should be infeasible to find a *different* input $x' \neq x$ such that:

$$
H(x') = H(x).
$$

Brute-force attacker:

1. Compute $y = H(x)$  
2. Guess $x' \leftarrow \{0,1\}^\ell$  
3. Check if $H(x') = y$

Success probability per attempt: $2^{-\lambda}$  
Expected work: $2^\lambda$

---

### ✅ Practical Second-Preimage Work Factor

**Question:**  
If $\lambda=128$, how many trials are needed?

**Solution:**  
About $2^{128}$ trials.

---

## Collision Resistance

It should be infeasible to find **any** pair $x \neq x'$ such that:

$$
H(x) = H(x').
$$

Brute-force collision attacker:

1. Choose random $m_i$
2. Store $H(m_i)$
3. Check for duplicates

Expected work governed by the **birthday bound**.

---

## Birthday Bound

For output length $\lambda$:

- Collision probability after $q$ samples:

$$
\text{Pr}[\text{collision}] \approx \frac{q(q-1)}{2^{\lambda+1}}
$$

- Collisions become likely when:

$$
q \approx 2^{\lambda/2}
$$

Thus:
- 128-bit output → $2^{64}$ collision security
- 256-bit output → $2^{128}$ collision security

---

### ✅ Practical Birthday Question

**Question:**  
How many hashes until a collision is likely for SHA-256?

**Solution:**  
About $2^{128}$ hashes.

---

## Relations Between Hash Properties

Intuition for well-designed hashes:

- Collision resistance ⇒ second-preimage resistance (informal)
- None of the three properties strictly implies the others in all provable models
- Good cryptographic hashes aim to satisfy **all three**

---

## Salting

A **salt** is a random value $s$ combined with the message:

$$
H'(s,m) = H(s \parallel m)
$$

Uses:
- Password hashing
- Prevents rainbow-table attacks
- Prevents cross-user hash comparison

Salt is:
- Public
- Stored next to the hash
- Must be unique per user

---

### Personalized Salts

Instead of global salt:

- Alice stores $(s_A, H(s_A,p_A))$
- Bob stores $(s_B, H(s_B,p_B))$

Now:
- Same passwords do **not** produce same hashes
- Each password must be cracked individually

---

### ✅ Practical Salting Question

**Question:**  
Does salting prevent brute-force guessing?

**Solution:**  
No. It prevents *precomputation* and *cross-user reuse*, but brute force is still possible for weak passwords.

---

## Password Hashing (Best Practice)

Never store:
- Plaintext passwords
- Fast hashes (SHA-256 alone)

Instead use **slow hash functions**:
- PBKDF2
- scrypt
- bcrypt
- Argon2

These:
- Intentionally slow down brute-force attacks
- Provide memory hardness

---

## Random Oracle Model (ROM)

In many proofs we **model $H$ as a truly random function**:

- Everyone can query it
- Outputs are independent and uniform
- Not realistic, but useful for proofs

Used in proofs of:
- FDH signatures
- OAEP encryption
- HMAC security

---

### ✅ Practical ROM Question

**Question:**  
Why do we use ROM if it’s unrealistic?

**Solution:**  
It allows provable security results when real-world hash behavior is hard to analyze directly.

---

## Formal Definition of Collision Resistance (Salted Model)

Because attackers could hard-code collisions, we define:

$$
H: \{0,1\}^{\lambda} \times \{0,1\}^* \to \{0,1\}^{\lambda}
$$

Library experiment:

- Sample random salt $s$
- Attacker outputs $(m,m')$
- Wins if $m \neq m'$ and:

$$
H(s,m) = H(s,m')
$$

Security: attacker wins only with negligible probability.

---

## Merkle–Damgård Construction

Used to build a hash from a compression function:

$$
h: \{0,1\}^{t+\lambda} \to \{0,1\}^{\lambda}
$$

Process:

1. Pad message with MD-padding:
   - Append zeros
   - Append encoding of $|m|$
2. Split into $t$-bit blocks: $m_1,\dots,m_k$
3. Initialization $y_0 = 0^\lambda$
4. Iterate:

$$
y_i = h(m_i \parallel y_{i-1})
$$

5. Output $H(m)=y_k$

---

### ✅ Practical MD-Padding Example

For $t=4$, $m=001101$:

- $|m|=6 → e=0110$
- Pad to blocks:

$$
0011 \; 0100 \; 0110
$$

---

## Collision Resistance of Merkle–Damgård

**Theorem:**  
If compression function $h$ is collision-resistant, then so is $H_h$.

Proof idea:
- Any collision in $H_h$ implies a collision in some invocation of $h$.
- Trace computation backward until first divergence.
- That divergence gives a collision on $h$.

---

## Length-Extension Attacks

For Merkle–Damgård hashes:

Given:
- $H(m)$
- $|m|$

Attacker can compute:

$$
H(m \parallel \text{pad}(m) \parallel m')
$$

**without knowing $m$**.

Implication:
- Never use $H(m)$ directly as a MAC.
- Always use:
  - HMAC, or
  - Key inside compression function.

---

### ✅ Practical Length-Extension Attack

**Question:**  
Why is $MAC(k,m)=H(k\parallel m)$ unsafe with MD hashes?

**Solution:**  
Because attackers can extend $m$ and compute a valid tag for the extended message.

---

## Final Exam Key Takeaways (Week 6)

You must be able to:

- Define:
  - Preimage resistance
  - Second preimage resistance
  - Collision resistance
- Apply:
  - Birthday bound
  - $2^{\lambda/2}$ rule
- Explain:
  - Why we need salts
  - Why slow hashes are required for passwords
  - Why Merkle–Damgård needs length in padding
  - Why raw hashes cannot be used as MACs
- Describe:
  - Random Oracle Model
  - Length-extension attacks

---

## Week 6 Cheat Sheet

**Hash function basics:**
- $H: \{0,1\}^* \to \{0,1\}^{\lambda}$; deterministic, efficient, compressing.
- Outputs should look random for security.

**Security properties:**
- Preimage resistance: given $y$, hard to find any $x$ s.t. $H(x)=y$ (cost $\approx 2^{\lambda}$).
- Second preimage: given $x$, hard to find $x'\neq x$ with $H(x')=H(x)$.
- Collision resistance: hard to find **any** pair $x\neq x'$ with same hash (birthday bound).

**Birthday bound:**
- Collision prob $\approx q(q-1)/2^{\lambda+1}$ after $q$ hashes.
- Collisions likely at $q \approx 2^{\lambda/2}$.

**Salts & passwords:**
- Salted hash: $H(s \parallel m)$; $s$ is random, public, per-user.
- Prevents precomputed rainbow tables and cross-user comparisons.
- Does **not** stop online/offline brute-force on weak passwords.

**Password hashing:**
- Use slow, memory-hard functions: PBKDF2, bcrypt, scrypt, Argon2.
- Goal: make each guess expensive to slow attackers.

**Merkle–Damgård (MD):**
- Build hash from compression function $h$ and MD-padding (includes length).
- If $h$ is collision resistant, MD construction is collision resistant.

**Length-extension attacks:**
- For MD-style hashes, knowing $H(m)$ and $|m|$ lets attacker compute $H(m\parallel\text{pad}(m)\parallel m')$.
- Therefore **never** do $MAC(k,m)=H(k\parallel m)$ with MD hashes; use HMAC or a different construction.

**Random Oracle Model (ROM):**
- Idealized model: treat $H$ as a truly random function.
- Simplifies proofs (e.g. signatures, OAEP, HMAC), but is an abstraction.

### Extra Mini Questions (Practice)

1. **Collision vs preimage:** Give a scenario where collision resistance is needed but preimage resistance alone would not be enough.
2. **Birthday calculation:** For a 160-bit hash (e.g. SHA-1), roughly how many hashes can you compute before collisions are likely? Show the $2^{\lambda/2}$ reasoning.
3. **Salting:** Explain why using the same global salt for all users is worse than using a fresh random salt per user.
4. **Length extension:** Sketch how an attacker could append data to a signed message when the “MAC” is actually just $H(k\parallel m)$ with MD hashing.

### True/False Practice – Week 6 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. A cryptographic hash function maps arbitrary-length inputs to fixed-length outputs and should be efficient and deterministic. (**True**)
2. Preimage resistance means that given $y$, it is hard to find any $x$ such that $H(x)=y$. (**True**)
3. Second preimage resistance means that given $x$, it is easy to find $x'\neq x$ such that $H(x')=H(x)$. (**False**)
4. Collision resistance requires that it is infeasible to find **any** distinct pair $(x,x')$ such that $H(x)=H(x')$. (**True**)
5. Due to the birthday bound, an $n$-bit hash function offers roughly $2^{n/2}$ security against generic collision attacks. (**True**)
6. For SHA-256, generic collisions are expected after about $2^{64}$ evaluations. (**False**)
7. Salting passwords with a unique random salt per user prevents attackers from reusing precomputed rainbow tables across different users. (**True**)
8. Salting alone (without slow hashing) completely prevents offline brute-force attacks on weak passwords. (**False**)
9. Password-hashing schemes like bcrypt, scrypt, and Argon2 are designed to increase the cost of each password-guess attempt. (**True**)
10. In the Random Oracle Model, the hash function is treated as a truly random function that parties can query as a black box. (**True**)
11. Real hash functions like SHA-256 are perfect random oracles in practice. (**False**)
12. In the salted collision-resistance definition, the attacker must find a collision for a randomly chosen salt $s$, not for a salt of its choice. (**True**)
13. Merkle–Damgård construction uses a compression function and MD-style padding (including the length) to hash arbitrarily long messages. (**True**)
14. If the compression function $h$ used in Merkle–Damgård is collision-resistant, then the resulting hash $H_h$ is also collision-resistant. (**True**)
15. Length-extension attacks exploit the ability to extend $H(m)$ to $H(m\parallel\text{pad}(m)\parallel m')$ when Merkle–Damgård is used. (**True**)
16. A construction $MAC(k,m)=H(k\parallel m)$ using a Merkle–Damgård hash is secure against length-extension attacks. (**False**)
17. HMAC is specifically designed to be secure even when the underlying hash function follows the Merkle–Damgård paradigm. (**True**)
18. Preimage resistance alone implies collision resistance for any cryptographic hash function. (**False**)
19. Collision resistance implies second-preimage resistance for well-designed hash functions, but not necessarily the other way around. (**True**)
20. In practice, using fast unsalted hashes for passwords is acceptable if the hash is at least 256 bits long. (**False**)

#### Extra True/False – Week 6 Interactions

21. Collision-resistant hash functions are sufficient to build many higher-level primitives, such as Merkle trees and some digital signature schemes. (**True**)
22. A function can be collision resistant but not preimage resistant, depending on the construction. (**True**)
23. HMAC’s security in the ROM relies on modeling the underlying hash function as a random oracle. (**True**)
24. Using $MAC(k,m)=H(m\parallel k)$ instead of $H(k\parallel m)$ automatically prevents length-extension attacks in Merkle–Damgård. (**False**)
25. In password hashing, making each evaluation slower effectively reduces an attacker’s offline guessing rate, even if the hash output length stays the same. (**True**)
