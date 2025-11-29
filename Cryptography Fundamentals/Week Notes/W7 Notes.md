##  Learning Goals
- Understand asymmetric encryption
- Learn the RSA system

##  [[W7 Key Definitions|Key Definitions]]
- Public key:
- Private key:
- Trapdoor function:

##  [[W7 Core Concepts|Core Concepts]]
- RSA key generation
- RSA encryption and decryption
- Euler’s totient φ(N)

##  [[W7 Attacks and Pitfalls|Attacks & Pitfalls]]
- Deterministic encryption
- Low-exponent attacks

##  [[W7 Intuition and Examples|Intuition & Examples]]
- Padlock analogy for PKC

##  Exam Notes
- Public-key crypto solves the key distribution problem.
- Public keys encrypt; private keys decrypt.
- RSA is based on modular exponentiation.
- RSA modulus is $N = p \cdot q$.
- Correctness follows from Euler’s theorem.
- Raw RSA is deterministic and not IND-CPA secure.
- RSA is malleable without padding.
- RSA must always be used with secure padding (e.g., OAEP in later weeks).
- RSA is used mainly for key exchange, not bulk encryption.
##  Links
- Prev: [[W6 Notes]]
- Next: [[W7 Notes]]