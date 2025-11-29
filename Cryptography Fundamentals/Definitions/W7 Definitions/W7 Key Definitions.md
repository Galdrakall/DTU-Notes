
## Public-Key Encryption (PKE)
An encryption scheme with two keys:
- Public key $pk$ (used for encryption)
- Secret key $sk$ (used for decryption)

Encryption:
$$
c = \text{Enc}_{pk}(m)
$$

Decryption:
$$
m = \text{Dec}_{sk}(c)
$$

---

## One-Way Function
A function that is easy to compute but hard to invert on average.

---

## Trapdoor One-Way Function
A one-way function that becomes easy to invert when a secret trapdoor is known.

---

## Modular Arithmetic
Arithmetic over $\mathbb{Z}_n$, where all values are reduced modulo $n$.

---

## RSA Modulus
A number of the form:
$$
N = p \cdot q
$$
where $p$ and $q$ are large secret primes.

---

## Euler’s Totient Function
For RSA:
$$
\varphi(N) = (p-1)(q-1)
$$

---

## Public Exponent
An integer $e$ such that:
$$
\gcd(e, \varphi(N)) = 1
$$

---

## Private Exponent
An integer $d$ such that:
$$
e \cdot d \equiv 1 \pmod{\varphi(N)}
$$

---

## RSA Encryption
$$
c = m^e \bmod N
$$

---

## RSA Decryption
$$
m = c^d \bmod N
$$
