
## MAC-Then-Encrypt (MtE)
If:
1. $t = \text{MAC}_k(m)$
2. Encrypt $(m, t)$

This can leak information through decryption errors and is not generally CCA-secure.

---

## Encrypt-and-MAC (E&M)
Encrypt and MAC the plaintext separately.
This can allow:
- Malleability attacks
- Tag replay attacks

---

## Variable-Length CBC-MAC Attack
If message lengths differ:
An attacker can combine valid tags to create forgeries.

---

## Padding Oracle Attacks
Occur when:
- CBC encryption is used
- Decryption error messages differ
- Attacker learns padding correctness

Leads to full plaintext recovery.

---

## Replay Attacks
Reusing old valid $(c,t)$ pairs can:
- Reinsert commands
- Break protocol logic

MACs do not prevent replay unless nonces are included.

---

## Using Encryption Without MAC
Allows:
- Silent data corruption
- Message injection
- Bit-flipping attacks
