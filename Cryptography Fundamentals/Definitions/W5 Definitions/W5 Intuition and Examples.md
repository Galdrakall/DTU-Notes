
## Intuition: Locking + Sealing a Message
Encryption is like putting a message in a locked box.
A MAC is like adding a tamper-proof seal.

You need both to be secure.

---

## Example: Bit-Flipping Without a MAC
If:
$$
c = \text{Enc}_k(m)
$$
An attacker can flip bits in $c$ to change $m$ after decryption.
Without a MAC, the receiver will not detect this.

---

## Intuition: What a MAC Proves
A valid MAC proves:
- The message was created by someone with the secret key
- The message was not modified in transit

But it does NOT prove:
- Freshness
- Sender identity beyond possession of the key

---

## Example: Simple PRF-Based MAC
Sender computes:
$$
t = F_k(m)
$$
and sends $(m,t)$.
Receiver recomputes $F_k(m)$ and compares tags.

An attacker cannot forge $t$ without knowing $k$.

---

## Intuition: Why Encrypt-then-MAC Is the Right Order
If you MAC the ciphertext:
- Any change is detected before decryption
- The decryption algorithm is never exposed to attacker-controlled input

This prevents many real-world attacks.

---

## Example: AEAD in Practice
TLS uses:
- AES-GCM
- ChaCha20-Poly1305

These provide:
- Encryption
- Authentication
- Protection of headers (associated data)
