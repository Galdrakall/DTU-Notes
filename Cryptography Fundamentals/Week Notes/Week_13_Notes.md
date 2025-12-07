---
title: "Week 13 Notes – Final Exam Meta-Topics & Glue Concepts"
week: 13
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/13, hmac, kdf, pki, side-channels, kem-dem, rom]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_12_Notes]]"
---

# Week 13 – Final Exam Meta-Topics & Glue Concepts

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_12_Notes]]

This week consolidates **composition rules, deployment models, real-world pitfalls, and proof models** that commonly appear as **trick or conceptual exam questions**.

---

## Deterministic vs Randomized Encryption

### Deterministic Encryption
- Same plaintext + same key → always same ciphertext
- Leaks equality information
- **Cannot be IND-CPA secure**

Examples:
- Textbook RSA
- Using PRFs directly as encryption: $c = F(k,m)$

---

### Randomized Encryption
- Uses randomness $r$:
$$
c \leftarrow Enc(k,m;r)
$$
- Same message encrypts to **different ciphertexts each time**
- Required for **IND-CPA security**

All modern secure encryption is:
- Randomized (CTR, CBC, OAEP)
- Or stateful (stream ciphers with counters)

---

### ✅ Exam Rule

> **All IND-CPA secure encryption schemes must be randomized or stateful.**

---

## Composition of Encryption & Authentication

Let:
- $Enc$ = CPA-secure encryption
- $MAC$ = EUF-CMA secure MAC

We have three natural compositions:

---

### ✅ Encrypt-then-MAC (SECURE)

1. $c = Enc(k_e,m)$  
2. $t = MAC(k_m,c)$  
3. Output $(c,t)$  

Verify **before** decrypting.

This construction is:
- IND-CPA
- IND-CCA
- Non-malleable

Used in practice:
- IPsec
- AEAD designs (conceptually)

---

### ❌ MAC-then-Encrypt (INSECURE)

1. $t = MAC(k,m)$  
2. $c = Enc(k,m\parallel t)$  

Attack:
- Padding oracle
- Decryption error reveals MAC validity
- Leads to CCA attacks

Used historically in:
- Old TLS/SSL → broken

---

### ⚠️ Encrypt-and-MAC (SUBTLE)

1. $c = Enc(k_e,m)$  
2. $t = MAC(k_m,m)$  

Ciphertext integrity is **not covered** by MAC.

Tampering with $c$ may still pass MAC.

Not generically secure.

---

### ✅ Exam Takeaway

> **Only Encrypt-then-MAC is generically IND-CCA secure.**

---

## Key Derivation Functions (KDFs)

A **KDF** extracts cryptographically strong keys from:
- Shared secrets
- Diffie–Hellman outputs
- Noisy material

Formal goal:
$$
KDF(x) \approx U_{\lambda}
$$
even if $x$ is biased or structured.

---

### Typical Uses
- TLS session key derivation
- Signal ratchet keys
- X3DH output expansion
- Password stretching

---

### Why PRFs Work as KDFs

If $F$ is a secure PRF:

$$
K = F(k,x)
$$

Then $K$ is:
- Pseudorandom
- Independent of structure in $x$

---

### ✅ Exam Insight

> **You never use raw DH outputs directly as keys — always through a KDF.**

---

## Hybrid Encryption & KEM–DEM Paradigm

Modern public-key encryption is almost never “pure” RSA/ElGamal.

Instead:

### KEM – Key Encapsulation Mechanism
- Public-key crypto encrypts a **random symmetric key**

### DEM – Data Encapsulation Mechanism
- Symmetric AEAD encrypts the actual data

---

### Full Hybrid Construction

1. $(K,c_1) \leftarrow KEM(pk)$  
2. $c_2 \leftarrow AEAD.Enc(K,m)$  
3. Output $(c_1,c_2)$  

Security reduces to:
- KEM IND-CPA/CCA
- DEM IND-CCA

---

### ✅ Exam Rule

> **All real systems use hybrid encryption because public-key crypto is slow and malleable.**

---

## Public-Key Infrastructure (PKI)

PKI solves the problem:

> “How do I know this public key actually belongs to Alice?”

---

### Certificate Authority (CA)

A CA:
- Has a well-known trusted public key
- Signs:
$$
Cert = Sign_{CA}(pk_{Alice}, ID_{Alice})
$$

Anyone verifying:
- Checks CA signature
- Then trusts $pk_{Alice}$

---

### Certificate Chains

Root CA → Intermediate CA → End-entity certificate

Trust flows downward.

---

### Revocation

If a key is compromised:
- CRL (Certificate Revocation List)
- OCSP (Online Certificate Status Protocol)

---

### Trust on First Use (TOFU)

Used in:
- SSH
- Signal contact verification

First connection is trusted; changes trigger warnings.

---

### ✅ Exam Warning

> PKI failures cause **authentication failures**, not cryptographic failures.

---

## Random Oracle Model vs Standard Model

### Random Oracle Model (ROM)
- Hash function is treated as:
$$
H: \{0,1\}^* \to \{0,1\}^\lambda
$$
with truly random outputs.
- Everyone has oracle access.
- Used in proofs of:
  - RSA-FDH
  - RSA-PSS
  - OAEP

---

### Standard Model
- Hash is a **concrete algorithm**
- No idealization
- Much harder to prove security

---

### Key Difference

| ROM | Standard Model |
|-----|----------------|
| Proofs easier | Proofs harder |
| Strong but idealized | Realistic |
| May break when instantiated | Fully sound |

---

### ✅ Exam Insight

> **ROM security does not guarantee real-world security, but it gives strong evidence.**

---

## Hash-Based MACs – HMAC

Raw Merkle–Damgård hashes are **not secure MACs**:

$$
MAC(k,m) = H(k\parallel m)
$$
is vulnerable to **length-extension attacks**.

---

### HMAC Construction

$$
HMAC(k,m) = H((k \oplus opad)\parallel H((k \oplus ipad)\parallel m))
$$

This:
- Breaks length extension
- Is provably secure under ROM and PRF assumptions
- Is used everywhere:
  - TLS
  - IPsec
  - APIs

---

### ✅ Exam Rule

> **Never use $H(k\parallel m)$ directly as a MAC. Use HMAC.**

---

## Side-Channel Attacks

These exploit **implementation leakage**, not math.

---

### Common Types

- Timing attacks
- Power analysis
- Cache attacks
- Fault injection
- Electromagnetic leakage

These can:
- Extract AES keys
- Extract RSA private exponents
- Break secure hardware

---

### Not Prevented By:
- IND-CPA
- IND-CCA
- EUF-CMA

These are **out-of-model attacks**.

---

### Mitigations

- Constant-time code
- Blinding
- Masking
- Hardware isolation

---

### ✅ Exam Insight

> Side-channel attacks bypass **mathematical security proofs**.

---

## Tight vs Loose Security Reductions

In a reduction, we show:

> If attacker breaks scheme with advantage $\epsilon$, then we can solve the hard problem with probability $f(\epsilon)$.

---

### Tight Reduction
$$
\epsilon' \approx \epsilon
$$
Security loss is minimal.

### Loose Reduction
$$
\epsilon' \ll \epsilon
$$
Concrete security degrades significantly.

---

### Why This Matters

Loose reductions force us to:
- Use larger parameters
- Over-engineer security margins

---

### ✅ Exam Insight

> Two schemes with identical asymptotic security may have very different **real-world security**.

---

## Randomness Failures

Cryptography **assumes good randomness**.

Failures cause catastrophic breaks:

- Debian OpenSSL bug → predictable RSA keys
- Android Bitcoin RNG bug → stolen private keys
- Reused nonces in ECDSA → private key recovery

---

### ✅ Exam Rule

> **Bad randomness completely destroys cryptographic security.**

---

## Final Master Checklist (Weeks 1–13)

You should now be able to:

- Prove IND-CPA and IND-CCA by game hopping
- Identify malleability instantly
- Construct secure MACs
- Explain AEAD security composition
- Perform RSA, ElGamal, Regev, and DH computations
- Explain X3DH + Double Ratchet
- Describe quantum threats precisely
- Apply Mosca’s theorem
- Explain PKI and hybrid encryption
- Distinguish ROM vs Standard Model
- Identify side-channel vs cryptographic attacks

---

## Final Exam Meta-Rule

> If a construction is:
> - Deterministic  
> - Malleable  
> - Uses unauthenticated encryption  
> - Reuses nonces  
> - Uses raw hashes as MACs  
>
> Then it is **almost certainly insecure**.

---

## Week 13 Cheat Sheet

**Deterministic vs randomized encryption:**
- Deterministic: same $(k,m)$ → same $c$; leaks equality; cannot be IND‑CPA.
- Randomized/stateful: includes randomness/nonce; required for IND‑CPA.

**Auth + encryption composition:**
- Encrypt‑then‑MAC (EtM): $c=Enc(k_e,m)$, $t=MAC(k_m,c)$ → verify before decrypt; generically IND‑CCA.
- MAC‑then‑Encrypt: $t=MAC(m)$, encrypt $(m\|t)$ → can leak MAC validity via padding/oracle errors; unsafe.
- Encrypt‑and‑MAC: encrypt $m$ and MAC $m$ separately; subtle and not generically safe.

**KDFs and KEM–DEM:**
- KDFs turn structured/biased secrets (e.g. DH outputs) into pseudorandom keys.
- Hybrid (KEM–DEM) = KEM for key, DEM (AEAD) for data; used everywhere (TLS, PGP, Signal).

**PKI basics:**
- CAs sign bindings between identities and public keys (certificates).
- Certificate chains, revocation (CRL/OCSP), and TOFU are typical trust models.
- Failures mainly cause **authentication** problems, not math breaks.

**ROM vs standard model:**
- ROM: hash modeled as random oracle; simplifies proofs (RSA‑FDH, OAEP, HMAC).
- Standard model: no idealization; proofs harder but closer to reality.

**HMAC vs raw hash MACs:**
- $MAC(k,m)=H(k\|m)$ insecure with MD hashes (length extension).
- HMAC construction avoids this and is widely deployed.

**Side channels:**
- Exploit timing, power, cache, EM, or faults.
- Outside standard IND‑CPA / IND‑CCA / EUF‑CMA models; defeated via constant‑time code, blinding, masking, and hardened hardware.

**Reductions & parameters:**
- Tight reductions give good concrete security; loose reductions may require larger parameter sizes.
- Asymptotic “security” alone is not enough; check concrete attack costs.

**Randomness quality:**
- Poor RNGs (low entropy, reuse, predictability) can completely destroy otherwise secure schemes.

### Extra Mini Questions (Practice)

1. **Composition check:** Given a system that encrypts with AES‑CTR and then appends HMAC over the ciphertext, identify the composition pattern and its expected security.
2. **ROM subtlety:** Give an example of why a scheme proven secure in ROM might still be broken when instantiated with a real hash function.
3. **Side‑channel scenario:** Describe a timing side‑channel against RSA decryption and one mitigation.
4. **Hybrid reasoning:** Explain why directly encrypting data with RSA instead of a KEM–DEM hybrid is both inefficient and risky.
