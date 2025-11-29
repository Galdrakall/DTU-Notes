
## Why Public-Key Cryptography Is Needed
Symmetric cryptography requires a shared secret key.
Public-key cryptography solves the **key distribution problem**.

Anyone can encrypt using the public key.
Only the owner of the secret key can decrypt.

---

## Structure of a Public-Key Encryption Scheme
A PKE scheme consists of:
1. Key generation: $(pk, sk) \leftarrow \text{KeyGen}()$
2. Encryption: $c = \text{Enc}_{pk}(m)$
3. Decryption: $m = \text{Dec}_{sk}(c)$

Correctness:
$$
\Pr[\text{Dec}_{sk}(\text{Enc}_{pk}(m)) = m] = 1
$$

---

## RSA Key Generation
1. Choose large primes $p, q$
2. Compute:
$$
N = p \cdot q
$$
$$
\varphi(N) = (p-1)(q-1)
$$
3. Choose $e$ such that:
$$
\gcd(e, \varphi(N)) = 1
$$
4. Compute:
$$
d \equiv e^{-1} \pmod{\varphi(N)}
$$

Public key: $(N, e)$  
Secret key: $(N, d)$

---

## RSA Encryption and Decryption
Encryption:
$$
c = m^e \bmod N
$$

Decryption:
$$
m = c^d \bmod N
$$

---

## Why RSA Works
Because:
$$
m^{ed} \equiv m \pmod N
$$
which follows from Euler’s theorem.

---

## Deterministic Nature of Raw RSA
For a fixed $m$:
$$
c = m^e \bmod N
$$
is always the same.

Therefore, **plain RSA is deterministic and not IND-CPA secure**.
