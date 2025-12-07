# Cryptography Fundamentals

This is the **central hub for all weekly lecture notes** in Cryptography.  
Each week links to a dedicated note with **formal definitions, proofs, attacks, worked examples, and exam-ready summaries**.

---

## [[Slides, Excercises and Solutions.canvas|Slides, Excercises and Solutions PDFs]]

---

## Problem Sheets (Solved)
- [[Problem Sheet 1]]
- [[Problem Sheet 2]]
- [[Problem Sheet 3]]
- [[Problem Sheet 4]]
- [[Problem Sheet 5]]
- [[Problem Sheet 6]]
- [[Problem Sheet 7]]
- [[Problem Sheet 8]]
- [[Problem Sheet 9]]
- [[Problem Sheet 10]]
- [[Problem Sheet 11]]
- [[Problem Sheet 12]]

---

# Weeks

---

## Week 1 – Introduction & Security Mindset
- [[Week_1_Notes]]

**Topics**
- What is cryptography?
- Threat models & adversaries
- Kerckhoffs’ Principle
- Perfect vs computational security
- Historical ciphers (Caesar, Vigenère)
- One-Time Pad intuition
- Formal symmetric encryption definitions
- Game-based security & Real-vs-Random
- Why informal security arguments fail

---

## Week 2 – Symmetric Encryption & One-Time Pad
- [[Week_2_Notes]]

**Topics**
- Caesar & Vigenère ciphers
- One-Time Pad (OTP)
- Information-theoretic security
- Perfect secrecy proofs
- Key reuse attacks on OTP
- Real-or-Random (RoR) security definition
- Computational vs perfect security

---

## Week 3 – Pseudorandomness, PRFs & Block Ciphers
- [[Week_3_Notes]]

**Topics**
- Negligible functions
- Computational security
- Pseudorandom Functions (PRFs)
- PRP vs PRF
- Birthday bound
- PRF–PRP switching lemma
- Feistel construction & Luby–Rackoff theorem
- DES & AES security context

---

## Week 4 – Block Ciphers & Modes of Operation
- [[Week_4_Notes]]

**Topics**
- AES overview & round structure
- SubBytes, ShiftRows, MixColumns, AddRoundKey
- ECB mode (why it is insecure)
- CBC mode
- CTR mode
- Nonce & IV reuse attacks
- Left–Right vs Real–Random security
- Malleability
- AEAD & IND-CCA preview

---

## Week 5 – MACs & Authenticated Encryption
- [[Week_5_Notes]]

**Topics**
- Message Authentication Codes (MACs)
- EUF-CMA security
- Real vs Fake MAC libraries
- MACs from PRFs
- ECB-MAC insecurity
- CBC-MAC (fixed-length only)
- ECBC-MAC (variable length)
- Encrypt-then-MAC
- Why MAC-then-Encrypt fails
- IND-CCA security

---

## Week 6 – Hash Functions
- [[Week_6_Notes]]

**Topics**
- Preimage resistance
- Second-preimage resistance
- Collision resistance
- Birthday bound
- Salting & password hashing
- Random Oracle Model (ROM)
- Merkle–Damgård construction
- Length-extension attacks
- Why raw hashes cannot be used as MACs

---

## Week 7 – Public-Key Cryptography & RSA
- [[Week_7_Notes]]

**Topics**
- Public vs private keys
- Trapdoor one-way permutations
- RSA key generation
- RSA encryption & decryption
- Correctness proof
- Domain restrictions
- Why textbook RSA is insecure
- RSA-OAEP
- IND-CPA & IND-CCA for PKE
- Hybrid encryption overview

---

## Week 8 – Discrete Logarithms, Diffie–Hellman, ElGamal & LWE
- [[Week_8_Notes]]

**Topics**
- Cyclic groups
- Discrete Log Problem (DLP)
- Computational & Decisional Diffie–Hellman
- Diffie–Hellman key exchange
- Man-in-the-middle attacks
- ElGamal encryption
- IND-CPA security
- Learning With Errors (LWE)
- Regev’s PKE
- Post-quantum security motivation

---

## Week 9 – Digital Signatures
- [[Week_9_Notes]]

**Topics**
- Digital signature schemes
- Signing & verification
- Replay attacks vs forgeries
- EUF-CMA & SUF-CMA
- Non-repudiation
- Textbook RSA signatures (insecure)
- RSA multiplicative forgery attacks
- RSA-FDH
- Random Oracle Model in signatures
- One-time signatures (Lamport)

---

## Week 10 – Discrete Logs, Diffie–Hellman & Elliptic Curve Cryptography
- [[Week_10_Notes]]

**Topics**
- Additive & multiplicative groups modulo $n$
- Generators & group order
- Euler’s totient function
- Square-and-multiply exponentiation
- Discrete Log, CDH, DDH
- Diffie–Hellman protocol
- MITM attacks & authentication
- ElGamal revisited
- Elliptic curve groups
- ECDLP & ECDH
- Small-subgroup & invalid-curve attacks
- ECC vs RSA/DH efficiency

---

## Week 11 – Secure Messaging, X3DH & Signal Protocol
- [[Week_11_Notes]]

**Topics**
- Forward secrecy (FS)
- Post-compromise security (PCS)
- Symmetric & DH ratchets
- Double Ratchet algorithm
- Asynchronous secure messaging
- Identity keys, signed pre-keys, one-time pre-keys
- X3DH protocol
- Deniability
- TLS vs Signal threat models
- Secure messaging attack surfaces

---

## Week 12 – Quantum Computing & Post-Quantum Cryptography
- [[Week_12_Notes]]

**Topics**
- Qubits & quantum gates
- Superposition & entanglement
- Shor’s Algorithm
- Grover’s Algorithm
- Quantum attacks on RSA, DH, ECC
- Store-now-decrypt-later
- Mosca’s Theorem
- Post-quantum cryptography (PQC)
- Lattice-based crypto (LWE, Ring-LWE)
- NIST PQC standardization
- Hybrid classical + PQ key exchange
- Quantum cryptography vs post-quantum cryptography

---

## ✅ Week 13 – Final Exam Meta-Topics & Glue Concepts
- [[Week_13_Notes]]

**Topics**
- Deterministic vs randomized encryption
- Composition of encryption & authentication
  - Encrypt-then-MAC
  - MAC-then-Encrypt
  - Encrypt-and-MAC
- Key Derivation Functions (KDFs)
- Hybrid encryption & KEM–DEM paradigm
- Public-Key Infrastructure (PKI)
  - Certificate Authorities
  - Trust chains
  - Revocation
  - TOFU
- Random Oracle Model vs Standard Model
- HMAC and secure hash-based MACs
- Side-channel attacks (timing, power, cache, fault)
- Tight vs loose security reductions
- Randomness failures in cryptosystems
- Final cryptographic design rules & exam traps
