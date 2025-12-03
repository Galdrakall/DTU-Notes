---
title: "Week 6 Notes – Hash Functions"
week: 6
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/6, hash]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_5_Notes]]"
  next: "[[Week_7_Notes]]"
---

# Week 6 – Hash Functions

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_5_Notes]] · Next: [[Week_7_Notes]]

## Preimage resistance

Given output $y$, it should be **computationally infeasible** to find any input $x$ such that:

$$
H(x) = y.
$$

Hash function acts like a “one-way” function.

---

## Second preimage resistance

Given an input $x$, it should be infeasible to find a **different** input $x' \neq x$ such that:

$$
H(x') = H(x).
$$

Here the attacker is challenged with a specific $x$, then asked to find a colliding $x'$.

---

## Collision resistance

It should be infeasible to find **any** pair $(x, x')$ with $x \neq x'$ such that:

$$
H(x) = H(x').
$$

Collision resistance is stronger than second preimage resistance (though not strictly in all formal models—but informally stronger).

Birthday paradox:

- For an $n$-bit hash output, collisions can be found by brute force in about $2^{n/2}$ operations.
- So to get ~128-bit collision security, we need a 256-bit output.

---

## Merkle–Damgård construction

A generic way to build a hash function from a **compression function** $f$:

- Compression function $f$ maps a fixed-size input (state + block) to a fixed-size output: $f: \{0,1\}^{n + b} \to \{0,1\}^n$.
- Initialize internal state with IV.
- Pad the message (including its length) to multiples of block size.
- Process message blocks sequentially:

$$
h_0 = IV, \quad
h_i = f(h_{i-1}, m_i)
$$

Final hash is $h_\ell$.

Properties:

- If compression function is collision resistant (in a suitable sense), Merkle–Damgård yields a collision resistant hash.
- Many classic hashes (MD5, SHA-1, SHA-2) use Merkle–Damgård or variants.

Important to include message length in padding (Merkle–Damgård strengthening) to avoid some extension attacks.

---

## Salting

**Salt**: a randomly chosen value $s$ combined with input $x$ before hashing.

For example: $H(s \| x)$.

Use cases:

- **Password hashing**: salts prevent precomputed rainbow table attacks.
- Makes each user’s hash unique even if they share the same password.
- Prevents an attacker from learning that two users share a password just by comparing hashes.

Notes:

- Salt is **not secret**; it is typically stored with the hash.
- For passwords, we also want **slow** hashes (PBKDF2, bcrypt, scrypt, Argon2) so brute-force guessing is expensive.

Salting alone does not prevent offline brute-force if the password is weak, but it removes large precomputation advantages and cross-user reuse.
