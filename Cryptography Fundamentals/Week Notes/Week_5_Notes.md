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

# Week 5 – MACs & Authenticated Encryption

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_4_Notes]] · Next: [[Week_6_Notes]]

## Message Authentication Codes (MACs)

A **MAC** provides integrity and authenticity:

- KeyGen: output secret key $k$.
- Tag: $t = \text{MAC}_k(m)$ (also called Sign or Tag).
- Verify: $\text{Verify}_k(m, t) \in \{\text{accept}, \text{reject}\}$.

Goal: prevent an adversary from forging valid pairs $(m, t)$.

MACs are symmetric-key analogues of digital signatures.

---

## MAC security definitions

Standard notion: **Existential unforgeability under chosen-message attack (EUF-CMA)**.

Game:

1. Challenger chooses random key $k$.
2. Adversary can query a **tag oracle** on messages of its choice, receiving $t_i = \text{MAC}_k(m_i)$.
3. Eventually, adversary outputs a purported forgery $(m^*, t^*)$.
4. Adversary wins if:
   - $\text{Verify}_k(m^*, t^*) = \text{accept}$, and
   - $m^*$ was **never** queried to the tag oracle.

A MAC is secure if any PPT adversary’s winning probability is negligible.

---

## CBC-MAC

Construction using a block cipher $E_k$:

- Message divided into blocks $m_1, \dots, m_\ell$ of block size.
- Initialization: $x_0 = 0^{\text{blocksize}}$.
- For $i$ from $1$ to $\ell$:

$$
x_i = E_k(x_{i-1} \oplus m_i)
$$

Tag is $t = x_\ell$.

Properties:

- Secure (with some caveats) for **fixed-length** messages.
- Insecure for variable-length messages unless length is included and scheme is adjusted (e.g. EMAC, CMAC, etc.).

Without the right adjustments, attackers can concatenate blocks and build forgeries.

---

## Encrypt-then-MAC

To achieve both confidentiality and integrity, we use **authenticated encryption**.

General approach: **Encrypt-then-MAC**:

1. Compute ciphertext: $c = \text{Enc}_{k_{\text{enc}}}(m)$.
2. Compute tag: $t = \text{MAC}_{k_{\text{mac}}}(c)$.
3. Send $(c, t)$.

Decryption:

1. Verify $t$ first: check $\text{MAC}_{k_{\text{mac}}}(c) \stackrel{?}{=} t$.
2. If valid, decrypt $c$ to recover $m$.
3. If invalid, reject.

Advantages:

- If encryption is IND-CPA and MAC is EUF-CMA, Encrypt-then-MAC can give strong authenticated encryption (IND-CCA).
- Improper compositions (MAC-then-encrypt, or encrypt-and-MAC) can be fragile or insecure in some settings.

---

## IND-CCA security

**IND-CCA** = indistinguishability under **chosen-ciphertext attack**.

Game:

1. Challenger chooses bit $b$ and key(s).
2. Adversary has access to:
   - **Encryption oracle**: encrypts messages of adversary’s choice.
   - **Decryption oracle**: decrypts ciphertexts of adversary’s choice, but not the challenge ciphertext.
3. Adversary chooses messages $m_0, m_1$ of same length.
4. Challenger returns challenge $c^* = \text{Enc}(m_b)$.
5. Adversary continues to use the decryption oracle for any ciphertexts $\neq c^*$.
6. Finally, adversary outputs guess $b'$.

Scheme is IND-CCA secure if:

$$
\left| P[b' = b] - \frac{1}{2} \right|
$$

is negligible.

IND-CCA is a strong notion: ensures ciphertexts remain secure even if the attacker can get decryptions of other ciphertexts (e.g. adaptive chosen-ciphertext scenarios).
