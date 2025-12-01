##  Learning Goals
- Authenticate senders
- Provide non-repudiation
[[W9 Theorems]]
##  [[W9 Key Definitions|Key Definitions]]
- Signature:
- Verification key:
- Signing key:

##  [[W9 Core Concepts|Core Concepts]]
- RSA signatures
- RSA-FDH
- Verification algorithms

## [[W9 Attacks and Pitfalls|Attacks & Pitfalls]]
- Replay attacks
- Chosen-message attacks

##  [[W9 Intuition and Examples|Intuition & Examples]]
-  Handwritten vs digital signatures

##  Exam Notes
- Digital signatures provide integrity, authenticity, and non-repudiation.
- They do NOT provide confidentiality.
- Security notion: EUF-CMA (unforgeability under chosen-message attack).
- Always use hash-and-sign.
- RSA signatures are based on RSA inversion.
- ECDSA is based on elliptic curve discrete logarithms.
- Collision resistance of the hash is critical.
- Reusing randomness in ECDSA breaks the private key.
- MACs are not signatures.

##  Links
- Prev: [[W8 Notes]]
- Next: [[W10 Notes]]