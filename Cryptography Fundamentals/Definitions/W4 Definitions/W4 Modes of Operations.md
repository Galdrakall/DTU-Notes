
This sheet compares the most important **block cipher modes of operation** used in symmetric cryptography.
It is intended for:
- Fast exam revision
- Security property comparison
- Attack pattern recognition

Applies to block ciphers modeled as PRPs (e.g., AES).

---

## 1. Quick Comparison Table

| Mode | Randomized? | CPA-Secure? | Integrity? | Parallel Encryption | Parallel Decryption | Safe to Use? |
|------|-------------|-------------|------------|----------------------|----------------------|---------------|
| ECB  | No          | No          | No         | Yes                  | Yes                  | Never         |
| CBC  | Yes (IV)    | Yes         | No         | No                   | Yes                  | Yes (with care) |
| CTR  | Yes (nonce) | Yes         | No         | Yes                  | Yes                  | Yes (with care) |
| CFB  | Yes (IV)    | Yes         | No         | No                   | No                   | Rare today |
| OFB  | Yes (IV)    | Yes         | No         | No                   | Yes                  | Rare today |
| GCM  | Yes (nonce) | Yes         | Yes        | Yes                  | Yes                  | Yes (recommended) |

Only **AEAD modes (e.g., GCM)** provide both:
- Confidentiality
- Integrity/authentication

---

## 2. General Mode Structure

Let:
- $E_k$ be a block cipher (PRP)
- $D_k$ its inverse
- $m_1, \dots, m_\ell$ message blocks
- $c_1, \dots, c_\ell$ ciphertext blocks

A **mode of operation** defines:
- How randomness (IV/nonce) is used
- How blocks depend on each other
- How encryption chains across blocks

---

## 3. ECB – Electronic Codebook (Baseline – Insecure)

### Encryption
$$
c_i = E_k(m_i)
$$

### Properties
- Deterministic
- Same plaintext block → same ciphertext block
- No randomness

### Security
- Not IND-CPA secure
- Leaks equality and structure

### Main Failure
Pattern leakage (images, formatted data, repeated headers).

### Verdict
Never use ECB under any circumstances.

---

## 4. CBC – Cipher Block Chaining

### Encryption
$$
c_0 = \text{IV}
$$
$$
c_i = E_k(m_i \oplus c_{i-1})
$$

### Decryption
$$
m_i = D_k(c_i) \oplus c_{i-1}
$$

### Properties
- Randomized via IV
- Each block depends on previous ciphertext
- Hides repeated plaintext blocks

### Security
- IND-CPA secure **if IV is random and never reused**
- No integrity protection

### Weaknesses
- IV reuse leaks information
- Vulnerable to padding-oracle attacks
- Bit-flipping attacks possible

### Verdict
Secure for confidentiality only, if used carefully.

---

## 5. CTR – Counter Mode

### Keystream
$$
K_i = E_k(\text{ctr} + i)
$$

### Encryption
$$
c_i = m_i \oplus K_i
$$

### Decryption
$$
m_i = c_i \oplus K_i
$$

### Properties
- Turns block cipher into a stream cipher
- Fully parallelizable
- No block chaining

### Security
- IND-CPA secure **if counter/nonce never repeats**
- No integrity protection

### Catastrophic Failure (Nonce Reuse)
If same keystream $K$ is reused:
$$
c_1 \oplus c_2 = m_1 \oplus m_2
$$

Same failure as OTP key reuse.

### Verdict
Fast, secure for confidentiality only, but very fragile under misuse.

---

## 6. GCM – Galois/Counter Mode (AEAD)

### Encryption Core
Uses CTR mode internally:
$$
c_i = m_i \oplus E_k(\text{ctr} + i)
$$

### Authentication
Adds a polynomial MAC over ciphertext blocks:
$$
\text{Tag} = \text{GHASH}(H, A, C)
$$

where:
- $H = E_k(0^{128})$
- $A$ = associated data
- $C$ = ciphertext

### Properties
- Confidentiality + integrity (AEAD)
- Fully parallelizable
- Widely used in TLS, IPsec, disk encryption

### Security
- IND-CPA + INT-CTXT
- Nonce must never repeat

### Verdict
Modern recommended standard for symmetric encryption.

---

## 7. CFB and OFB (Legacy Stream-like Modes)

### CFB (Cipher Feedback)

Encryption:
$$
c_i = m_i \oplus E_k(c_{i-1})
$$

- Sequential
- Error propagation
- Rare in new designs

### OFB (Output Feedback)

Keystream:
$$
K_i = E_k(K_{i-1})
$$
$$
c_i = m_i \oplus K_i
$$

- Similar to CTR but slower
- Requires careful IV handling
- Rare today

---

## 8. Deterministic vs Randomized Encryption

| Type | Uses Randomness? | CPA-Secure? |
|------|------------------|-------------|
| Deterministic (ECB) | No | No |
| Randomized (CBC, CTR, GCM) | Yes | Yes |

**Theorem:** Deterministic encryption can never achieve IND-CPA security.

---

## 9. Security Properties by Mode

Let:
- IND-CPA = confidentiality under chosen-plaintext attack
- INT-CTXT = ciphertext integrity

| Mode | IND-CPA | INT-CTXT | AEAD |
|------|----------|-----------|------|
| ECB  | No       | No        | No   |
| CBC  | Yes      | No        | No   |
| CTR  | Yes      | No        | No   |
| GCM  | Yes      | Yes       | Yes  |

Only **AEAD modes** (like GCM) provide full modern security.

---

## 10. Common Exam Traps

- “ECB is fine if messages are random.” → False  
- “CBC gives integrity.” → False  
- “CTR is secure if reused once.” → False  
- “Nonce equals key.” → False  
- “Block cipher = secure encryption.” → False without a mode  
- “IV can be secret.” → False (it must be unpredictable, not secret)

---

## 11. Mode Selection Guidelines

| Use Case | Correct Choice |
|----------|----------------|
| Modern network encryption | GCM |
| Disk encryption | XTS (specialized mode) |
| Legacy systems | CBC with Encrypt-then-MAC |
| High-performance parallel encryption | CTR + MAC |
| Anything with reuse risk | GCM with strict nonce management |

---

## 12. Final Exam Cheat Summary

- ECB: deterministic, insecure, never use.
- CBC: randomized, CPA-secure, no integrity.
- CTR: stream-cipher-like, CPA-secure, fragile under nonce reuse.
- GCM: CTR + authentication, modern standard.
- Any secure system must use:
  - Randomized encryption
  - Unique nonces/IVs
  - Integrity protection
