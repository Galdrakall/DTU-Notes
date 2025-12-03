---
title: "Week 7 Notes – Public-Key Crypto & RSA"
week: 7
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/7, public-key, rsa]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_6_Notes]]"
  next: "[[Week_8_Notes]]"
---

# Week 7 – Public-Key Cryptography & RSA

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_6_Notes]] · Next: [[Week_8_Notes]]

## Public vs private keys

In **public-key cryptography** (asymmetric cryptography):

- Each user has a **key pair** $(pk, sk)$:
  - Public key $pk$: shared with everyone.
  - Secret key $sk$: kept private.

Two main primitives:

- **Public-key encryption**: Anyone can encrypt to $pk$; only the owner of $sk$ can decrypt.
- **Digital signatures**: Only the owner of $sk$ can sign; anyone with $pk$ can verify.

Advantages:

- Solves key distribution problems that symmetric-key crypto alone struggles with.
- Enables secure communication without shared secret beforehand.

---

## Trapdoor functions

A **trapdoor function** $f$ is:

- Easy to compute $y = f(x)$.
- Hard to invert: given $y$, infeasible to find any $x$ such that $f(x) = y$.
- But with some secret “trapdoor” information, inversion becomes easy.

Public-key encryption often uses trapdoor permutations:

- Public key describes function $f$.
- Secret key is trapdoor that allows inversion.

Example: RSA function $f(x) = x^e \bmod N$ is easy to compute; inverting $y^d \bmod N$ is easy if you know the trapdoor (the factorization of $N$).

---

## RSA key generation

1. Choose two large random primes $p, q$.
2. Compute modulus $N = p \cdot q$.
3. Compute Euler’s totient: $\varphi(N) = (p-1)(q-1)$.
4. Choose public exponent $e$ relatively prime to $\varphi(N)$ (e.g. $65537$).
5. Compute private exponent $d$ as modular inverse of $e$ modulo $\varphi(N)$:

$$
ed \equiv 1 \pmod{\varphi(N)}.
$$

- **Public key**: $(N, e)$.
- **Private key**: $d$ (and optionally $p, q$ for more efficient decryption).

Security relies on hardness of **factoring** $N$ to get $p, q$, and thus $\varphi(N)$.

---

## RSA encryption & decryption

Basic (textbook) RSA:

- Message $m$ is an integer in $[0, N-1]$.
- Encryption with public key $(N, e)$:

$$
c = m^e \bmod N.
$$

- Decryption with private key $d$:

$$
m = c^d \bmod N.
$$

Correctness:

$$
m^{ed} \equiv m \pmod{N},
$$

because $ed \equiv 1 \pmod{\varphi(N)}$ and by Euler’s theorem.

---

## RSA-OAEP

Textbook RSA is deterministic and malleable, so it is not IND-CPA secure. **OAEP** (Optimal Asymmetric Encryption Padding) is a padding scheme:

- Randomized padding and encoding of message before applying RSA exponentiation.
- Uses hash functions and mask generation functions to ensure:
  - Randomness and non-determinism.
  - Structure that prevents partial information leakage and chosen-ciphertext issues (when combined with correct usage).

Result: RSA-OAEP is widely used as a practical **IND-CCA secure** public-key encryption scheme under certain assumptions.

---

## Why raw RSA is insecure

Problems with “raw” (textbook) RSA:

- **Deterministic**: identical plaintexts $\rightarrow$ identical ciphertexts; no IND-CPA security.
- **Malleability**: from $c = m^e$, an attacker can compute $(c \cdot r^e) \bmod N$ which corresponds to plaintext $m \cdot r \bmod N$.
- Without proper padding, many attacks exist (e.g. partial plaintext recovery, chosen-ciphertext oracle attacks, etc.).

Conclusion:

- Never use “bare” RSA on raw messages.
- Always use standardized padding (OAEP for encryption, PSS/FDH for signatures) or a hybrid encryption scheme: use RSA to encrypt a random symmetric key, then use symmetric AE for bulk data.
