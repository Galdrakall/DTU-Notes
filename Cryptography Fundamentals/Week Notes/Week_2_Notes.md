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

# Week 2 – Symmetric Encryption & One-Time Pad

> Linked index: [[Cryptography Fundamentals]]  
> Previous: [[Week_1_ Notes]] · Next: [[Week_3_Notes]]

## Caesar & Vigenère ciphers

### Caesar cipher

A simple substitution cipher:

- Alphabet: A–Z (26 letters).
- Secret key: integer $k \in \{0, \dots, 25\}$.
- Encryption: $c = (m + k) \bmod 26$.
- Decryption: $m = (c - k) \bmod 26$.

Weaknesses:

- Only 26 possible keys $\rightarrow$ brute-force is trivial.
- Preserves letter frequencies $\rightarrow$ vulnerable to frequency analysis.
- Provides essentially **no security** against any realistic attacker.

---

### Vigenère cipher

Generalizes Caesar using a **key word**:

- Key: $K = k_1 k_2 \dots k_\ell$, each $k_i \in \{0, \dots, 25\}$.
- To encrypt, repeat the key over the message:
  - position $i$: use key letter $k_{(i \bmod \ell)}$.
- This is equivalent to several interleaved Caesar ciphers.

Weaknesses:

- Periodicity: the same key letter reappears periodically.
- Once the key length is guessed (e.g. via Kasiski attack or index of coincidence), each position reduces to a Caesar cipher $\rightarrow$ frequency analysis again.
- Therefore **not secure** against modern cryptanalysis.

These historical ciphers illustrate why we need **formal security definitions**.

---

## One-Time Pad (OTP)

The One-Time Pad is a theoretically perfect encryption scheme.

### Setup

- Messages: bit strings $m \in \{0,1\}^n$.
- Key: uniformly random bit string $k \in \{0,1\}^n$.
- Encryption: $c = m \oplus k$.
- Decryption: $m = c \oplus k$ (since $(m \oplus k) \oplus k = m$).

### Security

- For any fixed ciphertext $c$, every possible plaintext $m$ is equally likely:
  - there is exactly one key $k = m \oplus c$ that maps $m$ to $c$.
- This gives **perfect secrecy** (Shannon).

### Limitations

- Key must be as long as the message.
- Key must be truly random.
- **Key reuse is forbidden**: reusing the same key for two messages $m_1, m_2$ gives:

$$
c_1 \oplus c_2 = (m_1 \oplus k) \oplus (m_2 \oplus k) = m_1 \oplus m_2
$$

which leaks information about both messages.

Due to these constraints, OTP is rarely practical for general communication.

---

## Perfect secrecy

A symmetric encryption scheme $\Pi = (\text{KeyGen}, \text{Enc}, \text{Dec})$ has **perfect secrecy** if for all messages $m_0, m_1$ of the same length and all ciphertexts $c$:

$$
P[\text{Enc}(k, m_0) = c] = P[\text{Enc}(k, m_1) = c]
$$

where probability is over the random key $k$.

Consequences:

- Ciphertext does not help an attacker to guess the plaintext.
- Even with infinite computational power, no information is gained.

Shannon’s theorem:

- Any perfectly secret scheme where messages are $n$-bit strings must have keys of length at least $n$.
- So perfect secrecy is “expensive” in key length.

---

## Real-or-Random security

Perfect secrecy is too strict. For practical schemes, we use **real-or-random (RoR)** (or IND-CPA) style definitions.

### Real-or-random (informal)

An adversary gets oracle access:

- If bit $b = 0$: oracle encrypts real messages $\text{Enc}_k(m)$.
- If bit $b = 1$: oracle instead encrypts random messages of the same length (or otherwise hides the message).

The adversary’s goal: output a guess $b'$ for $b$.

The scheme is secure if:

$$
\left| P[b' = b] - \frac{1}{2} \right|
$$

is **negligible** for all efficient adversaries.

Intuition:

- The adversary cannot tell whether they see encryptions of chosen messages or encryptions of random junk.
- This captures “ciphertexts don’t leak useful information about plaintexts” for computationally bounded attackers.

This real-or-random style security is the basis of **IND-CPA** (indistinguishability under chosen-plaintext attack).
