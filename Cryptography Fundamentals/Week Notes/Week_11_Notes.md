---
title: "Week 11 Notes – Secure Messaging & Signal"
week: 11
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/11, signal, secure-messaging]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_10_Notes]]"
  next: "[[Week_12_Notes]]"
---

# Week 11 – Secure Messaging & Signal Protocol

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_10_Notes]] · Next: [[Week_12_Notes]]

## Forward secrecy

A protocol has **forward secrecy (FS)** if:

> Compromise of long-term keys does *not* reveal past session keys.

Meaning:

- Even if an attacker later obtains your long-term private key, they cannot decrypt old recorded conversations.

Typically achieved by:

- Using **ephemeral Diffie–Hellman** key exchange for each session.
- Session keys derived from ephemeral secrets that are then erased.

---

## Post-compromise security

**Post-compromise security (PCS)**:

> After an attacker has compromised a device (e.g. learned all keys), and then later loses access, the protocol eventually “heals” so future messages are again secure.

In secure messaging:

- Even if an attacker steals your phone and reads current keys, once you recover and attacker no longer observes your device, new messages become confidential again.
- Achieved via continuous key updates (ratcheting).

---

## Key ratcheting

A **ratchet** continually updates keys, so old keys are discarded:

- **Symmetric ratchet**: repeatedly apply a KDF or hash to derive new keys:

$$
K_{i+1} = H(K_i)
$$

- **DH ratchet**: periodically perform new DH exchanges, mixing new entropy into key material.

The **Double Ratchet** (used in Signal):

- Combines a symmetric ratchet with a DH ratchet.
- Each message typically uses a new key.
- Provides both FS and PCS: keys evolve with every message and DH step.

---

## X3DH

**X3DH** (“Extended Triple Diffie–Hellman”) is a key agreement protocol used for establishing initial shared secrets in Signal.

High-level idea:

- Uses multiple DH computations between:
  - Long-term identity keys,
  - Signed prekeys,
  - One-time prekeys,
  - Ephemeral keys.

Goals:

- Asynchronous setup: recipient can be offline.
- Authenticated key exchange using pre-published keys.
- Provides forward secrecy and resilience to some key compromise scenarios.

---

## Signal protocol design

The Signal protocol (for apps like Signal, WhatsApp, etc.) combines:

1. **X3DH** to establish initial shared secret between two users.
2. **Double Ratchet** to ratchet keys from that shared secret over time.
3. **Authenticated encryption** (e.g. AES-GCM, ChaCha20-Poly1305) for message confidentiality and integrity.
4. Additional features:
   - Message ordering, replay protection.
   - Support for group chats (Sender Keys, etc.).
   - Metadata protection measures (depending on implementation).

Security properties:

- End-to-end encryption.
- Forward secrecy.
- Post-compromise security.
- Deniability (signatures are not used directly on messages; MACs provide “plausible deniability”).
