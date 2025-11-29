## Why We Need Modes of Operation
Block ciphers only encrypt fixed-size blocks.
Messages are usually longer than one block.
Modes allow secure encryption of arbitrary-length messages.

---

## AES as a Block Cipher
AES is a block cipher with:
- Block size: 128 bits
- Key sizes: 128, 192, 256 bits
- Modeled as a secure PRP

AES consists of repeated rounds using:
- SubBytes
- ShiftRows
- MixColumns
- AddRoundKey

---

## Electronic Codebook (ECB) Mode
Definition:
$$
c_i = E_k(m_i)
$$

Properties:
- Deterministic
- Same plaintext block → same ciphertext block
- Reveals patterns

---

## Cipher Block Chaining (CBC) Mode

Encryption:
$$
c_0 = \text{IV}
$$
$$
c_i = E_k(m_i \oplus c_{i-1})
$$

Decryption:
$$
m_i = D_k(c_i) \oplus c_{i-1}
$$

Properties:
- Randomized by IV
- Hides repeated plaintext blocks

---

## Counter (CTR) Mode

Keystream generation:
$$
K_i = E_k(\text{ctr} + i)
$$

Encryption:
$$
c_i = m_i \oplus K_i
$$

Decryption:
$$
m_i = c_i \oplus K_i
$$

Properties:
- Parallelizable
- Turns a block cipher into a stream cipher

---

## Deterministic vs Randomized Encryption
ECB is deterministic.
CBC and CTR are randomized using IVs or nonces.

Randomized encryption is required for CPA security.