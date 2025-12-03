---
title: "Week 1 Notes – Introduction & Security Mindset"
week: 1
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/1, intro]
links:
  index: "[[Cryptography Fundamentals]]"
  next: "[[Week_2_Notes]]"
---

# Week 1 – Introduction and Security Mindset

> Linked index: [[Cryptography Fundamentals]]  
> Next week: [[Week_2_Notes]]

## What is cryptography?

Cryptography is the science (and art) of designing systems that protect information in the presence of adversaries.

Classic goals (often called the CIA triad + more):

- **Confidentiality** – only authorized parties learn the message.
- **Integrity** – messages cannot be modified undetected.
- **Authenticity** – messages come from who they claim to come from.
- **Non-repudiation** – a sender cannot later deny having sent a message.

Cryptography provides building blocks (primitives) like:

- Symmetric encryption (e.g. AES)
- Public-key encryption (e.g. RSA, ElGamal)
- Hash functions (e.g. SHA-256)
- Message authentication codes (MACs)
- Digital signatures

These primitives are combined into **protocols**: secure messaging, TLS for HTTPS, encrypted storage, cryptocurrencies, password hashing, etc.

---

## Threat models and adversaries

A **threat model** specifies:

- What the adversary can do (listen, modify, inject messages, corrupt devices, etc.).
- What the adversary **wants** (read messages, impersonate others, cause denial of service, etc.).
- What is assumed to be secure (e.g. computers are malware-free, keys are stored safely).

### Typical adversary capabilities

- **Passive attacker**: can only eavesdrop on communication.
- **Active attacker**: can intercept, modify, inject or delete messages (man-in-the-middle).
- **Chosen-plaintext attacker (CPA)**: can ask for encryptions of messages of their choice.
- **Chosen-ciphertext attacker (CCA)**: can ask for decryptions of ciphertexts of their choice (with some restrictions).

We must be explicit: *If we don’t specify what the attacker can do, we can’t argue security.*

---

## Kerckhoffs’ Principle

> A cryptosystem should remain secure even if everything about the system, except the key, is public.

Implications:

- You **must** assume attackers know your algorithms, implementations, protocols, and code.
- Security should depend only on a **secret key** that is short enough to store but long enough to resist brute-force search.
- “Security by obscurity” (hoping attackers don’t learn how your system works) is considered bad practice.

Benefits:

- Public algorithms can be studied, attacked and improved by the cryptographic community.
- Vulnerabilities are more likely to be discovered and fixed.

---

## Perfect vs computational security

### Perfect security

A scheme has **perfect secrecy** if *even an unbounded adversary* who sees the ciphertext learns nothing about the plaintext.

Formally, for every two messages $m_0, m_1$ of the same length and every ciphertext $c$:

$$
P[C = c \mid M = m_0] = P[C = c \mid M = m_1]
$$

This means:

- The ciphertext distribution is independent of the message.
- Knowing the ciphertext gives absolutely no information about which message was sent.
- Achievable with the **One-Time Pad** (but only if key is truly random, as long as the message, and never reused).

Downside: requires huge keys and strict usage rules $\rightarrow$ impractical for many real-world systems.

---

### Computational security

Perfect secrecy is often too strong and impractical. Instead we use **computational security**:

> A scheme is secure if any efficient (polynomial-time) adversary has only negligible advantage in breaking it.

Key ideas:

- We only consider adversaries that are limited in time/memory (e.g. can’t brute-force $2^{128}$ possibilities).
- Security is defined via **games**: the adversary interacts with oracles and tries to distinguish, forge, or break some property.
- An advantage is **negligible** if it is smaller than any inverse polynomial (e.g. less than $1/n^k$ for all $k$) for large enough security parameter $n$.

Most modern cryptography (AES, RSA, ECC, etc.) is based on computational security, relying on assumptions like:

- No efficient algorithm can factor large integers.
- No efficient algorithm can solve discrete logarithms in the chosen group.
- No efficient algorithm can distinguish PRFs from truly random functions.
