---
title: "Week 8 Notes – DLP, DH & ElGamal"
week: 8
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/8, dlog, dh, elgamal]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_7_Notes]]"
  next: "[[Week_9_Notes]]"
---

# Week 8 – Discrete Logarithms, DH & ElGamal

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_7_Notes]] · Next: [[Week_9_Notes]]

## Cyclic groups

A group $G$ with operation $\cdot$ is **cyclic** if there exists element $g \in G$ such that:

$$
G = \{ g^0, g^1, g^2, \dots, g^{n-1} \}
$$

for some order $n$, where exponents are taken modulo $n$.

- $g$ is a **generator** of $G$.
- Example: multiplicative group $\mathbb{Z}_p^*$ for prime $p$ is cyclic of order $p-1$.

Cryptography often uses groups:

- $(\mathbb{Z}_p^*, \cdot)$,
- Subgroups of elliptic curves.

---

## Discrete Log Problem

Given:

- A cyclic group $G$ of order $n$ with generator $g$.
- Element $h \in G$ such that $h = g^x$.

The **discrete logarithm problem (DLP)** is:

> Given $(G, g, h)$, find $x$ such that $h = g^x$.

We choose groups where DLP is believed hard for large parameters.

Security of many protocols (Diffie–Hellman, ElGamal, some signature schemes) relies on hardness of DLP or related assumptions.

---

## Diffie–Hellman key exchange

Goal: two parties (Alice and Bob) establish a shared secret over a public channel.

Setup:

- Public parameters: large prime $p$ and generator $g \in \mathbb{Z}_p^*$.

Protocol:

1. Alice chooses secret $a$, sends $A = g^a \bmod p$ to Bob.
2. Bob chooses secret $b$, sends $B = g^b \bmod p$ to Alice.
3. Alice computes shared key: $K = B^a = g^{ba} \bmod p$.
4. Bob computes shared key: $K = A^b = g^{ab} \bmod p$.

They end up with the same shared secret $K = g^{ab}$.

Security relies on hardness of **Computational Diffie–Hellman (CDH)**:

- Given $g^a, g^b$, hard to compute $g^{ab}$.

And sometimes **Decisional Diffie–Hellman (DDH)**:

- Distinguishing $g^{ab}$ from random element given $g^a, g^b$ is hard.

---

## Man-in-the-middle attacks

Basic DH alone does not authenticate participants:

- Attacker Mallory intercepts $A = g^a$, replaces with $M = g^m$ towards Bob.
- Intercepts Bob’s $B = g^b$, replaces with another value to Alice.
- Mallory establishes keys $g^{am}$ with Alice and $g^{bm}$ with Bob.

Both sides think they share a key with each other, but actually they share keys with Mallory.

Remedy:

- Add authentication: signatures, MACs with pre-shared keys, certificates, etc.
- Use protocols that combine DH with authentication (e.g. authenticated key exchange).

---

## ElGamal encryption

Public-key encryption based on DH:

- Parameters: group $G$, generator $g$.
- Key generation:
  - Choose secret key $x$.
  - Compute public key $h = g^x$.
- Encryption of message $m \in G$ with public key $h$:
  1. Choose random $r$.
  2. Compute $c_1 = g^r$.
  3. Compute $c_2 = m \cdot h^r$.
  4. Ciphertext: $(c_1, c_2)$.
- Decryption with secret key $x$:

$$
m = c_2 \cdot (c_1^x)^{-1}
$$

since $c_1^x = g^{rx} = h^r$.

---

## IND-CPA security

ElGamal is **IND-CPA secure** under the **Decisional Diffie–Hellman (DDH)** assumption:

- Given $(g^a, g^b, g^c)$, hard to tell whether $c = ab$ or $c$ is random.

Intuition:

- Ciphertext contains $g^r$ and $m \cdot g^{xr}$.
- If DDH holds, then $g^{xr}$ looks random given $g^x, g^r$.
- So ciphertext hides $m$ against chosen-plaintext attacks.

However, ElGamal is **malleable** and not IND-CCA secure (cannot resist chosen-ciphertext attacks without further modifications).
