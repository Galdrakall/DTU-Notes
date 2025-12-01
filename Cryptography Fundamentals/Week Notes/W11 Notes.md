## Learning Goals
- Understand modern secure messaging
- Learn forward secrecy
[[W11 Theorems]]
##  [[W11 Key Definitions|Key Definitions]]
- Forward secrecy:
- Post-compromise security:
- Ratcheting:

## [[W11 Core Concepts|Core Concepts]]
- X3DH key exchange
- Signal key hierarchy
- Ephemeral vs long-term keys

## [[W11 Attacks and Pitfalls|Attacks & Pitfalls]]
- Key reuse
- Identity key compromise

##  [[W11 Intuition and Examples|Intuition & Examples]]
- Updating locks after every message

## Exam Notes
- Quantum computers break RSA, DH, and ECC via Shor’s algorithm.
- Grover’s algorithm weakens symmetric crypto by square root.
- Post-quantum cryptography is designed to resist quantum attacks.
- Main PQC families: lattice-based, code-based, hash-based.
- AES-256 and SHA-256 are considered quantum-safe.
- Hash-based signatures are provably quantum-safe (assuming strong hashes).
- Lattice-based crypto is the leading replacement for RSA/DH.
- Store-now-decrypt-later is a major real-world threat.

##  Links
- Prev: [[W10 Notes]]
- Next: [[W12 Notes]]