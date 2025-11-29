##  Learning Goals
- Key exchange over public channels
- Discrete log cryptography

##  [[W8 Key definitions|Key Definitions]]
- Discrete Log Problem:
- CDH:
- DDH:

##  [[W8 Core Concepts|Core Concepts]]
- Diffie–Hellman key exchange
- Man-in-the-middle attacks
- ElGamal encryption

##  [[W8 Attacks and Pitfalls|Attacks & Pitfalls]]
- Missing authentication in DH
- Reused randomness in ElGamal

##  [[W8 Intuition and Examples|Intuition & Examples]]
- Mixing secret colors analogy

[[ElGamal worked example]]
##  Exam Notes
- Diffie–Hellman solves the key agreement problem.
- Security is based on DLP, CDH, and DDH.
- Diffie–Hellman alone provides no authentication.
- Man-in-the-middle attacks break unauthenticated DH.
- ElGamal is a public-key encryption scheme based on DH.
- ElGamal is randomized and IND-CPA secure under DDH.
- Reusing ElGamal randomness breaks security.
- ElGamal is malleable and needs authentication for full security.
- Ephemeral keys provide forward secrecy.

##  Links
- Prev: [[W7 Notes]]
- Next: [[W9 Notes]]