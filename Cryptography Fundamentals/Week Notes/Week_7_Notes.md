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

---
# Week 7 – Public-Key Cryptography & RSA

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_6_Notes]] · Next: [[Week_8_Notes]]

---

## Public vs Private Keys

In **public-key cryptography**:

- Each user has a key pair $(pk, sk)$
- $pk$ is public, $sk$ is secret
- Anyone can encrypt to $pk$
- Only owner of $sk$ can decrypt

Main primitives:
- **Public-key encryption (PKE)**
- **Digital signatures**

Advantages:
- Solves key-distribution problem
- Enables secure communication without pre-shared secrets

---

### ✅ Practical Question – Why Public Keys?

**Question:**  
Why don’t we use only symmetric keys on the Internet?

**Solution:**  
Because secure symmetric key distribution requires a secure channel beforehand. Public-key crypto removes this bootstrapping problem.

---

## Trapdoor Functions

A **trapdoor function** $f$:

- Easy to compute: $y=f(x)$
- Hard to invert without trapdoor
- Easy to invert with secret trapdoor

Trapdoor permutations form the basis of PKE.

Example:
$$
f(x)=x^e \bmod N
$$

Trapdoor: factorization of $N$.

---

## RSA Key Generation

1. Choose large random primes $p,q$  
2. Compute:
   $$
   N=pq
   $$
3. Compute:
   $$
   \varphi(N)=(p-1)(q-1)
   $$
4. Choose $e$ with:
   $$
   \gcd(e,\varphi(N))=1
   $$
5. Compute $d$:
   $$
   ed\equiv1 \pmod{\varphi(N)}
   $$

Public key: $(N,e)$  
Private key: $d$ (and optionally $p,q$)

Security relies on **factoring hardness**.

---

### ✅ Practical RSA Keygen Example

**Question:**  
If $p=11$, $q=7$, find $N$ and $\varphi(N)$.

**Solution:**  
$N=77$, $\varphi(N)=60$.

---

## RSA Encryption & Decryption (Textbook RSA)

Encryption:
$$
c = m^e \bmod N
$$

Decryption:
$$
m = c^d \bmod N
$$

Correctness:
$$
m^{ed} \equiv m \pmod N
$$

Because:
$$
ed = 1 + k\varphi(N)
$$
and by Euler’s theorem:
$$
x^{\varphi(N)} \equiv 1 \pmod N
$$

---

### ✅ Practical RSA Encryption

**Question:**  
Encrypt $m=5$ with $N=77$, $e=7$.

**Solution:**
$$
c = 5^7 \bmod 77 = 78125 \bmod 77 = 47
$$

---

## Domain Restriction of RSA

Proof assumes:
$$
m \in \mathbb{Z}_N^*
$$

Solutions:
- Use Chinese Remainder Theorem
- Probability random $m \notin \mathbb{Z}_N^*$ is negligible for large $N$

---

## Why Raw RSA Is Insecure

Problems:

- **Deterministic** → not IND-CPA
- **Malleable**:
$$
(m^e \cdot r^e) \bmod N = (mr)^e \bmod N
$$
- Enables algebraic attacks
- Breaks under chosen-ciphertext attacks

Conclusion:
> Never use textbook RSA directly on messages.

---

### ✅ Practical Malleability Attack

**Question:**  
If attacker multiplies RSA ciphertext by $2^e$, what happens?

**Solution:**  
Plaintext doubles: $m \mapsto 2m \bmod N$.

---

## IND-CPA Security for Public-Key Encryption

Real world:
$$
c \leftarrow Enc(pk,m_b)
$$

Adversary chooses $m_0,m_1$ and guesses $b$.

Security requires:
$$
\left|P[b'=b]-\frac12\right| \text{ negligible}
$$

Textbook RSA fails because encryption is deterministic.

---

## IND-CCA Security for Public-Key Encryption

Attacker:
- Has encryption oracle
- Has decryption oracle
- Cannot decrypt challenge ciphertext

Goal:
- Prevent malleability
- Prevent oracle-based decryption attacks

Raw RSA also fails IND-CCA.

---

## RSA-OAEP (Optimal Asymmetric Encryption Padding)

Transforms RSA into **randomized, non-malleable encryption**.

Parameters:
- Message length $n$
- Modulus length $k$
- Hash functions:
  $$
  G:\{0,1\}^{k_0} \to \{0,1\}^{n+k_1}
  $$
  $$
  H:\{0,1\}^{n+k_1} \to \{0,1\}^{k_0}
  $$

Encryption:
1. Sample random $R \in \{0,1\}^{k_0}$
2. Compute:
   $$
   B_0 = m \parallel 0^{k_1} \oplus G(R)
   $$
   $$
   B_1 = R \oplus H(B_0)
   $$
3. Set:
   $$
   A = B_0 \parallel B_1
   $$
4. Output:
   $$
   c = A^e \bmod N
   $$

Decryption:
1. Compute:
   $$
   A = c^d \bmod N
   $$
2. Split $A=(B_0,B_1)$
3. Recover:
   $$
   R = B_1 \oplus H(B_0)
   $$
   $$
   m = B_0 \oplus G(R)
   $$
4. Verify padding

Security:
- IND-CCA secure in the **Random Oracle Model**

---

### ✅ Practical OAEP Concept

**Question:**  
Why does OAEP make RSA randomized?

**Solution:**  
Because fresh randomness $R$ is sampled for every encryption.

---

## ElGamal Encryption

Let $G=\langle g\rangle \subseteq \mathbb Z_p^*$ of prime order $q$.

Keygen:
- Choose $a\in \mathbb Z_q$
- $pk = g^a$, $sk=a$

Encryption of $m\in\{0,1\}$:
1. Choose random $r$
2. Output:
   $$
   c=(g^r, pk^r\cdot g^m)
   $$

Decryption:
$$
h=c_1\cdot c_0^{-a}=g^m
$$

If $h=1$ output 0, else 1.

Security based on **discrete logarithm hardness**.

---

### ✅ Practical ElGamal Correctness

**Question:**  
Why does decryption recover $g^m$?

**Solution:**
$$
pk^r = g^{ar} \Rightarrow
c_1 \cdot c_0^{-a} = g^{ar+m}\cdot g^{-ar}=g^m
$$

---

## If Discrete Log Is Easy…

Then attacker can:
- Recover $a$ from $pk = g^a$
- Compute all secret randomness
- Fully break ElGamal

Thus security rests on **Discrete Log Problem (DLP)**.

---

## Hybrid Encryption (Real-World Use)

1. Generate random symmetric key $K$
2. Encrypt $K$ with RSA/ElGamal
3. Encrypt data with AEAD (AES-GCM, ChaCha20-Poly1305)

Why:
- RSA is slow
- Symmetric crypto is fast
- Provides both confidentiality & integrity

---

## Final Exam Key Takeaways (Week 7)

You must be able to:

- Define public-key encryption formally
- Explain trapdoor functions
- Perform RSA key generation, encryption, decryption
- Explain why textbook RSA is insecure
- State IND-CPA and IND-CCA for PKE
- Explain RSA-OAEP at a conceptual level
- Describe ElGamal encryption and correctness
- Explain role of discrete logarithms
- Explain why hybrid encryption is used

---

## Week 7 Cheat Sheet

**Public-key basics:**
- Key pair $(pk,sk)$: public encryption/verification, secret decryption/signing.
- Solves key-distribution bootstrapping problem.

**Trapdoor functions / RSA:**
- Trapdoor function: easy to compute, hard to invert without secret trapdoor, easy with trapdoor.
- RSA keygen: pick primes $p,q$; $N=pq$; $\varphi(N)=(p-1)(q-1)$; pick $e$ with $\gcd(e,\varphi(N))=1$; compute $d$ with $ed\equiv1\pmod{\varphi(N)}$.
- Encrypt: $c=m^e \bmod N$; Decrypt: $m=c^d \bmod N$ (for $m\in\mathbb Z_N^*$).

**Why textbook RSA is insecure:**
- Deterministic → not IND-CPA.
- Malleable: multiply ciphertext by $r^e$ to scale plaintext by $r$.
- Vulnerable to chosen-ciphertext/oracle-style attacks.

**Security notions for PKE:**
- IND-CPA: indistinguishability of $Enc(pk,m_0)$ vs $Enc(pk,m_1)$.
- IND-CCA: IND-CPA + decryption oracle for all ciphertexts except challenge.

**RSA-OAEP:**
- Adds randomized padding around $m$ using hash-based mask generation.
- Makes RSA encryption randomized and non-malleable (IND-CCA in ROM).

**ElGamal & assumptions:**
- Group $G=\langle g\rangle$ of prime order $q$.
- Keygen: secret $a$, public $g^a$.
- Enc: $(g^r, m\cdot (g^a)^r)$; Dec: divide out $(g^r)^a$.
- Security from DLP/CDH/DDH (typically DDH).

**Hybrid encryption:**
- Use PKE (RSA/ElGamal/LWE-based) to encrypt a random symmetric key.
- Use AEAD (AES-GCM, ChaCha20-Poly1305) for the bulk data.
- Combines PKE key-distribution with fast symmetric encryption.

### Extra Mini Questions (Practice)

1. **RSA arithmetic:** Given $p=13$, $q=17$, compute $N$ and $\varphi(N)$. Suggest a valid small $e$ and explain how you’d find $d$ conceptually.
2. **Textbook RSA & IND-CPA:** Design a simple IND-CPA adversary that breaks deterministic RSA encryption.
3. **RSA malleability:** Show algebraically how an attacker can transform a ciphertext of $m$ into a ciphertext of $3m\bmod N$ without knowing $m$.
4. **Hybrid intuition:** Why is it a bad idea to directly encrypt a large file with RSA instead of using hybrid encryption?

### True/False Practice – Week 7 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. In public-key encryption, anyone can use the public key $pk$ to encrypt, but only the holder of the secret key $sk$ should be able to decrypt. (**True**)
2. Trapdoor functions are easy to compute in the forward direction and easy to invert even without the trapdoor. (**False**)
3. In RSA, the public modulus $N$ is the product of two large primes $p$ and $q$, and security relies on the hardness of factoring $N$. (**True**)
4. Textbook RSA encryption $c=m^e \bmod N$ is deterministic and therefore cannot be IND-CPA secure. (**True**)
5. Because RSA is based on a trapdoor permutation, textbook RSA encryption is automatically IND-CCA secure. (**False**)
6. In textbook RSA, multiplying a ciphertext $c$ by $r^e \bmod N$ corresponds to multiplying the underlying plaintext by $r$ modulo $N$, which is an example of malleability. (**True**)
7. OAEP padding transforms RSA into a randomized encryption scheme whose IND-CCA security is proven in the Random Oracle Model. (**True**)
8. In RSA-OAEP, fresh randomness $R$ is sampled for every encryption, which ensures that encrypting the same plaintext twice yields different ciphertexts with overwhelming probability. (**True**)
9. Public-key encryption schemes are typically used directly to encrypt large data files because they are faster than symmetric-key encryption. (**False**)
10. Hybrid encryption uses PKE (like RSA or ElGamal) only to encrypt a random symmetric key, and then uses a fast symmetric AEAD scheme for the bulk data. (**True**)
11. ElGamal encryption over a DDH-hard group is IND-CPA secure but remains vulnerable to IND-CCA attacks due to malleability. (**True**)
12. ElGamal ciphertexts consist of a single group element, so ciphertext length always exactly matches the plaintext length. (**False**)
13. If the discrete logarithm problem becomes easy in a given group, then both ElGamal encryption and any DH-based key exchange in that group become insecure. (**True**)
14. IND-CPA security for PKE means an adversary with only encryption oracle access cannot distinguish $Enc(pk,m_0)$ from $Enc(pk,m_1)$ for chosen messages of equal length. (**True**)
15. IND-CCA security for PKE additionally gives the adversary access to a decryption oracle, but never on the challenge ciphertext itself. (**True**)
16. An RSA public key $(N,e)$ must satisfy $\gcd(e,\varphi(N))=1$ so that a multiplicative inverse $d$ modulo $\varphi(N)$ exists. (**True**)
17. If an attacker can factor $N$ into $p$ and $q$, they can compute $\varphi(N)$ and thus recover the secret exponent $d$. (**True**)
18. In a correctly implemented RSA-OAEP scheme, observing many ciphertexts allows an attacker to test guesses for plaintexts by simple algebraic manipulation of ciphertexts alone. (**False**)
19. Hybrid encryption with RSA + AEAD can provide both confidentiality and integrity for messages, assuming the AEAD scheme is IND-CCA secure. (**True**)
20. Using textbook RSA directly on messages (without padding) is safe in practice as long as the modulus $N$ is sufficiently large. (**False**)

#### Extra True/False – Week 7 Interactions

21. In a real-world TLS-like setting, RSA is typically used only for authentication or key transport, while symmetric ciphers provide the actual data encryption. (**True**)
22. If RSA-OAEP is IND-CCA secure, it is still safe to use it directly for very large files as long as the modulus $N$ is big enough. (**False**)
23. ElGamal encryption over an elliptic-curve group provides the same IND-CPA guarantees as classical ElGamal over $\mathbb Z_p^*$, assuming the corresponding DDH assumption holds on the curve. (**True**)
24. A public-key encryption scheme that is IND-CCA secure can be safely combined with an AEAD scheme in a hybrid construction without weakening overall security, assuming keys are used in disjoint roles. (**True**)
25. If an attacker can query a decryption oracle on arbitrary ElGamal ciphertexts except the challenge, they still cannot exploit ElGamal’s malleability to gain any advantage in the IND-CCA game. (**False**)
