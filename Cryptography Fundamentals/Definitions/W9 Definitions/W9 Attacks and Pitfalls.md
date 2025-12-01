
## Message Reuse Attacks
If a raw RSA signature is used:
$$
\sigma = m^d \bmod N
$$
Then:
$$
(m_1 m_2)^d = m_1^d \cdot m_2^d
$$
allows existential forgeries.

---

## Weak Hash Functions
If the hash is not collision resistant:
- Attacker finds $m \neq m'$ with $H(m)=H(m')$
- One valid signature works for two different messages

---

## Randomness Failure in DSA/ECDSA
If the per-signature random value is:
- Reused
- Or predictable

Then the secret signing key can be recovered.

---

## Signature Replay
Reusing old signatures can:
- Bypass freshness checks
- Break protocol logic (unless timestamps/nonces are used)

---

## Signing the Wrong Data
If metadata is not included in the signature:
- Attacker can reorder or modify context
- Still pass verification
