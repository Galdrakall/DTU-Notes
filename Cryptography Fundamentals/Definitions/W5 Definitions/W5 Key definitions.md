
## IND-CCA Security (Indistinguishability under Chosen-Ciphertext Attack)
An encryption scheme is **IND-CCA secure** if no PPT adversary can distinguish encryptions of chosen messages **even if it can query a decryption oracle**, except for the challenge ciphertext.

### Formal Game Definition
1. The challenger generates a key $k$.
2. The adversary $\mathcal{A}$ gets access to:
   - An encryption oracle $\text{Enc}_k(\cdot)$
   - A decryption oracle $\text{Dec}_k(\cdot)$
3. $\mathcal{A}$ submits two equal-length messages $(m_0, m_1)$.
4. The challenger picks $b \in \{0,1\}$ and sends:
   $$
   c^* = \text{Enc}_k(m_b)
   $$
5. $\mathcal{A}$ continues to query the decryption oracle on any ciphertext **except $c^*$**.
6. $\mathcal{A}$ outputs a guess $b'$.

The adversary’s advantage is:
$$
\text{Adv}_{\mathcal{A}}^{\text{IND-CCA}} =
\left|\Pr[b' = b] - \tfrac12 \right|
$$

The scheme is **IND-CCA secure** if:
$$
\forall \mathcal{A} \in \text{PPT}, \quad 
\text{Adv}_{\mathcal{A}}^{\text{IND-CCA}} \le \text{negl}(\lambda)
$$
---
## Message Authentication Code (MAC)
A keyed algorithm that provides **integrity and authenticity** of a message.

Formally, a MAC consists of:
- Tag generation: $t = \text{MAC}_k(m)$
- Verification: $\text{Verify}_k(m, t) \in \{0,1\}$

---

## Unforgeability
A MAC is unforgeable if no PPT adversary can generate a **new valid (message, tag)** pair except with negligible probability.

---

## Ciphertext Integrity (INT-CTXT)
A security notion ensuring that an adversary cannot create a new valid ciphertext that decrypts successfully.

---

## Authenticated Encryption (AE)
An encryption scheme that provides:
- Confidentiality
- Integrity
- Authenticity

---

## Authenticated Encryption with Associated Data (AEAD)
Authenticated encryption that also authenticates **unencrypted metadata** (headers).

---

## Encrypt-then-MAC (EtM)
A composition where:
1. Encrypt the message
2. MAC the ciphertext

---

## MAC Forgery
A successful creation of a valid tag for a message that was never previously authenticated.
