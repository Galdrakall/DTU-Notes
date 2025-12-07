---
title: "Week 4 Notes – Block Ciphers & Modes"
week: 4
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/4, block-ciphers]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_3_Notes]]"
  next: "[[Week_5_Notes]]"
---

---
# Week 4 – Block Ciphers & Modes of Operation

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_3_Notes]] · Next: [[Week_5_Notes]]

---

## AES Overview

AES (Advanced Encryption Standard) is a **block cipher**:

- Block size: 128 bits.
- Key sizes: 128, 192, 256 bits.
- For each key $k$, AES defines a **permutation** $E_k$ with inverse $D_k$.
- Modeled as a **PRP**.

Old block ciphers:
- DES: 56-bit key, 64-bit block (insecure)
- 3DES: 168-bit key, 64-bit block (legacy)

Modern standard:
- AES: secure by all known practical attacks.

---

### ✅ Practical AES Facts

**Question:** Why is DES insecure today?  
**Solution:** Its 56-bit key can be brute-forced with modern hardware.

---

## AES Round Structure

AES-128 has:
- Initial **AddRoundKey**
- 9 full rounds
- 1 final round (no MixColumns)

Each full round:
1. **SubBytes** – nonlinear S-box
2. **ShiftRows** – row permutations
3. **MixColumns** – linear mixing
4. **AddRoundKey** – XOR with round key

State representation:
- 128-bit block = 16 bytes = $4\times4$ matrix.

Security intuition:
- SubBytes → **confusion**
- ShiftRows + MixColumns → **diffusion**
- AddRoundKey → inject secret material

---

### ✅ Practical AES Concept Question

**Question:** Which AES step provides non-linearity?  
**Solution:** SubBytes (S-box).

---

## Encrypting Long Messages

AES encrypts **only 128-bit blocks**.  
To encrypt long messages we use **modes of operation**.

---

## ECB Mode (Insecure)

Electronic Codebook (ECB):

$$
c_i = E_k(m_i)
$$

Properties:
- Deterministic
- Identical plaintext blocks → identical ciphertext blocks
- **Not IND-CPA secure**
- Leaks patterns (images remain visible)

ECB must **never** be used.

---

### ✅ Practical ECB Failure

**Question:** If two plaintext blocks are equal under ECB, what happens?  
**Solution:** Their ciphertext blocks are also equal → information leakage.

---

## CBC Mode (Cipher Block Chaining)

Encryption:

$$
c_0 = IV \\
c_i = E_k(m_i \oplus c_{i-1})
$$

Decryption:

$$
m_i = D_k(c_i) \oplus c_{i-1}
$$

Security conditions:
- IV must be **random and unpredictable**
- Same key + same IV must **never repeat**

With secure PRP + random IV → **IND-CPA secure**

---

### ✅ Practical CBC Computation

**Question:** If $m_1 = 0101$, $c_0 = 1111$, compute $m_1 \oplus c_0$.  
**Solution:**  
$0101 \oplus 1111 = 1010$ → input to block cipher.

---

## CTR Mode (Counter Mode)

Turns block cipher into a **stream cipher**.

Keystream:

$$
s_i = E_k(\text{nonce} \parallel i)
$$

Encryption:

$$
c_i = m_i \oplus s_i
$$

Decryption:

$$
m_i = c_i \oplus s_i
$$

Properties:
- Fully parallelizable
- Precomputation possible
- Requires **unique nonce per key**

---

### ✅ Practical CTR Encryption

**Question:**  
If $m=1011$, keystream $s=0110$, compute $c$.

**Solution:**

$$
c = 1011 \oplus 0110 = 1101
$$

---

## Nonce Reuse Attacks

### CTR Nonce Reuse

If the same nonce and key encrypt $m$ and $m'$:

$$
c \oplus c' = m \oplus m'
$$

Reveals xor of plaintexts → devastating leakage.

### CBC IV Reuse

IV reuse can:
- Break CPA security in some cases
- Enable subtle attacks if IV is predictable

**Rule:** Never reuse nonces/IVs under the same key.

---

### ✅ Practical Nonce Attack

**Question:** Why is nonce reuse as bad as OTP key reuse?  
**Solution:** Because CTR is OTP with AES-generated keystream.

---

## Security Definitions for Variable-Length Encryption

### Incorrect Security Definition

Allowing attacker to choose **different-length messages** trivially leaks which one was encrypted.

---

### Correct Left–Right (LR) Definition

Messages must satisfy:

$$
|m_0| = |m_1|
$$

Library encrypts either $m_0$ or $m_1$ and adversary must guess.

Security requires **indistinguishability**.

---

### Real–Random (RoR) Definition

REAL:
$$
c \leftarrow Enc_k(m)
$$

RANDOM:
$$
c \leftarrow C(|m|)
$$

Ciphertext must be indistinguishable from uniform.

CTR and CBC are proven secure under this model.

---

### ✅ Practical Security Definition Check

**Question:** Why must $|m_0|=|m_1|$ in IND-CPA?  
**Solution:** Otherwise the ciphertext length reveals which message was encrypted.

---

## CTR is IND-CPA Secure (Proof Intuition)

Key idea:
- Send counter values $r, r+1, \dots$
- With high probability, values never repeat
- AES behaves like random on fresh inputs
- XOR with random values hides plaintext

Thus CTR ≈ random encryption.

---

## Malleability & Chosen-Ciphertext Attacks (CCA)

CTR and CBC are **malleable**:

An attacker can flip bits:

$$
c' = c \oplus \Delta
$$

This causes:

$$
m' = m \oplus \Delta
$$

Attacker can **control plaintext modifications**.

---

### ✅ Practical Malleability Example

**Question:**  
If you flip one bit in CTR ciphertext, what happens to plaintext?

**Solution:**  
Exactly the same bit flips in plaintext.

---

## Why CPA Security Is Not Enough

CCA attacker can:
- Modify ciphertexts
- Submit modified ciphertexts for decryption
- Learn information via error messages
- Perform padding oracle attacks (CBC)

Real-world danger:
- Banking transactions
- Authentication tokens
- Encrypted cookies

---

## IND-CCA Security (Preview)

IND-CCA Game:
- Adversary has encryption oracle
- Also has **decryption oracle**
- Cannot decrypt the challenge ciphertext

Goal:
- Prevent malleability and active manipulation

Achieved using:
- Encrypt-then-MAC
- AEAD modes (e.g. GCM)

---

## AEAD Modes

Authenticated Encryption with Associated Data:

- **AES-GCM**
- **ChaCha20-Poly1305**

Provides:
- Confidentiality
- Integrity
- Authentication
- Non-malleability

Associated data:
- Headers, metadata
- Authenticated but not encrypted

---

### ✅ Practical AEAD Question

**Question:** Why is AEAD better than plain CTR?  
**Solution:** CTR gives confidentiality only; AEAD adds integrity and authentication.

---

## Final Exam Key Takeaways (Week 4)

You must be able to:

- Describe AES at a high level
- Explain SubBytes, ShiftRows, MixColumns, AddRoundKey
- State why ECB is insecure
- Write CBC and CTR encryption equations
- Explain nonce/IV reuse attacks
- Distinguish:
  - Left-Right vs Real-Random definitions
  - IND-CPA vs IND-CCA
- Explain malleability
- State why AEAD is needed

---

## Week 4 Cheat Sheet

**Block ciphers:**
- AES: 128-bit block; keys 128/192/256; modeled as PRP.
- DES: 56-bit key, brute-forceable today.

**AES rounds (128-bit):**
- Initial AddRoundKey, 9 full rounds, final round.
- SubBytes (nonlinear S-box) → confusion.
- ShiftRows + MixColumns → diffusion.
- AddRoundKey → injects key material.

**Modes of operation:**
- ECB: $c_i = E_k(m_i)$ → deterministic, pattern leakage, not IND-CPA.
- CBC: $c_0 = IV$, $c_i = E_k(m_i \oplus c_{i-1})$.
- CTR: keystream $s_i = E_k(\text{nonce} \parallel i)$, $c_i = m_i \oplus s_i$.

**IV / nonce rules:**
- CBC: IV must be random/unpredictable; never reuse IV with same key.
- CTR: nonce must be unique per key; reuse gives $c \oplus c' = m \oplus m'$.

**Security notions (variable length):**
- LR (left–right): require $|m_0| = |m_1|$; otherwise length leaks bit.
- RoR: ciphertext of $m$ vs random string of same length.

**Malleability:**
- In CBC/CTR, flipping bits in $c$ flips corresponding bits in $m$.
- CPA security alone does not prevent chosen-ciphertext attacks.

**AEAD:**
- AEAD (e.g. AES-GCM, ChaCha20-Poly1305) = encryption + integrity + authenticity.
- Authenticated associated data (AAD): authenticated but not encrypted.

### Extra Mini Questions (Practice)

1. **ECB pattern leakage:** Describe a real-world scenario (e.g. image encryption) where ECB visibly leaks structure. What does the attacker learn?
2. **CBC vs CTR:** For a high-throughput parallel system, which mode is more convenient and why? Mention both security and performance.
3. **Nonce/IV reuse:** Give a 2–3 sentence argument why CTR with nonce reuse effectively becomes OTP with key reuse.
4. **Malleability:** Suppose a banking protocol sends amount and account number under CTR. How could an attacker exploit malleability if there is no MAC?

### True/False Practice – Week 4 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. AES is modeled as a pseudorandom permutation (PRP) on 128-bit blocks for each fixed key. (**True**)
2. DES is considered insecure today mainly because its 56-bit key can be brute-forced with modern hardware. (**True**)
3. In ECB mode, encrypting identical plaintext blocks under the same key results in identical ciphertext blocks. (**True**)
4. ECB mode is IND-CPA secure as long as the underlying block cipher is a secure PRP. (**False**)
5. In CBC mode, the IV can be a fixed public constant because it is XORed with the first plaintext block, which already randomizes the input. (**False**)
6. CBC with a random, unpredictable IV and a secure PRP is IND-CPA secure. (**True**)
7. In CTR mode, the ciphertext length is always equal to the plaintext length, because encryption is just XOR with a keystream. (**True**)
8. Reusing a nonce in CTR mode under the same key can leak $m \oplus m'$ from $c \oplus c'$, making it analogous to OTP key reuse. (**True**)
9. In CTR mode, using a predictable but unique counter/nonce sequence (e.g., incrementing a counter) is acceptable for IND-CPA security. (**True**)
10. In CBC mode, reusing the same IV with the same key on different messages never threatens CPA security. (**False**)
11. The Left–Right (LR) IND-CPA definition requires that the two challenge messages have the same length to prevent trivial wins based on ciphertext size. (**True**)
12. In the Real–Random (RoR) definition for variable-length encryption, the random library returns a uniformly random bitstring of the same length as $Enc_k(m)$. (**True**)
13. CTR mode encryption is inherently non-malleable because any bit flip in the ciphertext causes decryption to fail completely. (**False**)
14. In both CBC and CTR, flipping a bit in the ciphertext results in a predictable flip in (part of) the decrypted plaintext, which is an example of malleability. (**True**)
15. IND-CPA security is sufficient to protect protocols from all chosen-ciphertext attacks in practice. (**False**)
16. AEAD schemes like AES-GCM provide confidentiality, integrity, and authentication in a single primitive. (**True**)
17. Using AEAD with associated data (AAD) ensures that the AAD is both encrypted and authenticated. (**False**)
18. An IND-CCA attacker has access to both encryption and decryption oracles, with the restriction that it cannot query the decryption oracle on the challenge ciphertext. (**True**)
19. Plain CTR mode without any MAC can be IND-CCA secure if the nonce is never reused. (**False**)
20. Encrypt-then-MAC over a CPA-secure encryption scheme and EUF-CMA-secure MAC can yield an IND-CCA-secure authenticated encryption scheme. (**True**)

#### Extra True/False – Week 4 Interactions

21. ECB, CBC, and CTR constructed from a secure PRP with appropriate randomness assumptions are all IND-CPA secure. (**False**)
22. In CBC, padding oracle attacks are possible because decryption error behavior can depend on the structure of the decrypted plaintext. (**True**)
23. AEAD modes like AES-GCM are typically designed to achieve both IND-CPA and integrity-of-ciphertexts (INT-CTXT), which together imply IND-CCA. (**True**)
24. In CTR mode, if an attacker knows part of the plaintext, they can directly recover the corresponding keystream segment. (**True**)
25. If a scheme is AEAD-secure, then ciphertext malleability is limited to changes that preserve MAC validity, which should be impossible without the key. (**True**)

##### Extra True/False – Week 4 Advanced

26. A block-cipher mode that is IND-CPA secure but does not authenticate ciphertexts can safely be used for messages that are never processed or acted upon by the receiver. (**False**)
27. Encrypt-then-MAC with a deterministic MAC (e.g., PRF-MAC) can still achieve IND-CCA security, even though the MAC output itself is deterministic. (**True**)
28. In AEAD schemes like AES-GCM, reusing a nonce with the same key is primarily a confidentiality problem and does not affect integrity. (**False**)
29. For variable-length messages, using CTR with a random nonce and a separate MAC over the ciphertext is functionally equivalent (from a security point of view) to using an AEAD scheme instantiated with CTR + MAC internally. (**True**)
30. If the underlying block cipher used in CBC or CTR mode is broken as a PRP, then IND-CPA and IND-CCA security of any scheme using it are also at risk. (**True**)
