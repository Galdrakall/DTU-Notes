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
---
# Week 10 – Discrete Logarithms, Diffie–Hellman & Elliptic Curve Cryptography

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_9_Notes]] · Next: [[Week_11_Notes]]

---
## Additive Groups Modulo $n$

Let:
$$
\mathbb{Z}_n = \{0,1,2,\dots,n-1\}
$$
with operation $+$ modulo $n$.

This forms a **group**:
- Closure
- Associativity
- Identity: $0$
- Inverse: $a + (-a) \equiv 0 \pmod n$

Example ($n=12$):
- $1 + 11 \equiv 0$
- $6 + 6 \equiv 0$

---

## Generator in Additive Groups

Element $a \in \mathbb{Z}_n$ is a **generator** if:
$$
\langle a \rangle = \{ka \bmod n : k \in \mathbb{Z}\} = \mathbb{Z}_n
$$

For $n=12$:
- Generators: $\{1,5,7,11\}$
- Non-generators: $\{0,2,3,4,6,8,9,10\}$

Number of generators:
$$
\varphi(n)
$$
(Euler’s totient function)

Example:
$$
\varphi(12) = 4
$$

---

## Order of an Element

The **order** of $a$:
$$
|a| = \min\{k>0 : ka \equiv 0 \pmod n\}
$$

Lagrange’s Theorem:
$$
|a| \mid |\mathbb{Z}_n|
$$

If $|a| = n$, then $a$ is a generator.

---

## Multiplicative Groups Modulo $n$

Define:
$$
\mathbb{Z}_n^* = \{a \in \mathbb{Z}_n : \gcd(a,n)=1\}
$$

This is the **group of units** under multiplication mod $n$.

Size:
$$
|\mathbb{Z}_n^*| = \varphi(n)
$$

Example ($n=12$):
$$
\mathbb{Z}_{12}^* = \{1,5,7,11\}
$$

---

## When Is $\mathbb{Z}_n^*$ Cyclic? (Gauss’ Theorem)

$\mathbb{Z}_n^*$ is cyclic **iff**:
$$
n = 1,2,4,p^k,2p^k
$$
for odd prime $p$.

If $p$ is prime:
$$
\mathbb{Z}_p^* \text{ is always cyclic.}
$$

Used heavily in discrete-log cryptography.

---

## Efficient Exponentiation (Square-and-Multiply)

To compute:
$$
y = g^x \bmod N
$$
efficiently:

Write $x$ in binary and repeatedly:
- Square
- Multiply when bit = 1

Runtime:
$$
O(\log N)
$$

Instead of naive $O(N)$ multiplications.

---

### ✅ Practical Square-and-Multiply

**Question:** Compute $3^{18} \bmod 31$.

**Solution:**
$$
3^2=9,\quad 3^4=19,\quad 3^8=20,\quad 3^{16}=28
$$
$$
3^{18}=3^{16}\cdot3^2=28\cdot9=252 \equiv 4 \pmod{31}
$$

---

## Discrete Logarithm Problem (DLP)

Given:
- Cyclic group $G = \langle g\rangle$
- $h = g^x$

Find:
$$
x = \log_g h
$$

This is believed to be computationally infeasible in:
- $\mathbb{Z}_p^*$
- Elliptic curve groups

Security parameters:
- $p$ must be large prime
- Often choose **safe primes**:
$$
p = 2q+1
$$

---

## DLP Security Game

1. Challenger samples $x \leftarrow G$
2. Computes $h=g^x$
3. Adversary is given $(g,h)$
4. Wins if it outputs $x$

DLP is hard if adversary success is negligible.

---

## Computational Diffie–Hellman (CDH)

Given:
$$
(g^a, g^b)
$$
compute:
$$
g^{ab}
$$

Hardness of CDH ⇒ attacker cannot compute shared secret in DH.

---

## Decisional Diffie–Hellman (DDH)

Given:
$$
(g^a, g^b, g^c)
$$

Decide whether:
$$
c = ab
$$

DDH is **stronger** than CDH.

Hierarchy:
$$
\text{DLP} \Rightarrow \text{CDH} \Rightarrow \text{DDH}
$$

(In some groups DDH may be easy.)

---

## Diffie–Hellman Key Exchange

1. Public parameters: $(G,g)$
2. Alice samples $a$, sends $A=g^a$
3. Bob samples $b$, sends $B=g^b$
4. Shared key:
$$
K = g^{ab}
$$

Alice computes $B^a$, Bob computes $A^b$.

Security relies on **CDH**.

---

## Man-in-the-Middle Attack (MITM)

Without authentication:

- Adversary replaces $A$ and $B$
- Forms two keys $K_{AE}$ and $K_{EB}$
- Decrypts, modifies, re-encrypts

**Fix:** authenticated Diffie–Hellman
- Certificates
- Digital signatures
- Pre-shared MACs

---

## Revisiting ElGamal Encryption

Keygen:
- $x \in \mathbb{Z}_q$
- $pk=g^x$

Encrypt $m \in G$:
$$
c_0 = g^r,\quad c_1 = m\cdot pk^r
$$

Decrypt:
$$
m = c_1 \cdot (c_0^x)^{-1}
$$

Security:
- One-way under **CDH**
- IND-CPA under **DDH**
- **Not IND-CCA secure** (malleable)

---

## Extending ElGamal to Arbitrary Messages

Encode general message $m$ via:
- Hash-to-group
- Hybrid encryption (encrypt key with ElGamal)

Used in practice only via:
- Hybrid schemes
- AEAD + KEM

---

# Elliptic Curve Cryptography (From Your Original Notes)

## Elliptic Curve Groups

Elliptic curve over $\mathbb{F}_p$:
$$
y^2 = x^3 + ax + b \mod p
$$

Points $(x,y)$ plus point at infinity $\mathcal{O}$ form an **abelian group**.

Security requires:
- Large prime-order subgroup
- Secure curve parameters

---

## Point Addition

- Line through $P,Q$ intersects curve at $R$
- Reflect $R$ → $P+Q$
- Tangent line for doubling $2P$

Scalar multiplication:
$$
kP = P + \dots + P
$$
computed by **double-and-add**.

---

## Elliptic Curve Discrete Logarithm (ECDLP)

Given:
$$
P,\ Q = kP
$$
Find $k$.

Much harder than DLP in $\mathbb{Z}_p^*$ for same bit size.

---

## Elliptic Curve Diffie–Hellman (ECDH)

1. Alice: $A=aG$
2. Bob: $B=bG$
3. Shared secret:
$$
K=abG
$$

Key derived from $x$-coordinate of $K$ using a KDF.

---

## ECC vs RSA/DH (Classical Security)

| Security | ECC | RSA |
|----------|-----|-----|
| 128-bit  | 256-bit | 3072-bit |
| 192-bit  | 384-bit | 7680-bit |

ECC advantages:
- Smaller keys
- Faster
- Less bandwidth

---

## Invalid-Curve & Small-Subgroup Attacks

If implementation fails to verify points:
- Attacker sends low-order points
- Extracts bits of private key

Mitigations:
- Check point lies on curve
- Multiply by subgroup order
- Reject point at infinity

---

## Final Exam Key Takeaways (Week 10)

You must be able to:

- Define additive and multiplicative groups modulo $n$
- Compute Euler’s totient $\varphi(n)$
- Define generators and order
- Explain DLP, CDH, and DDH
- Perform Diffie–Hellman key exchange
- Explain MITM attack & how to prevent it
- State ElGamal encryption and its security level
- Define elliptic curve groups
- Explain ECDLP and ECDH
- Compare ECC vs RSA/DH
- Explain invalid-curve attacks at a high level

---

## Week 10 Cheat Sheet

**Groups modulo n:**
- Additive group $\mathbb Z_n$: elements $0..n-1$ with $+$ mod $n$.
- Multiplicative group $\mathbb Z_n^*$: invertible elements with $\gcd(a,n)=1$.
- Order of $a$: smallest $k>0$ with $ka\equiv0\pmod n$ (additive) or $a^k\equiv1\pmod n$ (multiplicative).

**Generators & cyclic groups:**
- Generator $g$: its powers (or multiples) cover the entire group.
- $\mathbb Z_p^*$ is cyclic for prime $p$.

**Efficient exponentiation:**
- Square-and-multiply / double-and-add gives $O(\log x)$ multiplications.

**Hardness assumptions:**
- DLP: given $(g,g^x)$, find $x$.
- CDH: from $(g^a,g^b)$ compute $g^{ab}$.
- DDH: distinguish $(g^a,g^b,g^{ab})$ from $(g^a,g^b,g^c)$.

**Diffie–Hellman (DH):**
- Exchange $g^a$ and $g^b$; both compute $g^{ab}$.
- MITM attack if $g^a/g^b$ not authenticated.

**ElGamal (recap):**
- Enc: $(g^r, m\cdot g^{xr})$; Dec: divide by $(g^r)^x$.
- IND-CPA under DDH, but malleable, not IND-CCA.

**Elliptic curves:**
- Points $(x,y)$ satisfying $y^2 = x^3 + ax + b$ over $\mathbb F_p$ plus point at infinity $\mathcal O$.
- Group operation: geometric point addition and doubling; scalar multiplication $kP$ via double-and-add.

**ECDLP & ECDH:**
- ECDLP: given $P,Q=kP$, find $k$.
- ECDH: exchange $aG$ and $bG$, derive $abG$.
- Smaller keys than RSA/DH at same security level.

**Implementation pitfalls:**
- Invalid-curve/small-subgroup attacks if received points not validated.
- Always check that a point is on the curve and in the correct subgroup.

### Extra Mini Questions (Practice)

1. **Order calculation:** In $\mathbb Z_{12}$, what is the order of $5$ under addition? Is it a generator?
2. **DH security:** Briefly explain why knowing $g^a$ and $g^b$ does not (under CDH) allow computing $g^{ab}$ efficiently.
3. **ECDH vs DH:** For 128-bit security, why might you choose a 256-bit ECC group instead of a 3072-bit RSA modulus?
4. **Invalid-curve check:** Name two concrete checks an implementation should perform on a received ECC point before using it in ECDH.

### True/False Practice – Week 10 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. In classical Diffie–Hellman over a DDH-hard group, the resulting shared secret can be used as a symmetric key in an IND-CPA secure scheme, achieving overall IND-CPA security for the channel. (**True**)
2. Diffie–Hellman key exchange alone (without signatures or certificates) is secure against man-in-the-middle attacks. (**False**)
3. ElGamal encryption over a DDH-hard group is IND-CPA secure but not IND-CCA secure. (**True**)
4. ElGamal ciphertexts have the same bit-length as plaintext messages, so there is no ciphertext expansion. (**False**)
5. In both DH and ECDH, the resulting shared secret is an element of the underlying group, which is typically compressed and then passed through a KDF to derive symmetric keys. (**True**)
6. The Elliptic Curve Discrete Logarithm Problem (ECDLP) is believed to be easier than DLP in $\mathbb{Z}_p^*$ for the same bit-length, which is why ECC uses much larger keys. (**False**)
7. ECDH can be viewed as a direct analogue of DH where group operations are performed on elliptic curve points instead of integers modulo $p$. (**True**)
8. If an implementation of ECDH does not validate that received points lie on the correct curve, it may be vulnerable to invalid-curve attacks and key leakage. (**True**)
9. Small-subgroup attacks exploit points of low order in the group to gradually learn information about the private scalar. (**True**)
10. Using a safe prime $p=2q+1$ helps ensure that $\mathbb{Z}_p^*$ has a large prime-order subgroup, reducing the risk of small-subgroup attacks. (**True**)
11. CDH hardness implies DDH hardness in every finite cyclic group. (**False**)
12. ElGamal encryption constructed over an elliptic curve group inherits the same malleability issues as its classical counterpart. (**True**)
13. In a well-designed ECC system, the order of the base point $G$ is a large prime, and all keys are chosen as scalars modulo that order. (**True**)
14. If ECDLP is solved efficiently for a given curve, then both ECDH and any signatures (like ECDSA) using that curve become insecure. (**True**)
15. The security level of ECC is typically measured in bits relative to the size of the subgroup generated by $G$, not directly by the prime field size $p$. (**True**)
16. Invalid-curve checks are unnecessary if you always use random points from the curve parameter file provided by the standard. (**False**)
17. A DH-based key-exchange protocol combined with an IND-CCA secure AEAD scheme can yield an overall IND-CCA secure channel, assuming proper authentication. (**True**)
18. Because ElGamal is IND-CPA secure, combining it with a MAC in an Encrypt-then-MAC construction can provide IND-CCA secure public-key encryption. (**True**)
19. ECDH provides forward secrecy only when used with fresh ephemeral key pairs for each session. (**True**)
20. Using static (long-term) ECDH keys without any ephemeral contribution still provides forward secrecy as long as ECDLP is hard. (**False**)
