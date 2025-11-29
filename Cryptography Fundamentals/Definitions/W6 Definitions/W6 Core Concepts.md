
## Purpose of Hash Functions
Hash functions are used for:
- Data integrity
- Digital signatures
- Password storage
- Commitment schemes
- MAC constructions (HMAC)

They do NOT provide encryption or confidentiality.

---

## Fixed-Length Output
A hash function always produces a fixed-length output:
- SHA-256 → 256 bits
- SHA-512 → 512 bits

---

## One-Wayness
Hash functions are easy to compute but hard to invert.
This is captured by **preimage resistance**.

---

## Collision Resistance vs Preimage Resistance
Collision resistance is a **stronger** property than second preimage resistance.

Attacker freedom:
- Preimage: target is fixed
- Collision: attacker chooses both messages

---

## Merkle–Damgård Structure
Message is split into blocks:
$$
m = m_1, m_2, \dots, m_\ell
$$

Iterative hashing:
$$
h_0 = \text{IV}
$$
$$
h_i = f(h_{i-1}, m_i)
$$
Final output:
$$
H(m) = h_\ell
$$

---

## Padding
Messages are padded so that their length is a multiple of the block size.
Padding must include length encoding to avoid ambiguity.

---

## Why Salting Is Needed
Salts prevent:
- Rainbow table attacks
- Precomputed dictionary attacks

Salted hash:
$$
H(\text{salt} \parallel m)
$$

---

## HMAC (High-Level View)
A MAC built from a hash function:
$$
\text{HMAC}_k(m) = H\big((k \oplus \text{opad}) \parallel H((k \oplus \text{ipad}) \parallel m)\big)
$$
