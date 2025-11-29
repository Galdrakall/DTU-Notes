
## Purpose of Diffie–Hellman
Diffie–Hellman allows two parties to:
- Establish a shared secret
- Over a public, insecure channel
- Without any prior shared secret

This solves the key-agreement problem.

---

## Diffie–Hellman Setup
Public parameters:
- A cyclic group $G$
- Generator $g$

---

## Basic Diffie–Hellman Protocol

Alice:
$$
a \leftarrow \mathbb{Z}_q, \quad A = g^a
$$

Bob:
$$
b \leftarrow \mathbb{Z}_q, \quad B = g^b
$$

Shared secret:

Alice computes:
$$
K = B^a = g^{ba}
$$

Bob computes:
$$
K = A^b = g^{ab}
$$

Thus both compute the same $K$.

---

## Security Basis of DH
Security relies on the assumed hardness of:
- Discrete logarithm (DLP)
- Computational Diffie–Hellman (CDH)
- Decisional Diffie–Hellman (DDH)

---

## ElGamal Key Generation
1. Choose group $G$ and generator $g$
2. Secret key:
$$
x \leftarrow \mathbb{Z}_q
$$
3. Public key:
$$
h = g^x
$$

---

## ElGamal Encryption

To encrypt message $m \in G$:
1. Choose random:
$$
r \leftarrow \mathbb{Z}_q
$$
2. Compute:
$$
c_1 = g^r
$$
$$
c_2 = m \cdot h^r
$$

Ciphertext:
$$
c = (c_1, c_2)
$$

---

## ElGamal Decryption

Using secret key $x$:
$$
m = \frac{c_2}{c_1^x}
$$

Since:
$$
c_1^x = (g^r)^x = g^{rx}
$$
and:
$$
c_2 = m \cdot g^{rx}
$$

---

## Randomized Encryption
ElGamal is **randomized** because it uses fresh randomness $r$ for each encryption.
This makes ElGamal IND-CPA secure (under DDH).
