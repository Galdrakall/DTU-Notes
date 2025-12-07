---
title: "Week 2 Notes – Symmetric Encryption & One-Time Pad"
week: 2
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/2, symmetric]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_1_Notes]]"
  next: "[[Week_3_Notes]]"
---
---

> Linked index: [[Cryptography Fundamentals]]  
> Previous: [[Week_1_Notes]] · Next: [[Week_3_Notes]]

---

## Caesar & Vigenère Ciphers

### Caesar Cipher

- Alphabet: A–Z (26 letters)
- Key: $k \in \{0,\dots,25\}$
- Encryption: $c = (m + k) \bmod 26$
- Decryption: $m = (c - k) \bmod 26$

Weaknesses:
- Only 26 keys → brute force
- Preserves frequencies → frequency analysis
- Provides **no real security**

---

### ✅ Practical Caesar Example

**Question:** Encrypt HELLO with $k=3$.

**Solution:**
H→K, E→H, L→O, L→O, O→R  
Ciphertext = **KHOOR**

---

### Vigenère Cipher

- Key $K = k_1\ldots k_\ell$
- Key is repeated over the message
- Equivalent to interleaved Caesar ciphers

Weaknesses:
- Period detectable
- Reduces to Caesar ciphers
- Broken by classical cryptanalysis

---

### ✅ Practical Vigenère Example

**Question:** Encrypt ATTACK with key KEY.

**Solution:**

KEYKEY  
ATTACK  
Add letters mod 26 → ciphertext obtained character-wise by Caesar shifts.

---

## One-Time Pad (OTP)

- Message $m \in \{0,1\}^n$
- Key $k \in \{0,1\}^n$ uniform
- Encryption: $c = m \oplus k$
- Decryption: $m = c \oplus k$

Security:
- Every ciphertext equally likely
- Provides **perfect secrecy**

Key reuse is fatal:

$$
c_1 \oplus c_2 = m_1 \oplus m_2
$$

---

### ✅ Practical OTP Encryption

**Question:**  
$m = 110010$, $k = 011101$

**Solution:**

$$
c = 110010 \oplus 011101 = 101111
$$

---

### ✅ Practical OTP Reuse Attack

**Question:**  
If $c_1 \oplus c_2 = 011011$, what leaks?

**Solution:**  
The XOR of the two plaintexts. With known structure, both messages can be recovered.

---

## Perfect Secrecy

A scheme $\Pi = (\text{KeyGen},\text{Enc},\text{Dec})$ has perfect secrecy if:

$$
P[\text{Enc}(k,m_0)=c] = P[\text{Enc}(k,m_1)=c]
$$

for all $m_0, m_1, c$.

**Equivalent Bayesian view:** For all messages $m$ and ciphertexts $c$,

$$
P[M=m \mid C=c] = P[M=m],
$$

so observing $C=c$ does **not change** the attacker's belief about $M$.

Shannon’s theorem:
- Key length ≥ message length
- Perfect secrecy is **information-theoretic**

**Intuition:** If the key space is smaller than the message space, then there are not enough keys to “hide” every message equally well. For some ciphertext $c$ and message $m$ we must have $P[\text{Enc}(k,m)=c] = 0$ while another message $m'$ satisfies $P[\text{Enc}(k,m')=c] > 0$. This makes $P[M=m\mid C=c] \neq P[M=m]$ and leaks information.

---

### ✅ Practical Shannon Question

**Question:**  
Can a 128-bit key perfectly encrypt 1 GB of data?

**Solution:**  
No. Perfect secrecy requires key ≥ message length.

---

## Formal Model of Symmetric-Key Encryption (SKE)

An SKE is a triple:

- $Keygen(): \emptyset \to K$
- $Enc: K \times M \to C$
- $Dec: K \times C \to M$

Correctness:

$$
\forall k \in K,\forall m \in M:\quad Dec(k,Enc(k,m)) = m
$$

---

### ✅ Practical Correctness Check

**Question:**  
If a scheme decrypts correctly only 99% of the time, is it valid?

**Solution:**  
No. Correctness requires probability 1.

---

## Real-or-Random (RoR) Security

- Bit $b$ is sampled
- If $b=0$: encrypt real message
- If $b=1$: output random ciphertext
- Adversary guesses $b$

Secure if:

$$
\left|P[b'=b] - \frac12\right|
$$

is negligible.

This is (for symmetric encryption) equivalent to **IND-CPA** security.

---

### ✅ Practical RoR Distinguishability

**Question:**  
If encryptions of the same message always match, is RoR secure?

**Solution:**  
No. Deterministic encryption is distinguishable.

**Reason:** In the real library, querying the same message twice always yields the same ciphertext. In the random library, two independently sampled ciphertexts are equal only with probability about $1/|C|$, which is negligible. This gives an easy distinguisher.

---

## One-Time Real-or-Random Security of OTP

OTP is **one-time RoR secure**:
- For every message and ciphertext, probability is $1/|C|$.
- Every ciphertext appears exactly once for each message.

This proves:

$$
L_{\text{real}} \approx L_{\text{rand}}
$$

for OTP with fresh keys.

---

### ✅ Practical Proof Idea Question

**Question:**  
Why does OTP make every ciphertext equally likely?

**Solution:**  
Because for each $(m,c)$ there exists exactly one $k = m \oplus c$, and keys are uniform.

---

## Computational Security

Perfect secrecy is too strong.

We restrict attackers to **polynomial time** in security parameter $\lambda$.

Keyspace size: $|K| = 2^\lambda$

Brute force becomes infeasible for large $\lambda$.

---

### ✅ Key Length Intuition

**Question:**  
Why does increasing $\lambda$ by 1 double attack cost?

**Solution:**  
Because the keyspace doubles: $2^{\lambda+1} = 2 \cdot 2^\lambda$.

**Further intuition:** Going from $\lambda = 128$ to $\lambda = 256$ multiplies brute-force cost by another factor of $2^{128}$, which is astronomically large. This is why 128-bit keys are already considered very secure in practice.

---

## Negligible Functions

A function $f(\lambda)$ is negligible if:

$$
\forall p(\cdot):\quad \lim_{\lambda\to\infty} p(\lambda)f(\lambda)=0
$$

Example: $2^{-\lambda}$ is negligible.
Non-examples: $1/\lambda$, $1/\log \lambda$, $1/\lambda^{10}$ are **not** negligible (they do not go to zero faster than the inverse of every polynomial).

---

### ✅ Practical Negligibility Test

**Question:**  
Is $1/\lambda$ negligible?

**Solution:**  
No. It is polynomially small but not negligible.

---

## Library Indistinguishability

Let $A \circ L$ be an algorithm with black-box access.

Definition:

$$
L_0 \approx L_1
$$

if for all PPT $A$:

$$
\left|P[A\circ L_0=1] - P[A\circ L_1=1]\right|
$$

is negligible.

---

## Pseudorandom Functions (PRFs)

A deterministic function:

$$
F: \{0,1\}^\lambda \times \{0,1\}^n \to \{0,1\}^n
$$

is a PRF if it is indistinguishable from a truly random function.

Security requires:
- Secret key
- Adaptive distinguishers fail

---

### ✅ Practical PRF Pitfall

**Question:**  
Is $Enc(k,m)=F(k,m)$ a secure encryption scheme?

**Solution:**  
No. PRFs are not invertible and not random per message.

---

## Pseudorandom Permutations (PRPs)

A PRP is:
- A PRF that is bijective
- Efficiently invertible

Used to build block ciphers.

---

## SKE from a PRP

Let $F$ be a PRP:

- $Keygen$: sample $k$
- $Enc(k,m)=F(k,m)$
- $Dec(k,c)=F^{-1}(k,c)$

This forms a valid symmetric encryption scheme.

---

### ✅ Practical PRP Question

**Question:**  
Why must encryption be invertible?

**Solution:**  
Otherwise decryption is impossible.

---

## Final Exam Key Takeaways (Week 2)

You must be able to:

- Compute OTP encryptions
- Explain OTP security proof
- Perform XOR-based attacks
- Distinguish perfect vs computational security
- Apply RoR / IND-CPA definitions
- Recognize deterministic encryption failures
- Explain PRFs vs PRPs
- Justify why PRPs yield SKE schemes

---

## Week 2 Cheat Sheet

**Classical ciphers:**
- Caesar: shift by fixed $k$ mod 26; tiny keyspace; frequency leakage.
- Vigenère: repeated key → interleaved Caesar; period detection breaks it.

**OTP:**
- $c = m \oplus k$, $k$ uniform, $|k|=|m|$, never reuse $k$.
- Perfect secrecy: $P[M=m \mid C=c] = P[M=m]$.
- Reuse leak: $c_1 \oplus c_2 = m_1 \oplus m_2$.

**SKE model:**
- $KeyGen, Enc, Dec$ with correctness $Dec(k,Enc(k,m))=m$.

**RoR / IND-CPA:**
- Real-or-random game: adversary distinguishes real encryptions from random.
- Deterministic encryption fails RoR/IND-CPA (equality leakage).

**Computational security:**
- Security parameter $\lambda$; keyspace $2^\lambda$; brute-force cost $\approx 2^\lambda$.
- Negligible $f(\lambda)$: smaller than $1/p(\lambda)$ for every polynomial $p$.

**PRFs/PRPs (high level):**
- PRF: keyed function indistinguishable from random function.
- PRP: PRF that is a permutation with efficient inverse (block cipher).

### Extra Mini Questions (Practice)

1. **OTP misuse:** You know $m_1$ is an English text and $m_2$ is also English, and you observe $c_1 \oplus c_2$. Sketch how you might start recovering $m_1$ and $m_2$.
2. **RoR security:** Design an explicit distinguisher that breaks any deterministic encryption scheme in the RoR game.
3. **Key length choice:** For security parameter $\lambda = 80$, estimate the brute force effort vs $\lambda = 128$. Why is 80 bits considered borderline today, but 128 bits is not?

### True/False Practice – Week 2 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. A Caesar cipher over the English alphabet has an effective key space of size 26, which makes it vulnerable to brute-force attacks. (**True**)
2. Both Caesar and Vigenère ciphers are considered IND-CPA secure when used with long, random keys. (**False**)
3. In the One-Time Pad, the ciphertext has exactly the same length as the plaintext. (**True**)
4. The One-Time Pad is perfectly secret even if the key is reused across multiple messages, as long as the key is random. (**False**)
5. If an encryption scheme is perfectly secret in the Shannon sense, then every ciphertext is equally likely for every plaintext under a uniform key distribution. (**True**)
6. Real-or-Random (RoR) security for a symmetric encryption scheme implies IND-CPA security for that scheme. (**True**)
7. Any deterministic symmetric encryption scheme is secure in the RoR sense, because the ciphertexts always look the same for the same message. (**False**)
8. In the RoR game, the adversary may submit messages of arbitrary lengths as long as it stays within polynomial time. (**True**)
9. In a correct SKE scheme, $Dec(k,Enc(k,m))=m$ must hold with probability 1 over the randomness of key generation and encryption. (**True**)
10. A symmetric encryption scheme that decrypts incorrectly with probability $1/2^{128}$ on random inputs is still considered correct in the formal model. (**False**)
11. In an IND-CPA experiment, the two challenge messages $m_0$ and $m_1$ must have the same length to avoid trivial leakage through ciphertext length. (**True**)
12. A scheme can be RoR secure even if its ciphertext length reveals the exact length of the plaintext. (**True**)
13. In computational security, we require that every PPT adversary’s advantage is bounded by a negligible function of the security parameter. (**True**)
14. Increasing the security parameter $\lambda$ by 1 bit approximately multiplies brute-force attack cost by 4. (**False**)
15. The key space size $|K| = 2^\lambda$ means a brute-force key search has expected running time roughly $2^{\lambda-1}$ encryptions. (**True**)
16. A function $f(\lambda)=1/\log \lambda$ is negligible because it tends to zero as $\lambda$ grows. (**False**)
17. If $f(\lambda)=2^{-\lambda}$ and $g(\lambda)=2^{-\sqrt{\lambda}}$, then both are negligible functions. (**True**)
18. A PRF $F(k,\cdot)$ can be used directly as a secure symmetric encryption by defining $c=F(k,m)$, since PRFs are indistinguishable from random functions. (**False**)
19. A PRP is automatically invertible for each fixed key, which is why it can be used to construct a correct block-cipher-based encryption scheme. (**True**)
20. In a secure PRP-based SKE with deterministic $Enc(k,m)=F(k,m)$, encrypting the same plaintext twice under the same key may produce different ciphertexts. (**False**)

#### Extra True/False – Week 2 Interactions

21. Any perfectly secret scheme must have ciphertexts at least as long as plaintexts, but computationally secure schemes may use shorter ciphertexts than plaintexts. (**True**)
22. OTP is IND-CPA and IND-CCA secure as long as the key is used only once. (**True**)
23. A scheme that is one-time RoR-secure with fresh keys for each message is also one-time IND-CPA secure. (**True**)
24. If a scheme is IND-CPA secure, then its ciphertexts must be indistinguishable from uniformly random strings of the same length. (**False**)
25. In the RoR security game, leaking the exact message length is acceptable because security is defined for messages of arbitrary (possibly different) lengths. (**True**)
