---
title: "Week 11 Notes – Secure Messaging & Signal"
week: 11
course: "Cryptography Fundamentals"
tags: [cryptography, course/cryptography, week/11, signal, secure-messaging]
links:
  index: "[[Cryptography Fundamentals]]"
  prev: "[[Week_10_Notes]]"
  next: "[[Week_12_Notes]]"
---
---
# Week 11 – Secure Messaging, X3DH & Signal Protocol

> Index: [[Cryptography Fundamentals]]  
> Previous: [[Week_10_Notes]] · Next: [[Week_12_Notes]]

---

## Forward Secrecy (FS)

A protocol has **forward secrecy** if:

> Compromise of long-term keys does **not** reveal *past* session keys.

Meaning:
- Even if an attacker later steals your private key,
- Previously recorded encrypted traffic **remains secure**.

Achieved via:
- **Ephemeral Diffie–Hellman** keys
- Session keys erased after use

Formally:
- Session key $K_i$ must be statistically/computationally independent of long-term key once erased.

---

### ✅ Practical FS Question

**Question:**  
If an attacker records all traffic today and steals your RSA private key in one year, what protects your old messages?

**Solution:**  
Forward secrecy — if session keys were derived from ephemeral DH and erased.

---

## Post-Compromise Security (PCS)

**Post-Compromise Security** means:

> If an attacker compromises your device *now*, but later loses access, the protocol will eventually recover so that **future messages become secure again**.

This is critical for:
- Secure messaging apps
- Long-running conversations

Achieved by:
- Periodic **key updates**
- Continuous re-mixing of fresh DH material into session keys

---

### ✅ Practical PCS Question

**Question:**  
If malware reads your current session key but is removed tomorrow, what property ensures future messages are secure?

**Solution:**  
Post-compromise security.

---

## Key Ratcheting

A **ratchet** updates keys so that:
- Old keys are deleted
- New keys are derived continuously

### Symmetric Ratchet
Uses a hash or KDF:
$$
K_{i+1} = H(K_i)
$$

Provides:
- Forward secrecy
- But **no new entropy**

---

### Diffie–Hellman Ratchet
Uses fresh DH keys:
$$
K \leftarrow DH(a,b)
$$

Provides:
- Fresh entropy
- Post-compromise recovery

---

### Double Ratchet (Signal)

Combines:
- Symmetric ratchet
- DH ratchet

Result:
- Each message uses a new key
- Strong help for both **FS and PCS**

---

## Asynchronous Secure Messaging Problem

In messaging systems (Signal, WhatsApp, etc.):

- Bob may be offline
- Alice still wants to send encrypted messages
- They still must:
  - Authenticate each other
  - Derive a shared secret
  - Achieve FS and PCS

This motivates **X3DH**.

---

## Key Types in Signal

Signal uses **four different types of keys**:

1. **Identity Keys** $(IK_A, IK_B)$  
   - Long-term authenticated keys  
   - Used for identity verification

2. **Signed Pre-Keys** $(SPK_A, SPK_B)$  
   - Semi-permanent  
   - Signed using identity keys  
   - Provide authentication while offline

3. **One-Time Pre-Keys** $(OPK_{B,i})$  
   - Used at most once  
   - Improve forward secrecy

4. **Ephemeral Keys** $(EK_A, EK_B)$  
   - Fresh per session  
   - Provide fresh entropy

---

### ✅ Practical Key-Type Question

**Question:**  
Which key type in Signal provides *asynchronous authentication*?

**Solution:**  
The signed pre-key (SPK).

---

## Extended Triple Diffie–Hellman (X3DH)

X3DH establishes the **initial shared secret** between Alice and Bob.

Alice retrieves Bob’s:
- Identity key $IK_B$
- Signed pre-key $SPK_B$
- One-time pre-key $OPK_{B,i}$ (if available)

Alice generates:
- Ephemeral key pair $(EK_A)$

---

### Core Diffie–Hellman Computations

Alice computes:

$$
k_1 = DH(IK_A, SPK_B)
$$
$$
k_2 = DH(EK_A, IK_B)
$$
$$
k_3 = DH(EK_A, SPK_B)
$$

If one-time pre-key exists:

$$
k_4 = DH(EK_A, OPK_{B,i})
$$

Then derives session secret:

$$
K = KDF(k_1 \parallel k_2 \parallel k_3 \parallel k_4)
$$

Bob computes the same values when receiving Alice’s message.

---

### ✅ Practical X3DH Question

**Question:**  
Why are *multiple* DH computations used instead of one?

**Solution:**  
To combine:
- Identity authentication,
- Ephemeral secrecy,
- Offline capability,
- And stronger forward secrecy.

---

## What Security Does X3DH Provide?

- ✅ Mutual authentication (via identity + signed pre-keys)
- ✅ Forward secrecy (via ephemeral keys)
- ✅ Asynchronous key establishment
- ✅ Resistance to server compromise
- ✅ Key compromise *before* session ≠ message decryption

Limitations:
- X3DH alone does **not** give post-compromise security → this requires the **Double Ratchet**.

---

## From X3DH to Secure Messaging

Pipeline in Signal-like systems:

1. Use **X3DH** → establish initial secret $K$
2. Feed $K$ into **Double Ratchet**
3. Each message:
   - Uses a fresh derived key
   - Is encrypted with **AEAD** (e.g., AES-GCM, ChaCha20-Poly1305)

This ensures:
- Confidentiality
- Integrity
- Authentication
- Forward secrecy
- Post-compromise security
- Deniability

---

## Forward vs Post-Compromise Security (Timeline Intuition)

Let:
- Attacker learns key at time $t$

| Property | Protects |
|----------|----------|
| Forward secrecy | Messages **before** time $t$ |
| Post-compromise security | Messages **after recovery** from $t$ |

Without ratcheting:
- Learning one key breaks **all past and future messages**

With Signal:
- Only a limited window is exposed

---

## Deniability in Secure Messaging

Signal avoids permanent cryptographic proof:

- Messages are authenticated with **MACs**
- Keys are shared between parties
- Either party could have forged the transcript

Thus:
> Transcripts cannot be used as cryptographic proof of authorship.

This is different from:
- Email with digital signatures
- Blockchain transactions

---

### ✅ Practical Deniability Question

**Question:**  
Why not sign every message with a digital signature?

**Solution:**  
Because that would remove deniability — transcripts would become legal proof of authorship.

---

## Diffie–Hellman in TLS vs Signal

### TLS (Synchronous)

- Both parties online
- Certificate authentication
- Ephemeral DH → forward secrecy
- No need for pre-keys

### Signal (Asynchronous)

- Receiver may be offline
- Uses pre-published signed keys
- Uses Double Ratchet for PCS

---

## Threat Model in Secure Messaging

Attacker may:

- Record all traffic
- Compromise devices temporarily
- Control the server
- Attempt MITM at first contact

Security goals:

- Confidentiality
- Integrity
- Authentication
- Forward secrecy
- Post-compromise security
- Deniability

---

## Common Attacks & Defenses

| Attack | Defense |
|--------|---------|
| MITM during setup | Identity + signed pre-keys |
| Server key substitution | Signature verification |
| Device compromise | Ratcheting |
| Traffic recording | Forward secrecy |
| Message tampering | AEAD |

---

## Final Exam Key Takeaways (Week 11)

You must be able to:

- Define **forward secrecy** and **post-compromise security**
- Explain why secure messaging needs **both**
- Describe:
  - Identity keys
  - Signed pre-keys
  - One-time pre-keys
  - Ephemeral keys
- Write the **X3DH KDF input components**
- Explain why X3DH works when Bob is offline
- Explain the role of the **Double Ratchet**
- Explain what **deniability** means and why Signal provides it
- Compare **TLS vs Signal** key establishment

---

## Week 11 Cheat Sheet

**Forward secrecy (FS):**
- Past messages stay secure even if long-term keys are compromised later.
- Achieved via ephemeral DH keys and deletion of old session keys.

**Post-compromise security (PCS):**
- After a compromise is fixed, protocol “heals” so future messages become secure again.
- Requires continual key updates (ratcheting) with fresh entropy.

**Ratcheting:**
- Symmetric ratchet: $K_{i+1} = H(K_i)$ (FS only, no new entropy).
- DH ratchet: periodic new DH exchanges → fresh entropy and PCS.
- Double Ratchet (Signal): combines both for per-message keys.

**Signal key types:**
- Identity keys (IK): long-term identity/authentication.
- Signed pre-keys (SPK): medium term, signed by IK, server-stored.
- One-time pre-keys (OPK): single use, improve FS.
- Ephemeral keys (EK): fresh per session.

**X3DH (initial handshake):**
- Uses multiple DHs: $DH(IK_A,SPK_B)$, $DH(EK_A,IK_B)$, $DH(EK_A,SPK_B)$, and optionally $DH(EK_A,OPK_B)$.
- KDF over concatenated DH outputs → initial shared secret.
- Works asynchronously; Bob can be offline.

**From X3DH to Double Ratchet:**
- X3DH output feeds into Double Ratchet as initial root key.
- Each message uses a unique key → strong FS and PCS.

**Deniability:**
- Messages authenticated with shared keys/MACs, not public signatures.
- Either party could have produced a transcript, so no cryptographic proof for third parties.

**TLS vs Signal (high level):**
- TLS: synchronous, server authenticated via PKI, ephemeral DH for FS, no PCS.
- Signal: asynchronous, pre-keys + Double Ratchet for FS and PCS, deniability built in.

### Extra Mini Questions (Practice)

1. **FS vs PCS:** Give a short scenario where a protocol has FS but not PCS, and explain what an attacker can still do.
2. **X3DH components:** List which DH combinations in X3DH provide (a) identity authentication and (b) forward secrecy.
3. **Deniability vs signatures:** Why would using per-message digital signatures in Signal harm deniability?
4. **TLS/Signal contrast:** Explain one advantage TLS has over Signal’s approach, and one advantage Signal’s design has over TLS, in terms of their threat models.

### True/False Practice – Week 11 Definitions

Mark each statement as **True** or **False** and be ready to justify:

1. Forward secrecy ensures that compromise of long-term keys today does not reveal past session keys. (**True**)
2. Post-compromise security guarantees that once an attacker has compromised a device, all future messages remain permanently compromised. (**False**)
3. A purely symmetric ratchet $K_{i+1}=H(K_i)$ can provide forward secrecy but cannot by itself provide post-compromise security. (**True**)
4. A DH ratchet that regularly mixes in fresh DH outputs can help a protocol regain security after a compromise is removed (PCS). (**True**)
5. The Signal Double Ratchet combines both symmetric and DH ratchets to achieve per-message forward secrecy and post-compromise security. (**True**)
6. X3DH alone, without the Double Ratchet, already gives post-compromise security for long conversations. (**False**)
7. In Signal, identity keys (IK) are long-term keys used mainly for authenticating users. (**True**)
8. Signed pre-keys (SPK) in Signal allow asynchronous authentication when the receiver is offline. (**True**)
9. One-time pre-keys (OPK) in Signal are used at most once and strengthen forward secrecy. (**True**)
10. Ephemeral keys (EK) in X3DH provide fresh entropy for each new session establishment. (**True**)
11. X3DH uses multiple DH computations (e.g., $DH(IK_A,SPK_B)$, $DH(EK_A,IK_B)$, $DH(EK_A,SPK_B)$, $DH(EK_A,OPK_B)$) to combine authentication and forward secrecy. (**True**)
12. Deniability in Signal arises partly because messages are authenticated using shared symmetric keys/MACs rather than long-term public signatures. (**True**)
13. If every Signal message were signed with a static digital signature key, chat transcripts could become strong cryptographic evidence of authorship, undermining deniability. (**True**)
14. TLS with ephemeral ECDHE provides forward secrecy but not post-compromise security, since there is typically no ongoing ratchet. (**True**)
15. Secure messaging protocols like Signal need both IND-CPA secure encryption and EUF-CMA secure MACs/AEAD schemes to protect confidentiality and integrity. (**True**)
16. In Signal, AEAD (such as AES-GCM or ChaCha20-Poly1305) is used in combination with per-message keys derived from the Double Ratchet to provide IND-CCA-style protection for each message. (**True**)
17. Controlling the Signal server allows an attacker to break end-to-end encryption, even if clients verify identity keys. (**False**)
18. The threat model for secure messaging explicitly includes temporary device compromise and assumes the attacker can record all traffic. (**True**)
19. The initial X3DH handshake must be performed while both parties are online, similar to a TLS handshake. (**False**)
20. In a properly implemented Signal-like protocol, learning a single message key reveals all past and future message keys in that conversation. (**False**)

#### Extra True/False – Week 11 Interactions

21. Signal’s Double Ratchet can be combined with a post-quantum KEM in X3DH to provide both post-quantum confidentiality and post-compromise security in the same protocol. (**True**)
22. Forward secrecy in Signal is primarily provided by the symmetric ratchet alone, while the DH ratchet is mainly for authentication. (**False**)
23. Post-compromise security relies on injecting fresh DH outputs into the ratchet over time, so that once an attacker loses access to the device, new keys eventually become independent of the compromised state. (**True**)
24. If the Signal server is fully malicious but cannot break the underlying cryptography, it can still read plaintext messages as long as it controls device registration. (**False**)
25. Deniability in Signal is conceptually closer to the properties of MAC-based authentication than to traditional digital signatures, because verifiers only see symmetric evidence. (**True**)
