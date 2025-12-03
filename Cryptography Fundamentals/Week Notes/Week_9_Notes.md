---
title: "Week 9 Notes – Digital Signatures"
week: 9
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/9, signatures]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_8_Notes]]"
  next: "[[Week_10_Notes]]"
---

# Week 9 – Digital Signatures

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_8_Notes]] · Next: [[Week_10_Notes]]

## Signing & verification

A digital signature scheme:

- KeyGen: output $(pk, sk)$.
- Sign: $\sigma = \text{Sign}_{sk}(m)$.
- Verify: $\text{Verify}_{pk}(m, \sigma) \in \{\text{accept}, \text{reject}\}$.

Goals:

- Authenticity: signatures prove that the holder of $sk$ signed the message.
- Integrity: any change to the message invalidates the signature.
- Non-repudiation: signer cannot plausibly deny signing (this is more a legal/operational property but crypto helps).

Security notion: **EUF-CMA** (existential unforgeability under chosen-message attack):

- Adversary can get signatures on messages of its choice.
- Must not be able to produce a valid signature on any new message with non-negligible probability.

---

## Replay attacks

A **replay attack**:

- Adversary intercepts a valid signed message and simply re-sends (“replays”) it later.
- Signature verification still passes, so receiver may accept it as fresh.

Example: a signed transaction message being replayed multiple times.

Mitigations:

- Use **nonces**, timestamps, sequence numbers, or other context.
- Protocol should verify freshness, not only correctness of signature.
- Often signatures are over $(\text{message} \| \text{nonce} \| \text{context})$ rather than just message.

---

## RSA signatures

Textbook RSA signature (insecure in practice):

- Secret key $d$, public key $(N, e)$.
- To sign message integer $m$:

$$
\sigma = m^d \bmod N.
$$

- Verification: check $\sigma^e \equiv m \pmod{N}$.

Problems:

- No hashing: if messages can be structured, many attacks appear.
- Deterministic and algebraically simple $\rightarrow$ malleable and susceptible to various forgeries.

Safe real-world schemes use hashing and padding (like RSA-FDH, RSA-PSS).

---

## RSA-FDH (Full Domain Hash)

**Full Domain Hash (FDH)**:

- Let hash function $H: \{0,1\}^* \to \mathbb{Z}_N^*$ (modeled as random oracle).
- To sign message $m$:

$$
\sigma = H(m)^d \bmod N.
$$

- To verify:
  1. Compute $y = \sigma^e \bmod N$.
  2. Accept iff $y = H(m)$.

Benefits:

- Signature is on $H(m)$ which spreads messages uniformly across domain.
- Hard to find collisions $(m, m')$ with $H(m) = H(m')$ in random oracle model.
- Under standard assumptions (RSA and random oracle), RSA-FDH is EUF-CMA secure.

---

## Use cases of digital signatures

- **Software updates**: verify that binaries come from the vendor (e.g. OS updates).
- **TLS certificates**: CA signs certificates binding domain names to public keys.
- **Electronic documents**: sign contracts, PDF documents, code commits (e.g. git signing).
- **Cryptocurrencies**: authenticate ownership of coins (e.g. ECDSA in Bitcoin).

Across these applications, signatures enable verification of authenticity without pre-shared secrets.
