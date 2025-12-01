
## What Digital Signatures Provide
Digital signatures ensure:
- Authenticity
- Integrity
- Non-repudiation

They do NOT provide confidentiality.

---

## Structure of a Signature Scheme

Key generation:
$$
(pk, sk) \leftarrow \text{KeyGen}()
$$

Signing:
$$
\sigma = \text{Sign}_{sk}(m)
$$

Verification:
$$
\text{Verify}_{pk}(m, \sigma)
$$

---

## Security Goal
Even after seeing signatures on many chosen messages:
$$
(m_1, \sigma_1), \dots, (m_q, \sigma_q)
$$
an attacker cannot produce:
$$
(m^*, \sigma^*)
$$
with:
- $m^*$ new
- $\text{Verify}_{pk}(m^*, \sigma^*) = 1$

---

## Hash-and-Sign
Instead of signing $m$ directly, we sign:
$$
H(m)
$$

This:
- Avoids algebraic attacks
- Supports arbitrarily long messages
- Improves efficiency

---

## RSA Signatures (High-Level)
Signing:
$$
\sigma = H(m)^d \bmod N
$$

Verification:
$$
H(m) \stackrel{?}{=} \sigma^e \bmod N
$$

---

## ECDSA (High-Level View)
Based on:
- Elliptic curve discrete logarithm
- Fresh randomness for each signature
- Two output values $(r, s)$ per signature

---

## Digital Signatures vs MACs

| Property | Signature | MAC |
|----------|-----------|-----|
| Public verification | Yes | No |
| Requires shared secret | No | Yes |
| Non-repudiation | Yes | No |
| Multiple verifiers | Yes | No |
