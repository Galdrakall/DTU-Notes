
## Intuition: Lockbox with a Public Lock
Public-key crypto is like a box with:
- A lock that everyone can close (public key)
- A key that only the owner has (secret key)

Anyone can send a secret message.
Only the owner can open it.

---

## Example: Hybrid Encryption in Practice
In real systems:
1. A random symmetric key $k$ is generated
2. $k$ is encrypted with RSA:
$$
c_k = k^e \bmod N
$$
3. The message is encrypted with AES using $k$

This combines:
- Efficiency of symmetric crypto
- Key distribution of public-key crypto

---

## Intuition: Why Multiplication Matters Modulo $N$
RSA uses modular exponentiation.
Factoring $N$ breaks security because:
- Knowing $p$ and $q$ allows computing $\varphi(N)$
- Which allows computing $d$

---

## Example: Deterministic RSA Leak
If an attacker suspects the message is either:
- "YES" or "NO"

They encrypt both with the public key and compare with the observed ciphertext.

Determinism completely breaks confidentiality.

---

## Intuition: Padding Adds Randomness
Random padding:
- Makes RSA probabilistic
- Makes equal messages encrypt differently
- Restores IND-CPA security
