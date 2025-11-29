
## Why Encryption Alone Is Not Enough
Encryption provides confidentiality only.
It does NOT prevent:
- Message modification
- Message injection
- Replay attacks

MACs provide the missing **integrity and authenticity**.

---

## Structure of a MAC Scheme
A MAC scheme has:
- A secret key $k$
- A tagging algorithm $\text{MAC}_k(m)$
- A verification algorithm $\text{Verify}_k(m, t)$

Security goal: Prevent any valid forgery.

---

## Security Goal of a MAC
Even after seeing many valid tags:
$$
(m_1, t_1), \dots, (m_q, t_q)
$$
an adversary cannot produce:
$$
(m^*, t^*)
$$
such that:
$$
\text{Verify}_k(m^*, t^*) = 1
$$
and $m^*$ is new.

---

## PRF-Based MAC Construction
Given a PRF $F_k$:
$$
t = F_k(m)
$$

Security relies on:
- Pseudorandomness of $F_k$

---

## CBC-MAC
Constructed using a block cipher in CBC mode with a zero IV.

For message blocks $m_1, \dots, m_\ell$:
$$
c_0 = 0
$$
$$
c_i = E_k(m_i \oplus c_{i-1})
$$
$$
t = c_\ell
$$

---

## Why We Need Authenticated Encryption
Secure communication requires:
- Encryption → confidentiality
- MAC → integrity + authenticity

Modern systems therefore use **Authenticated Encryption (AE)**.

---

## Encrypt-then-MAC (EtM)
Construction:
1. $c = \text{Enc}_{k_1}(m)$
2. $t = \text{MAC}_{k_2}(c)$
3. Output $(c, t)$

Decryption:
- Verify $t$ first
- Only then decrypt $c$

---

## AEAD in Practice
Modern protocols use AEAD modes such as:
- GCM
- ChaCha20-Poly1305
