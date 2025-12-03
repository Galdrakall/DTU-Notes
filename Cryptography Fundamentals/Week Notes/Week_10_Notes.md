---
title: "Week 10 Notes – Elliptic Curve Cryptography"
week: 10
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/10, ecc]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_9_Notes]]"
  next: "[[Week_11_Notes]]"
---

# Week 10 – Elliptic Curve Cryptography (ECC)

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_9_Notes]] · Next: [[Week_11_Notes]]

## Elliptic curve groups

An elliptic curve over a finite field $\mathbb{F}_p$ is often given by:

$$
y^2 = x^3 + ax + b \mod p
$$

with parameters chosen so that the curve is non-singular (no cusps or self-intersections).

The set of points $(x, y)$ satisfying the equation plus a special point at infinity $\mathcal{O}$ form an **abelian group** under a defined addition law.

Security-critical properties:

- Group order should be large (e.g. around $2^{256}$).
- No small subgroups or special structure that enable attacks.

---

## Point addition

Group operation “$+$” on the curve is defined geometrically:

1. To add two points $P$ and $Q$ (with $P \neq Q$):
   - Draw line between $P$ and $Q$.
   - It intersects the curve at a third point $R$.
   - Reflect $R$ across the x-axis to get $P + Q$.

2. To compute $2P$ (point doubling):
   - Take tangent line at $P$.
   - Find intersection $R$.
   - Reflect across x-axis to get $2P$.

This intuitive geometric description corresponds to explicit formulas over finite fields.

Scalar multiplication:

- $kP = P + P + \dots + P$ (k times).
- Efficiently computed via double-and-add algorithms.

Hard problem:

- **Elliptic Curve Discrete Logarithm Problem (ECDLP)**: given $P$ and $Q = kP$, find $k$.

---

## Elliptic Curve Diffie–Hellman (ECDH)

Elliptic-curve analogue of DH:

- Public parameters: elliptic curve $E$, base point $G$ of large prime order.

Protocol:

1. Alice chooses secret scalar $a$, computes $A = aG$.
2. Bob chooses secret scalar $b$, computes $B = bG$.
3. Alice computes shared point: $K = aB = abG$.
4. Bob computes shared point: $K = bA = abG$.

Security relies on hardness of ECDLP / ECDH in chosen curve.

Advantages:

- Smaller keys for equivalent security vs RSA/DH over integers.
- Common choices: curves like `secp256r1`, `Curve25519`, etc.

---

## Benefits over RSA/DH

ECC advantages (for classical computers):

- **Smaller key sizes**:
  - ~256-bit ECC $\approx$ 3072-bit RSA security level.
- Performance:
  - Less bandwidth (smaller public keys, signatures).
  - Faster operations on constrained devices.
- Lower memory and computational footprint.

However:

- Implementation must be very careful (side-channel attacks, invalid curve attacks).
- Choice of curve parameters and standards matters.
