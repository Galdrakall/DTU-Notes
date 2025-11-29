## ECB Pattern Leakage
Since:
$$
c_i = E_k(m_i)
$$
identical plaintext blocks produce identical ciphertext blocks.
This leaks structure (e.g., images).

---

## IV Reuse in CBC
If the same IV is reused:
- The first block becomes vulnerable
- Relationships between plaintexts may leak

---

## Nonce Reuse in CTR
If the same counter value is reused:

$$
c_1 = m_1 \oplus K
$$
$$
c_2 = m_2 \oplus K
$$

Then:
$$
c_1 \oplus c_2 = m_1 \oplus m_2
$$

This is the same catastrophic failure as OTP key reuse.

---

## Bit-Flipping Attacks on CBC
Because:
$$
m_i = D_k(c_i) \oplus c_{i-1}
$$

An attacker can flip bits in $c_{i-1}$ to control bits in $m_i$.

---

## Using ECB in Practice
ECB has been used in real systems and has caused:
- Image leakage
- Data format exposure
- Serious real-world breaches